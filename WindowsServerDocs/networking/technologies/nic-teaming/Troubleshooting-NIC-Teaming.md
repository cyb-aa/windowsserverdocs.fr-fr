---
title: Résolution des problèmes d’association de cartes réseau
description: Cette rubrique fournit des informations sur la résolution des problèmes d’association de cartes réseau dans Windows Server 2016.
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fdee02ec-3a7e-473e-9784-2889dc1b6dbb
ms.author: lizross
author: eross-msft
ms.date: 09/13/2018
ms.openlocfilehash: 75b6ae2f2c7d6b4ab28aaedcc7309ccba3dcbd02
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316372"
---
# <a name="troubleshooting-nic-teaming"></a>Résolution des problèmes d’association de cartes réseau

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Dans cette rubrique, nous expliquons comment résoudre les problèmes liés à l’Association de cartes réseau, telles que le matériel et les valeurs de commutateur physiques.  Lorsque les implémentations matérielles des protocoles standard ne sont pas conformes aux spécifications, les performances de l’Association de cartes réseau peuvent être affectées. En outre, selon la configuration, l’Association de cartes réseau peut envoyer des paquets à partir de la même adresse IP avec plusieurs adresses MAC qui activent les fonctionnalités de sécurité sur le commutateur physique.

  
## <a name="hardware-that-doesnt-conform-to-specification"></a>Matériel non conforme à la spécification  
  
En mode de fonctionnement normal, l’Association de cartes réseau peut envoyer des paquets à partir de la même adresse IP, mais avec plusieurs adresses MAC. Selon les normes de protocole, les destinataires de ces paquets doivent résoudre l’adresse IP de l’hôte ou de la machine virtuelle sur une adresse MAC spécifique plutôt que de répondre à l’adresse MAC à partir du paquet de réception.  Les clients qui implémentent correctement les protocoles de résolution d’adresses (ARP et NDP) envoient des paquets avec les adresses MAC de destination appropriées, c’est-à-dire l’adresse MAC de la machine virtuelle ou de l’hôte propriétaire de cette adresse IP. 
  
Certains matériels incorporés n’implémentent pas correctement les protocoles de résolution d’adresses et ne peuvent pas non plus explicitement résoudre une adresse IP en adresse MAC à l’aide de ARP ou de NDP.  Par exemple, un contrôleur de réseau de zone de stockage (SAN) peut fonctionner de cette manière. Les appareils non conformes copient l’adresse MAC source à partir d’un paquet reçu, puis l’utilisent comme adresse MAC de destination dans les paquets sortants correspondants, ce qui entraîne l’envoi de paquets à une adresse MAC de destination incorrecte. Pour cette raison, les paquets sont supprimés par le commutateur virtuel Hyper-V, car ils ne correspondent à aucune destination connue.  
  
Si vous rencontrez des problèmes pour vous connecter à des contrôleurs SAN ou à un autre matériel intégré, vous devez prendre des captures de paquets pour déterminer si votre matériel implémente correctement ARP ou NDP, et contacter le fournisseur de votre matériel pour obtenir de l’aide.  

  
## <a name="physical-switch-security-features"></a>Fonctionnalités de sécurité du commutateur physique  
Selon la configuration, l’Association de cartes réseau peut envoyer des paquets à partir de la même adresse IP avec plusieurs adresses MAC source, en accumulant les fonctionnalités de sécurité sur le commutateur physique. Par exemple, l’inspection ARP dynamique ou la protection source IP, en particulier si le commutateur physique ne sait pas que les ports font partie d’une équipe, ce qui se produit lorsque vous configurez l’Association de cartes réseau en mode indépendant du commutateur. Examinez les journaux du commutateur pour déterminer si les fonctionnalités de sécurité du commutateur entraînent des problèmes de connectivité. 
  
## <a name="disabling-and-enabling-network-adapters-by-using-windows-powershell"></a>Désactivation et activation des cartes réseau à l’aide de Windows PowerShell  

L’échec d’une association de cartes réseau est souvent dû au fait que l’interface de l’équipe est désactivée, et dans de nombreux cas, par accident lors de l’exécution d’une séquence de commandes.  Cette séquence de commandes particulière n’active pas tous les adaptateurs réseau désactivés, car la désactivation de tous les membres physiques sous-jacents des cartes réseau supprime l’interface de l’équipe de cartes réseau. 

Dans ce cas, l’interface de l’équipe de cartes réseau ne s’affiche plus dans la boîte de passe et, pour cette raison, **Enable-NetAdapter \\** * n’active pas l’Association de cartes réseau. Toutefois, la commande **Enable-NetAdapter \\** * active les cartes réseau membres, qui recréent alors l’interface d’équipe (après un bref moment). L’interface d’équipe reste dans l’état désactivé jusqu’à ce que la réactivation soit possible, ce qui permet au trafic réseau de commencer à circuler. 

La séquence de commandes Windows PowerShell suivante peut désactiver l’interface d’équipe par accident :  
  
```PowerShell 
Disable-NetAdapter *  
Enable-NetAdapter *  
```  
  

  
## <a name="related-topics"></a>Rubriques connexes  
- [Association de cartes](NIC-Teaming.md)réseau : dans cette rubrique, nous vous proposons une vue d’ensemble de l’Association de cartes d’interface réseau (NIC) dans Windows Server 2016. L’Association de cartes réseau vous permet de grouper entre une et 32 cartes réseau Ethernet physiques dans une ou plusieurs cartes réseau virtuelles basées sur le logiciel. Ces cartes réseau virtuelles fournissent des performances élevées et une tolérance de panne importante en cas de défaillance de la carte réseau.   

- [Utilisation et gestion de l’Association de cartes réseau avec l’adresse Mac](NIC-Teaming-MAC-Address-Use-and-Management.md): quand vous configurez une association de cartes réseau avec le mode indépendant du commutateur et l’adresse de hachage d’adresse ou de charge dynamique, l’équipe utilise l’adresse Mac (Media Access Control) du membre de l’équipe de cartes réseau principales sur le trafic sortant. Le membre de l’Association de cartes réseau principale est une carte réseau sélectionnée par le système d’exploitation à partir de l’ensemble initial des membres de l’équipe.

- [Paramètres d’association de cartes réseau](nic-teaming-settings.md): dans cette rubrique, nous vous offrons une vue d’ensemble des propriétés de l’équipe de cartes réseau, telles que les modes d’association et d’équilibrage de charge. Nous vous fournissons également des détails sur le paramètre de l’adaptateur de secours et la propriété de l’interface d’équipe principale. Si vous disposez d’au moins deux cartes réseau dans une association de cartes réseau, vous n’avez pas besoin de désigner une carte de secours pour la tolérance de panne.
  


