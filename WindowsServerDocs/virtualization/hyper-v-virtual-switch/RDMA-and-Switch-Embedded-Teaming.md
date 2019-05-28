---
title: RDMA (Remote Direct Memory Access) et SET (Switch Embedded Teaming)
description: Cette rubrique fournit des informations sur la configuration des interfaces d’accès de mémoire Direct à distance (RDMA) avec Hyper-V dans Windows Server 2016, en plus d’informations sur l’association de cartes SET (Switch Embedded).
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: 68c35b64-4d24-42be-90c9-184f2b5f19be
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 485da451eb092336ec93eddfadc6ffa0e677452b
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222748"
---
# <a name="remote-direct-memory-access-rdma-and-switch-embedded-teaming-set"></a>Remote Direct Memory Access \(RDMA\) et Switch Embedded Teaming \(définie\)

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique fournit des informations sur la configuration de Remote Direct Memory Access \(RDMA\) interfaces avec Hyper-V dans Windows Server 2016, en outre, à des informations Switch Embedded Teaming \(définir\).  

> [!NOTE]
> Outre cette rubrique, le contenu suivant Switch Embedded Teaming est disponible. 
> - Téléchargement de la Galerie TechNet : [Carte réseau de Windows Server 2016 et Guide de l’utilisateur Switch Embedded Teaming](https://gallery.technet.microsoft.com/Windows-Server-2016-839cb607?redir=0)

## <a name="bkmk_rdma"></a>Configuration Interfaces RDMA avec Hyper-V  

Dans Windows Server 2012 R2, à l’aide de RDMA et Hyper-V sur le même ordinateur que les cartes réseau qui fournissent des services RDMA ne peuvent pas être liés à un commutateur virtuel Hyper-V. Cela augmente le nombre de cartes réseau physiques qui sont nécessaires pour être installé sur l’hôte Hyper-V.

>[!TIP]
>Dans les éditions de Windows Server antérieures à Windows Server 2016, il n’est pas possible de configurer RDMA sur les cartes réseau qui sont liées à une association de cartes réseau ou à un commutateur virtuel Hyper-V. Dans Windows Server 2016, vous pouvez activer RDMA sur les cartes réseau qui sont liés à un commutateur virtuel Hyper-V avec ou sans ensemble.

Dans Windows Server 2016, vous pouvez utiliser moins de cartes réseau lors de l’utilisation de RDMA avec ou sans ensemble.

L’image ci-dessous illustre les modifications d’architecture logicielle entre Windows Server 2012 R2 et Windows Server 2016.

![Modifications architecturales](../media/RDMA-and-SET/rdma_over.jpg)

Les sections suivantes fournissent des instructions sur l’utilisation des commandes Windows PowerShell pour activer Data Center Bridging (DCB), créez un commutateur virtuel Hyper-V avec une carte réseau virtuelle de RDMA \(carte réseau virtuelle\)et créer un commutateur virtuel Hyper-V avec et les cartes réseau virtuelles RDMA.

### <a name="enable-data-center-bridging-dcb"></a>Activer Data Center Bridging \(DCB\)

Avant d’utiliser n’importe quel RDMA over Converged Ethernet \(RoCE\) version de RDMA, vous devez activer DCB.  Bien que non requis pour le protocole Internet large zone RDMA \(iWARP\) réseaux, de test a déterminé que toutes les technologies RDMA basé sur Ethernet fonctionnent mieux avec DCB. Pour cette raison, vous devez envisager d’utiliser DCB même pour les déploiements de RDMA iWARP.

L’exemple de commande Windows PowerShell suivantes montrent comment activer et configurer DCB pour SMB Direct.

Activez DCB

    Install-WindowsFeature Data-Center-Bridging

Définir une stratégie pour SMB Direct :

    New-NetQosPolicy "SMB" -NetDirectPortMatchCondition 445 -PriorityValue8021Action 3

Activer le contrôle de flux pour SMB :

    Enable-NetQosFlowControl  -Priority 3

Vérifiez que le contrôle de flux est désactivée pour le reste du trafic :

    Disable-NetQosFlowControl  -Priority 0,1,2,4,5,6,7

Appliquer la stratégie aux cartes cibles :

    Enable-NetAdapterQos  -Name "SLOT 2"

Donnez à SMB Direct 30 % de la bande passante minimale :

`New-NetQosTrafficClass "SMB"  -Priority 3  -BandwidthPercentage 30  -Algorithm ETS`  

Si vous avez un débogueur du noyau installé dans le système, vous devez configurer le débogueur pour autoriser la qualité de service à définir en exécutant la commande suivante.

Remplacer le débogueur - par défaut, le débogueur bloque NetQos :
 
    Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 -Force

### <a name="create-a-hyper-v-virtual-switch-with-an-rdma-vnic"></a>Créer un commutateur virtuel Hyper-V avec une carte réseau virtuelle RDMA

Si le jeu n’est pas requis pour votre déploiement, vous pouvez utiliser les commandes Windows PowerShell suivantes pour créer un commutateur virtuel Hyper-V avec une carte réseau virtuelle RDMA.

> [!NOTE]
> À l’aide des équipes de jeu avec les cartes réseau physiques prenant en charge RDMA fournit davantage de ressources RDMA pour les cartes réseau virtuelles à consommer.

    New-VMSwitch -Name RDMAswitch -NetAdapterName "SLOT 2"

Ajoutez des cartes réseau virtuelles hôtes et qu’elles soient capables de RDMA :

    Add-VMNetworkAdapter -SwitchName RDMAswitch -Name SMB_1
    Enable-NetAdapterRDMA "vEthernet (SMB_1)" "SLOT 2"

Vérifiez les fonctionnalités RDMA :

    Get-NetAdapterRdma

###  <a name="bkmk_set-rdma"></a>Créer un commutateur virtuel Hyper-V avec des cartes réseau virtuelles RDMA et SET

Pour utiliser RDMA capacités sur Hyper-V hébergent des cartes réseau virtuelles \(cartes réseau virtuelles\) sur un commutateur virtuel Hyper-V qui prend en charge l’association de cartes RDMA, vous pouvez utiliser ces exemples de commandes Windows PowerShell.

    New-VMSwitch -Name SETswitch -NetAdapterName "SLOT 2","SLOT 3" -EnableEmbeddedTeaming $true

Ajouter des cartes réseau virtuelles hôtes :

    Add-VMNetworkAdapter -SwitchName SETswitch -Name SMB_1 -managementOS
    Add-VMNetworkAdapter -SwitchName SETswitch -Name SMB_2 -managementOS

De nombreux commutateurs ne sont pas transmettre des informations de classe de trafic sur le trafic réseau local virtuel non marqué, donc vous assurer que les adaptateurs d’hôte pour RDMA sont sur des réseaux locaux virtuels. Cet exemple affecte les deux SMB_ * cartes virtuelles hôtes à 42 de réseau local virtuel.
    
    Set-VMNetworkAdapterIsolation -ManagementOS -VMNetworkAdapterName SMB_1  -IsolationMode VLAN -DefaultIsolationID 42
    Set-VMNetworkAdapterIsolation -ManagementOS -VMNetworkAdapterName SMB_2  -IsolationMode VLAN -DefaultIsolationID 42
    

Activez RDMA sur les cartes réseau virtuelles hôtes :

    Enable-NetAdapterRDMA "vEthernet (SMB_1)","vEthernet (SMB_2)" "SLOT 2", "SLOT 3"

Vérifiez les fonctionnalités RDMA ; Assurez-vous que les fonctionnalités sont non nulle :

    Get-NetAdapterRdma | fl *


## <a name="switch-embedded-teaming-set"></a>Commutateur incorporé association (SET)  

Cette section fournit une vue d’ensemble de l’association de cartes SET (Switch Embedded) dans Windows Server 2016 et contient les sections suivantes.

- [Vue d’ensemble](#bkmk_over)

- [DÉFINIR la disponibilité](#bkmk_avail)

- [Cartes réseau prises en charge et non pris en charge pour le jeu](#bkmk_nics)

- [DÉFINITION de la compatibilité avec les Technologies de mise en réseau de Windows Server](#bkmk_compat)

- [Les paramètres et les Modes de jeu](#bkmk_modes)

- [ENSEMBLE et des files d’attente de la Machine virtuelle (Vmq)](#bkmk_vmq)

- [ENSEMBLE et la virtualisation de réseau Hyper-V (HNV)](#bkmk_hnv)

- [Migration définie et en direct](#bkmk_live)

- [Utilisation des adresses MAC sur les paquets transmis](#bkmk_mac)

- [Gestion d’une équipe de jeu](#bkmk_manage)

## <a name="bkmk_over"></a>Vue d’ensemble

JEU est une autre solution association de cartes réseau que vous pouvez utiliser dans les environnements qui incluent l’Hyper-V et le Sdn \(SDN\) pile dans Windows Server 2016. JEU intègre des fonctionnalités d’association de cartes réseau dans le commutateur virtuel Hyper-V.

ENSEMBLE vous permet de regrouper entre un et huit Ethernet de cartes réseau physiques dans un ou plusieurs adaptateurs de réseau virtuel basé sur le logiciel. Ces cartes réseau virtuelles fournissent des performances élevées et une tolérance de panne importante en cas de défaillance de la carte réseau.

Toutes les cartes réseau de l’ensemble doivent être installés dans le même hôte physique Hyper-V à placer dans une équipe.

> [!NOTE]
> L’utilisation d’un ensemble est uniquement pris en charge dans le commutateur virtuel Hyper-V dans Windows Server 2016. Vous ne pouvez pas déployer ensemble dans Windows Server 2012 R2.

Vous pouvez connecter vos cartes réseau associées au même commutateur physique ou à des commutateurs physiques différents. Si vous vous connectez les cartes réseau à différents commutateurs, les deux commutateurs doivent être sur le même sous-réseau.

L’illustration suivante représente l’architecture de l’ensemble.

![Architecture de jeu](../media/RDMA-and-SET/set_architecture.jpg)

Étant donné que le jeu est intégré dans le commutateur virtuel Hyper-V, vous ne pouvez pas utiliser ensemble à l’intérieur d’une machine virtuelle (VM). Vous pouvez toutefois utiliser l’association de cartes réseau au sein de machines virtuelles.

Pour plus d’informations, consultez [l’association de cartes réseau dans les Machines virtuelles (VM)](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nict-vms).

En outre, architecture de jeu n’expose pas les interfaces de l’équipe. Au lieu de cela, vous devez configurer les ports de commutateur virtuel Hyper-V.

## <a name="bkmk_avail"></a>DÉFINIR la disponibilité

ENSEMBLE est disponible dans toutes les versions de Windows Server 2016 incluant Hyper-V et la pile SDN. En outre, vous pouvez utiliser des commandes Windows PowerShell et les connexions Bureau à distance pour gérer l’ensemble des ordinateurs distants qui exécutent un système d’exploitation de client sur lequel les outils sont pris en charge.

## <a name="bkmk_nics"></a>Cartes d’interface réseau pris en charge pour le jeu

Vous pouvez utiliser une carte réseau Ethernet qui a passé la Qualification du matériel Windows et le Logo \(WHQL\) tester dans une équipe de jeu dans Windows Server 2016. JEU nécessite que toutes les cartes réseau qui sont membres d’une équipe ensemble doivent être identiques \(c'est-à-dire même fabricant, même modèle, même microprogramme et pilote\). Prend en charge entre un et huit cartes réseau dans une équipe.
  
## <a name="bkmk_compat"></a>DÉFINITION de la compatibilité avec les Technologies de mise en réseau de Windows Server

ENSEMBLE est compatible avec les technologies de mise en réseau suivantes dans Windows Server 2016.

- Pontage de centre de données \(DCB\)
  
- Virtualisation de réseau Hyper-V-NV-GRE et VxLAN sont tous deux pris en charge dans Windows Server 2016.  
- Déchargements de somme de contrôle côté réception \(IPv4, IPv6, TCP\) -ils sont pris en charge si un des membres de l’équipe ensemble les prennent en charge.

- Remote Direct Memory Access \(RDMA\)

- Virtualisation d’e/s de racine unique \(SR-IOV\)

- Déchargements de somme de contrôle côté transmission \(IPv4, IPv6, TCP\) -ils sont pris en charge si tous les membres d’équipe ensemble les prennent en charge.

- Files d’attente de la Machine virtuelle \(VMQ\)

- Virtuel du trafic entrant \(RSS\)

JEU n’est pas compatible avec les technologies de mise en réseau suivantes dans Windows Server 2016.

- Authentification 802. 1 X. 802. 1 x Extensible Authentication Protocol \(EAP\) les paquets sont automatiquement supprimées par Hyper\-V de commutateur virtuel dans les scénarios de jeu.
 
- Déchargement de tâches IPsec \(IPsecTO\). Il s’agit d’une technologie héritée qui n’est pas pris en charge par la plupart des cartes réseau, et où il n’existe pas, il est désactivé par défaut.

- À l’aide de QoS \(pacer.exe\) dans l’hôte ou des systèmes d’exploitation natifs. Ces scénarios de qualité de service ne sont pas Hyper\-scénarios V, donc les technologies ne se croisent pas. De plus, qualité de service est disponible mais ne sont pas activées par défaut, vous devez activer intentionnellement de qualité de service.

- Fusion du côté de réception \(RSC\). RSC est automatiquement désactivée par Hyper\-V Virtual Switch.

- Trafic entrant \(RSS\). Étant donné que Hyper-V utilise les files d’attente pour VMQ et VMMQ, RSS est toujours désactivé lorsque vous créez un commutateur virtuel.

- Déchargement TCP Chimney. Cette technologie est désactivée par défaut.

- Qualité de service de Machine virtuelle \(QoS de la machine virtuelle\). Qualité de service de machine virtuelle est disponible mais désactivé par défaut. Si vous configurez la qualité de service de machine virtuelle dans un environnement de jeu, les paramètres de QoS entraîne des résultats imprévisibles.

## <a name="bkmk_modes"></a>Les paramètres et les Modes de jeu

Contrairement à l’association de cartes réseau, lorsque vous créez une équipe de jeu, vous ne pouvez pas configurer un nom d’équipe. En outre, à l’aide d’un adaptateur de secours est pris en charge dans l’association de cartes réseau, mais il n’est pas pris en charge dans le jeu. Lorsque vous déployez ensemble, toutes les cartes réseau sont actives et aucune n’est en mode veille.

Une autre différence clé entre l’association de cartes réseau et de jeu est qu’association de cartes réseau permet de choisir parmi trois modes d’association, tandis que le jeu prend uniquement en charge **indépendant du commutateur** mode. Avec le mode indépendant du commutateur, le commutateur ou les commutateurs auxquels les membres d’équipe définie sont connectés n’ont pas conscience de la présence de l’équipe de jeu et ne déterminent pas comment répartir le trafic réseau pour définir les membres de l’équipe : au lieu de cela, l’équipe de jeu distribue réseau entrant trafic entre les membres d’équipe de jeu.

Lorsque vous créez une nouvelle équipe de jeu, vous devez configurer les propriétés d’équipe suivantes.

- Cartes membres

- Mode d’équilibrage de charge

### <a name="member-adapters"></a>Cartes membres

Lorsque vous créez une équipe de jeu, vous devez spécifier jusqu'à huit des cartes réseau identiques qui sont liées au commutateur virtuel Hyper-V en tant que jeu de cartes de membre.

### <a name="load-balancing-mode"></a>Mode d’équilibrage de charge

Les options pour l’ensemble de l’équipe l’équilibrage de charge en mode de distribution sont **Port Hyper-V** et **dynamique**.

**Port Hyper-V**

Machines virtuelles sont connectées à un port sur le commutateur virtuel Hyper-V. Lorsque vous utilisez le mode de Port Hyper-V pour les équipes de jeu, le port de commutateur virtuel Hyper-V et l’adresse MAC associé sont utilisés pour diviser le trafic réseau entre le jeu de membres de l’équipe.

> [!NOTE]
> Lorsque vous utilisez ensemble conjointement avec Packet Direct, le mode d’association **indépendant du commutateur** et la mode d’équilibrage de charge **Port Hyper-V** sont requis.

Étant donné que le commutateur adjacent voit toujours une adresse MAC spécifique sur un port donné, le commutateur distribue la charge de l’entrée (le trafic du commutateur à l’hôte) au port où se trouve l’adresse MAC. Cela est particulièrement utile lorsque les files d’attente de la Machine virtuelle (Vmq) sont utilisées, car une file d’attente permettre être placée sur la carte réseau spécifique où le trafic est supposé arriver.

Toutefois, si l’hôte a uniquement quelques machines virtuelles, ce mode ne peut pas être suffisamment précis pour obtenir une distribution bien équilibrée. Ce mode limite également toujours une seule machine virtuelle (par exemple, le trafic à partir d’un port de commutateur unique) pour la bande passante disponible sur une interface unique.

**Dynamic**

Ce mode d’équilibrage de charge offre les avantages suivants.

- Les charges sortantes sont distribuées en fonction du hachage des adresses IP et les Ports TCP.  Mode dynamique ré-équilibre également les charges en temps réel afin qu’un flux sortant donné peut aller et venir entre les membres de l’équipe ensemble.

- Charges de trafic entrants sont distribuées de la même manière que le mode de port Hyper-V.

Les charges sortantes dans ce mode sont dynamiquement réparties selon le concept de flowlets. Tout comme la voix humaine a les divisions naturelles situées aux terminaisons de mots et phrases, les flux TCP (flux de communication TCP) ont également sauts naturelles. La partie d’un flux TCP entre deux sauts de ce type est appelée un flowlet.

Lorsque l’algorithme de mode dynamique détecte qu’une limite de flowlet s’est produite - par exemple quand un saut de longueur suffisante s’est produite dans le flux TCP - l’algorithme rééquilibre automatiquement le flux à un autre membre de l’équipe si nécessaire.  Dans certaines circonstances rares, l’algorithme peut également régulièrement rééquilibrer les flux qui ne contiennent pas de n’importe quel flowlets. Pour cette raison, l’affinité entre les membres d’équipe et les flux TCP peut changer à tout moment comme l’algorithme d’équilibrage dynamique fonctionne pour équilibrer la charge de travail des membres de l’équipe.

## <a name="bkmk_vmq"></a>ENSEMBLE et des files d’attente de la Machine virtuelle (Vmq)

VMQ et ensemble fonctionnent bien ensemble, et vous devez activer VMQ chaque fois que vous utilisez Hyper-V et définissez.

> [!NOTE]
> Définissez toujours présente le nombre total de files d’attente qui sont disponibles sur l’ensemble de tous les membres de l’équipe. Dans l’association de cartes réseau, il s’agit de mode de la somme des files d’attentes.

La plupart des cartes réseau ont des files d’attente qui peuvent être utilisées pour soit l’échelle côté réception \(RSS\) ou VMQ, mais pas les deux en même temps.
  
Certains paramètres VMQ semblent être des paramètres pour les files d’attente RSS mais sont vraiment des paramètres sur les files d’attente génériques qui RSS et VMQ utilisation selon quelle fonctionnalité est actuellement en cours d’utilisation. Chaque carte réseau a dans ses propriétés avancées, les valeurs pour `*RssBaseProcNumber` et `*MaxRssProcessors`.

Voici quelques paramètres VMQ qui offrent de meilleures performances système.

- Dans l’idéal, chaque carte réseau doit avoir le `*RssBaseProcNumber` la valeur est un nombre pair supérieur ou égal à deux (2). Il s’agit, car le premier processeur physique, Core 0 \(des processeurs logiques, 0 et 1\), généralement effectue la plupart de système de traitement pour le traitement réseau doit être acheminé en dehors de ce processeur physique. 

>[!NOTE]
>Certaines architectures machine n’ont pas deux processeurs logiques par processeur physique, donc pour ces ordinateurs, le processeur de base doit être supérieur ou égal à 1. En cas de doute, supposons que votre hôte utilise un processeur logique 2 par architecture de processeur physique.

- Processeurs de membres de l’équipe doivent être dans la mesure où il est pratique, sans chevauchement. Par exemple, dans un hôte de 4 cœurs \(8 processeurs logiques\) avec l’équipe de 2 cartes réseau de 10 Gbits/s, vous pouvez définir la première condition à utiliser le processeur de base de 2 et à utiliser les 4 cœurs ; la seconde est définie sur utiliser base processeur 6 et utilisez 2 cœurs.

## <a name="bkmk_hnv"></a>Virtualisation de réseau Hyper-V et jeu \(HNV\)

ENSEMBLE est entièrement compatible avec la virtualisation de réseau Hyper-V dans Windows Server 2016. Le système de gestion de HNV fournit des informations pour le pilote de jeu qui permet à l’ensemble de distribuer la charge du trafic réseau d’une manière qui est optimisée pour le trafic HNV.
  
## <a name="bkmk_live"></a>Migration définie et en direct

Migration dynamique est pris en charge dans Windows Server 2016.

## <a name="bkmk_mac"></a>Utilisation des adresses MAC sur les paquets transmis

Lorsque vous configurez une équipe de jeu avec une distribution de charge dynamique, les paquets à partir d’une source unique \(comme une machine virtuelle unique\) sont distribuées simultanément sur plusieurs membres d’équipe. 

Pour empêcher le confondre avec les commutateurs et pour empêcher les alarmes ailes MAC, définissez remplace l’adresse MAC source avec une adresse MAC différente sur les images qui sont transmis sur les membres de l’équipe autre que le membre d’équipe avec affinité. Pour cette raison, chaque membre de l’équipe utilise une adresse MAC différente et conflits d’adresses MAC ne peuvent pas tant que l’échec se produit.

Lorsqu’une défaillance est détectée sur la carte réseau principale, le logiciel d’association de jeu démarre à l’aide d’adresse MAC de la machine virtuelle sur le membre de l’équipe qui est choisi comme membre de l’équipe avec affinité temporaire \(par exemple, celle qui sera au commutateur de la machine virtuelle interface\).

Cette modification s’applique uniquement au trafic qui a été va être envoyée sur le membre de l’équipe avec affinité de la machine virtuelle avec l’adresse MAC de la machine virtuelle propre en tant que son adresse MAC source. Tout autre trafic continue à être envoyées avec toute source adresse MAC il aurait utilisé avant la défaillance.

Voici les listes qui décrivent l’ensemble de l’association de comportement de remplacement d’adresse MAC, selon la configuration de l’équipe :

- En mode indépendant du commutateur avec la distribution de Port Hyper-V

    - Chaque port vmSwitch est une affinité avec un membre d’équipe
  
    - Chaque paquet est envoyé sur le membre d’équipe auquel le port est associé par affinité  
  
    - Aucune source de remplacement de MAC n’est effectuée.  
  
- En mode indépendant du commutateur avec la distribution dynamique
  
    - Chaque port vmSwitch est une affinité avec un membre d’équipe  
  
    - Tous les paquets ARP/NS sont envoyés sur le membre de l’équipe à laquelle le port est associé par affinité  
  
    - Paquets envoyés sur le membre de l’équipe qui est membre de l’équipe avec affinité n’aient aucun remplacement d’adresse MAC fait source  
  
    - Paquets envoyés sur un membre de l’équipe autre que le membre d’équipe avec affinité aura le remplacement d’adresse MAC source terminé  
  
## <a name="bkmk_manage"></a>Gestion d’une équipe de jeu

Il est recommandé d’utiliser System Center Virtual Machine Manager \(VMM\) pour gérer l’ensemble des équipes, toutefois, vous pouvez également utiliser Windows PowerShell pour gérer ensemble. Les sections suivantes fournissent que des commandes Windows PowerShell que vous pouvez utiliser pour gérer ensemble.

Pour plus d’informations sur la création d’un ensemble d’équipe à l’aide de VMM, voir la section « Configurer un commutateur logique » dans la rubrique de la bibliothèque System Center VMM [créer des commutateurs logiques](https://docs.microsoft.com/system-center/vmm/network-switch).
  
### <a name="create-a-set-team"></a>Créer une équipe de jeu

Vous devez créer une équipe de jeu en même temps que vous créez le commutateur virtuel Hyper-V à l’aide de la **New-VMSwitch** commande Windows PowerShell.

Lorsque vous créez le commutateur virtuel Hyper-V, vous devez inclure la nouvelle **EnableEmbeddedTeaming** paramètre dans la syntaxe de votre commande. Dans l’exemple suivant, un commutateur Hyper-V nommé **TeamedvSwitch** avec l’association de cartes incorporé et d’équipe initiale deux membres est créé.
  
```  
New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 2" -EnableEmbeddedTeaming $true  
```  
  
Le **EnableEmbeddedTeaming** paramètre est supposé par Windows PowerShell lorsque l’argument de **NetAdapterName** est un tableau de cartes réseau au lieu d’une seule carte réseau. Par conséquent, vous pouvez modifier la commande précédente de la façon suivante.

```  
New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 2"  
```  

Si vous souhaitez créer un jeu de commutateur avec un seul membre d’équipe afin que vous puissiez ajouter un membre de l’équipe à une date ultérieure, vous devez utiliser le paramètre EnableEmbeddedTeaming.

```  
New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1" -EnableEmbeddedTeaming $true  
```  

### <a name="adding-or-removing-a-set-team-member"></a>Ajout ou suppression d’un membre de l’équipe ensemble

Le **Set-VMSwitchTeam** commande inclut le **NetAdapterName** option. Pour modifier les membres d’équipe dans une équipe de jeu, entrez la liste souhaitée des membres d’équipe après le **NetAdapterName** option. Si **TeamedvSwitch** a été créée avec carte réseau 1 et 2 de la carte réseau, puis la commande suivante supprime le membre de l’équipe ensemble « NIC 2 » et ajoute le nouveau membre de l’équipe ensemble « carte réseau 3 ».
  
```  
Set-VMSwitchTeam -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 3"  
```  

### <a name="removing-a-set-team"></a>Suppression d’une équipe de jeu

Vous pouvez supprimer une équipe ensemble uniquement en supprimant le commutateur virtuel Hyper-V qui contient l’équipe de jeu.  Utilisez la rubrique [Remove-VMSwitch](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/remove-vmswitch) pour plus d’informations sur la façon de supprimer le commutateur virtuel Hyper-V. L’exemple suivant supprime un commutateur virtuel nommé **SETvSwitch**.

```  
Remove-VMSwitch "SETvSwitch"  
```  

### <a name="changing-the-load-distribution-algorithm-for-a-set-team"></a>Modification de l’algorithme de distribution de charge pour une équipe de jeu

Le **Set-VMSwitchTeam** applet de commande a un **LoadBalancingAlgorithm** option. Cette option prend l’une des deux valeurs possibles : **HyperVPort** ou **dynamique**. Pour définir ou modifier l’algorithme de distribution de charge pour une équipe incorporée de commutateur, utilisez cette option. 

Dans l’exemple suivant, nommé le VMSwitchTeam **TeamedvSwitch** utilise le **dynamique** algorithme d’équilibrage de charge.  
```  
Set-VMSwitchTeam -Name TeamedvSwitch -LoadBalancingAlgorithm Dynamic  
```  
### <a name="affinitizing-virtual-interfaces-to-physical-team-members"></a>Affinage des interfaces virtuelles aux membres d’équipe physique

ENSEMBLE, vous pouvez créer une affinité entre une interface virtuelle \(par exemple, port de commutateur virtuel Hyper-V\) et l’une des cartes réseau physiques dans l’équipe. 

Par exemple, si vous créez deux hébergent des cartes réseau virtuelles pour SMB\-Direct, comme dans la section [créer un commutateur virtuel Hyper-V avec des cartes réseau virtuelles RDMA et SET](#bkmk_set-rdma), vous pouvez vous assurer que les deux cartes réseau virtuelles utilisent différents membres d’équipe. 

Ajout du script dans cette section, vous pouvez utiliser les commandes Windows PowerShell suivantes.

    Set-VMNetworkAdapterTeamMapping -VMNetworkAdapterName SMB_1 –ManagementOS –PhysicalNetAdapterName “SLOT 2”
    Set-VMNetworkAdapterTeamMapping -VMNetworkAdapterName SMB_2 –ManagementOS –PhysicalNetAdapterName “SLOT 3”

Cette rubrique est examinée plus en détail dans la section 4.2.5 de la [carte réseau de Windows Server 2016 et Switch Embedded Teaming Guide de l’utilisateur](https://gallery.technet.microsoft.com/Windows-Server-2016-839cb607?redir=0).
