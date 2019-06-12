---
title: Association de cartes réseau
description: Dans cette rubrique, nous vous donner une vue d’ensemble de l’association de carte d’Interface réseau (NIC) dans Windows Server 2016. Association de cartes réseau vous permet de regrouper entre 1 et 32 de cartes réseau Ethernet physiques dans un ou plusieurs adaptateurs de réseau virtuel basé sur le logiciel. Ces cartes réseau virtuelles fournissent des performances élevées et une tolérance de panne importante en cas de défaillance de la carte réseau.
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: abded6f3-5708-4e35-9a9e-890e81924fec
ms.author: pashort
author: shortpatti
ms.date: 09/10/2018
ms.openlocfilehash: cf58956ead8e8a47b8ec6d189bf23e5c576d5f15
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812183"
---
# <a name="nic-teaming"></a>Association de cartes réseau

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Dans cette rubrique, nous vous donner une vue d’ensemble de l’association de carte d’Interface réseau (NIC) dans Windows Server 2016. Association de cartes réseau vous permet de regrouper entre 1 et 32 de cartes réseau Ethernet physiques dans un ou plusieurs adaptateurs de réseau virtuel basé sur le logiciel. Ces cartes réseau virtuelles fournissent des performances élevées et une tolérance de panne importante en cas de défaillance de la carte réseau.  
  
> [!IMPORTANT]
> Vous devez installer des cartes réseau de l’association de cartes réseau dans le même ordinateur hôte physique. 

> [!TIP]  
> Une association de cartes réseau qui contient une seule carte réseau ne peut pas fournir l’équilibrage de charge et le basculement. Toutefois, avec une carte réseau, vous pouvez utiliser association de cartes réseau pour séparer le trafic réseau lorsque vous utilisez également des réseaux locaux virtuels (VLAN).  
  
Lorsque vous configurez les cartes réseau dans une association de cartes réseau, ils se connectent à la carte réseau association solution tronc commun, qui présente ensuite un ou plusieurs cartes virtuelles (également appelés équipe cartes réseau [tNICs] ou interfaces de l’équipe) pour le système d’exploitation. 

Dans la mesure où Windows Server 2016 prend en charge jusqu'à 32 interfaces d’équipe par équipe, il existe une variété d’algorithmes qui distribuent le trafic sortant (charge) entre les cartes réseau.  L’illustration suivante représente une association de cartes réseau avec plusieurs tNICs.  
  
![Association de cartes réseau avec plusieurs tNICs](../../media/NIC-Teaming/nict_overview.jpg)  
  
En outre, vous pouvez connecter vos cartes réseau associées au même commutateur ou différents commutateurs. Si vous vous connectez les cartes réseau à différents commutateurs, les deux commutateurs doivent être sur le même sous-réseau.  
  
## <a name="availability"></a>Disponibilité  
Association de cartes réseau est disponible dans toutes les versions de Windows Server 2016. Vous pouvez utiliser un large éventail d’outils pour gérer l’association de cartes réseau des ordinateurs exécutant un système d’exploitation client, telles que : • • des applets de commande Windows PowerShell Bureau à distance • Outils d’Administration de serveur distant  
  
## <a name="supported-and-unsupported-nics"></a>Cartes réseau prises en charge et non pris en charge   
Vous pouvez utiliser une carte réseau Ethernet qui a passé le test de Qualification du matériel Windows et le Logo (tests WHQL) dans une association de cartes réseau dans Windows Server 2016.  
  
Vous ne pouvez pas placer les cartes réseau suivantes dans une association de cartes réseau :
  
-   Adaptateurs de réseau virtuel Hyper-V qui sont exposées en tant que cartes d’interface réseau dans la partition hôte des ports de commutateur virtuel Hyper-V.  
  
    > [!IMPORTANT]  
    > Ne placez pas les cartes réseau virtuelles de Hyper-V exposées dans la partition hôte (VNIC) dans une équipe. L’association de cartes réseau virtuelles à l’intérieur de la partition hôte n’est pas pris en charge dans une configuration. Tente de cartes réseau virtuelles de l’équipe peut entraîner une perte de communication complète en cas de défaillances du réseau.  
  
-   Adaptateur de réseau de débogage du noyau (KDNIC).  
  
-   Cartes d’interface réseau utilisés pour le démarrage réseau.  
  
-   Cartes réseau qui utilisent des technologies autres que Ethernet, tels que WWAN, WLAN/Wi-Fi, Bluetooth et Infiniband, y compris le protocole d’Internet sur les cartes réseau Infiniband (IPoIB).  
  
## <a name="compatibility"></a>Compatibilité  
Association de cartes réseau est compatible avec toutes les technologies de mise en réseau dans Windows Server 2016 avec les exceptions suivantes.  
  
-   **La virtualisation d’e/s de racine unique (SR-IOV)** . Dans le SR-IOV, les données sont fournies directement à la carte réseau sans passer par la pile réseau (dans le système d’exploitation hôte, dans le cas de virtualisation). Par conséquent, il n’est pas possible pour l’association de cartes réseau inspecter ou rediriger les données vers un autre chemin dans l’équipe.  
  
-   **Qualité de Service (QoS) de hôte natif**. Lorsque vous définissez des stratégies de QoS sur natif ou le système hôte et ces stratégies d’appel de limitations de bande passante minimale, le débit global d’une association de cartes réseau est inférieure à celle qu’elle serait sans les stratégies de la bande passante en place.  
  
-   **TCP Chimney**. TCP Chimney n’est pas pris en charge avec l’association de cartes réseau, car le déchargement TCP Chimney décharge toute la pile de mise en réseau directement à la carte réseau.  
  
-   **802. 1 x authentification**. Vous ne devez pas utiliser authentification de 802. 1 X avec association de cartes réseau, car certains commutateurs n’autorisent pas la configuration d’authentification de 802. 1 X et que l’association de cartes réseau sur le même port.  
  
Pour en savoir plus sur l’utilisation d’association de cartes réseau dans des machines virtuelles (VM) qui s’exécutent sur un hôte Hyper-V, consultez [créer une association de cartes réseau sur un ordinateur hôte ou d’une machine virtuelle](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md).
  
## <a name="virtual-machine-queues-vmqs"></a>Files d’attente de la Machine virtuelle (Vmq)  

Vmq est une fonctionnalité de carte réseau qui alloue une file d’attente pour chaque machine virtuelle.  Chaque fois que vous avez Hyper-V est activé ; Vous devez également activer des ordinateurs virtuels. Dans Windows Server 2016, Vmq permet vPorts de commutateur de la carte réseau avec une file d’attente unique affecté à la vPort fournissent les mêmes fonctionnalités. 

Selon le mode de configuration de commutateur et l’algorithme de distribution de charge, association de cartes réseau présente le plus petit nombre de files d’attente disponibles et pris en charge par n’importe quelle carte dans l’équipe (mode Min-files d’attente) ou le nombre total de files d’attente disponibles au sein de tous les équipe membres (mode de somme de files d’attente).  

Si l’équipe est en mode d’association indépendant du commutateur et vous définissez la distribution de charge au mode de Port Hyper-V ou dynamique, le nombre de files d’attente signalé est la somme de toutes les files d’attente disponibles à partir des membres de l’équipe (mode de somme de files d’attente). Sinon, le nombre de files d’attente signalé est le plus petit nombre de files d’attente de la prise en charge par n’importe quel membre de l’équipe (mode Min-files d’attente).

Voici pourquoi :  
  
-   Lorsque l’équipe indépendant du commutateur est en mode de Port Hyper-V ou dynamique du trafic entrant pour un port de commutateur Hyper-V (VM) toujours arrive sur le même membre de l’équipe. L’hôte peut prédire/contrôle le membre qui reçoit le trafic pour une machine virtuelle particulière afin de l’association de cartes réseau peut être plus réfléchie sur les files d’attente de VMQ à allouer sur un membre d’équipe particulier. Association de cartes réseau, utilisation avec le commutateur Hyper-V, définit les ordinateurs virtuels pour une machine virtuelle sur exactement un seul membre de l’équipe et de savoir que le trafic entrant atteint cette file d’attente.  
  
-   Lorsque l’équipe est dans n’importe quel mode dépendants du commutateur (association statique ou l’association LACP), le commutateur de l’équipe est connectée au contrôle de la distribution du trafic entrant. Le logiciel d’association de cartes réseau de l’hôte ne peut pas prévoir quelle équipe membre Obtient le trafic entrant pour une machine virtuelle, et il peut être que le commutateur répartit le trafic pour une machine virtuelle sur tous les membres de l’équipe. Comme un résultat de l’association de cartes réseau logiciels, avec le commutateur Hyper-V, les programmes une file d’attente pour la machine virtuelle sur chaque membre de l’équipe, pas seulement un membre de l’équipe.  
  
-   Lorsque l’équipe est dans le mode indépendant du commutateur et hachage d’adresse utilise l’équilibrage de charge, le trafic entrant arrive toujours sur une carte réseau (membre de l’équipe principal) - tout cela sur le membre de l’équipe qu’un seul. Étant donné que les autres membres de l’équipe ne sont pas affaire à trafic entrant, ils obtient programmés avec les files d’attente mêmes en tant que le membre principal afin que si le membre principal échoue, n’importe quel autre membre de l’équipe peut être utilisé pour prélever le trafic entrant, et les files d’attente sont déjà en place.  

- La plupart des cartes réseau ont des files d’attente utilisés pour la mise à l’échelle côté réception (RSS) ou VMQ, mais pas en même temps. Certains paramètres VMQ semblent être des paramètres pour les files d’attente RSS mais sont des paramètres sur les files d’attente génériques qui RSS et VMQ utilisation selon quelle fonctionnalité est actuellement en cours d’utilisation. Chaque carte réseau a dans ses propriétés avancées, les valeurs pour * RssBaseProcNumber et \*MaxRssProcessors. Voici quelques paramètres VMQ qui offrent de meilleures performances système.  
  
-   Dans l’idéal, chaque carte réseau doit avoir le * RssBaseProcNumber la valeur est un nombre pair supérieur ou égal à deux (2). Le premier processeur physique, en règle générale, les principaux 0 (processeurs logiques 0 et 1), effectue la majeure partie du système de traitement pour le traitement réseau doit diriger en dehors de ce processeur physique. Certaines architectures machine n’ont pas deux processeurs logiques par processeur physique, donc pour ces ordinateurs, le processeur de base doit être supérieur ou égal à 1. Si dans le doute supposer que votre hôte utilise un processeur logique 2 par architecture de processeur physique.  
  
-   Si l’équipe est en mode de la somme des files d’attentes des processeurs de membres de l’équipe doivent être sans chevauchement. Par exemple, dans un hôte de 4 cœurs (8 processeurs logiques) avec une équipe de 2 cartes réseau de 10 Gbits/s, vous pouvez définir la première condition à utiliser le processeur de base de 2 et à utiliser les 4 cœurs ; utiliser le processeur de base 6 et 2 cœurs, la seconde est définie.  
  
-   Si l’équipe est en mode de files d’attente de Min les jeux de processeur utilisés par les membres d’équipe doivent être identiques.  

  
## <a name="hyper-v-network-virtualization-hnv"></a>Virtualisation de réseau Hyper-V  
Association de cartes réseau est entièrement compatible avec la virtualisation de réseau Hyper-V (HNV).  Le système de gestion de HNV fournit des informations pour le pilote d’association de cartes réseau qui permet l’association de cartes réseau répartir la charge d’une manière qui optimise le trafic HNV.  
  
## <a name="live-migration"></a>Migration dynamique  
L’association de cartes réseau sur des machines virtuelles n’affecte pas la Migration dynamique. Les mêmes règles existent pour la Migration en direct déterminant la configuration d’association de cartes réseau dans la machine virtuelle.  


## <a name="virtual-local-area-networks-vlans"></a>Réseaux locaux virtuels (VLAN)
Lorsque vous utilisez l’association de cartes réseau, la création de plusieurs interfaces de l’équipe permet à un hôte pour vous connecter à différents réseaux locaux virtuels en même temps. Configurer votre environnement en suivant les recommandations suivantes :
  
- Avant d’activer association de cartes réseau, configurez les ports de commutateur physique connectés à l’hôte d’association à utiliser le mode trunk (proximité). Le commutateur physique doit passer tout le trafic à l’hôte pour le filtrage sans modifier le trafic.  

- Ne configurez pas les filtres de réseau local virtuel sur les cartes réseau à l’aide de la carte réseau que les paramètres de propriétés avancée. Laisser le logiciel d’association de cartes réseau ou le commutateur virtuel Hyper-V (le cas échéant) effectuer un filtrage du réseau local virtuel.  
  
### <a name="use-vlans-with-nic-teaming-in-a-vm"></a>Utiliser des réseaux locaux virtuels avec l’association de cartes réseau dans une machine virtuelle  
Lorsqu’une équipe se connecte à un commutateur virtuel Hyper-V, tous les ségrégation de réseau local virtuel doit être effectuée dans le commutateur virtuel Hyper-V plutôt que dans l’association de cartes réseau.  

Envisagez d’utiliser des réseaux locaux virtuels sur un ordinateur virtuel configuré avec une association de cartes réseau en suivant les recommandations suivantes :
  
-   La méthode de prise en charge de plusieurs réseaux locaux virtuels dans une machine virtuelle recommandée consiste à configurer la machine virtuelle avec plusieurs ports sur le commutateur virtuel Hyper-V et associer chaque port à un réseau local virtuel. Équipe jamais ces ports dans la machine virtuelle, car cela entraîne des problèmes de communication réseau.  

-   Si la machine virtuelle a plusieurs fonctions virtuelles (VFs) de SR-IOV, vérifiez qu’ils sont sur le même réseau local virtuel avant leur association dans la machine virtuelle. Vous pouvez facilement configurer le système de fichiers virtuel différent pour être sur différents réseaux locaux virtuels et que cela entraîne des problèmes de communication réseau.  
 
  
### <a name="manage-network-interfaces-and-vlans"></a>Gérer les interfaces réseau et les réseaux locaux virtuels 
Si vous devez disposer de plusieurs VLAN exposé dans un système d’exploitation, pensez à renommer les interfaces Ethernet pour clarifier le réseau local virtuel attribué à l’interface. Par exemple, si vous associez **Ethernet** interface avec 12 de réseau local virtuel et le **Ethernet 2** l’interface avec le réseau local virtuel 48, renommez l’interface Ethernet pour **EthernetVLAN12** et le autres à **EthernetVLAN48**. 

Renommer des interfaces à l’aide de la commande Windows PowerShell **Rename-NetAdapter** ou en effectuant la procédure suivante :
  
1.  Dans le Gestionnaire de serveur, dans **propriétés** pour la carte réseau que vous souhaitez renommer, cliquez sur le lien à droite du nom de l’adaptateur de réseau. 
  
2.  Avec le bouton droit de la carte réseau que vous souhaitez renommer, puis sélectionnez **renommer**.  
  
3.  Tapez le nouveau nom pour la carte réseau et appuyez sur ENTRÉE.  


## <a name="virtual-machines-vms"></a>Machines virtuelles (VM)

Si vous souhaitez utiliser l’association de cartes réseau dans une machine virtuelle, vous devez connecter les cartes réseau virtuelles dans la machine virtuelle pour Hyper-V commutateurs virtuels externes uniquement. Cela permet à la machine virtuelle pour supporter la connectivité réseau même dans le cas lorsqu’une des cartes réseau physiques connectés à un seul commutateur virtuel échoue ou est déconnectée. Les cartes réseau virtuelles connectées à interne ou privé des commutateurs virtuels Hyper-V ne sont pas en mesure de se connecter au commutateur lorsqu’ils se trouvent dans une équipe, et mise en réseau échoue pour la machine virtuelle.  
  
L’association de cartes réseau dans Windows Server 2016 prend en charge les équipes avec deux membres dans les machines virtuelles. Vous pouvez créer des équipes plus grandes, mais il n’existe aucune prise en charge pour les équipes plus grandes. Chaque membre de l’équipe doit se connecter à un commutateur virtuel externe Hyper-V différent, et les interfaces réseau de la machine virtuelle doivent être configurés pour autoriser l’association de cartes.

  
Si vous configurez une association de cartes réseau dans une machine virtuelle, vous devez sélectionner un **mode d’association** de _indépendant du commutateur_ et un **mode d’équilibrage de charge** de _hachage d’adresse_.   
  
  
## <a name="sr-iov-capable-network-adapters"></a>Cartes réseau compatibles SR-IOV  
Une association de cartes réseau dans ou sous l’hôte Hyper-V ne peut pas protéger le trafic SR-IOV, car il n’accède pas à travers le commutateur Hyper-V.  Avec l’option de l’association de cartes réseau machine virtuelle, vous pouvez configurer deux commutateurs virtuels Hyper-V externe, chacun étant connecté à son propre compatibles SR-IOV carte réseau.  
  
![Avec les cartes réseau compatibles SR-IOV une association de cartes réseau](../../media/NIC-Teaming-in-Virtual-Machines--VMs-/nict_in_vm.jpg)  
  
Chaque machine virtuelle peut avoir une fonction virtuelle (VF) à partir d’un ou deux cartes d’interface réseau SR-IOV et, en cas de déconnexion de la carte réseau, le basculement depuis le facteur principal vers l’adaptateur de secours (VF). Par ailleurs, la machine virtuelle peut avoir une fonction virtuelle à partir d’une carte réseau et un réseau d’ordinateur virtuel non-VF connectés à un autre commutateur virtuel. Si la carte réseau associée à la fonction virtuelle est déconnectée, le trafic peut basculer vers l’autre commutateur sans perte de connectivité.  
  
Étant donné que le basculement entre les cartes réseau dans une machine virtuelle peut provoquer le trafic envoyé avec l’adresse MAC de l’autre réseau d’ordinateur virtuel, chaque port de commutateur virtuel Hyper-V associé à une machine virtuelle à l’aide d’association de cartes réseau doit être définie pour permettre l’association. 


## <a name="related-topics"></a>Rubriques connexes

- [Utilisation des adresses MAC de l’association de cartes réseau et la gestion](NIC-Teaming-MAC-Address-Use-and-Management.md): Lorsque vous configurez une association de cartes réseau avec le mode indépendant du commutateur et de hachage d’adresse ou distribution de charge dynamique, l’équipe utilise que l’accès au média (adresse MAC control) du membre principal association de cartes réseau sur le trafic sortant. Le membre d’association de cartes réseau principal est une carte réseau sélectionnée par le système d’exploitation à partir de l’ensemble initial de membres de l’équipe.

- [Paramètres d’association de cartes réseau](nic-teaming-settings.md): Dans cette rubrique, nous vous donner une vue d’ensemble des propriétés d’association de cartes réseau telles que l’association de cartes et modes d’équilibrage de charge. Nous vous donnons également plus d’informations sur le paramètre de carte de mise en veille et de la propriété d’interface équipe principale. Si vous avez au moins deux cartes réseau dans une association de cartes réseau, il est inutile de désigner une carte de mise en veille pour une tolérance de panne.
  
- [Créer une association de cartes réseau sur un ordinateur hôte ou d’une machine virtuelle](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md): Dans cette rubrique, vous créez une nouvelle association de cartes réseau sur un ordinateur hôte ou sur une machine de virtuelle (VM) Hyper-V exécutant Windows Server 2016.

- [Résolution des problèmes d’association de cartes réseau](Troubleshooting-NIC-Teaming.md): Dans cette rubrique, nous abordons les façons de résoudre les problèmes d’association de cartes réseau, telles que le matériel, les titres de commutateur physique et la désactivation ou l’activation des cartes réseau à l’aide de Windows PowerShell. 
 
