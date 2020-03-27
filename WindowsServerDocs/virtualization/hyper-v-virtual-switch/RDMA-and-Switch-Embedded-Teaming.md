---
title: RDMA (Remote Direct Memory Access) et SET (Switch Embedded Teaming)
description: Cette rubrique fournit des informations sur la configuration des interfaces RDMA (Remote Direct Memory Access) avec Hyper-V dans Windows Server 2016, en plus des informations relatives à l’Association incorporée de commutateurs (SET).
manager: brianlic
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: 68c35b64-4d24-42be-90c9-184f2b5f19be
ms.author: lizross
author: eross-msft
ms.openlocfilehash: cfa8076b84a2fc62cec2a709fc15d3dc5be8eb77
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80307996"
---
# <a name="remote-direct-memory-access-rdma-and-switch-embedded-teaming-set"></a>Accès direct à la mémoire à distance \(\) RDMA et Switch Embedded Association \(définir\)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique fournit des informations sur la configuration de l’accès direct à la mémoire à distance \(les interfaces\) RDMA avec Hyper-V dans Windows Server 2016, en plus des informations relatives à l’Association incorporée de commutateur \(définir\).  

> [!NOTE]
> En plus de cette rubrique, le commutateur suivant incorporant du contenu d’association est disponible. 
> - Téléchargement de la Galerie TechNet : [Guide de l’utilisateur de la carte réseau Windows Server 2016 et du commutateur incorporé](https://gallery.technet.microsoft.com/Windows-Server-2016-839cb607?redir=0)

## <a name="configuring-rdma-interfaces-with-hyper-v"></a><a name="bkmk_rdma"></a>Configuration des interfaces RDMA avec Hyper-V  

Dans Windows Server 2012 R2, l’utilisation d’RDMA et d’Hyper-V sur le même ordinateur que les cartes réseau qui fournissent des services RDMA ne peut pas être liée à un commutateur virtuel Hyper-V. Cela augmente le nombre de cartes réseau physiques qui doivent être installées sur l’hôte Hyper-V.

>[!TIP]
>Dans les éditions de Windows Server antérieures à Windows Server 2016, il n’est pas possible de configurer RDMA sur des cartes réseau qui sont liées à une association de cartes réseau ou à un commutateur virtuel Hyper-V. Dans Windows Server 2016, vous pouvez activer RDMA sur les cartes réseau qui sont liées à un commutateur virtuel Hyper-V avec ou sans défini.

Dans Windows Server 2016, vous pouvez utiliser moins de cartes réseau tout en utilisant RDMA avec ou sans définir.

L’image ci-dessous illustre les modifications apportées à l’architecture logicielle entre Windows Server 2012 R2 et Windows Server 2016.

![Modifications architecturales](../media/RDMA-and-SET/rdma_over.jpg)

Les sections suivantes fournissent des instructions sur l’utilisation des commandes Windows PowerShell pour activer Data Center Bridging (DCB), créer un commutateur virtuel Hyper-V avec une carte réseau virtuelle RDMA \(carte réseau virtuelle\)et créer un commutateur virtuel Hyper-V avec des cartes réseau virtuelles SET et RDMA.

### <a name="enable-data-center-bridging-dcb"></a>Activer le pontage Data Center \(DCB\)

Avant d’utiliser une version Ethernet \(RoCE\) de RDMA avec convergence, vous devez activer DCB.  Bien qu’il ne soit pas nécessaire pour le protocole RDMA Internet \(les réseaux iWARP\), les tests ont déterminé que toutes les technologies RDMA basées sur Ethernet fonctionnent mieux avec DCB. Pour cette raison, vous devez envisager d’utiliser DCB même pour les déploiements RDMA iWARP.

Les exemples de commandes Windows PowerShell suivants montrent comment activer et configurer DCB pour SMB direct.

Activer DCB

    Install-WindowsFeature Data-Center-Bridging

Définir une stratégie pour SMB-direct :

    New-NetQosPolicy "SMB" -NetDirectPortMatchCondition 445 -PriorityValue8021Action 3

Activer le contrôle de Flow pour SMB :

    Enable-NetQosFlowControl  -Priority 3

Assurez-vous que le contrôle de flux est désactivé pour le reste du trafic :

    Disable-NetQosFlowControl  -Priority 0,1,2,4,5,6,7

Appliquer la stratégie aux cartes cibles :

    Enable-NetAdapterQos  -Name "SLOT 2"

Donnez au minimum SMB direct 30% de la bande passante :

`New-NetQosTrafficClass "SMB"  -Priority 3  -BandwidthPercentage 30  -Algorithm ETS`  

Si vous avez un débogueur de noyau installé dans le système, vous devez configurer le débogueur pour permettre la définition de la QoS en exécutant la commande suivante.

Remplacer le débogueur : par défaut, le débogueur bloque NetQos :
 
    Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 -Force

### <a name="create-a-hyper-v-virtual-switch-with-an-rdma-vnic"></a>Créer un commutateur virtuel Hyper-V avec un carte réseau virtuelle RDMA

Si vous ne devez pas définir cette option pour votre déploiement, vous pouvez utiliser les commandes Windows PowerShell suivantes pour créer un commutateur virtuel Hyper-V avec un carte réseau virtuelle RDMA.

> [!NOTE]
> L’utilisation de l’ensemble d’équipes avec des cartes réseau physiques qui prennent en charge RDMA offre davantage de ressources RDMA pour le cartes réseau virtuelles à utiliser.

    New-VMSwitch -Name RDMAswitch -NetAdapterName "SLOT 2"

Ajoutez des cartes réseau virtuelles hôtes et rendez-les RDMA :

    Add-VMNetworkAdapter -SwitchName RDMAswitch -Name SMB_1
    Enable-NetAdapterRDMA "vEthernet (SMB_1)" "SLOT 2"

Vérifier les fonctionnalités RDMA :

    Get-NetAdapterRdma

###  <a name="create-a-hyper-v-virtual-switch-with-set-and-rdma-vnics"></a><a name="bkmk_set-rdma"></a>Créer un commutateur virtuel Hyper-V avec SET et RDMA cartes réseau virtuelles

Pour utiliser des capacités RDMA sur des cartes réseau virtuelles hôtes Hyper-V \(cartes réseau virtuelles\) sur un commutateur virtuel Hyper-V qui prend en charge l’Association RDMA, vous pouvez utiliser ces exemples de commandes Windows PowerShell.

    New-VMSwitch -Name SETswitch -NetAdapterName "SLOT 2","SLOT 3" -EnableEmbeddedTeaming $true

Ajouter un cartes réseau virtuelles d’hôte :

    Add-VMNetworkAdapter -SwitchName SETswitch -Name SMB_1 -managementOS
    Add-VMNetworkAdapter -SwitchName SETswitch -Name SMB_2 -managementOS

De nombreux commutateurs ne transmettent pas les informations de classe de trafic sur le trafic de réseau local virtuel non balisé, donc assurez-vous que les adaptateurs hôtes pour RDMA se trouvent sur des réseaux locaux virtuels. Cet exemple affecte les deux adaptateurs virtuels SMB_ * hôtes à VLAN 42.
    
    Set-VMNetworkAdapterIsolation -ManagementOS -VMNetworkAdapterName SMB_1  -IsolationMode VLAN -DefaultIsolationID 42
    Set-VMNetworkAdapterIsolation -ManagementOS -VMNetworkAdapterName SMB_2  -IsolationMode VLAN -DefaultIsolationID 42
    

Activer RDMA sur l’hôte cartes réseau virtuelles :

    Enable-NetAdapterRDMA "vEthernet (SMB_1)","vEthernet (SMB_2)" "SLOT 2", "SLOT 3"

Vérifier les fonctionnalités RDMA ; Assurez-vous que les fonctionnalités ne sont pas égales à zéro :

    Get-NetAdapterRdma | fl *


## <a name="switch-embedded-teaming-set"></a>Basculer l’Association incorporée (SET)  

Cette section fournit une vue d’ensemble de switch Embedded Teaming (SET) dans Windows Server 2016 et contient les sections suivantes.

- [DÉFINIR la vue d’ensemble](#bkmk_over)

- [DÉFINIR la disponibilité](#bkmk_avail)

- [Cartes réseau prises en charge et non prises en charge pour SET](#bkmk_nics)

- [DÉFINIR la compatibilité avec les technologies de mise en réseau de Windows Server](#bkmk_compat)

- [DÉFINIR les modes et les paramètres](#bkmk_modes)

- [Définir et mettre en file d’attente d’ordinateurs virtuels (VMQ)](#bkmk_vmq)

- [DÉFINIR la virtualisation de réseau Hyper-V (HNV)](#bkmk_hnv)

- [DÉFINIR et Migration dynamique](#bkmk_live)

- [Utilisation des adresses MAC sur les paquets transmis](#bkmk_mac)

- [Gestion d’une équipe de jeu](#bkmk_manage)

## <a name="set-overview"></a><a name="bkmk_over"></a>DÉFINIR la vue d’ensemble

L’ensemble est une autre solution d’association de cartes réseau que vous pouvez utiliser dans les environnements qui incluent Hyper-V et la mise en réseau définie par le logiciel \(SDN\) Stack dans Windows Server 2016. SET intègre certaines fonctionnalités d’association de cartes réseau dans le commutateur virtuel Hyper-V.

L’ensemble vous permet de grouper entre une et huit cartes réseau Ethernet physiques dans une ou plusieurs cartes réseau virtuelles basées sur le logiciel. Ces cartes réseau virtuelles fournissent des performances élevées et une tolérance de panne importante en cas de défaillance de la carte réseau.

Les cartes réseau de membre de jeu doivent toutes être installées dans le même hôte Hyper-V physique à placer dans une équipe.

> [!NOTE]
> L’utilisation de SET est uniquement prise en charge dans le commutateur virtuel Hyper-V dans Windows Server 2016. Vous ne pouvez pas déployer SET dans Windows Server 2012 R2.

Vous pouvez connecter vos cartes réseau associées au même commutateur physique ou à des commutateurs physiques différents. Si vous connectez des cartes réseau à différents commutateurs, les deux commutateurs doivent se trouver sur le même sous-réseau.

L’illustration suivante représente l’architecture SET.

![DÉFINIR l’architecture](../media/RDMA-and-SET/set_architecture.jpg)

Étant donné que SET est intégré au commutateur virtuel Hyper-V, vous ne pouvez pas utiliser SET à l’intérieur d’un ordinateur virtuel (VM). Toutefois, vous pouvez utiliser l’Association de cartes réseau au sein des machines virtuelles.

Pour plus d’informations, consultez [Association de cartes réseau dans des machines virtuelles](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nict-vms).

En outre, SET architecture n’expose pas les interfaces d’équipe. Au lieu de cela, vous devez configurer les ports de commutateur virtuel Hyper-V.

## <a name="set-availability"></a><a name="bkmk_avail"></a>DÉFINIR la disponibilité

SET est disponible dans toutes les versions de Windows Server 2016 qui incluent Hyper-V et la pile SDN. En outre, vous pouvez utiliser les commandes Windows PowerShell et les connexions de Bureau à distance pour gérer les ensembles à partir d’ordinateurs distants qui exécutent un système d’exploitation client sur lequel les outils sont pris en charge.

## <a name="supported-nics-for-set"></a><a name="bkmk_nics"></a>Cartes réseau prises en charge pour l’ensemble

Vous pouvez utiliser n’importe quelle carte réseau Ethernet ayant passé la qualification et le logo de matériel Windows \(test\) WHQL dans une équipe de jeu dans Windows Server 2016. L’ensemble exige que toutes les cartes réseau membres d’une équipe de jeu soient identiques \(c’est-à-dire le même fabricant, le même modèle, le même microprogramme et le même pilote\). La définition de prend en charge entre une et huit cartes réseau dans une équipe.
  
## <a name="set-compatibility-with-windows-server-networking-technologies"></a><a name="bkmk_compat"></a>DÉFINIR la compatibilité avec les technologies de mise en réseau de Windows Server

L’ensemble est compatible avec les technologies de mise en réseau suivantes dans Windows Server 2016.

- Pontage de centre de \(DCB\)
  
- La virtualisation de réseau Hyper-V-NV-GRE et VxLAN sont prises en charge dans Windows Server 2016.  
- Les déchargements de somme de contrôle côté réception \(IPv4, IPv6, TCP\)-ils sont pris en charge si l’un des membres de l’équipe de jeu les prend en charge.

- Accès direct à la mémoire à distance \(\) RDMA

- Virtualisation d’e/s d’une racine unique \(SR-IOV\)

- Le déchargement de la somme de contrôle côté transmission \(IPv4, IPv6, TCP\)-ils sont pris en charge si tous les membres de l’équipe les prennent en charge.

- Files d’attente d’ordinateurs virtuels \(\) d’ordinateur virtuel

- Mise à l’échelle côté réception virtuelle \(RSS\)

L’ensemble n’est pas compatible avec les technologies de mise en réseau suivantes dans Windows Server 2016.

- authentification 802.1 x. le protocole d’authentification 802.1 x \(les paquets\) EAP sont automatiquement supprimés par le commutateur virtuel Hyper\-V dans les scénarios SET.
 
- Déchargement de tâche IPsec \(\)IPsecTO. Il s’agit d’une technologie héritée qui n’est pas prise en charge par la plupart des cartes réseau, et où elle existe, elle est désactivée par défaut.

- Utilisation de QoS \(Pacer. exe\) dans les systèmes d’exploitation hôtes ou natifs. Ces scénarios de qualité de service ne sont pas des scénarios hyper\-V, donc les technologies ne se croisent pas. En outre, la qualité de service (QoS) est disponible mais n’est pas activée par défaut. vous devez activer intentionnellement la QoS.

- La fusion côté réception \(\)RSC. RSC est automatiquement désactivé par le commutateur virtuel Hyper\-V.

- La mise à l’échelle côté réception \(\)RSS. Étant donné qu’Hyper-V utilise les files d’attente pour les ordinateurs virtuels et VMMQ, RSS est toujours désactivé lorsque vous créez un commutateur virtuel.

- Déchargement TCP Chimney. Cette technologie est désactivée par défaut.

- Ordinateur virtuel QoS \(machine virtuelle-QoS\). La QoS de la machine virtuelle est disponible mais désactivée par défaut. Si vous configurez la qualité de service de la machine virtuelle dans un environnement défini, les paramètres QoS entraînent des résultats imprévisibles.

## <a name="set-modes-and-settings"></a><a name="bkmk_modes"></a>DÉFINIR les modes et les paramètres

Contrairement à l’Association de cartes réseau, lorsque vous créez une équipe de jeu, vous ne pouvez pas configurer un nom d’équipe. En outre, l’utilisation d’une carte de secours est prise en charge dans l’Association de cartes réseau, mais n’est pas prise en charge dans SET. Lorsque vous déployez SET, toutes les cartes réseau sont actives et aucune n’est en mode veille.

Une autre différence clé entre l’Association de cartes réseau et la définition est que l’Association de cartes réseau offre le choix de trois modes d’association différents, tandis que SET prend en charge uniquement le mode **indépendant du commutateur** . Avec le mode de basculement indépendant, le ou les commutateurs auxquels les membres de l’équipe de jeu sont connectés n’ont pas conscience de la présence de l’équipe définie et ne déterminent pas comment distribuer le trafic réseau pour définir les membres de l’équipe, à la place, l’équipe chargée de distribuer le réseau entrant trafic entre les membres de l’équipe.

Lorsque vous créez une équipe de jeu, vous devez configurer les propriétés d’équipe suivantes.

- Adaptateurs membres

- Mode d’équilibrage de charge

### <a name="member-adapters"></a>Adaptateurs membres

Lorsque vous créez une équipe de jeu, vous devez spécifier jusqu’à huit cartes réseau identiques liées au commutateur virtuel Hyper-V en tant que définir des adaptateurs de membres d’équipe.

### <a name="load-balancing-mode"></a>Mode d’équilibrage de charge

Les options pour définir le mode de distribution d’équilibrage de charge d’équipe sont **port Hyper-V** et **dynamique**.

**Port Hyper-V**

Les machines virtuelles sont connectées à un port sur le commutateur virtuel Hyper-V. Lors de l’utilisation du mode de port Hyper-V pour les équipes de jeu, le port de commutateur virtuel Hyper-V et l’adresse MAC associée sont utilisés pour diviser le trafic réseau entre les membres de l’équipe définie.

> [!NOTE]
> Lorsque vous utilisez SET conjointement avec Packet direct, le mode d’association est **indépendant** et le **port Hyper-V** de mode d’équilibrage de charge est requis.

Étant donné que le commutateur adjacent voit toujours une adresse MAC particulière sur un port donné, le commutateur répartit la charge d’entrée (le trafic du commutateur vers l’hôte) vers le port où se trouve l’adresse MAC. Cela s’avère particulièrement utile lorsque des files d’attente d’ordinateurs virtuels (VMQ) sont utilisées, car une file d’attente peut être placée sur la carte réseau spécifique où le trafic est supposé arriver.

Toutefois, si l’hôte n’a que quelques machines virtuelles, ce mode peut ne pas être suffisamment granulaire pour obtenir une distribution bien équilibrée. Ce mode limite également toujours une seule machine virtuelle (c’est-à-dire le trafic à partir d’un port commuté unique) à la bande passante disponible sur une seule interface.

**Dynamique**

Ce mode d’équilibrage de charge offre les avantages suivants.

- Les charges sortantes sont distribuées en fonction d’un hachage des ports TCP et des adresses IP.  Le mode dynamique rééquilibre également les charges en temps réel afin qu’un workflow sortant donné puisse se déplacer entre les membres de l’équipe définie.

- Les charges entrantes sont distribuées de la même manière que le mode de port Hyper-V.

Les charges sortantes dans ce mode sont équilibrées dynamiquement en fonction du concept de flowlets. Tout comme la parole humaine a des ruptures naturelles à la fin de mots et de phrases, les flux TCP (flux de communication TCP) ont également des interruptions naturellement en cours. La partie d’un workflow TCP entre deux arrêts de ce type est appelée flowlet.

Lorsque l’algorithme en mode dynamique détecte qu’une limite flowlet a été rencontrée, par exemple lorsqu’une interruption de la longueur suffisante s’est produite dans le processus TCP, l’algorithme rééquilibre automatiquement le workflow avec un autre membre de l’équipe, le cas échéant.  Dans certains cas rares, l’algorithme peut également rééquilibrer périodiquement les flux qui ne contiennent pas de flowlets. Pour cette raison, l’affinité entre le canal TCP et le membre de l’équipe peut changer à tout moment, à mesure que l’algorithme d’équilibrage dynamique fonctionne pour équilibrer la charge de travail des membres de l’équipe.

## <a name="set-and-virtual-machine-queues-vmqs"></a><a name="bkmk_vmq"></a>Définir et mettre en file d’attente d’ordinateurs virtuels (VMQ)

Les ordinateurs virtuels et les jeux fonctionnent bien ensemble, et vous devez activer l’utilisation de l’ordinateurs virtuels chaque fois que vous utilisez Hyper-V et que vous définissez.

> [!NOTE]
> SET affiche toujours le nombre total de files d’attente disponibles parmi tous les membres d’équipe définis. Dans l’Association de cartes réseau, il s’agit du mode « total de files d’attente ».

La plupart des cartes réseau comportent des files d’attente qui peuvent être utilisées pour la mise à l’échelle côté réception \(les\) RSS ou les ordinateurs virtuels, mais pas les deux à la fois.
  
Certains paramètres de l’ordinateurs virtuels semblent être des paramètres pour les files d’attente RSS, mais sont en fait des paramètres sur les files d’attente génériques que les flux RSS et les ordinateurs virtuels utilisent en fonction de la fonctionnalité actuellement utilisée. Chaque carte réseau possède, dans ses propriétés avancées, des valeurs pour `*RssBaseProcNumber` et `*MaxRssProcessors`.

Voici quelques paramètres d’ordinateur virtuels qui offrent de meilleures performances système.

- Dans l’idéal, chaque carte réseau doit avoir le `*RssBaseProcNumber` défini sur un nombre pair supérieur ou égal à deux (2). Cela est dû au fait que le premier processeur physique, le noyau 0 \(les processeurs logiques 0 et 1\), effectue généralement la majeure partie du traitement du système, de sorte que le traitement réseau doit être destiné à s’éloigner de ce processeur physique. 

>[!NOTE]
>Certaines architectures d’ordinateur n’ont pas deux processeurs logiques par processeur physique. par conséquent, pour ces machines, le processeur de base doit être supérieur ou égal à 1. En cas de doute, supposons que votre hôte utilise un processeur logique de 2 par architecture de processeur physique.

- Les processeurs des membres de l’équipe doivent être dans la mesure où ils ne se chevauchent pas. Par exemple, dans un hôte à 4 cœurs \(8 processeurs logiques\) avec une équipe de 2 cartes réseau 10Gbps, vous pouvez définir la première pour utiliser le processeur de base 2 et utiliser 4 cœurs. la seconde est définie pour utiliser le processeur de base 6 et utiliser 2 cœurs.

## <a name="set-and-hyper-v-network-virtualization-hnv"></a><a name="bkmk_hnv"></a>DÉFINIR et la virtualisation de réseau Hyper-V \(HNV\)

SET est entièrement compatible avec la virtualisation de réseau Hyper-V dans Windows Server 2016. Le système de gestion HNV fournit des informations au pilote SET qui permet à de configurer de distribuer la charge du trafic réseau d’une manière optimisée pour le trafic HNV.
  
## <a name="set-and-live-migration"></a><a name="bkmk_live"></a>DÉFINIR et Migration dynamique

Migration dynamique est pris en charge dans Windows Server 2016.

## <a name="mac-address-use-on-transmitted-packets"></a><a name="bkmk_mac"></a>Utilisation des adresses MAC sur les paquets transmis

Quand vous configurez une équipe de jeu avec une distribution de charge dynamique, les paquets d’une seule source \(tels qu’une seule machine virtuelle\) sont répartis simultanément entre plusieurs membres de l’équipe. 

Pour éviter toute confusion entre les commutateurs et pour empêcher les alarmes à ailes MAC, SET remplace l’adresse Mac source par une autre adresse MAC sur les trames transmises sur les membres de l’équipe autres que le membre de l’équipe affinité. Pour cette raison, chaque membre de l’équipe utilise une adresse MAC différente, et les conflits d’adresses MAC sont empêchés à moins qu’une défaillance ne se produise.

Quand une défaillance est détectée sur la carte réseau principale, le logiciel d’association de groupe commence à utiliser l’adresse MAC de la machine virtuelle sur le membre de l’équipe qui est choisi comme membre de l’équipe affinité temporaire \(c.-à-d., celui qui apparaîtra maintenant au commutateur comme l’interface de la machine virtuelle\).

Cette modification s’applique uniquement au trafic qui devait être envoyé sur le membre de l’équipe affinité de la machine virtuelle avec la propre adresse MAC de la machine virtuelle en tant qu’adresse MAC source. D’autres trafics continuent d’être envoyés avec l’adresse MAC source qu’il aurait utilisé avant la défaillance.

Vous trouverez ci-dessous des listes qui décrivent le comportement de remplacement de l’adresse MAC de l’équipe, en fonction de la configuration de l’équipe :

- En mode indépendant du commutateur avec distribution de port Hyper-V

    - Chaque port vmSwitch est affinité à un membre de l’équipe
  
    - Chaque paquet est envoyé sur le membre de l’équipe sur lequel le port est affinité  
  
    - Aucun remplacement de MAC source n’est effectué  
  
- En mode indépendant du commutateur avec distribution dynamique
  
    - Chaque port vmSwitch est affinité à un membre de l’équipe  
  
    - Tous les paquets ARP/NS sont envoyés sur le membre de l’équipe sur lequel le port est affinité  
  
    - Les paquets envoyés sur le membre de l’équipe qui est le membre de l’équipe affinité n’ont aucun remplacement d’adresse MAC source effectué  
  
    - Les paquets envoyés sur un membre de l’équipe autre que le membre de l’équipe affinité comporteront le remplacement de l’adresse MAC source  
  
## <a name="managing-a-set-team"></a><a name="bkmk_manage"></a>Gestion d’une équipe de jeu

Nous vous recommandons d’utiliser System Center Virtual Machine Manager \(VMM\) pour gérer les équipes de groupe, mais vous pouvez également utiliser Windows PowerShell pour gérer SET. Les sections suivantes fournissent les commandes Windows PowerShell que vous pouvez utiliser pour gérer SET.

Pour plus d’informations sur la création d’une équipe de jeu à l’aide de VMM, consultez la section « configurer un commutateur logique » dans la rubrique de la bibliothèque VMM de System Center [créer des commutateurs logiques](https://docs.microsoft.com/system-center/vmm/network-switch).
  
### <a name="create-a-set-team"></a>Créer une équipe de jeu

Vous devez créer une équipe de jeu en même temps que vous créez le commutateur virtuel Hyper-V à l’aide de la commande Windows PowerShell **New-VMSwitch** .

Lorsque vous créez le commutateur virtuel Hyper-V, vous devez inclure le nouveau paramètre **EnableEmbeddedTeaming** dans la syntaxe de votre commande. Dans l’exemple suivant, un commutateur Hyper-V nommé **TeamedvSwitch** avec l’Association incorporée et deux membres initiaux de l’équipe sont créés.
  
```  
New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 2" -EnableEmbeddedTeaming $true  
```  
  
Le paramètre **EnableEmbeddedTeaming** est utilisé par Windows PowerShell lorsque l’argument de **NetAdapterName** est un tableau de cartes réseau au lieu d’une seule carte réseau. Par conséquent, vous pouvez modifier la commande précédente de la façon suivante.

```  
New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 2"  
```  

Si vous souhaitez créer un commutateur avec un seul membre d’équipe afin de pouvoir ajouter un membre de l’équipe ultérieurement, vous devez utiliser le paramètre EnableEmbeddedTeaming.

```  
New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1" -EnableEmbeddedTeaming $true  
```  

### <a name="adding-or-removing-a-set-team-member"></a>Ajout ou suppression d’un membre de l’équipe SET

La commande **Set-VMSwitchTeam** comprend l’option **NetAdapterName** . Pour modifier les membres de l’équipe dans une équipe définie, entrez la liste souhaitée des membres de l’équipe après l’option **NetAdapterName** . Si **TeamedvSwitch** a été créé à l’origine avec la carte réseau 1 et la carte réseau 2, l’exemple de commande suivant supprime Set Team Member « NIC 2 » et ajoute New Set Team Member « NIC 3 ».
  
```  
Set-VMSwitchTeam -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 3"  
```  

### <a name="removing-a-set-team"></a>Suppression d’une équipe de jeu

Vous pouvez supprimer une équipe de jeu uniquement en supprimant le commutateur virtuel Hyper-V qui contient l’équipe définie.  Utilisez la rubrique [Remove-VMSwitch](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/remove-vmswitch) pour plus d’informations sur la façon de supprimer le commutateur virtuel Hyper-V. L’exemple suivant supprime un commutateur virtuel nommé **SETvSwitch**.

```  
Remove-VMSwitch "SETvSwitch"  
```  

### <a name="changing-the-load-distribution-algorithm-for-a-set-team"></a>Modification de l’algorithme de distribution de charge pour une équipe de jeu

L’applet de commande **Set-VMSwitchTeam** a une option **LoadBalancingAlgorithm** . Cette option prend l’une des deux valeurs possibles : **HyperVPort** ou **Dynamic**. Pour définir ou modifier l’algorithme de distribution de charge pour une équipe Switch-Embedded, utilisez cette option. 

Dans l’exemple suivant, le VMSwitchTeam nommé **TeamedvSwitch** utilise l’algorithme d’équilibrage de charge **dynamique** .  
```  
Set-VMSwitchTeam -Name TeamedvSwitch -LoadBalancingAlgorithm Dynamic  
```  
### <a name="affinitizing-virtual-interfaces-to-physical-team-members"></a>Affinage les interfaces virtuelles aux membres de l’équipe physique

SET vous permet de créer une affinité entre une interface virtuelle \(c.-à-d., le port de commutateur virtuel Hyper-V\) et l’une des cartes réseau physiques de l’équipe. 

Par exemple, si vous créez deux cartes réseau virtuelles hôtes pour SMB\-direct, comme dans la section [créer un commutateur virtuel Hyper-V avec set et RDMA cartes réseau virtuelles](#bkmk_set-rdma), vous pouvez vous assurer que les deux cartes réseau virtuelles utilisent des membres d’équipe différents. 

Si vous ajoutez au script dans cette section, vous pouvez utiliser les commandes Windows PowerShell suivantes.

    Set-VMNetworkAdapterTeamMapping -VMNetworkAdapterName SMB_1 –ManagementOS –PhysicalNetAdapterName “SLOT 2”
    Set-VMNetworkAdapterTeamMapping -VMNetworkAdapterName SMB_2 –ManagementOS –PhysicalNetAdapterName “SLOT 3”

Cette rubrique est examinée plus en détail dans la section 4.2.5 du Guide de l’utilisateur de la [carte réseau Windows Server 2016 et du commutateur Embedded Teaming](https://gallery.technet.microsoft.com/Windows-Server-2016-839cb607?redir=0).
