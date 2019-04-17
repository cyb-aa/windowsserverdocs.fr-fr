---
title: Résolution des problèmes d’association de cartes réseau
description: Cette rubrique fournit des informations sur la résolution des problèmes d’association de cartes réseau dans Windows Server2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fdee02ec-3a7e-473e-9784-2889dc1b6dbb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 634ec1a5eee0cd661ca89c2673e5b74abd458cd6
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="troubleshooting-nic-teaming"></a>Résolution des problèmes d’association de cartes réseau

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Cette rubrique fournit des informations sur la résolution des problèmes d’association de cartes réseau et contient les sections suivantes qui décrivent les causes possibles des problèmes avec l’association de cartes réseau.  
  
-   [Matériel qui n’est pas conforme aux spécifications](#bkmk_hardware)  
  
-   [Fonctionnalités de sécurité commutateur physique](#bkmk_switch)  
  
-   [Désactivation et activation avec Windows PowerShell](#bkmk_ps)  
  
## <a name="bkmk_hardware"></a>Matériel qui n’est pas conforme aux spécifications  
Lorsque les implémentations matérielles de protocoles standard ne sont pas conformes à la spécification, les performances d’association de cartes réseau peut être affectée.  
  
En fonctionnement normal, association de cartes réseau peuvent envoyer des paquets à partir de la même adresse IP, mais avec l’accès de support source différent plusieurs adresses control (MAC). Selon les normes de protocole, les destinataires de ces paquets doivent résoudre l’adresse IP de l’ordinateur hôte ou un ordinateur virtuel à une adresse MAC spécifique plutôt que de répondre à l’adresse MAC à partir duquel le paquet a été reçu.  Les clients qui implémentent correctement les protocoles de résolution adresse protocole ARP du IPv4 (Address Resolution) ou le protocole de découverte de voisin de IPv6 (NDP), envoie les paquets avec l’adresse MAC (l’adresse MAC de l’ordinateur virtuel ou d’un hôte qui possède cette adresse IP) cible correcte.  
  
Certains incorporé matériel n’implémentent pas correctement les protocoles de résolution d’adresses et également ne peut-être pas explicitement résoudre une adresse IP à une adresse MAC à l’aide de ARP ou NDP.  Un contrôleur de réseau SAN de zone de stockage est un exemple d’un périphérique qui peut effectuer de cette manière. Périphériques non conformes copier la source des adresses MAC qui sont contenue dans un paquet reçu et l’utiliser en tant que l’adresse MAC dans les paquets sortants correspondants de destination.  
  
Ainsi, les paquets envoyés à l’adresse MAC de destination incorrecte. Pour cette raison, les paquets sont perdus par le commutateur virtuel Hyper-V, car ils ne correspondent pas toutes les destinations connues.  
  
Si vous ne rencontrez des problèmes pour se connecter à des contrôleurs de SAN ou d’autres incorporé matériel, vous devez prendre des captures de paquets et déterminer si votre matériel est correctement mise en œuvre ARP ou NDP et contactez votre fournisseur de matériel pour la prise en charge.  
  
## <a name="bkmk_switch"></a>Fonctionnalités de sécurité commutateur physique  
Selon la configuration, l’association de cartes réseau peuvent envoyer des paquets à partir de la même adresse IP avec plusieurs adresses MAC source différente.  Cela peut déclencher la sécurité protéger de fonctionnalités sur le commutateur physique comme dynamique inspection ARP ou IP source, en particulier si le commutateur physique n’est pas en charge que les ports font partie d’une équipe. Cela peut se produire si vous configurez l’association de cartes réseau en mode indépendant du commutateur.  Vous devez examiner les journaux de commutateur pour déterminer si les fonctionnalités de sécurité de commutateur sont à l’origine des problèmes de connectivité avec l’association de cartes réseau.  
  
## <a name="bkmk_ps"></a>Désactivation et activation de cartes réseau à l’aide de Windows PowerShell  
Une raison courante d’une association échec est que l’interface d’équipe est désactivée. Dans de nombreux cas, l’interface est désactivée par inadvertance lors de l’exécution de la séquence de commandes Windows PowerShell suivante:  
  
```  
Disable-NetAdapter *  
Enable-NetAdapter *  
```  
  
Cette séquence de commandes n’active pas toutes les NetAdapters il désactivé.  
  
Il s’agit, car la désactivation de toutes les cartes réseau de membres physique sous-jacent entraîne l’interface équipe de cartes réseau à supprimer et n’apparaissent plus dans Get-NetAdapter. Pour cette raison, les **Enable-NetAdapter \ *** commande n’active pas l’équipe de cartes réseau, car cette carte est supprimée.  
  
Le **Enable-NetAdapter \ *** commande est le cas, toutefois, activer le membre, les cartes réseau qui entraîne l’interface d’équipe à être recréés puis (après un bref moment). Dans ce cas, l’interface de l’équipe est toujours dans un état «disabled», car il n’a pas été réactivé. L’activation de l’interface d’équipe une fois qu’il est recréé autorise le trafic réseau commencer à transmettre à nouveau.  
  
## <a name="see-also"></a>Voir aussi  
[Association de cartes réseau](NIC-Teaming.md)  
  


