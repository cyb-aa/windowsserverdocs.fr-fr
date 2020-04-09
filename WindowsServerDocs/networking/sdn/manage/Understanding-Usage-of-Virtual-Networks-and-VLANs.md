---
title: Comprendre l’utilisation des réseaux virtuels et des réseaux locaux virtuels
description: Dans cette rubrique, vous allez découvrir les réseaux virtuels de virtualisation de réseau Hyper-V et leurs différences par rapport aux réseaux locaux virtuels (VLAN). Avec la virtualisation de réseau Hyper-V, vous créez des réseaux virtuels de superposition, également appelés réseaux virtuels.
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 84ac2458-3fcf-4c4f-acfe-6105443dd83f
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/26/2018
ms.openlocfilehash: 56e90966d38b8a138dd8781863a4eaad1db639fb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854482"
---
# <a name="understand-the-usage-of-virtual-networks-and-vlans"></a>Comprendre l’utilisation des réseaux virtuels et des réseaux locaux virtuels

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Dans cette rubrique, vous allez découvrir les réseaux virtuels de virtualisation de réseau Hyper-V et leurs différences par rapport aux réseaux locaux virtuels (VLAN). Avec la virtualisation de réseau Hyper-V, vous créez des réseaux virtuels de superposition, également appelés réseaux virtuels.



  
La mise en réseau SDN (Software Defined Networking) dans Windows Server 2016 est basée sur la stratégie de programmation pour les réseaux virtuels de superposition au sein d’un commutateur virtuel Hyper-V. Vous pouvez créer des réseaux virtuels de superposition, également appelés réseaux virtuels, avec la virtualisation de réseau Hyper-V. 
  
Lorsque vous déployez la virtualisation de réseau Hyper-V, les réseaux de superposition sont créés en encapsulant la trame Ethernet de couche 2 de la machine virtuelle du locataire d’origine avec un en-tête de superposition ou de tunnel (par exemple, VXLAN ou NVGRE) et une adresse IP de couche 3 et une Ethernet de couche 2. en-têtes du réseau Underlay (ou physique). Les réseaux virtuels de superposition sont identifiés par un identificateur de réseau virtuel (VNI) de 24 bits pour assurer l’isolation du trafic du client et autoriser les adresses IP qui se chevauchent. VNI se compose d’un ID de sous-réseau virtuel, d’un ID de commutateur logique et d’un ID de tunnel.  
  
En outre, chaque locataire se voit attribuer un domaine de routage (semblable à un routage et un transfert virtuel-VRF) de manière à ce que plusieurs préfixes de sous-réseau virtuel (chacun représenté par un VNI) puissent être acheminés directement entre eux. Le routage entre locataires (ou domaine de routage croisé) n’est pas pris en charge sans passer par une passerelle.   
  
Le réseau physique sur lequel le trafic encapsulé de chaque locataire est tunnelé est représenté par un réseau logique appelé réseau logique du fournisseur. Ce réseau logique de fournisseur est constitué d’un ou de plusieurs sous-réseaux, chacun représenté par un préfixe IP et, éventuellement, d’une étiquette 802.1 q VLAN.  
  
Vous pouvez créer des réseaux logiques et des sous-réseaux logiques à des fins d’infrastructure pour transporter le trafic de gestion, le trafic de stockage, le trafic de migration dynamique, etc.  
  
Microsoft SDN ne prend pas en charge l’isolation des réseaux locataires à l’aide de réseaux locaux virtuels. L’isolation des locataires s’effectue uniquement à l’aide de la virtualisation de réseau Hyper-V superposition des réseaux virtuels et de l’encapsulation. 


