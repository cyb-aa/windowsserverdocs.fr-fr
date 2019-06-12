---
title: Résolution des problèmes d’association de cartes réseau
description: Cette rubrique fournit des informations sur la résolution de l’association de cartes réseau dans Windows Server 2016.
manager: dougkim
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
ms.date: 09/13/2018
ms.openlocfilehash: a6af3cbd038e97d889269b83d72c77c50680e513
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446171"
---
# <a name="troubleshooting-nic-teaming"></a>Résolution des problèmes d’association de cartes réseau

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Dans cette rubrique, nous abordons les façons de résoudre les problèmes d’association de cartes réseau, telles que le matériel et les titres de commutateur physique.  Lorsque les implémentations matérielles de protocoles standard ne sont pas conformes aux spécifications, les performances d’association de cartes réseau peut être affectée. En outre, selon la configuration, association de cartes réseau peuvent envoyer des paquets à partir de la même adresse IP avec plusieurs adresses MAC aller-retour des fonctionnalités de sécurité sur le commutateur physique.

  
## <a name="hardware-that-doesnt-conform-to-specification"></a>Matériel qui ne sont pas conformes à la spécification  
  
En fonctionnement normal, association de cartes réseau peuvent envoyer des paquets à partir de la même adresse IP, encore avec plusieurs adresses MAC. Selon les normes de protocole, les destinataires de ces paquets doivent résoudre l’adresse IP de la machine virtuelle ou un hôte à une adresse MAC spécifique plutôt qu’à répondre à l’adresse MAC à partir de la réception du paquet.  Les clients qui implémentent correctement les protocoles de résolution d’adresse (ARP et NDP) envoient des paquets avec les adresses MAC de destination correcte, autrement dit, l’adresse MAC de la machine virtuelle ou un hôte qui possède cette adresse IP. 
  
Certains matériels embedded n’implémente pas correctement les protocoles de résolution d’adresse et également peut ne pas explicitement résoudre une adresse IP à une adresse MAC à l’aide de ARP ou NDP.  Par exemple, un contrôleur de réseau SAN de zone de stockage peut effectuer de cette manière. Appareils non conformes copier l’adresse MAC source à partir d’un paquet reçu et ensuite l’utiliser comme l’adresse MAC de destination dans les paquets sortants correspondants, ce qui entraîne les paquets envoyés à l’adresse MAC de destination incorrecte. Pour cette raison, les paquets sont ignorés par le commutateur virtuel Hyper-V, car ils ne correspondent pas n’importe quelle destination connue.  
  
Si vous êtes avoir arrive pas à se connecter à des contrôleurs de réseau SAN ou autre incorporé matériel, vous devez prendre des captures de paquets pour déterminer si votre matériel est correctement mise en œuvre ARP ou NDP et contactez votre fournisseur de matériel pour la prise en charge.  

  
## <a name="physical-switch-security-features"></a>Fonctionnalités de sécurité de commutateur physique  
Selon la configuration, association de cartes réseau peuvent envoyer des paquets à partir de la même adresse IP avec plusieurs adresses MAC source retour des fonctionnalités de sécurité sur le commutateur physique. Par exemple, l’inspection dynamique ARP ou la fonctionnalité IP source guard, surtout si le commutateur physique ne prenant en charge que les ports font partie d’une équipe, ce qui se produit lorsque vous configurez association de cartes réseau en mode indépendant du commutateur. Examinez les journaux de commutateur pour déterminer si les fonctionnalités de sécurité de commutateur sont à l’origine des problèmes de connectivité. 
  
## <a name="disabling-and-enabling-network-adapters-by-using-windows-powershell"></a>Désactivation et activation des cartes réseau à l’aide de Windows PowerShell  

Une raison courante pour une association de cartes réseau échec est que l’interface de l’équipe est désactivée et dans de nombreux cas, par inadvertance lors de l’exécution d’une séquence de commandes.  Cette séquence particulière de commandes n’active pas toutes la NetAdapters désactivée, car la désactivation de tous les membres physiques sous-jacent de cartes réseau supprime l’interface d’équipe de carte réseau. 

Dans ce cas, l’interface d’équipe de carte réseau n’affiche plus dans Get-NetAdapter et pour cette raison, **Enable-NetAdapter \\** * n’active pas l’association de cartes réseau. Le **Enable-NetAdapter \\** * commande est le cas, toutefois, activer le membre cartes réseau, qui recrée l’interface d’équipe puis (après un bref instant). L’interface de l’équipe reste dans l’état « désactivé » jusqu'à ce que réactivée, autorisant le trafic réseau commencer à circuler. 

La séquence de commandes Windows PowerShell suivante peut désactiver l’interface de l’équipe par inadvertance :  
  
```PowerShell 
Disable-NetAdapter *  
Enable-NetAdapter *  
```  
  

  
## <a name="related-topics"></a>Rubriques connexes  
- [Association de cartes réseau](NIC-Teaming.md): Dans cette rubrique, nous vous donner une vue d’ensemble de l’association de carte d’Interface réseau (NIC) dans Windows Server 2016. Association de cartes réseau vous permet de regrouper entre 1 et 32 de cartes réseau Ethernet physiques dans un ou plusieurs adaptateurs de réseau virtuel basé sur le logiciel. Ces cartes réseau virtuelles fournissent des performances élevées et une tolérance de panne importante en cas de défaillance de la carte réseau.   

- [Utilisation des adresses MAC de l’association de cartes réseau et la gestion](NIC-Teaming-MAC-Address-Use-and-Management.md): Lorsque vous configurez une association de cartes réseau avec le mode indépendant du commutateur et de hachage d’adresse ou distribution de charge dynamique, l’équipe utilise que l’accès au média (adresse MAC control) du membre principal association de cartes réseau sur le trafic sortant. Le membre d’association de cartes réseau principal est une carte réseau sélectionnée par le système d’exploitation à partir de l’ensemble initial de membres de l’équipe.

- [Paramètres d’association de cartes réseau](nic-teaming-settings.md): Dans cette rubrique, nous vous donner une vue d’ensemble des propriétés d’association de cartes réseau telles que l’association de cartes et modes d’équilibrage de charge. Nous vous donnons également plus d’informations sur le paramètre de carte de mise en veille et de la propriété d’interface équipe principale. Si vous avez au moins deux cartes réseau dans une association de cartes réseau, il est inutile de désigner une carte de mise en veille pour une tolérance de panne.
  


