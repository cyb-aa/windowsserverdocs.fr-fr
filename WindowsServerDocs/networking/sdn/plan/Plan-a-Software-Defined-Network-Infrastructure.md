---
title: Planifier une infrastructure SDN (Software Defined Networking)
description: Cette rubrique fournit des informations sur la façon de planifier le déploiement de votre infrastructure SDN (Software Defined Network).
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.service: virtual-network
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: ea7e53c8-11ec-410b-b287-897c7aaafb13
ms.author: pashort
author: shortpatti
ms.date: 08/10/2018
ms.openlocfilehash: e2c125867b461cee9f694849db5c8f61be91211d
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70869943"
---
# <a name="plan-a-software-defined-network-infrastructure"></a>Planifier une infrastructure SDN (Software Defined Networking)

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

En savoir plus sur la planification du déploiement d’une infrastructure réseau à définition logicielle, y compris la configuration matérielle et logicielle requise. 


## <a name="prerequisites"></a>Prérequis
Cette rubrique décrit un certain nombre de configurations matérielle et logicielle requises, notamment :

-   **Groupes de sécurité configurés, emplacements des fichiers journaux et inscription DNS dynamique** Vous devez préparer votre centre de informations pour le déploiement du contrôleur de réseau, ce qui nécessite un ou plusieurs ordinateurs ou machines virtuelles et un ordinateur ou une machine virtuelle. Avant de pouvoir déployer le contrôleur de réseau, vous devez configurer les groupes de sécurité, les emplacements des fichiers journaux (si nécessaire) et l’inscription DNS dynamique.  Si vous n’avez pas préparé votre centre de données pour le déploiement du contrôleur de réseau, consultez [Configuration requise pour l’installation et la préparation pour le déploiement du contrôleur de réseau](Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md) .

-   **Réseau physique**  Vous devez accéder à vos périphériques réseau physiques pour configurer des réseaux locaux virtuels (VLAN), le routage, le protocole BGP, le pontage de centre de données (ETS) si vous utilisez une technologie RDMA et le protocole PFC (Data Center Bridging) si vous utilisez une technologie RDMA basée sur RoCE. Cette rubrique présente la configuration manuelle du commutateur, ainsi que l’homologation BGP sur les commutateurs/routeurs de couche 3, ou sur une machine virtuelle de routage et de serveur d’accès à distance (RRAS).   

-   **Hôtes de calcul physique**  Ces ordinateurs hôtes exécutent Hyper-V et sont requis pour héberger l’infrastructure SDN et les machines virtuelles clientes.  Un matériel réseau spécifique est requis dans ces ordinateurs hôtes pour des performances optimales, qui est décrit plus loin dans la section **matériel réseau** .  


## <a name="physical-network-and-compute-host-configuration"></a>Configuration du réseau physique et de l’hôte de calcul

Chaque hôte de calcul physique requiert une connectivité réseau via une ou plusieurs cartes réseau connectées à un ou plusieurs ports de commutateur physique.  Un réseau [local virtuel](https://en.wikipedia.org/wiki/Virtual_LAN) de couche 2 prend en charge les réseaux divisés en plusieurs segments de réseau logique. 

>[!TIP]
>Utilisez VLAN 0 pour les réseaux logiques en mode d’accès ou sans balise. 

>[!IMPORTANT]
>La mise en réseau définie par le logiciel Windows Server 2016 prend en charge l’adressage IPv4 pour Underlay et la superposition. IPv6 n'est pas pris en charge.

### <a name="logical-networks"></a>réseaux logiques

#### <a name="management-and-hnv-provider"></a>Fournisseur HNV et de gestion 

Tous les hôtes de calcul physiques doivent accéder au réseau logique de gestion et au réseau logique du fournisseur HNV.  Pour la planification des adresses IP, chaque ordinateur hôte de calcul physique doit avoir au moins une adresse IP affectée à partir du réseau logique de gestion. Le contrôleur de réseau requiert une adresse IP réservée pour servir d’adresse IP REST. 

Un serveur DHCP peut attribuer automatiquement des adresses IP pour le réseau de gestion, ou vous pouvez affecter manuellement une adresse IP statique. La pile SDN affecte automatiquement des adresses IP pour le réseau logique de fournisseur HNV pour les ordinateurs hôtes Hyper-V individuels à partir d’un pool d’adresses IP spécifié via et géré par le contrôleur de réseau. 

>[!NOTE]
>Le contrôleur de réseau affecte une adresse IP de fournisseur HNV à un hôte de calcul physique uniquement après que l’agent hôte du contrôleur de réseau a reçu la stratégie de réseau pour un ordinateur virtuel client spécifique. 


|                                                               Si…                                                               |                                                                                                                                                                          Alors…                                                                                                                                                                           |
|-----------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                  Les réseaux logiques utilisent des réseaux locaux virtuels,                                                  |                                                                 l’hôte de calcul physique doit se connecter à un port de commutateur Trunk qui a accès à ces réseaux locaux virtuels. Il est important de noter que les cartes réseau physiques sur l’ordinateur hôte ne doivent pas être activées pour le filtrage VLAN.                                                                 |
|                À l’aide de l’Association commutée intégrée (SET) et de plusieurs membres de l’équipe de cartes réseau, tels que les cartes réseau,                |                                                                                                                        vous devez connecter tous les membres de l’équipe de cartes réseau pour cet ordinateur hôte particulier au même domaine de diffusion de couche 2.                                                                                                                         |
| L’hôte de calcul physique exécute des machines virtuelles d’infrastructure supplémentaires, telles que le contrôleur de réseau, le SLB/MUX ou la passerelle. | cet ordinateur hôte doit avoir une adresse IP supplémentaire affectée à partir du réseau logique de gestion pour chacun des ordinateurs virtuels hébergés.<p>En outre, chaque machine virtuelle de l’infrastructure SLB/MUX doit avoir une adresse IP réservée pour le réseau logique du fournisseur HNV. Si vous ne disposez pas d’une adresse IP réservée, vous risquez d’obtenir des adresses IP en double sur votre réseau. |

---

Pour plus d’informations sur la virtualisation de réseau Hyper-V (HNV), que vous pouvez utiliser pour virtualiser des réseaux dans un déploiement Microsoft SDN, consultez [virtualisation de réseau Hyper-v](../technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md).  



#### <a name="gateways-and-the-software-load-balancer"></a>Passerelles et logiciels Load Balancer

Des réseaux logiques supplémentaires doivent être créés et configurés pour l’utilisation de la passerelle et de SLB. Veillez à obtenir les préfixes IP corrects, les ID de réseau local virtuel et les adresses IP de passerelle pour ces réseaux.


|                                 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **Réseau logique de transit**   | La passerelle RAS et SLB/MUX utilisent le réseau logique de transit pour échanger les informations d’appairage BGP et le trafic du client Nord/Sud (externe-interne). La taille de ce sous-réseau est généralement inférieure à celle des autres. Seuls les hôtes de calcul physique qui exécutent la passerelle RAS ou les machines virtuelles SLB/MUX doivent disposer d’une connectivité à ce sous-réseau avec ces réseaux locaux virtuels reliés et accessibles sur les ports de commutateur auxquels les cartes réseau des hôtes de calcul sont connectées. Chaque ordinateur virtuel de passerelle SLB/MUX ou RAS est affecté de manière statique à une adresse IP à partir du réseau logique de transit. |
| **Réseau logique d’adresses IP virtuelles publiques**  |                                                                                                                             Le réseau logique d’adresses IP virtuelles publiques doit avoir des préfixes de sous-réseau IP routables en dehors de l’environnement Cloud (généralement Internet routable).  Il s’agit des adresses IP frontales utilisées par les clients externes pour accéder aux ressources des réseaux virtuels, y compris l’adresse IP virtuelle frontale pour la passerelle de site à site.                                                                                                                             |
| **Réseau logique d’adresses IP virtuelles privées** |                                                                                                                                                                                       Le réseau logique d’adresses IP virtuelles privées ne doit pas obligatoirement être routable en dehors du Cloud, car il est utilisé pour les adresses IP virtuelles accessibles uniquement à partir de clients Cloud internes, tels que les Manager SLB ou les services privés.                                                                                                                                                                                       |
|   **Réseau logique d’adresses IP virtuelles GRE**   |                                                                                                                                           Le réseau d’adresses IP virtuelles GRE est un sous-réseau qui existe uniquement pour définir les adresses IP virtuelles qui sont affectées aux machines virtuelles de passerelle en cours d’exécution sur votre infrastructure SDN pour un type de connexion GRE S2S. Ce réseau n’a pas besoin d’être préconfiguré dans vos commutateurs physiques ou votre routeur et aucun réseau local virtuel n’est nécessaire.                                                                                                                                            |

---


#### <a name="sample-network-topology"></a>Exemple de topologie de réseau
Modifiez l’exemple de préfixes de sous-réseau IP et d’ID de réseau local virtuel pour votre environnement. 


| **Nom du réseau** |  **Sous-réseau**  | **Filtrage** | **ID de réseau local virtuel sur le camion** | **Gateway**  |                                                           **Réservations (exemples)**                                                           |
|------------------|--------------|----------|----------------------|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
|    gestion    | 10.184.108.0 |    24    |          7           | 10.184.108.1 | 10.184.108.1 – routeur 10.184.108.4-contrôleur de réseau 10.184.108.10-hôte de calcul 110.184.108.11-hôte de calcul 210.184.108. X-hôte de calcul X |
|   Fournisseur HNV   |  10.10.56.0  |    23    |          11          |  10.10.56.1  |                                                    10.10.56.1 – routeur 10.10.56.2-SLB/MUX1                                                     |
|     Transition      |  10.10.10.0  |    24    |          10          |  10.10.10.1  |                                                               10.10.10.1 – routeur                                                               |
|    Adresse IP virtuelle publique    |  41.40.40.0  |    27    |          N/D          |  41.40.40.1  |                                    41.40.40.1 – routeur 41.40.40.2-SLB/multiplex VIP 41.40.40.3-adresse IP virtuelle VPN S2S IPSec                                    |
|   Adresse IP virtuelle privée    |  20.20.20.0  |    27    |          N/D          |  20.20.20.1  |                                                        20.20.20.1-GW (routeur) par défaut                                                         |
|     ADRESSE IP VIRTUELLE GRE      |  31.30.30.0  |    24    |          N/D          |  31.30.30.1  |                                                             31.30.30.1-GW par défaut                                                             |

---

### <a name="logical-networks-required-for-rdma-based-storage"></a>Réseaux logiques requis pour le stockage basé sur RDMA  

Si vous utilisez un stockage basé sur RDMA, définissez un réseau local virtuel et un sous-réseau pour chaque carte physique (deux cartes par nœud) dans vos hôtes de calcul et de stockage.  

>[!IMPORTANT]
>Pour que la qualité de service (QoS) soit appliquée de manière appropriée, les commutateurs physiques nécessitent un VLAN balisé pour le trafic RDMA.

| **Nom du réseau** |  **Sous-réseau**  | **Filtrage** | **ID de réseau local virtuel sur le camion** | **Gateway**  |                                                           **Réservations (exemples)**                                                            |
|------------------|--------------|----------|----------------------|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
|     Storage1     |  10.60.36.0  |    25    |          8           |  10.60.36.1  |  10.60.36.1 – routeur<p>10.60.36. x-hôte de calcul X<p>10.60.36. y-hôte de calcul Y<p>10.60.36. V-cluster de calcul<p>10.60.36. W-cluster de stockage  |
|     Storage2     | 10.60.36.128 |    25    |          9           | 10.60.36.129 | 10.60.36.129 – routeur<p>10.60.36. x-hôte de calcul X<p>10.60.36. y-hôte de calcul Y<p>10.60.36. V-cluster de calcul<p>10.60.36. W-cluster de stockage |

---


## <a name="routing-infrastructure"></a>Infrastructure de routage  

Si vous déployez votre infrastructure SDN à l’aide de scripts, les sous-réseaux de gestion, de fournisseur HNV, de transit et d’adresse IP virtuelle doivent être routables les uns avec les autres sur le réseau physique.     

Les informations \(de routage, par exemple\) le tronçon suivant pour les sous-réseaux d’adresses IP virtuelles, sont publiées par les passerelles de type SLB/MUX et RAS dans le réseau physique à l’aide de l’appairage BGP interne. Les réseaux logiques d’adresses IP virtuelles n’ont pas de réseau local virtuel affecté et ne sont pas préconfigurés dans le commutateur de couche 2 (par exemple, commutateur haut de la rack).  

Vous devez créer un homologue BGP sur le routeur utilisé par votre infrastructure SDN pour recevoir des itinéraires pour les réseaux logiques d’adresses IP virtuelles publiés par les passerelles de type SLB/multiplexeurs et RAS. L’homologation BGP ne doit se produire qu’une seule méthode (de la passerelle SLB/MUX ou RAS vers l’homologue BGP externe).  Au-dessus de la première couche de routage, vous pouvez utiliser des itinéraires statiques ou un autre protocole de routage dynamique tel que OSPF. Toutefois, comme indiqué précédemment, le préfixe de sous-réseau IP pour les réseaux logiques d’adresses IP virtuelles doit être routable à partir du réseau physique vers l’homologue BGP externe.   

L’homologation BGP est généralement configurée dans un commutateur ou un routeur géré dans le cadre de l’infrastructure réseau. L’homologue BGP peut également être configuré sur un serveur Windows avec le rôle de serveur d’accès à distance (RAS) installé en mode routage uniquement. Cet homologue de routeur BGP dans l’infrastructure réseau doit être configuré pour avoir son propre ASN et autoriser l’homologation à partir d’un ASN affecté aux \(\)passerelles SDN et SLB/MUX et RAS. Vous devez obtenir les informations suivantes auprès de votre routeur physique, ou à partir de l’administrateur réseau pour le contrôle de ce routeur :

- ASN du routeur  
- Adresse IP du routeur  
- ASN pour une utilisation par les composants SDN (peut être n’importe quel numéro à partir de la plage ASN privée)

>[!NOTE]
>Les ASN de quatre octets ne sont pas pris en charge par le SLB/MUX. Vous devez allouer deux ASN d’octets au SLB/MUX et au routeur WO qu’il connecte. Vous pouvez utiliser des ASN de 4 octets ailleurs dans votre environnement.  

Vous ou votre administrateur réseau devez configurer l’homologue de routeur BGP pour qu’il accepte les connexions à partir de l’adresse IP et de l’adresse IP ou du sous-réseau du réseau logique de transit que votre passerelle RAS et SLB/multiplexeurs utilisent.

Pour plus d’informations, consultez [Border Gateway Protocol (BGP)](../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md).

## <a name="default-gateways"></a>Passerelles par défaut
Les ordinateurs qui sont configurés pour se connecter à plusieurs réseaux, tels que les ordinateurs hôtes physiques et les machines virtuelles de passerelle, ne doivent avoir qu’une seule passerelle par défaut configurée. Configurez la passerelle par défaut sur la carte utilisée pour accéder à Internet.

Pour les machines virtuelles, suivez ces règles pour décider quel réseau utiliser comme passerelle par défaut :

1. Utilisez le réseau logique de transit comme passerelle par défaut si un ordinateur virtuel se connecte au réseau de transit, ou s’il est multi-hébergé sur le réseau de transit ou sur tout autre réseau.
2. Utilisez le réseau de gestion comme passerelle par défaut si un ordinateur virtuel se connecte uniquement au réseau de gestion. 
3. Utilisez le réseau du fournisseur HNV pour les passerelles SLB/multiplexeurs et RAS. N’utilisez pas le réseau du fournisseur HNV comme passerelle par défaut. 
4. Ne connectez pas les machines virtuelles directement aux réseaux d’adresses IP virtuelles Storage1, Storage2, VIP publiques ou privées.

Pour les hôtes Hyper-V et les nœuds de stockage, utilisez le réseau de gestion comme passerelle par défaut.  Aucune passerelle par défaut ne doit être affectée aux réseaux de stockage.


## <a name="network-hardware"></a>Matériel réseau

Vous pouvez utiliser les sections suivantes pour planifier le déploiement de matériel réseau.

### <a name="network-interface-cards-nics"></a>Cartes d’interface réseau (NIC)

Les cartes d’interface réseau (NIC) utilisées dans vos hôtes Hyper-V et les hôtes de stockage requièrent des fonctionnalités spécifiques pour obtenir des performances optimales. 

L’accès direct à la mémoire à distance (RDMA, Remote Direct Memory Access) est une technique de contournement du noyau qui permet de transférer de grandes quantités de données sans utiliser le processeur de l’ordinateur hôte, ce qui libère le processeur afin d’effectuer d’autres tâches. 

Switch Embedded Teaming (SET) est une autre solution d’association de cartes réseau que vous pouvez utiliser dans les environnements qui incluent Hyper-V et la pile SDN (Software Defined Networking) dans Windows Server 2016. SET intègre certaines fonctionnalités d’association de cartes réseau dans le commutateur virtuel Hyper-V. 

Pour plus d’informations, consultez [accès direct à la mémoire à distance (RDMA) et Switch Embedded Teaming (Set)](../../../virtualization//hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).   

Pour tenir compte de la surcharge liée au trafic de réseau virtuel client causé par des en-têtes d’encapsulation VXLAN ou NVGRE, l’unité de transmission maximale (MTU) du réseau de l’infrastructure de couche 2 (commutateurs \(et hôtes) doit être définie sur une valeur supérieure ou égale à 1674 octets, y compris Ethernet de couche 2 en-têtes.\) 

Les cartes réseau qui prennent en charge le nouveau mot clé *EncapOverhead* Advanced adapter fixent la MTU automatiquement via l’agent hôte du contrôleur de réseau. Les cartes réseau qui ne prennent pas en charge le nouveau mot clé *EncapOverhead* doivent définir la taille MTU manuellement sur chaque hôte physique à\) l’aide du mot clé *JumboPacket* \(ou équivalent. 


### <a name="switches"></a>Active

Lorsque vous sélectionnez un commutateur physique et un routeur pour votre environnement, assurez-vous qu’il prend en charge l’ensemble de fonctionnalités suivant :  

- Paramètres \(MTU switchport requis\)  
- MTU défini sur > = 1674 octets \(, y compris l’en-tête L2-Ethernet\)  
- Protocoles \(L3 requis\)  
- ECMP  
- ECMP \(basé sur BGP\)IETF RFC 4271\-

Les implémentations doivent prendre en charge les instructions dans les normes IETF suivantes.

- RFC 2545 : « BGP-4 Multi-Protocol Extensions for IPv6 Inter-Domain Routing »  
- RFC 4760 : « Extensions multiprotocoles pour BGP-4 »  
- RFC 4893 : « Prise en charge du protocole BGP pour les quatre octets comme un espace numérique »  
- RFC 4456 : «Réflexion de l’itinéraire BGP : Alternative à la maille complète BGP interne (IBGP)»  
- RFC 4724 : « Mécanisme de redémarrage approprié pour BGP »  

Les protocoles de balisage suivants sont requis.

- Isolement VLAN de différents types de trafic
- 802.1 q Trunk

Les éléments suivants fournissent un contrôle de lien.

- Quality of service \(PFC requis uniquement si vous utilisez RoCE\)
- Sélection \(de trafic améliorée 802.1 Qaz\)
- 802.1 de contrôle \(de workflow basé sur la priorité p/Q et 802.1 qbb\)

Les éléments suivants assurent la disponibilité et la redondance.

- Disponibilité du commutateur (obligatoire)
- Un routeur à haut niveau de disponibilité est requis pour exécuter des fonctions de passerelle. Pour ce faire, vous pouvez utiliser un commutateur à plusieurs châssis \ ou des technologies telles que VRRP.

Les éléments suivants fournissent des fonctionnalités de gestion.

**Surveillance**

- SNMP v1 ou SNMP v2 (requis si le contrôleur de réseau est utilisé pour l’analyse du commutateur physique)  
- MIB \(SNMP requis si vous utilisez le contrôleur de réseau pour l’analyse du commutateur physique\)  
- MIB-II (RFC 1213), LLDP, interface MIB \(RFC 2863\), if-MIB, IP-MIB, IP-Forward-MIB, Q-Bridge-MIB, Bridge-MIB, LLDB-MIB, entité-MIB, IEEE8023-lag-MIB  

Les diagrammes suivants montrent un exemple de configuration à quatre nœuds. À des fins de clarté, le premier diagramme affiche uniquement le contrôleur de réseau, le second montre le contrôleur de réseau plus l’équilibreur de charge logiciel et le troisième schéma montre le contrôleur de réseau, l’équilibreur de charge logiciel et la passerelle.  

Ces diagrammes n’affichent pas les réseaux de stockage et cartes réseau virtuelles. Si vous envisagez d’utiliser le stockage SMB, ceux-ci sont requis.

Les machines virtuelles de l’infrastructure et du locataire peuvent être redistribuées sur n’importe quel hôte de calcul physique (en supposant que la connectivité réseau correcte existe pour les réseaux logiques corrects).  



## <a name="switch-configuration-examples"></a>Exemples de configuration de commutateur  

Pour faciliter la configuration de votre commutateur ou routeur physique, un ensemble d’exemples de fichiers de configuration pour un large éventail de modèles de commutateur et de fournisseurs sont disponibles dans le [dépôt github Microsoft SDN](https://github.com/microsoft/SDN/tree/master/SwitchConfigExamples). Un fichier Lisez-moi détaillé et des commandes d’interface de ligne de commande (CLI) testées pour des commutateurs spécifiques sont fournis.         


## <a name="compute"></a>Calcul  
Pour tous les ordinateurs hôtes Hyper-V, Windows Server 2016 doit être installé, Hyper-V est activé et un commutateur virtuel Hyper-V externe créé avec au moins une carte physique connectée au réseau logique de gestion. L’hôte doit être accessible via une adresse IP de gestion affectée à l’hôte de gestion carte réseau virtuelle.  

Tout type de stockage compatible avec Hyper-V, partagé ou local, peut être utilisé.   

> [!TIP]  
> C’est pratique si vous utilisez le même nom pour tous vos commutateurs virtuels, mais ce n’est pas obligatoire. Si vous envisagez de déployer à l’aide de scripts, consultez `vSwitchName` le commentaire associé à la variable dans le fichier config. psd1.  

**Exigences de calcul pour l’hôte**  
Le tableau suivant indique la configuration matérielle et logicielle minimale requise pour les quatre hôtes physiques utilisés dans l’exemple de déploiement.  

Hôte|Configuration matérielle requise|Configuration logicielle|  
--------|-------------------------|-------------------------  
|Hôte Hyper-v physique|PROCESSEUR 4 cœurs 2,66 GHz<br /><br />32 Go de RAM<br /><br />300 Go d’espace disque<br /><br />carte réseau physique 1 Go/s (ou plus rapide)|SYSTÈME D’EXPLOITATION Windows Server 2016<br /><br />Rôle Hyper-V installé|  


**Configuration requise pour le rôle d’ordinateur virtuel de l’infrastructure SDN**  

Rôle|exigences relatives aux processeurs virtuels|Mémoire requise|Configuration requise pour le disque|  
--------|---------------------|-----------------------|---------------------  
|Contrôleur de réseau (trois nœuds)|4 processeurs virtuels|4 Go min (8 Go recommandés)|75 Go pour le lecteur de système d’exploitation  
|SLB/MUX (trois nœuds)|8 processeurs virtuels|8 Go recommandés|75 Go pour le lecteur de système d’exploitation  
|Passerelle du serveur d’accès à distance<br /><br />(pool unique de trois passerelles de nœud, deux actives, une passive)|8 processeurs virtuels|8 Go recommandés|75 Go pour le lecteur de système d’exploitation  
|Routeur BGP de la passerelle RAS pour l’homologation SLB/MUX<br /><br />(vous pouvez également utiliser le commutateur comme routeur BGP)|2 processeurs virtuels|2 Go|75 Go pour le lecteur de système d’exploitation|  


Si vous utilisez VMM pour le déploiement, des ressources de machine virtuelle d’infrastructure supplémentaires sont requises pour VMM et d’autres infrastructures non SDN. Pour plus d’informations, consultez [recommandations matérielles minimales pour System Center Technical Preview.](https://technet.microsoft.com/library/dn997303.aspx)  

## <a name="extending-your-infrastructure"></a>Extension de votre infrastructure  
Le dimensionnement et les besoins en ressources de votre infrastructure dépendent des machines virtuelles de charge de travail client que vous envisagez d’héberger. Le processeur, la mémoire et les disques requis pour les machines virtuelles d’infrastructure (par exemple, contrôleur de réseau, SLB, passerelle, etc.) sont répertoriés dans le tableau précédent. Vous pouvez ajouter d’autres machines virtuelles d’infrastructure pour monter en charge en fonction des besoins. Toutefois, les machines virtuelles clientes qui s’exécutent sur les hôtes Hyper-V ont leurs propres besoins en termes de processeur, de mémoire et de disque que vous devez prendre en compte.   

Lorsque les machines virtuelles de charge de travail des locataires commencent à consommer trop de ressources sur les hôtes Hyper-V physiques, vous pouvez étendre votre infrastructure en ajoutant des hôtes physiques supplémentaires. Pour ce faire, vous pouvez utiliser Virtual Machine Manager ou des scripts PowerShell (selon la façon dont vous avez déployé initialement l’infrastructure) pour créer des ressources de serveur via le contrôleur de réseau. Si vous devez ajouter des adresses IP supplémentaires pour le réseau du fournisseur HNV, vous pouvez créer des sous-réseaux logiques (avec les pools d’adresses IP correspondants) que les hôtes peuvent utiliser.  


## <a name="see-also"></a>Voir aussi  
[Configuration requise pour l’installation et la préparation du déploiement du contrôleur de réseau](Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md)  
[SDN de mise &#40;en réseau définie par logiciel&#41;](../Software-Defined-Networking--SDN-.md)  



