---
title: Gestion et utilisation des adresses MAC d'association de cartes réseau
description: Lorsque vous configurez une association de cartes réseau avec le mode indépendant du commutateur et de hachage d’adresse ou distribution de charge dynamique, l’équipe utilise que l’accès au média (adresse MAC control) du membre principal association de cartes réseau sur le trafic sortant. Le membre d’association de cartes réseau principal est une carte réseau sélectionnée par le système d’exploitation à partir de l’ensemble initial de membres de l’équipe.
manager: dougkim
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
ms.openlocfilehash: 074a55d2f0a5423af5892ed73dc8a2b8dbda9d5b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824250"
---
# <a name="nic-teaming-mac-address-use-and-management"></a>Gestion et utilisation des adresses MAC d'association de cartes réseau

>S’applique à : Windows Server 2016

Lorsque vous configurez une association de cartes réseau avec le mode indépendant du commutateur et de hachage d’adresse ou distribution de charge dynamique, l’équipe utilise que l’accès au média (adresse MAC control) du membre principal association de cartes réseau sur le trafic sortant. Le membre d’association de cartes réseau principal est une carte réseau sélectionnée par le système d’exploitation à partir de l’ensemble initial de membres de l’équipe.  Il est le premier membre d’équipe pour la lier à l’équipe après sa création ou après le redémarrage de l’ordinateur hôte. Étant donné que le membre de l’équipe principale peut changer de manière non déterministe à chaque démarrage, carte réseau activer/désactiver action, ou autres activités de reconfiguration, membre de l’équipe principale peuvent modifier et l’adresse MAC de l’équipe peuvent varier.  
  
Dans la plupart des cas, cela n’entraîne des problèmes, mais il existe quelques cas où des problèmes peuvent survenir.  
  
Si le membre de l’équipe principale est supprimé de l’équipe et puis placé dans l’opération peut renfermer un conflit d’adresses MAC. Pour résoudre ce conflit, désactiver, puis activez l’interface de l’équipe. Le processus de désactiver et d’activer l’interface d’équipe provoque l’interface sélectionner une nouvelle adresse MAC à partir des membres de l’équipe restantes, éliminant ainsi le conflit d’adresse MAC.  
  
Vous pouvez définir l’adresse MAC de l’association de cartes réseau à une adresse MAC spécifique en la définissant dans l’interface de l’équipe principale, tout comme vous pouvez effectuer lors de la configuration de l’adresse MAC de toute carte physique.  
  
## <a name="mac-address-use-on-transmitted-packets"></a>Adresse MAC utiliser sur les paquets transmis  
Lorsque vous configurez une association de cartes réseau en mode indépendant du commutateur et hachage d’adresse ou distribution de charge dynamique, les paquets à partir d’une source unique (par exemple, une seule machine virtuelle) est simultanément réparti entre plusieurs membres d’équipe. Pour empêcher le confondre avec les commutateurs et éviter les alarmes de bagottement de MAC, l’adresse MAC source est remplacée par une autre adresse MAC sur les trames transmises sur les membres de l’équipe autre que le membre de l’équipe principale. Pour cette raison, chaque membre de l’équipe utilise une adresse MAC différente et conflits d’adresses MAC ne peuvent pas tant que l’échec se produit.  
  
Lorsqu’une défaillance est détectée sur la carte réseau principale, le logiciel d’association de cartes réseau démarre à l’aide de l’adresse MAC d’équipe principale sur le membre de l’équipe qui est choisi comme membre de l’équipe principal temporaire (par exemple, celle qui s’affiche maintenant au commutateur en tant que l’équipe principale me BRE).  Cette modification s’applique uniquement au trafic qui a été va être envoyée sur le membre de l’équipe principale avec une adresse MAC du membre de l’équipe principale en tant que son adresse MAC source. Tout autre trafic continue à être envoyées avec toute source adresse MAC il aurait utilisé avant la défaillance.  
  
Voici les listes qui décrivent le comportement de remplacement d’adresse MAC de l’association de cartes NIC, selon la configuration de l’équipe :  
  
1.  **En mode indépendant du commutateur avec distribution par hachage d’adresse**  
  
    -   Tous les paquets ARP et NS sont envoyés sur le membre de l’équipe principale  
  
    -   Tout le trafic envoyé sur les cartes réseau autre que le membre de l’équipe principale sont envoyés avec l’adresse MAC source modifié pour correspondre à la carte réseau sur lequel ils sont envoyés  
  
    -   Tout le trafic envoyé sur le membre de l’équipe principale est envoyé avec la source d’origine adresse MAC (qui peut être adresse MAC de la source de l’équipe)  
  
2.  **En mode indépendant du commutateur avec la distribution de Port Hyper-V**  
  
    -   Chaque port vmSwitch est une affinité avec un membre d’équipe  
  
    -   Chaque paquet est envoyé sur le membre d’équipe auquel le port est associé par affinité  
  
    -   Aucune source de remplacement de MAC n’est effectuée.  
  
3.  **En mode indépendant du commutateur avec la distribution dynamique**  
  
    -   Chaque port vmSwitch est une affinité avec un membre d’équipe  
  
    -   Tous les paquets ARP/NS sont envoyés sur le membre de l’équipe à laquelle le port est associé par affinité  
  
    -   Paquets envoyés sur le membre de l’équipe qui est membre de l’équipe avec affinité n’aient aucun remplacement d’adresse MAC fait source  
  
    -   Paquets envoyés sur un membre de l’équipe autre que le membre d’équipe avec affinité aura le remplacement d’adresse MAC source terminé  
  
4.  **En mode dépendants du commutateur (toutes les distributions)**  
  
    -   Aucune source de remplacement d’adresse MAC n’est effectuée.  
  
## <a name="related-topics"></a>Rubriques connexes
- [Association de cartes réseau](NIC-Teaming.md): Dans cette rubrique, nous vous donner une vue d’ensemble de l’association de carte d’Interface réseau (NIC) dans Windows Server 2016. Association de cartes réseau vous permet de regrouper entre 1 et 32 de cartes réseau Ethernet physiques dans un ou plusieurs adaptateurs de réseau virtuel basé sur le logiciel. Ces cartes réseau virtuelles fournissent des performances élevées et une tolérance de panne importante en cas de défaillance de la carte réseau.  

- [Paramètres d’association de cartes réseau](nic-teaming-settings.md): Dans cette rubrique, nous vous donner une vue d’ensemble des propriétés d’association de cartes réseau telles que l’association de cartes et modes d’équilibrage de charge. Nous vous donnons également plus d’informations sur le paramètre de carte de mise en veille et de la propriété d’interface équipe principale. Si vous avez au moins deux cartes réseau dans une association de cartes réseau, il est inutile de désigner une carte de mise en veille pour une tolérance de panne.
  
- [Créer une association de cartes réseau sur un ordinateur hôte ou d’une machine virtuelle](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md): Dans cette rubrique, vous créez une nouvelle association de cartes réseau sur un ordinateur hôte ou sur une machine de virtuelle (VM) Hyper-V exécutant Windows Server 2016.

- [Résolution des problèmes d’association de cartes réseau](Troubleshooting-NIC-Teaming.md): Dans cette rubrique, nous abordons les façons de résoudre les problèmes d’association de cartes réseau, telles que le matériel, les titres de commutateur physique et la désactivation ou l’activation des cartes réseau à l’aide de Windows PowerShell. 
  


