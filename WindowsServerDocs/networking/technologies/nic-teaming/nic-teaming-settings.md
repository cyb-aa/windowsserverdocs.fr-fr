---
title: Paramètres d’association de cartes réseau
description: Dans cette rubrique, nous vous offrons une vue d’ensemble des propriétés de l’équipe de cartes réseau, telles que les modes d’association et d’équilibrage de charge. Nous vous fournissons également des détails sur le paramètre de l’adaptateur de secours et la propriété de l’interface d’équipe principale. Si vous disposez d’au moins deux cartes réseau dans une association de cartes réseau, vous n’avez pas besoin de désigner une carte de secours pour la tolérance de panne.
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a4caaa86-5799-4580-8775-03ee213784a3
ms.author: lizross
author: eross-msft
ms.date: 09/13/2018
ms.openlocfilehash: fc9ab0fa3d1da0e7af8a7a1d7a8706ba6d128648
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316488"
---
# <a name="nic-teaming-settings"></a>Paramètres d’association de cartes réseau
Dans cette rubrique, nous vous offrons une vue d’ensemble des propriétés de l’équipe de cartes réseau, telles que les modes d’association et d’équilibrage de charge. Nous vous fournissons également des détails sur le paramètre de l’adaptateur de secours et la propriété de l’interface d’équipe principale. Si vous disposez d’au moins deux cartes réseau dans une association de cartes réseau, vous n’avez pas besoin de désigner une carte de secours pour la tolérance de panne.


  
![Propriétés de l’Association de cartes réseau](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_06_properties.jpg)  

## <a name="teaming-modes"></a>Modes d’association 
Les options pour le mode d’association sont **indépendantes du commutateur** et dépendent du **commutateur**. Le mode dépendant du commutateur comprend l' **Association statique** et le **protocole LACP (Link Aggregation Control Protocol)** . 

>[!TIP]
>Pour optimiser les performances de l’Association de cartes réseau, nous vous recommandons d’utiliser un mode d’équilibrage de charge de distribution dynamique.  
  
### <a name="switch-independent"></a>Indépendant du commutateur
  
[!INCLUDE [switch-independent-shortdesc-include](../../includes/switch-independent-shortdesc-include.md)] 
  
Lorsque vous utilisez le mode indépendant du commutateur avec la distribution dynamique, la charge du trafic réseau est distribuée en fonction du hachage d’adresse des ports TCP modifié par l’algorithme d’équilibrage de charge dynamique. L’algorithme d’équilibrage de charge dynamique redistribue les flux pour optimiser l’utilisation de la bande passante des membres de l’équipe, de sorte que les transmissions de flux individuelles peuvent passer d’un membre de l’équipe actif à un autre. L’algorithme prend en compte la petite possibilité que la redistribution du trafic puisse entraîner la livraison des paquets dans le désordre. il faut donc prendre les mesures nécessaires pour réduire ce risque.  
  
### <a name="switch-dependent"></a>Dépend du commutateur  

[!INCLUDE [switch-dependent-shortdesc-include](../../includes/switch-dependent-shortdesc-include.md)]  
  
> [!IMPORTANT]  
> L’Association dépendante des commutateurs requiert que tous les membres de l’équipe soient connectés au même commutateur physique ou à un commutateur à plusieurs châssis qui partage un ID de commutateur entre plusieurs châssis.


- **Association statique.** L’Association statique vous oblige à configurer manuellement le commutateur et l’hôte pour identifier les liens qui forment l’équipe. Étant donné qu’il s’agit d’une solution configurée de manière statique, il n’existe aucun protocole supplémentaire pour aider le commutateur et l’hôte à identifier les câbles mal branchés ou d’autres erreurs susceptibles de provoquer l’échec de l’équipe. Ce mode est généralement pris en charge par les commutateurs préconisés pour les serveurs.

- **Protocole LACP (Link Aggregation Control Protocol).** Contrairement à l’Association statique, le mode d’association LACP identifie de manière dynamique les liens qui sont connectés entre l’hôte et le commutateur. Cette connexion dynamique permet la création automatique d’une équipe et, en théorie, mais rarement en pratique, le développement et la réduction d’une équipe simplement par la transmission ou la réception de paquets LACP à partir de l’entité homologue. Tous les commutateurs de classe de serveur prennent en charge le protocole LACP, et tous requièrent que l’opérateur de réseau active à la fois le protocole LACP sur le port commuté. Quand vous configurez le mode d’association du protocole LACP, l’Association de cartes réseau fonctionne toujours en mode actif du protocole LACP avec une brève minuterie.  Aucune option n’est actuellement disponible pour modifier le minuteur ou modifier le mode LACP.


Lorsque vous utilisez des modes dépendant du commutateur avec la distribution dynamique, la charge du trafic réseau est distribuée en fonction du hachage d’adresse TransportPorts modifié par l’algorithme d’équilibrage de charge dynamique.  L’algorithme d’équilibrage de charge dynamique redistribue les flux pour optimiser l’utilisation de la bande passante des membres de l’équipe. Les transmissions de fluide individuelles peuvent passer d’un membre de l’équipe actif à un autre dans le cadre de la distribution dynamique. L’algorithme prend en compte la petite possibilité que la redistribution du trafic puisse entraîner la livraison des paquets dans le désordre. il faut donc prendre les mesures nécessaires pour réduire ce risque.  
  
Comme avec toutes les configurations dépendantes du commutateur, le commutateur détermine comment distribuer le trafic entrant entre les membres de l’équipe.  Le commutateur est censé effectuer un travail raisonnable de distribution du trafic entre les membres de l’équipe, mais il dispose d’une indépendance totale pour déterminer son fonctionnement.  


## <a name="load-balancing-modes"></a>Modes d’équilibrage de charge  
Les options pour le mode de distribution d’équilibrage de charge sont **hachage d’adresse**, **port Hyper-V**et **dynamique**.  
  
### <a name="address-hash"></a>Hachage d’adresse
  
[!INCLUDE [address-hash-shortdesc-include](../../includes/address-hash-shortdesc-include.md)]
  
Utilisez Windows PowerShell pour spécifier des valeurs pour les composants de fonction de hachage suivants.  
  
-   Ports TCP source et de destination, adresses IP source et de destination. Il s’agit de la valeur par défaut lorsque vous sélectionnez le mode d’équilibrage de charge **adresse** .  
  
-   Adresses IP source et de destination uniquement.  
  
-   Adresses MAC source et de destination uniquement.  
  
Le hachage de ports TCP crée la distribution la plus granulaire des flux de trafic, ce qui permet de déplacer des flux plus petits qui peuvent être déplacés de manière indépendante entre les membres de l’Association de cartes réseau. Toutefois, vous ne pouvez pas utiliser le hachage de ports TCP pour le trafic qui n’est pas basé sur TCP ou UDP, ou lorsque les ports TCP et UDP sont masqués dans la pile, par exemple avec le trafic protégé par IPsec. Dans ce cas, le hachage utilise automatiquement le hachage d’adresse IP ou, si le trafic n’est pas un trafic IP, le hachage d’adresse MAC est utilisé.  
  
### <a name="hyper-v-port"></a>Port Hyper-V
  
[!INCLUDE [hyper-v-port-shortdesc-include](../../includes/hyper-v-port-shortdesc-include.md)]  
  
Étant donné que le commutateur adjacent voit toujours une adresse MAC particulière sur un port, le commutateur répartit la charge d’entrée (le trafic du commutateur vers l’ordinateur hôte) sur plusieurs liens en fonction de l’adresse MAC de destination (MAC MAC). Cela s’avère particulièrement utile lorsque des files d’attente d’ordinateurs virtuels (VMQ) sont utilisées, car une file d’attente peut être placée sur la carte réseau spécifique où le trafic est supposé arriver.  
  
Toutefois, si l’hôte n’a que quelques machines virtuelles, ce mode peut ne pas être suffisamment granulaire pour obtenir une distribution bien équilibrée. Ce mode limite également toujours une seule machine virtuelle (c’est-à-dire le trafic à partir d’un port commuté unique) à la bande passante disponible sur une seule interface. L’Association de cartes réseau utilise le port de commutateur virtuel Hyper-V comme identificateur au lieu d’utiliser l’adresse MAC source car, dans certains cas, une machine virtuelle peut être configurée avec plusieurs adresses MAC sur un port commuté.  
  
### <a name="dynamic"></a>Dynamic
  
[!INCLUDE [dynamic-shortdesc-include](../../includes/dynamic-shortdesc-include.md)]
  
Les charges sortantes dans ce mode sont équilibrées dynamiquement en fonction du concept de flowlets. Tout comme la parole humaine a des ruptures naturelles à la fin de mots et de phrases, les flux TCP (flux de communication TCP) ont également des interruptions naturellement en cours. La partie d’un workflow TCP entre deux arrêts de ce type est appelée flowlet.  
  
Lorsque l’algorithme en mode dynamique détecte qu’une limite flowlet a été rencontrée, par exemple lorsqu’une interruption de la longueur suffisante s’est produite dans le processus TCP, l’algorithme rééquilibre automatiquement le workflow avec un autre membre de l’équipe, le cas échéant.  Dans certains cas, l’algorithme peut également rééquilibrer périodiquement les flux qui ne contiennent pas de flowlets. Pour cette raison, l’affinité entre le canal TCP et le membre de l’équipe peut changer à tout moment, à mesure que l’algorithme d’équilibrage dynamique fonctionne pour équilibrer la charge de travail des membres de l’équipe.  
  
Que l’équipe soit configurée avec le commutateur indépendant ou l’un des modes dépendants du commutateur, il est recommandé d’utiliser le mode de distribution dynamique pour des performances optimales.  
  
Il existe une exception à cette règle lorsque l’Association de cartes réseau a seulement deux membres de l’équipe, est configurée en mode indépendant des commutateurs et qu’elle est activée/en mode veille, avec une carte réseau active et l’autre configurée pour la mise en veille. Avec cette configuration d’association de cartes réseau, la distribution par hachage des adresses offre des performances légèrement supérieures à celles de la distribution dynamique.  


## <a name="standby-adapter-setting"></a>Paramètre de l’adaptateur de secours  
Les options de l’adaptateur de secours sont **None (toutes les cartes actives)** ou votre sélection d’une carte réseau spécifique dans l’Association de cartes réseau qui agit comme un adaptateur de secours. Quand vous configurez une carte réseau en tant que carte de secours, tous les autres membres de l’équipe non sélectionnés sont actifs, et aucun trafic réseau n’est envoyé ou traité par l’adaptateur jusqu’à ce qu’une carte réseau active échoue. Après la défaillance d’une carte réseau active, la carte réseau de secours devient active et traite le trafic réseau. Lorsque tous les membres de l’équipe sont restaurés au service, le membre de l’équipe de secours revient à l’état de veille.  

Si vous avez une association de deux cartes réseau et que vous choisissez de configurer une carte réseau comme adaptateur de secours, vous perdez les avantages de l’agrégation de bande passante qui existent avec deux cartes réseau actives.  Vous n’avez pas besoin de désigner un adaptateur de secours pour obtenir une tolérance de panne. la tolérance de panne est toujours présente chaque fois qu’il y a au moins deux cartes réseau dans une association de cartes réseau.
 
  
## <a name="primary-team-interface-property"></a>Propriété de l’interface d’équipe principale  
Pour accéder à la boîte de dialogue interface principale de l’équipe, vous devez cliquer sur le lien mis en surbrillance dans l’illustration ci-dessous.  
  
![Propriété de l’interface d’équipe principale](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_10_primaryteaminterface.jpg)  
  
Une fois que vous avez cliqué sur le lien mis en surbrillance, la boîte **de dialogue nouvelle interface d’équipe** suivante s’ouvre.  
  
![Boîte de dialogue Nouvelle interface d'équipe](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_newteaminterface.jpg)  
  
Si vous utilisez des réseaux locaux virtuels, vous pouvez utiliser cette boîte de dialogue pour spécifier un numéro de réseau local virtuel.  
  
Que vous utilisiez ou non des réseaux locaux virtuels, vous pouvez spécifier un nom de tNIC pour l’Association de cartes réseau.  
  


---