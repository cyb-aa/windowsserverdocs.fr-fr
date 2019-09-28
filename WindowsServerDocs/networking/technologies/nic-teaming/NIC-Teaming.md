---
title: Association de cartes réseau
description: Dans cette rubrique, nous vous proposons une vue d’ensemble de l’Association de cartes d’interface réseau (NIC) dans Windows Server 2016. L’Association de cartes réseau vous permet de grouper entre une et 32 cartes réseau Ethernet physiques dans une ou plusieurs cartes réseau virtuelles basées sur le logiciel. Ces cartes réseau virtuelles fournissent des performances élevées et une tolérance de panne importante en cas de défaillance de la carte réseau.
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: abded6f3-5708-4e35-9a9e-890e81924fec
ms.author: pashort
author: shortpatti
ms.date: 09/10/2018
ms.openlocfilehash: 2356de674bfc6e57c9444136b1244934464a2d02
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396503"
---
# <a name="nic-teaming"></a>Association de cartes réseau

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Dans cette rubrique, nous vous proposons une vue d’ensemble de l’Association de cartes d’interface réseau (NIC) dans Windows Server 2016. L’Association de cartes réseau vous permet de grouper entre une et 32 cartes réseau Ethernet physiques dans une ou plusieurs cartes réseau virtuelles basées sur le logiciel. Ces cartes réseau virtuelles fournissent des performances élevées et une tolérance de panne importante en cas de défaillance de la carte réseau.  
  
> [!IMPORTANT]
> Vous devez installer les cartes réseau des membres de l’Association de cartes réseau dans le même ordinateur hôte physique. 

> [!TIP]  
> Une association de cartes réseau qui contient une seule carte réseau ne peut pas assurer l’équilibrage de charge et le basculement. Toutefois, avec une carte réseau, vous pouvez utiliser l’Association de cartes réseau pour séparer le trafic réseau lorsque vous utilisez également des réseaux locaux virtuels (VLAN).  
  
Quand vous configurez des cartes réseau dans une association de cartes réseau, elles se connectent au Common Core de solution d’association de cartes réseau, qui présente ensuite une ou plusieurs cartes virtuelles (également appelées cartes réseau d’équipe [tNICs] ou interfaces d’équipe) au système d’exploitation. 

Étant donné que Windows Server 2016 prend en charge jusqu’à 32 interfaces d’équipe par équipe, il existe un large éventail d’algorithmes qui distribuent le trafic sortant (chargement) entre les cartes réseau.  L’illustration suivante représente une association de cartes réseau avec plusieurs tNICs.  
  
![Association de cartes réseau avec plusieurs tNICs](../../media/NIC-Teaming/nict_overview.jpg)  
  
En outre, vous pouvez connecter vos cartes réseau associées au même commutateur ou à différents commutateurs. Si vous connectez des cartes réseau à différents commutateurs, les deux commutateurs doivent se trouver sur le même sous-réseau.  
  
## <a name="availability"></a>Disponibilité  
L’Association de cartes réseau est disponible dans toutes les versions de Windows Server 2016. Vous pouvez utiliser divers outils pour gérer l’Association de cartes réseau à partir d’ordinateurs exécutant un système d’exploitation client, par exemple : • applets de commande Windows PowerShell • Bureau à distance • Outils d’administration de serveur distant  
  
## <a name="supported-and-unsupported-nics"></a>Cartes réseau prises en charge et non prises en charge   
Vous pouvez utiliser n’importe quelle carte réseau Ethernet qui a réussi le test de qualification et de logo de matériel Windows (tests WHQL) dans une association de cartes réseau dans Windows Server 2016.  
  
Vous ne pouvez pas placer les cartes réseau suivantes dans une association de cartes réseau :
  
-   Cartes réseau virtuelles Hyper-V qui sont des ports de commutateur virtuel Hyper-V exposés en tant que cartes réseau dans la partition hôte.  
  
    > [!IMPORTANT]  
    > Ne placez pas les cartes réseau virtuelles Hyper-V exposées dans la partition de l’hôte (cartes réseau virtuelles) dans une équipe. L’Association de cartes réseau virtuelles à l’intérieur de la partition hôte n’est prise en charge dans aucune configuration. Les tentatives d’association de cartes réseau virtuelles peuvent entraîner une perte de communication complète en cas de défaillance du réseau.  
  
-   Carte réseau de débogage du noyau (KDNIC).  
  
-   Cartes réseau utilisées pour le démarrage réseau.  
  
-   Cartes réseau utilisant des technologies autres que Ethernet, telles que WWAN, WLAN/Wi-Fi, Bluetooth et InfiniBand, y compris les cartes réseau Internet Protocol sur InfiniBand (IPoIB).  
  
## <a name="compatibility"></a>Compatibilité  
L’Association de cartes réseau est compatible avec toutes les technologies de mise en réseau dans Windows Server 2016, avec les exceptions suivantes.  
  
-   **Virtualisation d’e/s d’une racine unique (SR-IOV)** . Pour SR-IOV, les données sont transmises directement à la carte réseau sans passer par la pile de mise en réseau (dans le système d’exploitation hôte, dans le cas de la virtualisation). Par conséquent, il n’est pas possible pour l’équipe de cartes réseau d’inspecter ou de rediriger les données vers un autre chemin d’accès de l’équipe.  
  
-   **Qualité de service (QoS) hôte Native**. Lorsque vous définissez des stratégies de QoS sur un système hôte ou natif, et que ces stratégies appellent des limitations de bande passante minimales, le débit global pour une association de cartes réseau est inférieur à celui des stratégies de bande passante en place.  
  
-   **TCP Chimney**. TCP Chimney n’est pas pris en charge avec l’Association de cartes réseau, car TCP Chimney décharge l’intégralité de la pile de mise en réseau directement sur la carte réseau.  
  
-   **authentification 802.1 x**. Vous ne devez pas utiliser l’authentification 802.1 X avec l’Association de cartes réseau, car certains commutateurs n’autorisent pas la configuration de l’authentification 802.1 X et de l’Association de cartes réseau sur le même port.  
  
Pour en savoir plus sur l’Association de cartes réseau dans des machines virtuelles qui s’exécutent sur un ordinateur hôte Hyper-V, consultez [créer une nouvelle association de cartes réseau sur un ordinateur hôte ou une machine virtuelle](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md).
  
## <a name="virtual-machine-queues-vmqs"></a>Files d’attente d’ordinateurs virtuels (VMQ)  

VMQ est une fonctionnalité de carte réseau qui alloue une file d’attente pour chaque machine virtuelle.  À chaque fois que Hyper-V est activé ; vous devez également activer l’ordinateurs virtuels. Dans Windows Server 2016, VMQ utilise le commutateur de carte réseau vPorts avec une file d’attente unique assignée au vPort pour fournir les mêmes fonctionnalités. 

En fonction du mode de configuration du commutateur et de l’algorithme de distribution de charge, l’Association de cartes réseau présente le plus petit nombre de files d’attente disponibles et prises en charge par n’importe quelle carte de l’équipe (mode min-queues) ou le nombre total de files d’attente disponibles dans toute l’équipe. membres (mode total de files d’attente).  

Si l’équipe est en mode d’association indépendant du commutateur et que vous définissez la distribution de la charge sur le mode de port Hyper-V ou le mode dynamique, le nombre de files d’attente signalées est la somme de toutes les files d’attente disponibles à partir des membres de l’équipe (mode total de files d’attente). Dans le cas contraire, le nombre de files d’attente signalées est le plus petit nombre de files d’attente prises en charge par les membres de l’équipe (mode min-queues).

Voici pourquoi:  
  
-   Lorsque l’équipe indépendante du commutateur est en mode de port Hyper-V ou en mode dynamique, le trafic entrant pour un port de commutateur Hyper-V arrive toujours sur le même membre de l’équipe. L’hôte peut prédire/contrôler quel membre reçoit le trafic pour une machine virtuelle particulière. l’Association de cartes réseau peut donc être plus réfléchie sur les files d’attente d’ordinateurs virtuels à allouer à un membre de l’équipe particulier. Association de cartes réseau, utilisation du commutateur Hyper-V, définit l’ordinateur virtuel pour une machine virtuelle sur un seul membre de l’équipe et sait que le trafic entrant atteint cette file d’attente.  
  
-   Lorsque l’équipe est en mode dépendant du commutateur (Association statique ou association LACP), le commutateur auquel l’équipe est connectée contrôle la distribution du trafic entrant. Le logiciel d’association de cartes réseau de l’hôte ne peut pas prédire quel membre de l’équipe obtient le trafic entrant pour une machine virtuelle et il peut être que le commutateur répartit le trafic pour une machine virtuelle parmi tous les membres de l’équipe. En raison de l’utilisation du logiciel d’association de cartes réseau, avec le commutateur Hyper-V, programme une file d’attente pour la machine virtuelle sur chaque membre de l’équipe, pas seulement un membre de l’équipe.  
  
-   Lorsque l’équipe est en mode indépendant du commutateur et utilise l’équilibrage de charge de hachage d’adresse, le trafic entrant arrive toujours sur une carte réseau (le membre de l’équipe principal), tout cela sur un seul membre de l’équipe. Étant donné que les autres membres de l’équipe ne traitent pas du trafic entrant, ils sont programmés avec les mêmes files d’attente que le membre principal, de sorte que si le membre principal échoue, tous les autres membres de l’équipe peuvent être utilisés pour récupérer le trafic entrant et les files d’attente sont déjà en place.  

- La plupart des cartes réseau ont des files d’attente utilisées pour la mise à l’échelle côté réception (RSS) ou l’ordinateurs virtuels, mais pas en même temps. Certains paramètres de l’ordinateurs virtuels semblent être des paramètres pour les files d’attente RSS, mais les paramètres sur les files d’attente génériques qui sont à la fois des flux RSS et des ordinateurs virtuels sont utilisés en fonction de la fonctionnalité actuellement utilisée. Chaque carte réseau possède, dans ses propriétés avancées, des valeurs pour * \*RssBaseProcNumber et MaxRssProcessors. Voici quelques paramètres d’ordinateur virtuels qui offrent de meilleures performances système.  
  
-   Dans l’idéal, chaque carte réseau doit avoir le * RssBaseProcNumber défini sur un nombre pair supérieur ou égal à deux (2). Le premier processeur physique, le cœur 0 (processeurs logiques 0 et 1), effectue généralement la majeure partie du traitement du système, de sorte que le traitement réseau doit s’éloigner de ce processeur physique. Certaines architectures d’ordinateur n’ont pas deux processeurs logiques par processeur physique. par conséquent, pour ces machines, le processeur de base doit être supérieur ou égal à 1. En cas de doute, supposons que votre hôte utilise un processeur logique de 2 par architecture de processeur physique.  
  
-   Si l’équipe est en mode total de files d’attente, les processeurs des membres de l’équipe ne doivent pas se chevaucher. Par exemple, dans un hôte à 4 cœurs (8 processeurs logiques) avec une équipe de 2 cartes réseau 10Gbps, vous pouvez définir la première pour utiliser le processeur de base de 2 et pour utiliser 4 cœurs. la seconde est définie pour utiliser le processeur de base 6 et utiliser 2 cœurs.  
  
-   Si l’équipe est en mode min-queues, les jeux de processeurs utilisés par les membres de l’équipe doivent être identiques.  

  
## <a name="hyper-v-network-virtualization-hnv"></a>Virtualisation de réseau Hyper-V  
L’Association de cartes réseau est entièrement compatible avec la virtualisation de réseau Hyper-V (HNV).  Le système de gestion HNV fournit des informations au pilote d’association de cartes réseau qui permet à l’Association de cartes réseau de répartir la charge d’une manière qui optimise le trafic HNV.  
  
## <a name="live-migration"></a>Migration dynamique  
L’Association de cartes réseau dans les machines virtuelles n’affecte pas les Migration dynamique. Les mêmes règles existent pour Migration dynamique que la configuration de l’Association de cartes réseau dans la machine virtuelle ou non.  


## <a name="virtual-local-area-networks-vlans"></a>Réseaux locaux virtuels (VLAN)
Lorsque vous utilisez l’Association de cartes réseau, la création de plusieurs interfaces d’équipe permet à un hôte de se connecter à différents réseaux locaux virtuels en même temps. Configurez votre environnement à l’aide des instructions suivantes :
  
- Avant d’activer l’Association de cartes réseau, configurez les ports de commutateur physique connectés à l’hôte d’association pour utiliser le mode Trunk (promiscuité). Le commutateur physique doit transmettre tout le trafic à l’hôte pour le filtrage sans modifier le trafic.  

- Ne configurez pas de filtres de réseau local virtuel sur les cartes réseau à l’aide des paramètres des propriétés avancées de la carte réseau. Laissez le logiciel d’association de cartes réseau ou le commutateur virtuel Hyper-V (le cas échéant) effectuer un filtrage VLAN.  
  
### <a name="use-vlans-with-nic-teaming-in-a-vm"></a>Utiliser des réseaux locaux virtuels avec Association de cartes réseau dans une machine virtuelle  
Quand une équipe se connecte à un commutateur virtuel Hyper-V, toute la répartition VLAN doit être effectuée dans le commutateur virtuel Hyper-V plutôt que dans l’Association de cartes réseau.  

Envisagez d’utiliser des réseaux locaux virtuels dans une machine virtuelle configurée avec une association de cartes réseau à l’aide des instructions suivantes :
  
-   La méthode recommandée pour la prise en charge de plusieurs réseaux locaux virtuels dans une machine virtuelle consiste à configurer la machine virtuelle avec plusieurs ports sur le commutateur virtuel Hyper-V et à associer chaque port à un réseau local virtuel. Ne jamais associer ces ports à la machine virtuelle, car cela entraîne des problèmes de communication réseau.  

-   Si la machine virtuelle possède plusieurs fonctions virtuelles SR-IOV (VFs), assurez-vous qu’elles se trouvent sur le même réseau local virtuel avant de les associer à la machine virtuelle. Il est facile de configurer les différents VFss sur différents réseaux locaux virtuels, ce qui entraîne des problèmes de communication réseau.  
 
  
### <a name="manage-network-interfaces-and-vlans"></a>Gérer les interfaces réseau et les réseaux locaux virtuels 
Si vous devez avoir plusieurs réseaux locaux virtuels exposés dans un système d’exploitation invité, pensez à renommer les interfaces Ethernet pour clarifier le VLAN affecté à l’interface. Par exemple, si vous associez une interface **Ethernet** à un réseau local virtuel (VLAN) 12 et une interface **Ethernet 2** avec VLAN 48, renommez l’interface Ethernet en **EthernetVLAN12** et l’autre en **EthernetVLAN48**. 

Renommez les interfaces en utilisant le nom de commande Windows PowerShell **Rename-NetAdapter** ou en effectuant la procédure suivante :
  
1.  Dans Gestionnaire de serveur, dans les **Propriétés** de la carte réseau que vous souhaitez renommer, cliquez sur le lien situé à droite du nom de la carte réseau. 
  
2.  Cliquez avec le bouton droit sur la carte réseau que vous souhaitez renommer, puis sélectionnez **Renommer**.  
  
3.  Tapez le nouveau nom de la carte réseau, puis appuyez sur entrée.  


## <a name="virtual-machines-vms"></a>Machines virtuelles (VM)

Si vous souhaitez utiliser l’Association de cartes réseau dans une machine virtuelle, vous devez connecter les cartes réseau virtuelles de la machine virtuelle aux commutateurs virtuels Hyper-V externes uniquement. Cela permet à la machine virtuelle de maintenir la connectivité réseau même si l’une des cartes réseau physiques connectées à un commutateur virtuel échoue ou est déconnectée. Les cartes réseau virtuelles connectées à des commutateurs virtuels Hyper-V internes ou privés ne sont pas en mesure de se connecter au commutateur lorsqu’ils sont en équipe, et la mise en réseau échoue pour la machine virtuelle.  
  
L’Association de cartes réseau dans Windows Server 2016 prend en charge les équipes avec deux membres dans des machines virtuelles. Vous pouvez créer des équipes de plus grande taille, mais il n’existe pas de prise en charge pour les équipes de plus grande taille. Chaque membre de l’équipe doit se connecter à un autre commutateur virtuel Hyper-V externe, et les interfaces réseau de la machine virtuelle doivent être configurées pour autoriser l’Association.

  
Si vous configurez une association de cartes réseau dans une machine virtuelle, vous devez sélectionner un **mode d’association** _indépendant_ et un mode d' **équilibrage de charge** de hachage d' _adresse_.   
  
  
## <a name="sr-iov-capable-network-adapters"></a>Cartes réseau compatible SR-IOV  
Une association de cartes réseau dans ou sous l’hôte Hyper-V ne peut pas protéger le trafic SR-IOV, car elle ne passe pas par le commutateur Hyper-V.  Avec l’option d’association de cartes réseau de machines virtuelles, vous pouvez configurer deux commutateurs virtuels Hyper-V externes, chacun étant connecté à sa propre carte réseau compatible SR-IOV.  
  
![Association de cartes réseau avec des cartes réseau compatible SR-IOV](../../media/NIC-Teaming-in-Virtual-Machines--VMs-/nict_in_vm.jpg)  
  
Chaque machine virtuelle peut avoir une fonction virtuelle (VF) à partir d’une ou des deux cartes réseau SR-IOV et, en cas de déconnexion de la carte réseau, un basculement du VF principal vers l’adaptateur de sauvegarde (VF). Sinon, la machine virtuelle peut avoir un VF d’une carte réseau et un carte réseau non-VF connecté à un autre commutateur virtuel. Si la carte réseau associée au VF est déconnectée, le trafic peut basculer vers l’autre commutateur sans perte de connectivité.  
  
Dans la mesure où le basculement entre les cartes d’interface réseau d’une machine virtuelle peut entraîner l’envoi du trafic avec l’adresse MAC des autres carte réseau, chaque port commuté virtuel Hyper-V associé à une machine virtuelle utilisant l’Association de cartes réseau doit être défini pour permettre l’Association. 


## <a name="related-topics"></a>Rubriques connexes

- [Utilisation et gestion de l’Association de cartes réseau avec l’adresse Mac](NIC-Teaming-MAC-Address-Use-and-Management.md): Quand vous configurez une association de cartes réseau avec le mode indépendant du commutateur et le hachage d’adresse ou la distribution de charge dynamique, l’équipe utilise l’adresse MAC (Media Access Control) du membre de l’équipe de carte réseau principale sur le trafic sortant. Le membre de l’Association de cartes réseau principale est une carte réseau sélectionnée par le système d’exploitation à partir de l’ensemble initial des membres de l’équipe.

- [Paramètres d’association de cartes réseau](nic-teaming-settings.md): Dans cette rubrique, nous vous offrons une vue d’ensemble des propriétés de l’équipe de cartes réseau, telles que les modes d’association et d’équilibrage de charge. Nous vous fournissons également des détails sur le paramètre de l’adaptateur de secours et la propriété de l’interface d’équipe principale. Si vous disposez d’au moins deux cartes réseau dans une association de cartes réseau, vous n’avez pas besoin de désigner une carte de secours pour la tolérance de panne.
  
- [Créer une nouvelle association de cartes réseau sur un ordinateur hôte ou une machine virtuelle](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md): Dans cette rubrique, vous allez créer une nouvelle association de cartes réseau sur un ordinateur hôte ou dans une machine virtuelle Hyper-V exécutant Windows Server 2016.

- [Résolution des problèmes d’association de cartes réseau](Troubleshooting-NIC-Teaming.md): Dans cette rubrique, nous expliquons comment résoudre les problèmes liés à l’Association de cartes réseau, telles que le matériel, les titres de commutateur physique et la désactivation ou l’activation de cartes réseau à l’aide de Windows PowerShell. 
 
