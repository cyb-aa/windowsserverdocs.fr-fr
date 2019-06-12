---
title: Planifier une infrastructure SDN (Software Defined Networking)
description: Cette rubrique fournit des informations sur la façon de planifier votre déploiement d’infrastructure de réseau SDN (Software Defined).
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
ms.openlocfilehash: 16511628e979a95433360b0a3e900a89e9ba56eb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446277"
---
# <a name="plan-a-software-defined-network-infrastructure"></a>Planifier une infrastructure SDN (Software Defined Networking)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

En savoir plus sur la planification du déploiement pour une infrastructure de réseau à définition logicielle, y compris la matérielle et logicielle requise. 


## <a name="prerequisites"></a>Prérequis
Cette rubrique décrit certaines logicielles et matérielles requises, y compris :

-   **Configuré des groupes de sécurité, les emplacements des fichiers journaux et l’inscription DNS dynamique** vous devez préparer votre centre de données pour le déploiement de contrôleur de réseau, ce qui nécessite un ou plusieurs ordinateurs ou machines virtuelles et un ordinateur ou machine virtuelle. Avant de déployer le contrôleur de réseau, vous devez configurer les groupes de sécurité, emplacements des fichiers journaux (si nécessaire) et l’inscription DNS dynamique.  Si vous n’avez pas préparé votre centre de données pour le déploiement du contrôleur de réseau, consultez [Installation en matière de préparation pour le déploiement d’un contrôleur de réseau](Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md) pour plus d’informations.

-   **Réseau physique** vous devez accéder à vos périphériques réseau physiques pour configurer des réseaux locaux virtuels, routage, BGP, Data Center Bridging (ETS) si vous utilisez une technologie RDMA, et Data Center Bridging (PFC) si vous utilisez un RoCE en fonction de la technologie RDMA. Cette rubrique présente une configuration manuelle de commutateur, ainsi que l’homologation BGP sur les commutateurs de couche 3 / routeurs ou une machine virtuelle de routage et le serveur d’accès distant (RRAS).   

-   **Hôtes de calcul physiques** ces hôtes exécutent Hyper-V et sont nécessaires pour héberger SDN infrastructure et client des ordinateurs virtuels.  Matériel réseau spécifique est requis dans ces ordinateurs hôtes pour de meilleures performances, ce qui est décrit plus loin dans le **matériel réseau** section.  


## <a name="physical-network-and-compute-host-configuration"></a>Configuration d’hôte de calcul et réseau physique

Chaque hôte physique calcul nécessite une connectivité réseau via un ou plusieurs cartes réseau attachée à un ou plusieurs ports de commutateur physique.  Une couche 2 [VLAN](https://en.wikipedia.org/wiki/Virtual_LAN) prend en charge les réseaux divisés en plusieurs segments de réseau logique. 

>[!TIP]
>Utilisez des VLAN 0 pour les réseaux logiques en mode d’accès ou sans balises. 

>[!IMPORTANT]
>Windows Server 2016 Sdn prend en charge l’adressage IPv4 pour la sous-couche et la superposition. IPv6 n'est pas pris en charge.

### <a name="logical-networks"></a>Réseaux logiques

#### <a name="management-and-hnv-provider"></a>Gestion et fournisseur HNV 

Tous les hôtes de calcul physiques doivent accéder au réseau logique de gestion et le réseau logique de fournisseur HNV.  Pour l’adresse IP à des fins de planification, chaque hôte de calcul physique doit avoir au moins une adresse IP affectée à partir du réseau logique de gestion. Le contrôleur de réseau nécessite une adresse IP réservée pour servir de l’adresse IP REST. 

Un serveur DHCP peut affecter automatiquement des adresses IP pour le réseau de gestion, ou vous pouvez affecter manuellement l’adresse IP statique. La pile SDN affecte automatiquement des adresses IP pour le fournisseur HNV est spécifié par le biais de réseau logique pour les hôtes Hyper-V individuels à partir d’un Pool d’adresses IP et gérés par le contrôleur de réseau. 

>[!NOTE]
>Le contrôleur de réseau attribue une adresse IP de fournisseur HNV à un hôte de calcul physiques uniquement une fois que l’Agent hôte du contrôleur de réseau reçoit la stratégie de réseau pour une machine virtuelle de locataire spécifique. 


|                                                               Si…                                                               |                                                                                                                                                                          Alors…                                                                                                                                                                           |
|-----------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                  Les réseaux logiques utilisent des réseaux locaux virtuels,                                                  |                                                                 l’hôte de calcul physique doit se connecter à un port de commutateur relié en mode trunk qui a accès à ces réseaux locaux virtuels. Il est important de noter que les cartes réseau physiques sur l’ordinateur hôte ne doivent pas avoir un filtrage de réseau local virtuel activé.                                                                 |
|                À l’aide de Switched-Embedded Teaming (SET) et avoir plusieurs membres de l’équipe de cartes réseau, telles que des cartes réseau,                |                                                                                                                        Vous devez connecter tous les membres de l’équipe de cartes réseau de cet hôte particulier pour le même domaine de diffusion de couche 2.                                                                                                                         |
| L’hôte de calcul physiques s’exécute les machines virtuelles infrastructure supplémentaires, telles que le contrôleur de réseau, SLB/MUX ou passerelle, | Cet hôte doit avoir une adresse IP supplémentaire attribuée à partir du réseau logique de gestion pour chacune des machines virtuelles hébergées.<p>En outre, chaque machine virtuelle d’infrastructure SLB/MUX doit avoir une adresse IP réservée pour le réseau logique de fournisseur HNV. L’absence d’une adresse IP réservée peut entraîner des adresses IP en double sur votre réseau. |

---

Pour plus d’informations sur Hyper-V virtualisation de réseau (HNV), qui vous permet de virtualiser des réseaux dans un déploiement de Microsoft SDN, consultez [virtualisation de réseau Hyper-V](../technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md).  



#### <a name="gateways-and-the-software-load-balancer"></a>Passerelles et l’équilibreur de charge logiciel

Les réseaux logiques supplémentaires doivent être créés et configurés pour la passerelle et l’utilisation SLB. Veillez à obtenir des préfixes d’adresse IP correctes, ID de VLAN, les adresses IP de passerelle pour ces réseaux.


|                                 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **Réseau logique de transit**   | La passerelle RAS et le SLB/MUX utilisent le réseau logique de Transit pour échanger des informations sur l’homologation BGP et le trafic de locataire (externe interne) Nord/Sud. La taille de ce sous-réseau sera généralement plus petite que les autres. Uniquement les hôtes de calcul physiques qui exécutent passerelle RAS ou machines virtuelles SLB/MUX doivent être connectés à ce sous-réseau avec ces réseaux locaux virtuels reliés et accessibles sur les ports de commutateur auquel les cartes réseau hôtes du calcul sont connectées. Chaque SLB/MUX ou une machine virtuelle de passerelle RAS est statiquement une adresse IP affectée à partir du réseau logique de Transit. |
| **Réseau logique d’adresses IP virtuelles publiques**  |                                                                                                                             Le réseau logique d’adresse IP virtuelle publique est nécessaire pour que les préfixes de sous-réseau IP qui sont routables en dehors de l’environnement de cloud (généralement Internet routable).  Ils seront les adresses IP frontales utilisés par les clients externes pour accéder aux ressources dans les réseaux virtuels, y compris l’adresse IP virtuelle de front-end pour la passerelle de Site à site.                                                                                                                             |
| **Réseau logique d’adresses IP virtuelles privées** |                                                                                                                                                                                       Le réseau logique d’adresses IP virtuelles privées n’est pas obligatoire pour être routable en dehors du cloud qu’il est utilisé pour les adresses IP virtuelles qui sont uniquement accessibles à partir de clients cloud internes, tels que le SLB Manager ou les services privé.                                                                                                                                                                                       |
|   **Réseau logique d’adresses IP virtuelles GRE**   |                                                                                                                                           Le réseau d’adresses IP virtuelles GRE est un sous-réseau qui existe uniquement pour définir les adresses IP virtuelles affectées aux machines virtuelles de passerelle en cours d’exécution sur votre infrastructure SDN pour une connexion S2S GRE. Ce réseau ne pas besoin d’être préconfiguré dans vos commutateurs physiques ou d’un routeur et un réseau local virtuel attribué n’ont ne pas besoin.                                                                                                                                            |

---


#### <a name="sample-network-topology"></a>Exemple de topologie de réseau
Modifier les préfixes de sous-réseau IP exemple et l’ID de VLAN pour votre environnement. 


| **Nom de réseau** |  **Sous-réseau**  | **Masque** | **ID VLAN sur camion** | **Passerelle**  |                                                           **Réservations (exemples)**                                                           |
|------------------|--------------|----------|----------------------|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
|    Gestion    | 10.184.108.0 |    24    |          7           | 10.184.108.1 | 10.184.108.1 – Router10.184.108.4 - réseau Controller10.184.108.10 - calcul host 110.184.108.11 - calcul héberger 210.184.108.X - calcul hôte X |
|   Fournisseur HNV   |  10.10.56.0  |    23    |          11          |  10.10.56.1  |                                                    10.10.56.1 – Router10.10.56.2 - SLB/MUX1                                                     |
|     Transit      |  10.10.10.0  |    24    |          10          |  10.10.10.1  |                                                               10.10.10.1 – routeur                                                               |
|    Adresse IP virtuelle publique    |  41.40.40.0  |    27    |          N/A          |  41.40.40.1  |                                    41.40.40.1 – Router41.40.40.2 SLB/MUX VIP41.40.40.3 - IPSec S2S VPN VIP                                    |
|   Adresses IP virtuelles privées    |  20.20.20.0  |    27    |          N/A          |  20.20.20.1  |                                                        20.20.20.1 - GW par défaut (routeur)                                                         |
|     ADRESSES IP VIRTUELLES GRE      |  31.30.30.0  |    24    |          N/A          |  31.30.30.1  |                                                             31.30.30.1 - passerelle par défaut                                                             |

---

### <a name="logical-networks-required-for-rdma-based-storage"></a>Réseaux logiques requis pour le stockage basé sur RDMA  

Si vous utilisez le stockage basé sur le RDMA, vous devez définir un réseau local virtuel et un sous-réseau pour chaque carte physique (deux cartes par nœud) dans vos hôtes de calcul et de stockage.  

>[!IMPORTANT]
>Pour la qualité de Service (QoS) pour être appliquées de façon appropriée, commutateurs physiques nécessitent un réseau local virtuel balisé pour le trafic RDMA.

| **Nom de réseau** |  **Sous-réseau**  | **Masque** | **ID VLAN sur camion** | **Passerelle**  |                                                           **Réservations (exemples)**                                                            |
|------------------|--------------|----------|----------------------|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
|     storage1     |  10.60.36.0  |    25    |          8           |  10.60.36.1  |  10.60.36.1 – routeur<p>10.60.36.X - calcul hôte X<p>10.60.36.Y - calcul hôte Y<p>10.60.36.V - cluster de calcul<p>10.60.36.W - cluster de stockage  |
|     storage2     | 10.60.36.128 |    25    |          9           | 10.60.36.129 | 10.60.36.129 – routeur<p>10.60.36.X - calcul hôte X<p>10.60.36.Y - calcul hôte Y<p>10.60.36.V - cluster de calcul<p>10.60.36.W - cluster de stockage |

---


## <a name="routing-infrastructure"></a>Infrastructure de routage  

Si vous déployez votre infrastructure SDN à l’aide de scripts, la gestion, de fournisseur HNV, de Transit, et les sous-réseaux d’adresses IP virtuelles doivent être routables entre eux sur le réseau physique.     

Informations de routage \(saut suivant, par exemple\) pour l’adresse IP virtuelle sous-réseaux est publié par le SLB/MUX et les passerelles de RAS au réseau physique à l’aide de l’homologation de BGP interne. Les réseaux logiques d’adresses IP virtuelles n’ont pas d’un réseau local virtuel attribué et n’est pas préconfigurés dans le commutateur de couche 2 (par exemple, le commutateur Top-of-Rack).  

Vous devez créer un homologue BGP sur le routeur qui est utilisé par votre infrastructure SDN pour recevoir des itinéraires pour les réseaux logiques VIP publiés par le SLB/MUX et les passerelles de RAS. L’homologation BGP doit uniquement se produire une façon (à partir de SLB/MUX ou passerelle RAS pour l’homologue BGP externe).  Au-dessus de la première couche de routage vous pouvez utiliser des itinéraires statiques ou un autre protocole de routage dynamique comme OSPF, toutefois, comme indiqué précédemment, le préfixe de sous-réseau IP pour les réseaux logiques d’adresses IP virtuelles doivent-ils être routable à partir du réseau physique de l’homologue BGP externe.   

L’homologation BGP est généralement configurée dans un commutateur géré ou un routeur dans le cadre de l’infrastructure réseau. L’homologue BGP peut également être configuré sur un serveur Windows avec le rôle de serveur d’accès distant (RAS) installé dans un mode de routage uniquement. Ce routeur de l’homologue BGP dans l’infrastructure réseau doit être configuré pour avoir son propre ASN et autoriser l’homologation à partir d’un ASN est affecté aux composants SDN \(SLB/MUX et les passerelles de RAS\). Vous devez obtenir les informations suivantes à partir de votre routeur physique, ou à partir de l’administrateur de réseau dans le contrôle de ce routeur :

- Routeur ASN  
- Adresse IP du routeur  
- ASN pour une utilisation par les composants SDN (peut être pas n’importe quel numéro AS à partir de la plage ASN privée)

>[!NOTE]
>ASN de quatre octets n’est pas pris en charge par le SLB/MUX. Vous devez allouer deux octets NSA pour le SLB/MUX et le routeur wo dont il se connecte. Vous pouvez utiliser sur 4 octets NSA ailleurs dans votre environnement.  

Vous ou votre administrateur réseau doit configurer le routeur de l’homologue BGP pour accepter les connexions à partir de l’ASN et l’adresse IP ou une adresse de sous-réseau du réseau logique de Transit qui utilisent votre passerelle RAS et le SLB/MUX.

Pour plus d’informations, consultez [protocole BGP (Border Gateway)](../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md).

## <a name="default-gateways"></a>Passerelles par défaut
Les ordinateurs qui sont configurés pour se connecter à plusieurs réseaux, tels que les hôtes physiques et les machines virtuelles de passerelle doivent avoir uniquement une seule passerelle par défaut configurée. Configurer la passerelle par défaut sur l’adaptateur utilisé pour accéder à Internet.

Pour les machines virtuelles, suivre ces règles pour décider quel réseau à utiliser en tant que la passerelle par défaut :

1. Utiliser le réseau logique de Transit en tant que la passerelle par défaut, si une machine virtuelle se connecte au réseau de Transit, ou si elle est multirésident pour le réseau de Transit ou tout autre réseau.
2. Utiliser le réseau de gestion en tant que la passerelle par défaut, si une machine virtuelle se connecte uniquement au réseau de gestion. 
3. Utiliser le réseau de fournisseur HNV pour SLB/MUX et les passerelles de RAS. N’utilisez pas le réseau de fournisseur HNV comme une passerelle par défaut. 
4. Ne connectez pas les machines virtuelles directement vers les réseaux Storage1, Storage2, adresse IP virtuelle publique ou adresses IP virtuelles privées.

Pour les hôtes Hyper-V et les nœuds de stockage, utilisez le réseau de gestion en tant que la passerelle par défaut.  Les réseaux de stockage ne doivent jamais avoir une passerelle par défaut affectée.


## <a name="network-hardware"></a>Matériel réseau

Vous pouvez utiliser les sections suivantes pour planifier le déploiement de matériel réseau.

### <a name="network-interface-cards-nics"></a>Cartes d’interface réseau (NIC)

Les cartes d’interface réseau (NIC) utilisés dans vos ordinateurs hôtes Hyper-V et les hôtes de stockage requièrent des fonctionnalités spécifiques pour obtenir de meilleures performances. 

Accès de mémoire Direct à distance (RDMA) est un noyau contourner technique qui permet de transférer de grandes quantités de données sans l’aide de l’hôte du processeur, ce qui libère le processeur pour effectuer un autre travail. 

L’association de cartes SET (switch Embedded) est une alternative solution association de cartes réseau que vous pouvez utiliser dans les environnements qui incluent Hyper-V et la pile de mise en réseau SDN (Software Defined) dans Windows Server 2016. JEU intègre des fonctionnalités d’association de cartes réseau dans le commutateur virtuel Hyper-V. 

Pour plus d’informations, consultez [accès de mémoire Direct à distance (RDMA) et l’association de cartes SET (Switch Embedded)](../../../virtualization//hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).   

Pour prendre en compte la surcharge de trafic de réseau virtuel locataire dû au fait des en-têtes d’encapsulation VXLAN ou NVGRE, l’unité MTU de réseau de l’infrastructure de couche 2 (commutateurs et les ordinateurs hôtes) doit être définie supérieure ou égale à octets 1674 \(notamment Ethernet de couche 2 en-têtes\). 

Cartes réseau qui prennent en charge la nouvelle *EncapOverhead* mot clé avancée adaptateur définit la taille MTU automatiquement par le biais de l’Agent hôte du contrôleur de réseau. Cartes réseau ne prenant pas en charge la nouvelle *EncapOverhead* mot clé besoin de définir la taille MTU manuellement sur chaque hôte physique à l’aide de la *JumboPacket* \(ou équivalent\) mot clé. 


### <a name="switches"></a>Commutateurs

Lorsque vous sélectionnez un commutateur physique et un routeur pour votre environnement, assurez-vous qu’il prend en charge l’ensemble des fonctionnalités suivantes :  

- Paramètres MTU de Switchport \(requis\)  
- La valeur MTU > = octets 1674 \(notamment en-tête L2-Ethernet\)  
- Les protocoles L3 \(requis\)  
- ECMP  
- BGP \(IETF RFC 4271\)\-en fonction de routage ECMP

Les implémentations doivent prendre en charge les instructions doit dans les normes IETF suivants.

- RFC 2545 : « Extensions BGP-4 Multiprotocol pour le routage d’Interdomain IPv6 »  
- RFC 4760 : « Extensions multiprotocoles pour BGP-4 »  
- RFC 4893 : « Prise en charge BGP de quatre octets AS numéro espace »  
- RFC 4456 : « Itinéraire BGP réflexion : Une Alternative à maille pleine de BGP interne (IBGP) »  
- RFC 4724 : « Redémarrage sans perte de données de mécanisme pour BGP »  

Les protocoles de balisage suivants sont requis.

- VLAN - l’Isolation des différents types de trafic
- 802. 1 q trunk

Les éléments suivants fournissent un contrôle de lien.

- Qualité de service \(PFC nécessaire uniquement si vous utilisez RoCE\)
- Amélioré la sélection de trafic \(802.1Qaz\)
- Contrôle de flux basé sur la priorité \(802.1 p/Q et 802.1qbb\)

Les éléments suivants fournissent la disponibilité et la redondance.

- Disponibilité du commutateur (obligatoire)
- Un routeur hautement disponible est requis pour effectuer des fonctions de passerelle. Pour cela, à l’aide d’un routeur multichâssis switch\ ou des technologies telles que VRRP.

Les éléments suivants fournissent des fonctionnalités de gestion.

**Surveillance**

- SNMP v1 ou v2 SNMP (obligatoire si vous utilisez un contrôleur de réseau pour la surveillance de commutateur physique)  
- Bases MIB SNMP \(requis si vous utilisez le contrôleur de réseau pour la surveillance de commutateur physique\)  
- MIB-II (RFC 1213), LLDP, Interface MIB \(RFC 2863\), IF-MIB, IP-MIB, IP-avant-MIB, Q-pont-MIB, MIB de pont, LLDB-MIB, MIB de l’entité, IEEE8023-LAG-MIB  

Les diagrammes suivants montrent un exemple de configuration de quatre nœuds. À des fins de clarté, le premier diagramme affiche simplement le contrôleur de réseau, le second montre le contrôleur de réseau ainsi que l’équilibrage de charge logiciel et le troisième diagramme illustre le contrôleur de réseau, équilibrage de charge et la passerelle.  

Ces diagrammes n’affichent pas les cartes réseau virtuelles et réseaux de stockage. Si vous envisagez d’utiliser le stockage basé sur SMB, ils sont nécessaires.

Les machines virtuelles de l’infrastructure et locataire peuvent être redistribuées sur n’importe quel hôte de calcul physiques (en supposant que la connectivité réseau appropriée existe pour les réseaux logiques corrects).  



## <a name="switch-configuration-examples"></a>Exemples de configuration de commutateur  

Pour aider à configurer votre commutateur physique ou un routeur, un ensemble d’exemples de fichiers de configuration pour une variété de modèles de commutateur et fournisseurs sont disponibles sur le [dépôt Microsoft SDN Github](https://github.com/microsoft/SDN/tree/master/SwitchConfigExamples). Un fichier Lisez-moi détaillé et commandes testé interface de ligne de commande (CLI) pour les commutateurs spécifiques sont fournies.         


## <a name="compute"></a>Calcul :  
Tous les hôtes Hyper-V doivent avoir installé Windows Server 2016, Hyper-V activé et un commutateur virtuel Hyper-V externe créé avec au moins une carte physique connectée au réseau logique de gestion. L’hôte doit être accessible via une adresse IP de gestion attribuée à la carte réseau virtuelle hôte de gestion.  

N’importe quel type de stockage qui est compatible avec Hyper-V, partagé ou local peut être utilisé.   

> [!TIP]  
> Il est pratique si vous utilisez le même nom pour tous vos commutateurs virtuels, mais il n’est pas obligatoire. Si vous envisagez de déployer avec des scripts, consultez le commentaire associé à la `vSwitchName` variable dans le fichier config.psd1.  

**Exigences de calcul d’hôte**  
Le tableau suivant présente la configuration matérielle et logicielle minimale requise pour les quatre hôtes physiques utilisés dans l’exemple de déploiement.  

Host|Configuration matérielle requise|Configuration logicielle requise|  
--------|-------------------------|-------------------------  
|Hôte Hyper-v physique|4 cœurs, 2,66 GHz<br /><br />32 Go de RAM<br /><br />300 Go d’espace disque<br /><br />1 Go/s (ou plus rapide) carte réseau physique|SYSTÈME D’EXPLOITATION : Windows Server 2016<br /><br />Rôle Hyper-V installé|  


**Exigences de rôle machine virtuelle l’infrastructure SDN**  

Rôle|configuration requise du processeur virtuel|Mémoire requise|Configuration requise pour le disque|  
--------|---------------------|-----------------------|---------------------  
|Contrôleur de réseau (trois nœuds)|4 processeurs virtuels|4 Go minimum (8 Go recommandés)|75 Go pour le lecteur du système d’exploitation  
|SLB/MUX (trois nœuds)|8 processeurs virtuels|8 Go recommandés|75 Go pour le lecteur du système d’exploitation  
|Passerelle du serveur d’accès à distance<br /><br />(pool unique de trois passerelles de nœud, deux actif, un passif)|8 processeurs virtuels|8 Go recommandés|75 Go pour le lecteur du système d’exploitation  
|Routeur BGP de la passerelle RAS pour l’homologation de SLB/MUX<br /><br />(vous pouvez également utiliser commutateur ToR en tant que routeur BGP)|2 processeurs virtuels|2 Go|75 Go pour le lecteur du système d’exploitation|  


Si vous utilisez VMM pour le déploiement, les ressources de machine virtuelle d’infrastructure supplémentaires sont requis pour VMM et d’autres infrastructures non SDN. Pour plus d’informations, consultez [recommandations relatives au matériel Minimum pour System Center Technical Preview.](https://technet.microsoft.com/library/dn997303.aspx)  

## <a name="extending-your-infrastructure"></a>Extension de votre infrastructure  
Les dimensionnement et les ressources requises pour votre infrastructure sont dépendants sur les machines virtuelles de charge de travail client que vous prévoyez d’héberger. Processeur, mémoire et disque requis pour les machines virtuelles d’infrastructure (par exemple : passerelle de contrôleur, SLB, réseau, etc.) sont répertoriées dans le tableau précédent. Vous pouvez ajouter plusieurs de ces machines virtuelles d’infrastructure pour faire évoluer en fonction des besoins. Toutefois, les machines virtuelles de locataire en cours d’exécution sur les ordinateurs hôtes Hyper-V ont leur propre processeur, mémoire et les exigences de disque que vous devez prendre en compte.   

Lorsque les ordinateurs virtuels de la charge de travail clients commencent à consommer trop de ressources sur les hôtes Hyper-V physiques, vous pouvez étendre votre infrastructure en ajoutant des hôtes physiques supplémentaires. Cela est possible avec Virtual Machine Manager ou à l’aide de scripts PowerShell (selon la façon dont vous avez déployé initialement l’infrastructure) pour créer des ressources de serveur par le biais du contrôleur de réseau. Si vous avez besoin ajouter des adresses IP supplémentaires pour le réseau de fournisseur HNV, vous pouvez créer de nouveaux sous-réseaux logiques (avec les Pools d’adresses IP correspondante) que les hôtes peuvent utiliser.  


## <a name="see-also"></a>Voir aussi  
[Installation en matière de préparation pour le déploiement de contrôleur de réseau](Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md)  
[Software Defined Networking &#40;SDN&#41;](../Software-Defined-Networking--SDN-.md)  



