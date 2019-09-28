---
title: Gestion et utilisation des adresses MAC d'association de cartes réseau
description: Quand vous configurez une association de cartes réseau avec le mode indépendant du commutateur et le hachage d’adresse ou la distribution de charge dynamique, l’équipe utilise l’adresse MAC (Media Access Control) du membre de l’équipe de carte réseau principale sur le trafic sortant. Le membre de l’Association de cartes réseau principale est une carte réseau sélectionnée par le système d’exploitation à partir de l’ensemble initial des membres de l’équipe.
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 26d105e0-afc3-44b5-bb5e-0c884a4c5d62
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3ff03dae44600ff79ed22d298ee338c570e61e36
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405483"
---
# <a name="nic-teaming-mac-address-use-and-management"></a>Gestion et utilisation des adresses MAC d'association de cartes réseau

>S’applique à : Windows Server 2016

Quand vous configurez une association de cartes réseau avec le mode indépendant du commutateur et le hachage d’adresse ou la distribution de charge dynamique, l’équipe utilise l’adresse MAC (Media Access Control) du membre de l’équipe de carte réseau principale sur le trafic sortant. Le membre de l’Association de cartes réseau principale est une carte réseau sélectionnée par le système d’exploitation à partir de l’ensemble initial des membres de l’équipe.  Il s’agit du premier membre d’équipe à lier à l’équipe après sa création ou après le redémarrage de l’ordinateur hôte. Étant donné que le membre de l’équipe principale peut changer d’une manière non déterministe à chaque démarrage, action de Désactivation/activation de la carte réseau ou d’autres activités de reconfiguration, le membre de l’équipe principale peut changer et l’adresse MAC de l’équipe peut varier.  
  
Dans la plupart des cas, cela ne provoque pas de problèmes, mais dans certains cas, des problèmes peuvent survenir.  
  
Si le membre de l’équipe principal est supprimé de l’équipe, puis placé dans l’opération, il peut y avoir un conflit d’adresse MAC. Pour résoudre ce conflit, désactivez, puis activez l’interface d’équipe. Le processus de désactivation et d’activation de l’interface d’équipe entraîne la sélection par l’interface d’une nouvelle adresse MAC des autres membres de l’équipe, ce qui élimine le conflit d’adresses MAC.  
  
Vous pouvez définir l’adresse MAC de l’Association de cartes réseau sur une adresse MAC spécifique en la définissant dans l’interface d’équipe principale, comme vous le feriez lors de la configuration de l’adresse MAC d’une carte réseau physique.  
  
## <a name="mac-address-use-on-transmitted-packets"></a>Utilisation des adresses MAC sur les paquets transmis  
Quand vous configurez une association de cartes réseau en mode indépendant du commutateur et de hachage d’adresse ou de charge dynamique, les paquets d’une seule source (par exemple, une machine virtuelle unique) sont répartis simultanément entre plusieurs membres de l’équipe. Pour éviter toute confusion entre les commutateurs et pour empêcher les alarmes à ailes MAC, l’adresse MAC source est remplacée par une autre adresse MAC sur les trames transmises sur les membres de l’équipe autres que le membre de l’équipe principal. Pour cette raison, chaque membre de l’équipe utilise une adresse MAC différente, et les conflits d’adresses MAC sont empêchés à moins qu’une défaillance ne se produise.  
  
Quand une défaillance est détectée sur la carte réseau principale, le logiciel d’association de cartes réseau commence à utiliser l’adresse MAC du membre de l’équipe principale sur le membre de l’équipe qui est choisi comme membre de l’équipe principale temporaire (c’est-à-dire, celui qui apparaît maintenant au commutateur comme l’équipe principale). mbre).  Cette modification s’applique uniquement au trafic qui devait être envoyé sur le membre de l’équipe principal avec l’adresse MAC du membre de l’équipe principale comme adresse MAC source. D’autres trafics continuent d’être envoyés avec l’adresse MAC source qu’il aurait utilisé avant la défaillance.  
  
Vous trouverez ci-dessous des listes qui décrivent le comportement de remplacement des adresses MAC de l’Association de cartes réseau, en fonction de la configuration de l’équipe :  
  
1.  **En mode indépendant du commutateur avec distribution de hachage d’adresse**  
  
    -   Tous les paquets ARP et NS sont envoyés sur le membre de l’équipe principal  
  
    -   Tout le trafic envoyé sur des cartes d’interface réseau autres que le membre de l’équipe principal est envoyé avec l’adresse MAC source modifiée pour correspondre à la carte réseau sur laquelle il est envoyé.  
  
    -   Tout le trafic envoyé sur le membre de l’équipe principal est envoyé avec l’adresse MAC source d’origine (qui peut être l’adresse MAC source de l’équipe)  
  
2.  **En mode indépendant du commutateur avec distribution de port Hyper-V**  
  
    -   Chaque port vmSwitch est affinité à un membre de l’équipe  
  
    -   Chaque paquet est envoyé sur le membre de l’équipe sur lequel le port est affinité  
  
    -   Aucun remplacement de MAC source n’est effectué  
  
3.  **En mode indépendant du commutateur avec distribution dynamique**  
  
    -   Chaque port vmSwitch est affinité à un membre de l’équipe  
  
    -   Tous les paquets ARP/NS sont envoyés sur le membre de l’équipe sur lequel le port est affinité  
  
    -   Les paquets envoyés sur le membre de l’équipe qui est le membre de l’équipe affinité n’ont aucun remplacement d’adresse MAC source effectué  
  
    -   Les paquets envoyés sur un membre de l’équipe autre que le membre de l’équipe affinité comporteront le remplacement de l’adresse MAC source  
  
4.  **En mode dépendant du commutateur (toutes les distributions)**  
  
    -   Aucun remplacement d’adresse MAC source n’est effectué  
  
## <a name="related-topics"></a>Rubriques connexes
- [Association de cartes réseau](NIC-Teaming.md): Dans cette rubrique, nous vous proposons une vue d’ensemble de l’Association de cartes d’interface réseau (NIC) dans Windows Server 2016. L’Association de cartes réseau vous permet de grouper entre une et 32 cartes réseau Ethernet physiques dans une ou plusieurs cartes réseau virtuelles basées sur le logiciel. Ces cartes réseau virtuelles fournissent des performances élevées et une tolérance de panne importante en cas de défaillance de la carte réseau.  

- [Paramètres d’association de cartes réseau](nic-teaming-settings.md): Dans cette rubrique, nous vous offrons une vue d’ensemble des propriétés de l’équipe de cartes réseau, telles que les modes d’association et d’équilibrage de charge. Nous vous fournissons également des détails sur le paramètre de l’adaptateur de secours et la propriété de l’interface d’équipe principale. Si vous disposez d’au moins deux cartes réseau dans une association de cartes réseau, vous n’avez pas besoin de désigner une carte de secours pour la tolérance de panne.
  
- [Créer une nouvelle association de cartes réseau sur un ordinateur hôte ou une machine virtuelle](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md): Dans cette rubrique, vous allez créer une nouvelle association de cartes réseau sur un ordinateur hôte ou dans une machine virtuelle Hyper-V exécutant Windows Server 2016.

- [Résolution des problèmes d’association de cartes réseau](Troubleshooting-NIC-Teaming.md): Dans cette rubrique, nous expliquons comment résoudre les problèmes liés à l’Association de cartes réseau, telles que le matériel, les titres de commutateur physique et la désactivation ou l’activation de cartes réseau à l’aide de Windows PowerShell. 
  


