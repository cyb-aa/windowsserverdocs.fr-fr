---
title: Créer une nouvelle équipe de cartes réseau sur un ordinateur hôte ou un ordinateur virtuel
description: Cette rubrique fournit des informations sur la configuration d’association de cartes réseau afin que vous comprenez les sélections que vous devez effectuer lorsque vous configurez une nouvelle équipe de cartes réseau dans Windows Server2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a4caaa86-5799-4580-8775-03ee213784a3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6a9866d1f4e72b3c77c3233b5e5582d250cfe6a2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="create-a-new-nic-team-on-a-host-computer-or-vm"></a>Créer une nouvelle équipe de cartes réseau sur un ordinateur hôte ou un ordinateur virtuel

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Cette rubrique fournit des informations sur la configuration d’association de cartes réseau afin que vous comprenez les sélections que vous devez effectuer lorsque vous configurez une nouvelle équipe de cartes réseau. Cette rubrique contient les sections suivantes.  
  
-   [Choix d’un Mode d’association](#bkmk_teaming)  
  
-   [Choix d’un Mode d’équilibrage de charge](#bkmk_lb)  
  
-   [Choix d’un paramètre de mise en veille de carte](#bkmk_standby)  
  
-   [À l’aide de la propriété d’Interface équipe principale](#bkmk_primary)  
  
> [!NOTE]  
> Si vous comprenez déjà ces éléments de configuration, vous pouvez utiliser les procédures suivantes pour configurer l’association de cartes réseau.  
>   
> -   [Créer une nouvelle équipe de cartes réseau dans une machine virtuelle](../../technologies/nic-teaming/Create-a-New-NIC-Team-in-a-VM.md)  
> -   [Créer une nouvelle équipe de cartes réseau](../../technologies/nic-teaming/Create-a-New-NIC-Team.md)  
  
Lorsque vous créez une nouvelle équipe de cartes réseau, vous devez configurer les propriétés d’association de cartes réseau suivantes.  
  
-   Nom de l’équipe  
  
-   Cartes membres  
  
-   Mode d’association de cartes réseau  
  
-   Mode d’équilibrage de la charge  
  
-   Carte de mise en veille  
  
Vous pouvez également configurer l’interface d’équipe principale et configurer un numéro de réseau local virtuel (VLAN) virtuel.  
  
Ces propriétés de l’association de cartes réseau sont affichées dans l’illustration suivante, qui contient des exemples de valeurs pour certaines propriétés de l’association de cartes réseau.  
  
![Propriétés de l’association de cartes réseau](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_06_properties.jpg)  
  
## <a name="bkmk_teaming"></a>Choix d’un Mode d’association  
Les options de mode d’association sont **indépendant du commutateur**, **association statique**, et **contrôle protocole LACP (Link Aggregation)**. Association statique et le protocole LACP sont modes dépendants du commutateur. Pour de meilleures performances d’association de cartes réseau avec tous les trois modes d’association, il est recommandé que vous utilisez un mode d’équilibrage de charge de la distribution dynamique.  
  
**Indépendants du commutateur**  
  
Avec mode basculer indépendant, le commutateur ou les commutateurs sur lesquels l’association de membres sont connectés ne seront pas informés de la présence de l’association de cartes réseau et ne déterminent pas comment distribuer le trafic réseau vers les membres de l’équipe de cartes réseau - au lieu de cela, l’équipe de cartes réseau répartit le trafic réseau entrant sur les membres de l’équipe de cartes réseau.  
  
Lorsque vous utilisez le mode indépendant du commutateur avec une distribution dynamique, la charge de trafic réseau est distribuée basée sur le hachage d’adresse de Ports TCP modifié par l’algorithme d’équilibrage de charge dynamique. L’algorithme d’équilibrage de charge dynamique redistribué par flux pour optimiser l’utilisation de la bande passante de membre de l’équipe afin que les transmissions de flux individuels peuvent déplacer à partir d’un membre de l’équipe active vers un autre. L’algorithme prend en compte la possibilité de petite que redistribution du trafic pouvant entraîner la sortie de livraison des paquets, afin qu’il prend les mesures nécessaires pour minimiser ce risque.  
  
**Dépendants du commutateur**  
  
Avec les modes basculer dépendant, le commutateur pour les membres de l’équipe de cartes réseau sont connectés détermine comment distribuer le trafic réseau entrant parmi les membres de l’équipe de cartes réseau. Le commutateur a indépendance pour déterminer comment répartir le trafic réseau entre les membres de l’équipe de cartes réseau.  
  
> [!IMPORTANT]  
> Commutateur dépendant d’association de cartes réseau nécessite que tous les membres de l’équipe sont connectés au même commutateur physique ou un châssis multi commutateur qui partage un ID de commutateur parmi plusieurs châssis.  
  
Association statique, vous devez configurer manuellement le commutateur et l’ordinateur hôte pour identifier les liens à partir de l’équipe. Dans la mesure où il s’agit d’une solution configurée de manière statique, il n’existe aucun protocole supplémentaire pour aider le commutateur et l’hôte d’identifier correctement branché câbles ou autres erreurs pouvant entraîner l’équipe ne parviennent pas à effectuer. Ce mode est généralement pris en charge par les commutateurs de classe serveur.  
  
Contrairement à une association statique, mode d’association de protocole LACP identifie dynamiquement des liens qui sont connectés entre l’ordinateur hôte et le commutateur. Cette connexion dynamique permet la création automatique d’une association et, en théorie, mais rarement dans la pratique, la réduction d’une équipe et l’extension simplement en la transmission ou la réception de paquets LACP à partir de l’entité d’homologue. Tous les commutateurs de classe serveur prennent en charge le protocole LACP et toutes nécessitent l’opérateur de réseau pour activer le protocole LACP sur le port de commutateur administrativement. Lorsque vous configurez un mode d’association de protocole LACP, association de cartes réseau fonctionne toujours en mode actif du protocole LACP avec un minuteur court.  Aucune option n’est actuellement disponible pour modifier le minuteur ou modifier le mode LACP.  
  
Lorsque vous utilisez des modes dépendants du commutateur avec une distribution dynamique, la charge de trafic réseau est distribuée basé sur le hachage d’adresse TransportPorts modifié par l’algorithme d’équilibrage de charge dynamique.  L’algorithme d’équilibrage de la charge dynamique redistribué par flux pour optimiser l’utilisation de la bande passante de membre de l’équipe. Les transmissions de flux individuels peuvent déplacer membres de l’équipe active à l’autre dans le cadre de la distribution dynamique. L’algorithme prend en compte la possibilité de petite que redistribution du trafic pouvant entraîner la sortie de livraison des paquets, afin qu’il prend les mesures nécessaires pour minimiser ce risque.  
  
Comme avec toutes les configurations dépendantes de commutateur, le commutateur détermine comment distribuer le trafic entrant parmi les membres de l’équipe.  Le commutateur est prévu pour effectuer une tâche raisonnable de répartir le trafic des membres de l’équipe, mais il a indépendance pour déterminer comment elle le fait.  
  
## <a name="bkmk_lb"></a>Choix d’un Mode d’équilibrage de charge  
Les options pour l’équilibrage de charge le mode de distribution sont **hachage d’adresse**, **Port Hyper-V**, et **dynamique**.  
  
**Hachage d’adresse**  
  
Ce mode d’équilibrage de charge crée un hachage basé sur les composants d’adresses du paquet. Il affecte alors les paquets qui ont cette valeur de hachage à une des cartes disponibles. Généralement, ce seul mécanisme suffise est suffisant pour créer un équilibre raisonnable entre les cartes disponibles.  
  
Vous pouvez utiliser Windows PowerShell pour spécifier des valeurs pour les composants de fonction de hachage suivants.  
  
-   Les ports TCP source et de destination et source et destination des adresses IP. Il s’agit de la valeur par défaut lorsque vous sélectionnez **hachage d’adresse** en tant que le mode d’équilibrage de charge.  
  
-   Source et destination uniquement des adresses IP.  
  
-   Adresses source et destination MAC uniquement.  
  
Le hachage de ports TCP crée la distribution plus fine des flux de trafic, aboutissant à un flux de plus petits qui peuvent être déplacés de manière indépendante entre les membres de l’équipe de cartes réseau. Toutefois, vous ne pouvez pas utiliser le hachage de ports TCP pour le trafic non TCP ou UDP, ou lorsque les ports TCP et UDP sont masqués à partir de la pile, comme avec le trafic protégé par IPsec. Dans ces cas, la valeur de hachage utilise automatiquement le hachage d’adresse IP, ou si le trafic n’est pas le trafic IP, le hachage d’adresse MAC est utilisé.  
  
**Port Hyper-V**  
  
Il existe un avantage de l’utilisation du mode de Port Hyper-V pour les associations de cartes réseau qui sont configurés sur des ordinateurs hôtes Hyper-V. Étant donné que les ordinateurs virtuels ont des adresses MAC indépendantes, adresse MAC de l’ordinateur - ou le port de que l’ordinateur virtuel est connecté par le commutateur virtuel Hyper-V - peut servir de base sur lequel vous souhaitez diviser le trafic réseau entre les membres de l’association de cartes réseau.  
  
> [!IMPORTANT]  
> Associations de cartes réseau que vous créez dans des ordinateurs virtuels ne peuvent pas être configurées avec le mode d’équilibrage de la charge Port Hyper-V. Utiliser le hachage d’adresse à la place de mode d’équilibrage de la charger.  
  
Étant donné que le commutateur adjacent voit toujours une adresse MAC spécifique sur un seul port, le commutateur distribue la charge de pénétration (le trafic du commutateur à l’ordinateur hôte) sur plusieurs liens en fonction de l’adresse MAC (VM MAC) de destination. Cela est particulièrement utile lorsque les files d’attente de la Machine virtuelle (Vmq) sont utilisés, car une file d’attente peut être placé sur la carte d’interface réseau spécifique où le trafic est supposé arriver.  
  
Toutefois, si l’ordinateur hôte a seuls les quelques ordinateurs virtuels, ce mode peut-être pas suffisamment précis pour obtenir une distribution bien équilibrée. Ce mode limite également toujours un seul ordinateur virtuel (par exemple, le trafic à partir d’un port de commutateur unique) pour la bande passante disponible sur une interface unique. Association de cartes réseau utilise le Port de commutateur virtuel Hyper-V en tant que l’identificateur au lieu d’utiliser l’adresse MAC source car, dans certains cas, un ordinateur virtuel peut être configuré avec plusieurs adresses MAC sur un port de commutateur.  
  
**Dynamique**  
  
Ce mode d’équilibrage utilise les meilleurs aspects de chacune des deux autres modes et les regroupe dans un seul mode:  
  
-   Charges de trafic sortants sont distribués basé sur un hachage des adresses IP et Ports TCP.  Mode dynamique rééquilibre également charges en temps réel afin qu’un flux de trafic sortant donné peut aller et venir entre les membres de l’équipe.  
  
-   Charges de trafic entrants sont distribuées de la même manière que le mode de port Hyper-V.  
  
Les charges de trafic sortants dans ce mode sont équilibrées dynamiquement en fonction de la notion de flowlets. Tout comme la parole a sauts naturelles à la fin des mots et des phrases, les flux TCP (flux de communication TCP) ont également naturelles sauts. La partie d’un flux TCP entre deux sauts de ce type est appelée un flowlet.  
  
Lorsque l’algorithme de mode dynamique détecte qu’une limite flowlet s’est produite - par exemple, quand un saut de longueur suffisante s’est produite dans le flux TCP - l’algorithme rééquilibre automatiquement le flux à un autre membre de l’équipe si nécessaire.  Dans certains cas l’algorithme peut également régulièrement rééquilibrer les flux qui ne contiennent pas de n’importe quel flowlets. Pour cette raison, l’affinité entre les membres d’équipe et des flux TCP peut modifier à tout moment que l’algorithme d’équilibrage de la dynamique fonctionne afin d’équilibrer la charge de travail membre de l’équipe.  
  
Si l’équipe est configurée avec le commutateur indépendant ou un des modes dépendants du commutateur, il est recommandé d’utiliser le mode de distribution dynamique pour de meilleures performances.  
  
Il existe une exception à cette règle lorsque l’équipe de cartes réseau a seulement deux membres de l’équipe, est configuré en mode indépendant du commutateur et a activé, avec une carte réseau active le mode actif/mise en veille et l’autre configuré pour la mise en veille. Avec cette configuration de l’association de cartes réseau, distribution de hachage d’adresse légèrement plus performant que dynamique distribution.  
  
## <a name="bkmk_standby"></a>Choix d’un paramètre de mise en veille de carte  
Les options de mise en veille carte sont None (toutes les cartes Active) ou votre sélection d’une carte réseau spécifique dans l’équipe de cartes réseau qui agira en tant qu’une carte de mise en veille, tandis que les autres membres de l’équipe non sélectionnés sont actifs. Lorsque vous configurez une carte réseau en tant qu’une carte de mise en veille, aucun trafic réseau est envoyé vers ou traitée par la carte, sauf si et jusqu’au moment où lorsqu’une carte réseau Active échoue. Après l’échec d’une carte réseau Active, la carte réseau de secours devient active et traite le trafic réseau. Si tous les membres de l’équipe sont remis en service, membre de l’équipe en attente est renvoyé à l’état de secours.  
  
Si vous disposez d’une association de deux et que vous choisissez de configurer une carte réseau en tant qu’une carte de mise en veille, vous perdez la bande passante avantages d’agrégation qui existent avec deux cartes réseau actives.  
  
> [!IMPORTANT]  
> Vous n’avez pas besoin de désigner une carte de veille pour atteindre une tolérance de panne; tolérance de panne est toujours présente chaque fois qu’il existe au moins deux cartes réseau dans une association de cartes réseau.  
  
## <a name="bkmk_primary"></a>À l’aide de la propriété d’Interface équipe principale  
Pour accéder à la boîte de dialogue Interface d’équipe principale, vous devez cliquer sur le lien est mis en surbrillance dans l’illustration ci-dessous.  
  
![Propriété d’Interface équipe principale](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_10_primaryteaminterface.jpg)  
  
Après avoir cliqué sur le lien en surbrillance, les éléments suivants **nouvelle Interface d’équipe** boîte de dialogue s’ouvre.  
  
![Boîte de dialogue Nouvelle Interface d’équipe](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_newteaminterface.jpg)  
  
Si vous utilisez des réseaux locaux virtuels, vous pouvez utiliser cette boîte de dialogue pour spécifier un numéro de réseau local virtuel.  
  
Si vous utilisez des réseaux locaux virtuels, ou non, vous pouvez spécifier un nom de tNIC pour l’équipe de cartes réseau.  
  
## <a name="see-also"></a>Voir aussi  
[Créer une nouvelle équipe de cartes réseau dans une machine virtuelle](../../technologies/nic-teaming/Create-a-New-NIC-Team-in-a-VM.md)  
[Association de cartes réseau](NIC-Teaming.md)  
  


