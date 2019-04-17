---
title: Planifier une Infrastructure de réseau définies de logiciel
description: Cette rubrique fournit des informations sur la planification de votre déploiement de l’infrastructure logicielle définie réseau (SDN).
manager: brianlic
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
ms.openlocfilehash: bb3b9313996637fa5ee7367c538fe04d7cbefea9
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="plan-a-software-defined-network-infrastructure"></a>Planifier une Infrastructure de réseau définies de logiciel

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Passez en revue les informations suivantes pour vous aider à planifier votre déploiement de l’infrastructure logicielle définie réseau (SDN). Une fois que vous passez en revue ces informations, voir [déployer une infrastructure réseau à définition logicielle](../deploy/Deploy-a-Software-Defined-Network-Infrastructure.md) pour des informations sur le déploiement.

>[!NOTE]
>Outre cette rubrique, le contenu de la planification SDN suivant est disponible.  
>
> - [Installation et configuration requise de préparation pour le déploiement de contrôleur de réseau](Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md)  

Pour plus d’informations sur Hyper-V virtualisation de réseau (HNV), qui vous permet de virtualiser des réseaux dans un déploiement SDN Microsoft, consultez [la virtualisation de réseau Hyper-V](../technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md).  

## <a name="prerequisites"></a>Conditions préalables
Cette rubrique décrit un certain nombre de logicielles et matérielles requises, y compris:

-   **Réseau physique**  
    Vous devez accéder à vos appareils réseau physique pour configurer les réseaux locaux virtuels, routage, BGP, pontage de centre de données (ETS) si vous utilisez une technologie RDMA et données Center Bridging (PFC) si vous utilisez un RoCE en fonction de la technologie RDMA. Cette rubrique montre une configuration manuelle de commutateur ainsi que l’homologation BGP sur les commutateurs de couche 3 / routeurs ou un ordinateur virtuel de routage et le serveur d’accès à distance (RRAS).   

-   **Ordinateurs hôtes physiques de calcul**  
Ces ordinateurs hôtes exécutant Hyper-V et sont requis pour héberger SDN infrastructure et client des ordinateurs virtuels.  Matériel réseau spécifique est requise dans ces ordinateurs hôtes pour de meilleures performances, ce qui est décrit plus loin dans le **matériel réseau** section.  
      
  
## <a name="physical-network-configuration"></a>Configuration du réseau physique

Chaque ordinateur hôte physique calcul nécessite une connectivité réseau par le biais d’un ou plusieurs cartes réseau sont connectées à des ports de commutateur physique. Le réseau est différenciée en plusieurs segments de réseau logique éventuellement soutenus par la couche 2 [VLAN](https://en.wikipedia.org/wiki/Virtual_LAN). Les préfixes de sous-réseau IP et l’ID de VLAN ci-dessous sont des exemples et doivent être personnalisées pour votre environnement en fonction des conseils de votre administrateur réseau. Si un de vos réseaux logiques sont non marqué ou en mode d’accès, utilisez l’ID de VLAN 0 pour ces réseaux lors de la configuration des sous-réseaux logiques dans les fichiers de configuration de script PowerShell ou SystemCenter Virtual Machine Manager.

>[!IMPORTANT]
>Windows Server2016 logiciel défini de mise en réseau prend en charge l’adressage IPv4 pour la sous-couche et la superposition. IPv6 n’est pas pris en charge.
  
### <a name="management-and-hnv-provider-logical-networks"></a>Fournisseur HNV et gestion des réseaux logiques

Physiques de tous les ordinateurs hôtes doivent avoir accès au réseau logique gestion et le réseau logique HNV fournisseur de calcul. Si les réseaux logiques utilisent des réseaux locaux virtuels, les ordinateurs hôtes physiques calcul doivent être connectée à un port de commutateur système qui a accès à ces réseaux locaux virtuels. De même, les cartes réseau physiques sur l’ordinateur hôte de calcul ne doivent pas être tout filtrage de réseau local virtuel activé. Si vous utilisez Switch-Embedded SET (Teaming) et que vous avez plusieurs membres de l’équipe de cartes réseau (autrement dit, les cartes réseau) dans vos hôtes de calcul, vous devez connecter tous les membres de l’équipe de cartes réseau de cet hôte particulier pour le même domaine de diffusion de couche 2.  
  
Pour une adresse IP à des fins de planification, chaque hôte physique calcul doit avoir au moins une adresse IP affectée à partir du réseau logique de gestion. Le contrôleur de réseau attribue automatiquement exactement deux adresses IP à partir du réseau logique fournisseur HNV. Si l’hôte de calcul physique exécute une infrastructure supplémentaire des ordinateurs virtuels (par exemple, le contrôleur de réseau, SLB/MUX ou passerelle) cet hôte doit avoir une adresse IP supplémentaire affectée à partir du réseau logique de gestion pour chacun des ordinateurs virtuels infrastructure hébergés.   
  
En outre, chaque ordinateur virtuel d’infrastructure SLB/MUX doit avoir une adresse IP réservée à partir du réseau logique fournisseur HNV. 

>[!IMPORTANT]
>Ces adresses IP SLB/MUX doivent être affectés à partir en dehors du pool d’adresses IP qui est configuré pour le réseau logique fournisseur HNV. Échec pour effectuer cette opération peut entraîner des adresses IP en double sur votre réseau. 

Le contrôleur de réseau nécessite une adresse réservée à partir du réseau de gestion pour servir de l’adresse IP reste. Vous devez créer manuellement l’enregistrement d’un ordinateur hôte dans DNS pour l’adresse IP du reste.  
  
Un serveur DHCP peut affecter automatiquement des adresses IP pour le réseau de gestion, ou vous pouvez attribuer manuellement l’adresse IP statique. La pile SDN affecte automatiquement des adresses IP pour le réseau de fournisseur HNV pour les hôtes Hyper-V individuels à partir d’un Pool IP spécifiée par le biais et gérés par le contrôleur de réseau.   
  
Statiquement, l’administrateur de structure attribue les adresses IP du fournisseur HNV utilisés par le SLB/MUX via des scripts PowerShell ou de VMM. Le contrôleur de réseau attribue une adresse IP du fournisseur HNV vers un ordinateur hôte physique calcul uniquement une fois l’Agent hôte du contrôleur de réseau reçoit la stratégie de réseau pour un ordinateur virtuel client spécifique.  
  
#### <a name="sample-network-topology"></a>Exemple de topologie de réseau

Personnaliser les préfixes de sous-réseau, ID de VLAN et adresses IP de passerelle selon les instructions de l’administrateur de votre réseau.  
  
Nom du réseau|Sous-réseau|Masque|ID de VLAN sur trunk|Passerelle|Réservations<br />(exemples)  
----------------|----------|--------|--------------------|-----------|-----------------------------  
|**Gestion**|10.184.108.0|24|7|10.184.108.1|10.184.108.1 - routeur<br /><br />10.184.108.4 - contrôleur de réseau<br /><br />10.184.108.10 - compute hôte 1<br /><br />10.184.108.11 - hôte de calcul 2<br /><br />10.184.108.X - hôte de calcul X  
|**Fournisseur HNV**|10.10.56.0|23|11|10.10.56.1|10.10.56.1 - routeur<br /><br />10.10.56.2 - SLB/MUX1  
  
### <a name="logical-networks-for-gateways-and-the-software-load-balancer"></a>Réseaux logiques pour les passerelles et l’équilibrage de charge logicielle
  
Les réseaux logiques supplémentaires doivent être créés et configurés pour la passerelle et l’utilisation de différentes. Une fois encore, vous devrez travailler avec votre administrateur réseau pour obtenir les préfixes d’IP correctes, ID de VLAN et adresses IP de passerelle pour ces réseaux.

#### <a name="transit-logical-network"></a>Réseau logique de transit
  
La passerelle et SLB/MUX utilisent le réseau logique de Transit pour échanger des informations homologation BGP et le trafic de locataire (externe interne) Nord/Sud. La taille de ce sous-réseau sera généralement plus petite que les autres. Calcul physique uniquement les hôtes qui exécutent cette passerelle ou SLB/multiplexer des ordinateurs virtuels doivent disposent d’une connectivité vers ce sous-réseau avec ces réseaux locaux virtuels reliés et accessibles sur les ports de commutateur auquel les cartes réseau hôtes du calcul sont connectés. Chaque SLB/MUX ou un ordinateur virtuel de passerelle est affectée de manière statique une seule adresse IP à partir du réseau logique de Transit.

#### <a name="public-vip-logical-network"></a>Réseau logique des adresses IP virtuelles publique  
  
Le réseau logique d’adresse IP virtuelle publique est requis pour que les préfixes de sous-réseau IP routables en dehors de l’environnement de cloud (généralement Internet routable).  Ces seront les adresses IP frontal utilisés par les clients externes d’accéder aux ressources dans les réseaux virtuels, y compris les serveurs frontaux VIP pour la passerelle de site.

#### <a name="private-vip-logical-network"></a>Réseau logique des adresses IP virtuelles privé
  
Le réseau logique VIP privé n’est pas nécessaire pour être routable en dehors du cloud, qu’il est utilisé pour les adresses IP qui est uniquement accessibles à partir de clients cloud interne, tels que le gestionnaire SLB ou services privés.
  
#### <a name="gre-vip-logical-network"></a>Réseau logique GRE VIP

Le réseau de l’adresse IP virtuelle GRE est un sous-réseau qui existe uniquement pour la définition d’adresses IP qui est attribués aux ordinateurs virtuels de passerelle en cours d’exécution sur votre infrastructure SDN pour un type de connexion S2S GRE. Ce réseau ne pas doivent être configurés au préalable dans vos commutateurs physiques ou un routeur et doivent ne pas avoir un réseau local virtuel affecté.   

### <a name="sample-network-topology"></a>Exemple de topologie de réseau

Personnaliser les préfixes de sous-réseau, ID de VLAN et adresses IP de passerelle selon les instructions de l’administrateur de votre réseau.  
  
Nom du réseau|Sous-réseau|Masque|ID de VLAN sur trunk|Passerelle|Réservations<br />(exemples)  
----------------|----------|--------|--------------------|-----------|-----------------------------  
|**Transit**|10.10.10.0|24|10|10.10.10.1|10.10.10.1 - routeur  
|**Adresse IP virtuelle publique**|41.40.40.0|27|NA|41.40.40.1|41.40.40.1 - routeur<br /> 41.40.40.2 - SLB/MUX VIP<br />41.40.40.3 - VIP de VPN S2S IPSec  
|**Adresse IP virtuelle privé**|20.20.20.0|27|NA|20.20.20.1|20.20.20.1 - GW par défaut (routeur)  
|**GRE VIP**|31.30.30.0|24|NA|31.30.30.1|31.30.30.1 - GW par défaut|  
  
### <a name="logical-networks-required-for-rdma-based-storage"></a>Réseaux logiques requis pour le stockage basé sur RDMA  
  
Si vous êtes à l’aide de RDMA du stockage basé sur un, puis vous devez définir un réseau local virtuel et le sous-réseau pour chaque carte physique dans vos hôtes de calcul et de stockage. En règle générale, vous aurez deux cartes physiques par nœud pour cette configuration.  
  
> [!IMPORTANT]  
> Commutateurs physiques plus nécessitent le trafic RDMA à être envoyé sur un réseau local virtuel référencé dans l’ordre de la qualité de paramètres de service soit appliquée correctement.  Ne placez pas le trafic RDMA sur un réseau local virtuel non marqué, ou sur un port de mode d’accès physique.  
  
Nom du réseau  |Sous-réseau  |Masque  |ID de VLAN sur trunk  |Passerelle  |Réservations<br />(exemples)    
---------|---------|---------|---------|---------|---------  
**Storage1**     |    10.60.36.0     | 25        |   8      |  10.60.36.1       |  10.60.36.1 - routeur<br />10.60.36.x - hôte de calcul x<br />10.60.36.y - compute hôte y<br />10.60.36.v - cluster de calcul<br />10.60.36.w - cluster de stockage  
|**Stockage2**|10.60.36.128|25|9|10.60.36.129|10.60.36.129 - routeur<br />10.60.36.x - hôte de calcul x<br />10.60.36.y - compute hôte y<br />10.60.36.v - cluster de calcul<br />10.60.36.w - cluster de stockage  
  
Pour plus d’informations sur la configuration des commutateurs, consultez le **exemples de Configuration** section.  
  
## <a name="routing-infrastructure"></a>Infrastructure de routage  
  
Si vous déployez votre infrastructure SDN à l’aide de scripts, la gestion, le fournisseur de HNV, le Transit, et les sous-réseaux d’adresses IP virtuelles doivent être routables entre eux sur le réseau physique.     
  
Informations de routage \ (par exemple, hop\ suivant) pour l’adresse IP virtuelle sous-réseaux est publié par les SLB/MUX et les passerelles RAS au réseau physique à l’aide de l’homologation BGP interne. Les réseaux logiques VIP ne disposent pas d’un réseau local virtuel attribué et n’est pas préconfigurés dans le commutateur de couche 2 (par exemple, le commutateur Top-of-Rack).  
  
Vous devez créer un homologue BGP sur le routeur qui est utilisé par votre infrastructure SDN pour recevoir des itinéraires pour les réseaux logiques VIP publiés par les SLB/MUXes et les passerelles RAS. Homologation BGP ne doit se produire une façon (à partir de SLB/MUX ou la passerelle à l’homologue BGP externe).  Au-dessus de la première couche de routage vous pouvez utiliser des itinéraires statiques ou un autre protocole de routage dynamique comme OSPF, toutefois, comme indiqué plus haut, le préfixe de sous-réseau IP pour les réseaux logiques VIP doivent-elles être routable à partir du réseau physique de l’homologue BGP externe.   
  
Homologation BGP est généralement configuré dans un commutateur ou routeur géré dans le cadre de l’infrastructure réseau. L’homologue BGP peut également être configurée sur un serveur Windows avec le rôle de serveur d’accès distant (RAS) installé dans un mode de routage uniquement. Cet homologue du routeur BGP dans l’infrastructure réseau doit être configuré pour son propre ASN et permet de disposer d’un ASN qui est affecté aux composants SDN l’homologation \ (SLB/MUX et Gateways\ RAS). Vous devez obtenir les informations suivantes à partir de votre routeur physique, ou à partir de l’administrateur réseau de contrôler ce routeur:

- Routeur ASN  
- Adresse IP du routeur  
- ASN pour une utilisation par les composants SDN (peut être d’un nombre quelconque en tant qu’à partir de la plage ASN privé)

>[!NOTE]
>Quatre octets homologations ne sont pas pris en charge par le SLB/MUX. Vous devez allouer deux octets homologations le SLB/MUX et le routeur wo auquel il se connecte. Vous pouvez utiliser 4octets homologations ailleurs dans votre environnement.  
  
Vous ou votre administrateur réseau doit configurer le routeur de l’homologue BGP pour accepter les connexions à partir du ASN et l’adresse IP ou l’adresse de sous-réseau du réseau logique Transit que votre passerelle et les SLB/MUXes à l’aide.
  
Pour plus d’informations, voir [Border Gateway Protocol et #40; protocole BGP et #41;](../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md).
  
## <a name="default-gateways"></a>Passerelles par défaut

Ordinateurs virtuels qui sont configurés pour se connecter à plusieurs réseaux, tels que les hôtes physiques et virtuels de passerelle doivent avoir uniquement une passerelle par défaut configurée. La passerelle par défaut sera généralement être configurée sur la carte utilisée pour accéder à Internet.

Pour les ordinateurs virtuels, utilisez les règles suivantes pour décider quel réseau à utiliser en tant que la passerelle par défaut:

1. Utiliser le réseau de Transit en tant que la passerelle par défaut, si un ordinateur virtuel est connecté au réseau de Transit, ou s’il est multirésident Transit et tout autre réseau.  
2. Utiliser le réseau de gestion en tant que la passerelle par défaut, si un ordinateur virtuel est uniquement connecté au réseau de gestion.  
3.  Le réseau de fournisseur de HNV ne doit jamais être utilisé en tant que passerelle par défaut. Les seuls ordinateurs virtuels connectés à ce réseau sera SLB/MUXes les passerelles RAS.  
4.  Ordinateurs virtuels sera jamais être connectés directement aux réseaux Storage1, stockage2, adresse IP virtuelle publique ou VIP privé.  
  
Pour les hôtes Hyper-V et les nœuds de stockage, utilisez le réseau de gestion en tant que la passerelle par défaut.  Les réseaux de stockage ne doivent jamais avoir une passerelle par défaut attribuée.
  
## <a name="network-hardware"></a>Matériel réseau

Vous pouvez utiliser les sections suivantes pour planifier un déploiement de matériel réseau.

### <a name="network-interface-cards-nics"></a>Cartes d’Interface réseau (NIC)

Pour obtenir de meilleures performances, des fonctionnalités spécifiques sont nécessaires dans les cartes réseau que vous utilisez dans vos ordinateurs hôtes Hyper-V et stockage.  
 
Accès à la mémoire Direct à distance (RDMA) est un noyau contourner technique qui permet de transférer de grandes quantités de données sans impliquer l’hôte du processeur. Étant donné que le moteur DMA sur la carte réseau effectue le transfert, le processeur n’est pas utilisé pour le déplacement de la mémoire.  Cela permet de libérer le processeur pour effectuer d’autres tâches.  

Commutateur SET (Embedded Teaming) est une alternative la solution d’association de cartes réseau que vous pouvez utiliser dans les environnements qui incluent Hyper-V et la pile logicielle définie de mise en réseau (SDN) dans Windows Server2016. ENSEMBLE intègre des fonctionnalités d’association de cartes réseau dans le commutateur virtuel Hyper-V.

Pour plus d’informations, voir [Remote Direct Memory Access et #40; rDMA et #41; et Switch Embedded Teaming et #40; sET et #41;](../../../virtualization//hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).  

Pour prendre en compte la surcharge dans le trafic de réseau virtuel client provoqué par des en-têtes d’encapsulation VXLAN ou NVGRE, la MTU du réseau de structure de couche 2 (commutateurs et les ordinateurs hôtes) doit être définie à supérieure ou égale à octets 1674 \ (y compris headers\ Ethernet de couche 2). Cartes réseau qui prennent en charge la nouvelle *EncapOverhead* mot clé carte avancées définit la MTU automatiquement par le biais de l’Agent hôte du contrôleur de réseau. Cartes réseau ne prenant pas en charge la nouvelle *EncapOverhead* mot clé doivent définir la taille de la MTU manuellement sur chaque ordinateur hôte physique à l’aide de la *JumboPacket* mot clé \(or equivalent\).

### <a name="switches"></a>Commutateurs
  
Lors de la sélection d’un commutateur physique et le routeur pour votre environnement vous assurer qu’il prend en charge l’ensemble des fonctionnalités suivantes.  

- Switchport MTU paramètres \(required\)  
- La valeur MTU > = octets 1674 \ (y compris Header\ L2-Ethernet)  
- L3 protocoles \(required\)  
- ROUTAGE ECMP  
- Protocole BGP \(IETF RFC4271\)\-based ECMP

Les implémentations doivent prendre en charge les instructions doit dans les normes IETF suivantes.

- RFC2545: «BGP-4 multi-protocole extensions pour le service de routage IPv6 entre domaines»  
- RFC4760: «Extensions multi-protocole BGP-4»  
- RFC4893: «BGP prise en charge quatre octets comme numéro espace»  
- RFC4456: «réflexion de routage BGP: une Alternative à plein de maillage BGP interne (IBGP)»  
- RFC4724: «mécanisme redémarrage progressif pour BGP»  

Les protocoles de marquage suivants sont requis.

- Réseau local virtuel - l’Isolation des différents types de trafic
- 802. 1 q trunk

Les éléments suivants offrent un contrôle de lien.

- Qualité de service \ (PFC nécessaire uniquement si vous utilisez RoCE\)
- Amélioré le trafic \(802.1Qaz\) sélection
- Contrôle de flux basé sur la priorité \ (802.1 p/Q et 802.1Qbb\)

Les éléments suivants fournissent la disponibilité et la redondance.

- Disponibilité du commutateur (obligatoire)
- Un routeur haut est requis pour effectuer les fonctions de passerelle. Pour cela, à l’aide d’un routeur switch\ châssis multiples ou des technologies telles que VRRP.
        
Les éléments suivants fournissent des fonctionnalités de gestion.

**Analyse**

- SNMP v1 ou v2 SNMP (obligatoire si vous utilisez le contrôleur de réseau pour l’analyse du commutateur physique)  
- MIB SNMP \ (obligatoire si vous utilisez le contrôleur de réseau pour serveur\ commutateur physique)  
- MIB-II (RFC1213), LLDP, Interface MIB \(RFC2863\), IF-MIB, IP-MIB, IP-directe-MIB, Q-pont-MIB, BRIDGE-MIB, LLDB-MIB, entité-MIB, IEEE8023-différé-MIB  
  
Les schémas suivants montrent un exemple de configuration à quatre nœuds. Pour des raisons de clarté, le premier diagramme présente simplement le contrôleur de réseau, la seconde affiche le contrôleur de réseau, ainsi que l’équilibrage de charge logicielle et le troisième diagramme montre le contrôleur de réseau, équilibrage de charge logicielle et la passerelle.  
  
Cartes réseau virtuelles et des réseaux de stockage ne sont pas shonwn dans ces schémas. Si vous envisagez d’utiliser le stockage SMB, ils sont requis.    
  
Les machines virtuelles l’infrastructure et le client peut être redistribués sur n’importe quel hôte de calcul physique (en supposant que la connectivité réseau approprié existe pour les réseaux logiques appropriés).  
  
### <a name="network-controller-deployment"></a>Déploiement de contrôleur de réseau

Avant de déployer le contrôleur de réseau, vous devez passer en revue installation et configuration logicielle requise, ainsi que la configuration des groupes de sécurité et de l’inscription DNS dynamique. Pour plus d’informations, voir [Installation et préparation pour le déploiement d’un contrôleur de réseau](Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md).

Le programme d’installation est hautement disponible avec trois nœuds de contrôleur de réseau configurés sur les ordinateurs virtuels. Est également affiché de deux clients avec le réseau virtuel du client 2 divisé en deux sous-réseaux virtuels pour simuler un niveau web et un niveau de base de données.  

![Planification de contexte de nommage SDN](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-NC-Planning.png)

### <a name="network-controller-and-software-load-balancer-deployment"></a>Logiciels et le contrôleur de réseau chargent le déploiement d’un équilibreur

Pour une haute disponibilité, il existe deux ou plusieurs nœuds SLB/MUX.
   
![Planification de contexte de nommage SDN](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-SLB-Deployment.png)
  
### <a name="network-controller-software-load-balancer-and-ras-gateway-deployment"></a>Déploiement de contrôleur de réseau, équilibrage de charge logicielle et passerelle

Il existe trois ordinateurs virtuels passerelle; deux sont active, et un est redondant.

![Planification de contexte de nommage SDN](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-GW-Deployment.png)  
  
  
  
Pour l’automatisation du déploiement basé sur TP5, ActiveDirectory doit être disponible et accessible à partir de ces sous-réseaux. Pour plus d’informations sur ActiveDirectory, voir [présentation de Services de domaine ActiveDirectory](https://technet.microsoft.com/en-us/library/mt703721.aspx).  
  
>[!IMPORTANT] 
>Si vous déployez à l’aide de VMM, vérifiez vos ordinateurs virtuels infrastructure (serveur VMM, DNS/ActiveDirectory, SQLServer, etc.) ne sont pas hébergé sur un des quatre hôtes affichées dans les diagrammes.  
  
## <a name="switch-configuration-examples"></a>Exemples de configuration de commutateur  
  
Pour configurer votre routeur ou un commutateur physique, un ensemble d’exemples de fichiers de configuration pour un large éventail de modèles de commutateurs et les fournisseurs sont disponibles sur le [référentiel Github de SDN Microsoft](https://github.com/microsoft/SDN/tree/master/SwitchConfigExamples). Un fichier Lisezmoi détaillé et commandes testée interface de ligne de commande (CLI) pour les commutateurs spécifiques sont fournies.         
  
  
## <a name="compute"></a>Calcul  
Tous les ordinateurs hôtes Hyper-V doivent avoir installé Windows Server2016, Hyper-V activé et un commutateur virtuel Hyper-V externe créé avec au moins une carte physique connectée à ce réseau logique. L’ordinateur hôte doit être accessible via une adresse IP de gestion attribuée à la carte réseau virtuelle hôte de gestion.  
  
N’importe quel type de stockage est compatible avec Hyper-V, partagé ou local peut être utilisée.   
  
> [!TIP]  
> Il est pratique si vous utilisez le même nom pour tous vos commutateurs virtuels, mais il n’est pas obligatoire. Si vous prévoyez de déployer avec des scripts, voir le commentaire associé le `vSwitchName`variable dans le fichier config.psd1.  
  
**Exigences de calcul l’hôte**  
Le tableau suivant présente la configuration matérielle et logicielle requise pour les quatre hôtes physiques utilisés dans l’exemple de déploiement.  
  
Ordinateur hôte|Configuration matérielle requise|Configuration logicielle requise|  
--------|-------------------------|-------------------------  
|Hôte Hyper-v physique|4cœurs 2,66GHz du processeur<br /><br />32Go de RAM<br /><br />300Go d’espace disque<br /><br />1Gbits/s (ou plus rapide) carte réseau physique|Système d’exploitation: Windows Server2016<br /><br />Rôle Hyper-V installé|  
  
  
**Exigences de rôle machine virtuelle l’infrastructure SDN**  
  
Rôle|Configuration requise de processeurs virtuels|Besoins en mémoire|Disque requis|  
--------|---------------------|-----------------------|---------------------  
|Contrôleur de réseau (trois nœuds)|4vCPUs|4Go minimum (8Go recommandés)|75Go pour le lecteur du système d’exploitation  
|SLB/MUX (trois nœuds)|8vCPUs|8Go recommandés|75Go pour le lecteur du système d’exploitation  
|Passerelle<br /><br />(pool unique de trois passerelles de nœud, deux actif, un passif)|8vCPUs|8Go recommandés|75Go pour le lecteur du système d’exploitation  
|Routeur BGP de la passerelle RAS pour l’homologation SLB/MUX<br /><br />(vous pouvez également utiliser commutateur ToR en tant que routeur BGP)|2vCPUs|2GO|75Go pour le lecteur du système d’exploitation|  
  
  
Si vous utilisez VMM pour le déploiement, les ressources d’ordinateur virtuel infrastructure supplémentaires sont requis pour VMM et toute autre infrastructure non SDN. Pour plus d’informations, voir [recommandations relatives au matériel Minimum pour SystemCenter Technical Preview.](https://technet.microsoft.com/library/dn997303.aspx)  
  
## <a name="extending-your-infrastructure"></a>Extension de votre infrastructure  
Les dimensionnement et les ressources requises pour votre infrastructure dépendent des charge de travail ordinateurs virtuels clients que vous prévoyez d’héberger. Le processeur, mémoire et disque requis pour les ordinateurs virtuels infrastructure (par exemple: passerelle de contrôleur, SLB, réseau, etc.) sont répertoriés dans le tableau précédent. Vous pouvez ajouter plusieurs de ces ordinateurs virtuels infrastructure à l’échelle en fonction des besoins. Toutefois, les ordinateurs virtuels clients en cours d’exécution sur les ordinateurs hôtes Hyper-V ont leurs propres du processeur, mémoire et disque requis que vous devez prendre en compte.   
  
Lors de la charge de travail virtuels clients commencent à consommer trop de ressources sur les hôtes Hyper-V physiques, vous pouvez étendre votre infrastructure en ajoutant des hôtes physiques supplémentaires. Cela est possible avec Virtual Machine Manager ou à l’aide de scripts PowerShell (selon la façon dont vous avez déployé initialement l’infrastructure) pour créer de nouvelles ressources de serveur par le biais du contrôleur de réseau. Si vous avez besoin ajouter des adresses IP pour le réseau de fournisseur de HNV, vous pouvez créer des nouveaux sous-réseaux logiques (avec les Pools IP correspondante) que les ordinateurs hôtes peuvent utiliser.  
  
  
## <a name="see-also"></a>Voir aussi  
[Installation et configuration requise de préparation pour le déploiement de contrôleur de réseau](Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md)  
[Software Defined Networking & #40; SDN & #41;](../Software-Defined-Networking--SDN-.md)  
  


