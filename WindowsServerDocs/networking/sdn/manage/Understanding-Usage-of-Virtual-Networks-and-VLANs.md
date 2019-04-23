---
title: Comprendre l’utilisation des réseaux virtuels et réseaux locaux virtuels
description: Dans cette rubrique, vous en savoir plus sur les réseaux virtuels de virtualisation de réseau Hyper-V et la façon dont ils diffèrent des réseaux locaux virtuels (VLAN). Avec la virtualisation de réseau Hyper-V, vous créez des réseaux virtuels de superposition, également appelés réseaux virtuels.
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 84ac2458-3fcf-4c4f-acfe-6105443dd83f
ms.author: pashort
author: shortpatti
ms.date: 08/26/2018
ms.openlocfilehash: d126e97a91e4c61ecff00cc2b5a527618b2d4d0f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875530"
---
# <a name="understand-the-usage-of-virtual-networks-and-vlans"></a>Comprendre l’utilisation des réseaux virtuels et réseaux locaux virtuels

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Dans cette rubrique, vous en savoir plus sur les réseaux virtuels de virtualisation de réseau Hyper-V et la façon dont ils diffèrent des réseaux locaux virtuels (VLAN). Avec la virtualisation de réseau Hyper-V, vous créez des réseaux virtuels de superposition, également appelés réseaux virtuels.



  
Mise en réseau SDN (Software Defined) dans Windows Server 2016 est basée sur la programmation de stratégie pour les réseaux virtuels de superposition au sein d’un commutateur virtuel Hyper-V. Vous pouvez créer des réseaux virtuels de superposition, également appelés réseaux virtuels, avec la virtualisation de réseau Hyper-V. 
  
Lorsque vous déployez la virtualisation de réseau Hyper-V, les réseaux de superposition sont créés en encapsulant la trame Ethernet de couche 2 de d’origine client de la machine virtuelle avec une superposition - ou tunnel - en-tête (par exemple, VXLAN ou NVGRE), IP de couche 3 et Ethernet de couche 2 réseau d’en-têtes à partir de la sous-couche (ou physiques). Les réseaux virtuels de superposition sont identifiés par un 24 bits virtuels réseau identificateur (VNI) pour garantir l’isolation du trafic client et pour autoriser les adresses IP qui se chevauchent. Le VNI est composé d’un sous-réseau virtuel ID (VSID), l’ID du commutateur logique et ID de tunnel.  
  
En outre, chaque client est attribué à un domaine de routage (semblable à un routage virtuel et le transfert - VRF) afin que plusieurs préfixes de sous-réseau virtuel (chacun représenté par un VNI) peuvent être acheminées directement entre eux. Inter-clients (ou entre domaines de routage) routage n’est pas pris en charge sans passer par une passerelle.   
  
Le réseau physique sur lequel le trafic encapsulé de chaque locataire est acheminée est représenté par un réseau logique appelé le réseau logique de fournisseur. Ce réseau logique de fournisseur se compose d’un ou plusieurs sous-réseaux, chacun représenté par un préfixe IP et, éventuellement, un VLAN 802. 1 q balise.  
  
Vous pouvez créer des réseaux logiques supplémentaires et sous-réseaux pour des raisons d’infrastructure transporter le trafic de gestion, le trafic de stockage en direct, le trafic de migration, etc.  
  
Microsoft SDN ne prend pas en charge l’isolation des réseaux de clients à l’aide de réseaux locaux virtuels. Isolation des locataires s’effectue uniquement à l’aide de la superposition de virtualisation de réseau Hyper-V de réseaux virtuels et l’encapsulation. 


