---
title: Association de cartes réseau
description: Cette rubrique fournit une vue d’ensemble de l’association de cartes d’Interface réseau (NIC) dans Windows Server 2016.
manager: brianlic
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
ms.openlocfilehash: 142f56153187368effdb802c0c1b50359fffc36a
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="nic-teaming"></a>Association de cartes réseau

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Cette rubrique fournit une vue d’ensemble de l’association de cartes d’Interface réseau (NIC) dans Windows Server 2016.

> [!NOTE]  
> Outre cette rubrique, le contenu suivant d’association de cartes réseau est disponible.  
>   
> - [Association de cartes dans des Machines virtuelles & #40; Ordinateurs virtuels & #41;](nict-vms.md)
> - [Association de cartes réseau et les réseaux locaux virtuels & #40; Réseaux locaux virtuels & #41;](nict-and-vlans.md)
> - [Association de gestion et utilisation des adresses MAC](NIC-Teaming-MAC-Address-Use-and-Management.md)
> - [Résolution des problèmes d’association de cartes réseau](Troubleshooting-NIC-Teaming.md) 
> - [Créer une nouvelle équipe de cartes réseau sur un ordinateur hôte ou un ordinateur virtuel](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md)
> - [Association de cartes applets de commande (NetLBFO) dans Windows PowerShell](https://technet.microsoft.com/library/jj130849.aspx)
> - Téléchargement de la Galerie TechNet: [Windows Server 2016 cartes réseau et le commutateur Embedded Teaming Guide de l’utilisateur](https://gallery.technet.microsoft.com/Windows-Server-2016-839cb607?redir=0)
  
## <a name="bkmk_over"></a>Vue d’ensemble de l’association de cartes réseau  
Association de cartes réseau vous permet de regrouper entre une et trente deux cartes réseau Ethernet physiques dans une ou plusieurs cartes réseau virtuel basé sur les logiciels. Ces cartes réseau virtuelles fournissent des performances élevées et la tolérance de panne en cas de défaillance de la carte réseau.  
  
Toutes les cartes réseau de l’association de cartes réseau doivent être installés sur le même ordinateur hôte physique doivent être placés dans une équipe.  
  
> [!NOTE]  
> Une association qui contient une seule carte réseau ne peut pas fournir l’équilibrage de charge et basculement; Toutefois avec une carte réseau, vous pouvez utiliser l’association de séparer le trafic réseau lorsque vous utilisez également des réseaux locaux virtuels (VLAN).  
  
Lorsque vous configurez les cartes réseau dans une association de cartes réseau, ils sont connectés à la carte réseau association solution tronc commun, qui présente ensuite une ou plusieurs cartes virtuelles (également appelés équipe cartes réseau [tNICs] ou interfaces équipe) pour le système d’exploitation. Windows Server 2016 prend en charge jusqu'à 32 interfaces équipe par l’équipe. Il existe une variété d’algorithmes qui distribuent le trafic sortant (chargement) entre les cartes réseau.  
  
L’illustration suivante représente une association avec plusieurs tNICs.  
  
![Équipe de cartes réseau avec plusieurs tNICs](../../media/NIC-Teaming/nict_overview.jpg)  
  
En outre, vous pouvez connecter vos cartes réseau associées au même commutateur ou à des commutateurs distincts. Si vous vous connectez les cartes réseau à différents commutateurs, les deux commutateurs doivent être sur le même sous-réseau.  
  
## <a name="bkmk_avail"></a>Disponibilité de l’association de cartes réseau  
Association de cartes réseau est disponible dans toutes les versions de Windows Server 2016. En outre, vous pouvez utiliser les commandes Windows PowerShell, les services Bureau à distance et les outils d’Administration de serveur distant pour gérer l’association de cartes réseau à partir d’ordinateurs qui exécutent un système d’exploitation de client sur lequel les outils sont pris en charge.  
  
## <a name="bkmk_nics"></a>Cartes réseau prises en charge et non pris en charge pour l’association de cartes réseau  
Vous pouvez utiliser une carte réseau Ethernet a passé le test de Qualification de matériel Windows et le Logo (tests WHQL) dans une association de cartes réseau dans Windows Server 2016.  
  
Les cartes réseau suivantes ne peut pas être placés dans une association.  
  
-   Cartes réseau virtuel Hyper-V qui sont exposés en tant que cartes réseau dans la partition hôte des ports de commutateur virtuel Hyper-V.  
  
    > [!IMPORTANT]  
    > Cartes réseau virtuelles Hyper-V qui sont exposées dans la partition hôte (cartes réseau virtuelles) ne doivent pas être placés dans une équipe. L’association de cartes réseau virtuelles au sein de la partition de l’ordinateur hôte n’est pas pris en charge dans une configuration ou une combinaison. Tentatives de cartes réseau virtuelles de l’équipe peuvent entraîner une perte complète de communication cas de panne réseau.  
  
-   (KDNIC) de la carte du réseau de débogage du noyau.  
  
-   Cartes réseau qui sont utilisés pour le démarrage réseau.  
  
-   Cartes réseau qui utilisent des technologies de Ethernet, tels que WWAN, WLAN/Wi-Fi, Bluetooth et Infiniband, y compris Internet Protocol sur les cartes réseau Infiniband (IPoIB).  
  
## <a name="bkmk_compat"></a>Compatibilité de l’association de cartes réseau  
Association de cartes réseau est compatible avec toutes les technologies de mise en réseau dans Windows Server 2016 avec les exceptions suivantes.  
  
-   **Virtualisation d’e/s de racine unique (SR-IOV)**. Pour SR-IOV, les données sont fournies directement à la carte réseau sans passer par la pile de mise en réseau (dans le système d’exploitation, dans le cas de virtualisation). Par conséquent, il n’est pas possible pour l’équipe de cartes réseau pour inspecter ou rediriger les données vers un autre chemin dans l’équipe.  
  
-   **Hôte natif qualité de Service (QoS)**. Lorsque les stratégies de QoS sont définies sur natif ou système hôte et ces stratégies appellent des limitations de bande passante minimale, le débit global pour une équipe de cartes réseau est moins qu’il serait sans les stratégies de la bande passante en place.  
  
-   **TCP Chimney**. TCP Chimney n’est pas pris en charge avec l’association de cartes réseau, car le déchargement TCP Chimney décharge toute la pile de mise en réseau directement à la carte réseau.  
  
-   **802. 1 x d’authentification**. 802. 1 l’authentification X ne doit pas être utilisée avec l’association de cartes réseau. Certains commutateurs n’autorisent pas la configuration d’authentification de 802. 1 X et que l’association de cartes réseau sur le même port.  
  
Pour en savoir plus sur l’utilisation de l’association de cartes réseau dans les machines virtuelles (VM) qui s’exécutent sur un ordinateur hôte Hyper-V, voir [association de cartes réseau dans les Machines virtuelles & #40; Ordinateurs virtuels & #41; ](../../technologies/nic-teaming/../../technologies/nic-teaming/NIC-Teaming-in-Virtual-Machines--VMs-.md).  
  
## <a name="bkmk_vmq"></a>Association de cartes réseau et les files d’attente de la Machine virtuelle (Vmq)  
VMQ et association de cartes réseau fonctionnent bien ensemble; VMQ doit être activé à tout moment Hyper-V est activé. Selon le mode de configuration du commutateur et l’algorithme de distribution de charge, l’association de cartes réseau soit présente les fonctionnalités VMQ au commutateur Hyper-V qui indiquent le nombre de files d’attente disponibles pour être le plus petit nombre de files d’attente de la prise en charge par n’importe quelle carte dans l’équipe (mode Min-files d’attente) ou le nombre total de files d’attente disponibles sur tous les membres de l’équipe (mode de somme de files d’attente).  
  
Plus précisément, si l’équipe est dans indépendants du commutateur association de cartes réseau en mode et la Distribution de la charge est défini sur le mode de Port Hyper-V ou dynamiques, puis le nombre de files d’attente signalé est la somme de toutes les files d’attente disponibles à partir des membres de l’équipe (mode somme de files d’attente); dans le cas contraire, le nombre de files d’attente signalé est le plus petit nombre de files d’attente de la prise en charge par tous les membres de l’équipe (mode Min-files d’attente).  
  
Voici pourquoi:  
  
-   Lorsque l’équipe indépendants du commutateur est en mode de Port Hyper-V ou dynamique du trafic entrant pour un port de commutateur Hyper-V (VM) sera toujours arrivent sur le même membre de l’équipe. L’ordinateur hôte peut prévoir/contrôle le membre qui reçoit le trafic pour un ordinateur virtuel afin de l’association de cartes réseau peut être plus réfléchi sur les files d’attente de VMQ à allouer sur un membre particulier. Association de cartes réseau fonctionne avec le commutateur Hyper-V, pour définir la file pour un ordinateur virtuel sur un membre de l’équipe et savoir que le trafic entrant sera atteint cette file d’attente.  
  
-   Lorsque l’équipe est dans n’importe quel mode dépendants du commutateur (association statique ou LACP association de cartes réseau), le commutateur connecté à l’équipe de contrôle de la distribution du trafic entrant. Logiciel d’association de cartes réseau de l’ordinateur hôte ne peut pas prédire les de l’équipe membre obtiendra le trafic entrant pour un ordinateur virtuel, et il peut être que le commutateur répartit le trafic pour un ordinateur virtuel sur tous les membres de l’équipe. Comme le logiciel d’association de cartes réseau, fonctionne avec le commutateur Hyper-V, un résultat de programmes une file d’attente pour l’ordinateur virtuel sur chaque membre de l’équipe, pas seulement un membre de l’équipe.  
  
-   Lorsque l’équipe est en mode indépendants du commutateur et utilise un algorithme de distribution de charge de hachage adresse, le trafic entrant toujours arriveront sur une carte réseau (membre de l’équipe principal) - tout membre de l’équipe qu’un seul. Dans la mesure où les autres membres de l’équipe ne sont pas gérer le trafic entrant, avec qu'ils obtient programmées la même file d’attente en tant que le membre principal afin que si le membre principal échoue de n’importe quel autre membre de l’équipe peut être utilisé afin de sélectionner le trafic entrant et les files d’attente sont déjà en place.  
  
La plupart des cartes réseau ont des files d’attente qui peuvent être utilisés pour la mise à l’échelle côté réception (RSS) ou file, mais pas les deux en même temps. Certains paramètres VMQ paramètres pour les files d’attente RSS mais ne sont vraiment paramètres sur les files d’attente génériques qui RSS et VMQ utilisation en fonction de la fonctionnalité qui est actuellement en cours d’utilisation. Chaque carte réseau a dans ses propriétés avancées, des valeurs pour * RssBaseProcNumber et \*MaxRssProcessors. Voici quelques paramètres VMQ qui offrent de meilleures performances système.  
  
-   Dans l’idéal, chaque carte réseau doit avoir le * RssBaseProcNumber définie sur un nombre pair supérieur ou égal à deux (2). Il s’agit dans la mesure où le premier processeur physique, Core 0 (processeurs logiques 0 et 1), en règle générale effectue la plupart du système de traitement pour le traitement réseau doit être directrices hors de ce processeur physique. (Certains architectures de l’ordinateur n’ont pas deux processeurs logiques par processeur physique aussi pour ces ordinateurs virtuels, le processeur de base doit être supérieure ou égale à 1. Si des doutes supposent que votre hôte est équipé d’un processeur logique 2 par architecture de processeur physique.)  
  
-   Si l’équipe est en mode de somme de files d’attente des processeurs de membres de l’équipe doivent être l’étendue pratique, sans chevauchement. Par exemple, dans un hôte de 4 cœurs (8 processeurs logiques) avec une association de cartes réseau de 10 Gbit/s 2, vous pouvez définir l’utilisation du processeur de base de 2 et d’utiliser 4 cœurs; la seconde serait définie à utiliser le processeur base 6 et 2 cœurs.  
  
-   Si l’équipe est en mode Min-files d’attente, les jeux de processeurs utilisés par les membres de l’équipe doivent être identiques.  
  
## <a name="bkmk_hnv"></a>Virtualisation de réseau association de cartes réseau et Hyper-V (HNV)  
Association de cartes réseau est entièrement compatible avec la virtualisation de réseau Hyper-V (HNV).  Le système de gestion HNV fournit des informations sur le pilote d’association de cartes réseau qui permet l’association de cartes réseau distribuer la charge d’une manière qui est optimisée pour le trafic HNV.  
  
## <a name="bkmk_live"></a>Association de cartes réseau et la Migration dynamique  
Association de cartes réseau dans des machines virtuelles n’affecte pas la Migration dynamique. Les mêmes règles existent pour la Migration dynamique ou non association de cartes réseau est configuré dans l’ordinateur virtuel.  
  
## <a name="see-also"></a>Voir aussi  
[Association de cartes dans des Machines virtuelles & #40; Ordinateurs virtuels & #41;](../../technologies/nic-teaming/../../technologies/nic-teaming/NIC-Teaming-in-Virtual-Machines--VMs-.md)  
  


