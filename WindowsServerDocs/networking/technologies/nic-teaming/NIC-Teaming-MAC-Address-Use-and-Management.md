---
title: Association de gestion et utilisation des adresses MAC
description: Cette rubrique fournit des informations sur comment utilise l’association de que l’accès au média (adresse MAC control) dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 26d105e0-afc3-44b5-bb5e-0c884a4c5d62
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6ab624665c964b100e6b1ed633120299b55e37df
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="nic-teaming-mac-address-use-and-management"></a>Association de gestion et utilisation des adresses MAC

>S’applique à: Windows Server2016

Lorsque vous configurez une association avec le mode indépendant du commutateur et hachage d’adresse ou distribution dynamique de la charge, les composants de l’équipe que l’accès au média (adresse MAC contrôlent) du membre principal association de cartes réseau sur le trafic sortant. Membre de l’équipe de cartes réseau principal est une carte réseau sélectionnée par le système d’exploitation à partir de l’ensemble initial de membres de l’équipe.  
  
Le membre principal est le premier membre de l’équipe pour effectuer une liaison à l’équipe après sa création ou après le redémarrage de l’ordinateur hôte. Dans la mesure où le membre principal peut changer de manière non déterministe à chaque démarrage, cartes réseau activer/désactiver une action ou autres activités de reconfiguration, le membre principal peuvent modifier et l’adresse MAC de l’équipe peut varier.  
  
Dans la plupart des cas, cela ne causer des problèmes, mais il existe quelques cas où les problèmes pouvant survenir.  
  
Si le membre principal est supprimé de l’équipe et placé dans opération il peut être un conflit d’adresses MAC. Pour résoudre ce conflit, désactiver, puis activez l’interface d’équipe. Le processus de désactivation, puis d’activer l’interface d’équipe provoque l’interface sélectionner une nouvelle adresse MAC à partir des membres de l’équipe restants, éliminant ainsi le conflit d’adresse MAC.  
  
Vous pouvez définir l’adresse MAC de l’association à une adresse MAC spécifique en la définissant dans l’interface d’équipe principale, tout comme vous pouvez effectuer lors de la configuration de l’adresse MAC de toute carte physique.  
  
## <a name="mac-address-use-on-transmitted-packets"></a>Adresse MAC utiliser sur les paquets transmis  
Lorsque vous configurez une association dans le mode indépendant du commutateur et hachage d’adresse ou distribution dynamique de la charge, les paquets à partir d’une source unique (par exemple, un seul ordinateur virtuel) est répartie simultanément plusieurs membres de l’équipe. Pour empêcher le confondre avec les commutateurs et empêcher MAC représentant les alarmes, l’adresse MAC source est remplacée par une autre adresse MAC sur les trames transmises sur les membres de l’équipe autre que le membre principal. Pour cette raison, chaque membre de l’équipe utilise une autre adresse MAC, et les conflits d’adresses MAC ne peuvent pas tant que la défaillance se produit.  
  
Si une défaillance est détectée sur la carte réseau principale, le logiciel d’association de cartes réseau démarre à l’aide de l’adresse MAC de l’équipe principal sur le membre de l’équipe qui est choisi comme le membre de l’équipe principal temporaire (c'est-à-dire celui qui s’affichent maintenant au commutateur en tant que membre de l’équipe principal).  Cette modification s’applique uniquement au trafic qui devait être envoyés sur le membre de l’équipe principal avec l’adresse MAC de l’équipe principal en tant que son adresse MAC source. Le reste du trafic continue d’être envoyés avec l’adresse MAC il aurait utilisé avant la défaillance du source.  
  
Voici les listes qui décrivent le comportement de remplacement des adresses MAC d’association de cartes réseau, selon la configuration de l’équipe:  
  
1.  **En mode indépendant du commutateur avec la distribution de hachage d’adresse**  
  
    -   Tous les paquets ARP et NS sont envoyés sur le membre principal  
  
    -   Tout le trafic envoyé sur les cartes réseau autre que le membre principal sont envoyées avec l’adresse MAC source modifiée pour correspondre à la carte réseau sur lequel elles sont envoyées  
  
    -   Tout le trafic envoyé sur le membre principal est envoyé avec la source d’origine adresse MAC (qui peut être la source de l’équipe adresse MAC)  
  
2.  **En mode indépendant du commutateur avec la distribution de Port Hyper-V**  
  
    -   Chaque port vmSwitch est activés avec affinité pour un membre de l’équipe  
  
    -   Chaque paquet est envoyé sur le membre de l’équipe à laquelle le port est apparenté  
  
    -   Aucun remplacement MAC source n’est effectuée.  
  
3.  **En mode indépendant du commutateur avec une distribution dynamique**  
  
    -   Chaque port vmSwitch est activés avec affinité pour un membre de l’équipe  
  
    -   Tous les paquets ARP/NS sont envoyés sur le membre de l’équipe à laquelle le port est apparenté  
  
    -   Paquets envoyés sur le membre de l’équipe qui est membre de l’équipe associés n’ont aucune source de remplacement d’adresse MAC terminé  
  
    -   Paquets envoyés sur un membre de l’équipe autre que membre de l’équipe associés auront remplacement d’adresse MAC source terminé  
  
4.  **En mode dépendants du commutateur (toutes les distributions)**  
  
    -   Aucun remplacement d’adresse MAC source n’est effectuée.  
  
## <a name="see-also"></a>Voir aussi  
[Créer une nouvelle équipe de cartes réseau sur un ordinateur hôte ou un ordinateur virtuel](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md)  
[Association de cartes réseau](NIC-Teaming.md)  
  


