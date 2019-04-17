---
title: Présentation de l’utilisation des réseaux virtuels et réseaux locaux virtuels
description: Cette rubrique fait partie du guide logiciel défini de mise en réseau sur la façon de gérer les charges de travail clientes et des réseaux virtuels dans Windows Server2016.
manager: brianlic
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
ms.openlocfilehash: fcf84c5c1f0be2fa1c7524592f8d02e4b11d3a2b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="understanding-usage-of-virtual-networks-and-vlans"></a>Présentation de l’utilisation des réseaux virtuels et réseaux locaux virtuels

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour en savoir plus sur les réseaux virtuels de la virtualisation de réseau Hyper-V et comment elles diffèrent des réseaux locaux virtuels (VLAN).  
  
Logiciel défini de mise en réseau (SDN) dans Windows Server2016 est basée sur la programmation de stratégie pour les réseaux virtuels superposition au sein d’un commutateur virtuel Hyper-V. Vous pouvez créer des réseaux virtuels superposition, également nommé réseaux virtuels, avec la virtualisation de réseau Hyper-V.   
  
Lorsque vous déployez la virtualisation de réseau Hyper-V, les réseaux de superposition sont créés en encapsulant trame Ethernet de couche 2 du locataire ordinateur virtuel d’origine avec un en-tête de superposition - ou tunnel - (par exemple, VXLAN ou NVGRE) et le réseau IP de couche 3 Ethernet de couche 2 en-têtes des sous-couche (physique). Les réseaux virtuels superposition sont identifiés par un 24bits virtuel réseau identificateur (VNI) pour gérer l’isolation du trafic client et autoriser les adresses IP qui se chevauchent. La VNI se compose d’un sous-réseau virtuel ID (VSID), l’ID de commutateur logique et ID de tunnel.  
  
En outre, chaque client est attribué à un domaine de routage (similaire au routage virtuel et le transfert - VRF) afin que plusieurs préfixes de sous-réseau virtuel (chacun représenté par un VNI) peuvent être routés directement entre eux. Cross-locataire (ou entre domaines de routage) routage n’est pas prise en charge sans passer par le biais d’une passerelle.   
  
Le réseau physique sur lequel tunnel le trafic encapsulé de chaque client est représenté par un réseau logique appelé le réseau logique du fournisseur. Ce réseau logique fournisseur se compose d’un ou plusieurs sous-réseaux, chacune étant représentée par un préfixe IP et, éventuellement, un VLAN 802. 1 q balise.  
  
Vous pouvez créer des réseaux logiques supplémentaires et de sous-réseaux pour des raisons d’infrastructure gérer le trafic de gestion, le trafic de stockage direct le trafic de migration, etc..  
  
SDN Microsoft ne gère pas l’isolation des réseaux des clients à l’aide de réseaux locaux virtuels. Isolation des locataires s’effectue uniquement à l’aide de la virtualisation de réseau Hyper-V superposition des réseaux virtuels et l’encapsulation. 


