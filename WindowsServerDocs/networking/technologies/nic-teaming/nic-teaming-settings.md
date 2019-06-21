---
title: Paramètres d’association de cartes réseau
description: Dans cette rubrique, nous vous donner une vue d’ensemble des propriétés d’association de cartes réseau telles que l’association de cartes et modes d’équilibrage de charge. Nous vous donnons également plus d’informations sur le paramètre de carte de mise en veille et de la propriété d’interface équipe principale. Si vous avez au moins deux cartes réseau dans une association de cartes réseau, il est inutile de désigner une carte de mise en veille pour une tolérance de panne.
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a4caaa86-5799-4580-8775-03ee213784a3
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: dd222cdbcd8b4eee19da6b79e12bd11f6bdd8629
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283727"
---
# <a name="nic-teaming-settings"></a>Paramètres d’association de cartes réseau
Dans cette rubrique, nous vous donner une vue d’ensemble des propriétés d’association de cartes réseau telles que l’association de cartes et modes d’équilibrage de charge. Nous vous donnons également plus d’informations sur le paramètre de carte de mise en veille et de la propriété d’interface équipe principale. Si vous avez au moins deux cartes réseau dans une association de cartes réseau, il est inutile de désigner une carte de mise en veille pour une tolérance de panne.


  
![Propriétés de l’association de cartes réseau](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_06_properties.jpg)  

## <a name="teaming-modes"></a>Modes d’association 
Les options de mode d’association sont **indépendant du commutateur** et **dépendants du commutateur**. Le commutateur dépendants mode inclut **association statique** et **protocole LACP (Link Aggregation contrôle Protocol)** . 

>[!TIP]
>Pour de meilleures performances de l’association de cartes réseau, nous vous recommandons d’utiliser un mode d’équilibrage de charge de la distribution dynamique.  
  
### <a name="switch-independent"></a>Indépendant du commutateur
  
[!INCLUDE [switch-independent-shortdesc-include](../../includes/switch-independent-shortdesc-include.md)] 
  
Lorsque vous utilisez le mode indépendant du commutateur avec la distribution dynamique, la charge du trafic réseau est distribuée selon le hachage d’adresse de Ports TCP modifié par l’algorithme d’équilibrage de charge dynamique. L’algorithme d’équilibrage de charge dynamique redistribue les flux afin d’optimiser l’utilisation de la bande passante de membre équipe afin que les transmissions de flux individuels peuvent déplacer un membre d’équipe actif vers un autre. L’algorithme prend en compte la faible probabilité que le trafic de redistribution peut entraîner hors-de la livraison de paquets, afin qu’il prenne des mesures pour minimiser ce risque.  
  
### <a name="switch-dependent"></a>Dépendants du commutateur  

[!INCLUDE [switch-dependent-shortdesc-include](../../includes/switch-dependent-shortdesc-include.md)]  
  
> [!IMPORTANT]  
> Association dépendants du commutateur nécessite que tous les membres de l’équipe sont connectés au même commutateur physique ou un châssis multi commutateur qui partage un ID de commutateur entre plusieurs châssis.


- **Association statique.** Association statique, vous devez configurer manuellement le commutateur et l’hôte d’identifier les liens forment l’association. Comme il s’agit d’une solution configurée de manière statique, il n’existe aucun protocole supplémentaire afin de faciliter le commutateur et l’hôte pour identifier de manière incorrecte sur secteur câbles ou autres erreurs susceptibles de l’équipe ne parviennent pas à effectuer. Ce mode est généralement pris en charge par les commutateurs préconisés pour les serveurs.

- **Link Aggregation Control Protocol (LACP).** Contrairement à une association statique, mode d’association LACP identifie dynamiquement des liens qui sont connectés entre l’hôte et le commutateur. Cette connexion dynamique permet la création automatique d’une équipe et, en théorie, mais rarement dans la pratique, la réduction d’une association et l’extension simplement par la transmission ou la réception des paquets LACP à partir de l’entité pair. Tous les commutateurs de classe du serveur prend en charge le protocole LACP et requièrent tous l’opérateur de réseau à activer de manière administrative LACP sur le port de commutateur. Lorsque vous configurez un mode d’association du protocole LACP, association de cartes réseau fonctionne toujours en mode actif du protocole LACP avec un minuteur court.  Aucune option n’est actuellement disponible pour modifier la minuterie ou de modifier le mode LACP.


Lorsque vous utilisez des modes dépendants du commutateur avec la distribution dynamique, la charge du trafic réseau est distribuée selon le hachage d’adresse TransportPorts modifié par l’algorithme d’équilibrage de charge dynamique.  L’algorithme d’équilibrage de charge dynamique redistribue les flux afin d’optimiser l’utilisation de la bande passante équipe membre. Transmissions de flux peuvent déplacer des membres de l’équipe active vers un autre cadre de la distribution dynamique. L’algorithme prend en compte la faible probabilité que le trafic de redistribution peut entraîner hors-de la livraison de paquets, afin qu’il prenne des mesures pour minimiser ce risque.  
  
Comme avec toutes les configurations dépendantes de commutateur, le commutateur détermine comment distribuer le trafic entrant parmi les membres d’équipe.  Le commutateur est attendu pour effectuer un travail raisonnable de distribuer le trafic entre les membres d’équipe, mais il a une indépendance complète pour déterminer comment il le fait.  


## <a name="load-balancing-modes"></a>Modes d’équilibrage de charge  
Les options de mode de distribution d’équilibrage de charge sont **hachage d’adresse**, **Port Hyper-V**, et **dynamique**.  
  
### <a name="address-hash"></a>Hachage d’adresse
  
[!INCLUDE [address-hash-shortdesc-include](../../includes/address-hash-shortdesc-include.md)]
  
Utiliser Windows PowerShell pour spécifier des valeurs pour les composants de fonction de hachage suivants.  
  
-   Les ports TCP source et destination et source et destination des adresses IP. Ceci est la valeur par défaut lorsque vous sélectionnez **hachage d’adresse** comme mode d’équilibrage de charge.  
  
-   Adresses source et destination IP uniquement.  
  
-   Adresses source et destination MAC uniquement.  
  
Le hachage de ports TCP crée la distribution plus granulaire des flux de trafic, ce qui entraîne des flux de données plus petits qui permettre être déplacés de manière indépendante entre les membres d’équipe de carte réseau. Toutefois, vous ne pouvez pas utiliser le hachage de ports TCP pour le trafic non TCP ou UDP, ou où les ports TCP et UDP sont masqués de la pile, comme avec un trafic protégé par IPsec. Dans ce cas, la valeur de hachage utilise automatiquement le hachage d’adresse IP ou, si le trafic n’est pas le trafic IP, le hachage d’adresse MAC est utilisé.  
  
### <a name="hyper-v-port"></a>Port Hyper-V
  
[!INCLUDE [hyper-v-port-shortdesc-include](../../includes/hyper-v-port-shortdesc-include.md)]  
  
Étant donné que le commutateur adjacent voit toujours une adresse MAC spécifique sur un seul port, le commutateur distribue la charge de l’entrée (le trafic du commutateur à l’hôte) sur plusieurs liens en fonction de l’adresse MAC (machine virtuelle MAC) de destination. Cela est particulièrement utile lorsque les files d’attente de la Machine virtuelle (Vmq) sont utilisées, car une file d’attente permettre être placée sur la carte réseau spécifique où le trafic est supposé arriver.  
  
Toutefois, si l’hôte a uniquement quelques machines virtuelles, ce mode ne peut pas être suffisamment précis pour obtenir une distribution bien équilibrée. Ce mode limite également toujours une seule machine virtuelle (par exemple, le trafic à partir d’un port de commutateur unique) pour la bande passante disponible sur une interface unique. Association de cartes réseau utilise le Port de commutateur virtuel Hyper-V en tant que l’identificateur au lieu d’utiliser l’adresse MAC source car, dans certains cas, une machine virtuelle peut être configurée avec plusieurs adresses MAC sur un port de commutateur.  
  
### <a name="dynamic"></a>dynamique
  
[!INCLUDE [dynamic-shortdesc-include](../../includes/dynamic-shortdesc-include.md)]
  
Les charges sortantes dans ce mode sont dynamiquement réparties selon le concept de flowlets. Tout comme la voix humaine a les divisions naturelles situées aux terminaisons de mots et phrases, les flux TCP (flux de communication TCP) ont également sauts naturelles. La partie d’un flux TCP entre deux sauts de ce type est appelée un flowlet.  
  
Lorsque l’algorithme de mode dynamique détecte qu’une limite de flowlet s’est produite - par exemple quand un saut de longueur suffisante s’est produite dans le flux TCP - l’algorithme rééquilibre automatiquement le flux à un autre membre de l’équipe si nécessaire.  Dans certains cas l’algorithme peut également régulièrement rééquilibrer les flux qui ne contiennent pas de n’importe quel flowlets. Pour cette raison, l’affinité entre les membres d’équipe et les flux TCP peut changer à tout moment comme l’algorithme d’équilibrage dynamique fonctionne pour équilibrer la charge de travail des membres de l’équipe.  
  
Si l’équipe est configurée avec indépendant du commutateur ou un des modes dépendants du commutateur, il est recommandé d’utiliser le mode de distribution dynamique pour de meilleures performances.  
  
Il existe une exception à cette règle lorsque l’association de cartes réseau a seulement deux membres de l’équipe, est configurée en mode indépendant du commutateur et a le mode actif/passif activé, avec une carte réseau active et l’autre est configuré pour la mise en veille. Avec cette configuration de l’association de cartes réseau, distribution de hachage d’adresse légèrement plus performante que dynamique distribution.  


## <a name="standby-adapter-setting"></a>Paramètre de carte de secours  
Les options de carte de mise en veille sont **None (toutes les cartes actif)** ou votre sélection d’une carte réseau spécifique dans l’association de cartes réseau qui agit comme une carte de mise en veille. Lorsque vous configurez une carte réseau comme une carte de mise en veille, tous les autres membres de l’équipe non sélectionnés sont actifs, et aucun trafic réseau est envoyé vers ou traité par l’adaptateur jusqu'à ce qu’une carte réseau Active échoue. Après l’échec d’une carte réseau Active, la carte réseau secondaire devient active et traite le trafic réseau. En cas de tous les membres de l’équipe sont restaurés au service, le membre de l’équipe de secours retourne à l’état de secours.  

Si vous avez une équipe de deux cartes d’interface réseau et que vous choisissez de configurer une carte réseau comme une carte de mise en veille, vous perdez la bande passante des avantages d’agrégation qui existent avec deux cartes réseau active.  Vous n’avez pas besoin de désigner une carte de mise en veille pour atteindre une tolérance de panne ; une tolérance de panne est toujours présente chaque fois qu’il existe au moins deux cartes réseau dans une association de cartes réseau.
 
  
## <a name="primary-team-interface-property"></a>Propriété d’interface équipe principale  
Pour accéder à la boîte de dialogue principale Interface d’équipe, vous devez cliquer sur le lien qui est mis en surbrillance dans l’illustration ci-dessous.  
  
![Propriété d’Interface équipe principale](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_10_primaryteaminterface.jpg)  
  
Après avoir cliqué sur le lien en surbrillance, ce qui suit **nouvelle équipe Interface** boîte de dialogue s’ouvre.  
  
![Boîte de dialogue Nouvelle interface d'équipe](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_newteaminterface.jpg)  
  
Si vous utilisez des réseaux locaux virtuels, vous pouvez utiliser cette boîte de dialogue pour spécifier un numéro de réseau local virtuel.  
  
Si vous utilisez des réseaux locaux virtuels, vous pouvez spécifier un nom de tNIC pour l’association de cartes réseau.  
  


---