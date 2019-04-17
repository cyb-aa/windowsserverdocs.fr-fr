---
title: Configurer la qualité de Service (QoS) pour une carte réseau de machine virtuelle locataire
description: Cette rubrique fait partie du guide logiciel défini de mise en réseau sur la façon de gérer les charges de travail clientes et des réseaux virtuels dans Windows Server2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6d783ff6-7dd5-496c-9ed9-5c36612c6859
ms.author: pashort
author: shortpatti
ms.openlocfilehash: cde4e21beaec58a98a5d5fbe5c4631e2f113dbf7
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-quality-of-service-qos-for-a-tenant-vm-network-adapter"></a>Configurer la qualité de Service (QoS) pour une carte réseau de machine virtuelle locataire

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Lorsque vous configurez la qualité de service pour une carte réseau de machine virtuelle locataire, vous avez le choix entre Data Center Bridging \ (DCB\) ou \(SDN\) logiciel défini de mise en réseau QoS.

1.  **DCB**. Vous pouvez configurer DCB en utilisant les applets de commande WindowsPowerShellNetQoS. Pour obtenir un exemple, consultez la section «Activer Data Center Bridging» dans la rubrique [accès à la mémoire Direct à distance (RDMA) et le commutateur SET (Embedded Teaming)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).

2.  **Qualité de service SDN**. Vous pouvez activer la qualité de service SDN à l’aide du contrôleur de réseau, qui peut être définie pour limiter la bande passante sur une interface virtuelle pour empêcher un ordinateur virtuel à trafic élevé de bloquer des autres utilisateurs.  Vous pouvez également configurer la QoS SDN pour réserver une certaine quantité de bande passante pour un ordinateur virtuel pour vous assurer que la machine virtuelle est accessible, quelle que soit la quantité de trafic réseau.  

Tous les paramètres de qualité de service SDN sont appliquées via les paramètres de Port des propriétés d’Interface réseau selon le tableau suivant.

|Nom de l’élément|Description|
|------------|-----------| 
|macSpoofing|Spécifie si les ordinateurs virtuels peuvent modifier l’adresse source media access control \(MAC\) dans les paquets sortants pour une adresse MAC n’est pas attribuée à l’ordinateur virtuel. Autorisé de valeurs sont «activés» \ (ce qui permet d’utiliser un autre address\ MAC de l’ordinateur virtuel) et «désactivé» \ (qui autorise l’ordinateur virtuel à utiliser uniquement l’adresse MAC affectée à it\).|
|arpGuard|Spécifie si guard ARP est activé.  Guard ARP permet uniquement les adresses qui sont spécifiés dans ArpFilter à passer par le port.  Les valeurs autorisées sont «activés» ou «disabled».
|dhcpGuard|Spécifie s’il faut supprimer les messages DHCP à partir d’un ordinateur virtuel qui prétend être un serveur DHCP. Les valeurs autorisées sont «activés», qui ignore les messages DHCP car le serveur DHCP virtualisé est considéré comme non approuvé, ou «disabled», ce qui permet la réception car le serveur DHCP virtualisé est considéré comme étant digne de confiance du message.
|stormLimit|Spécifie le nombre de paquets de diffusion, multidiffusion et de monodiffusion inconnue par seconde un ordinateur virtuel est autorisé à envoyer par le biais de la carte réseau virtuelle spécifiée. Les paquets de diffusion, multidiffusion et de monodiffusion inconnue au-delà de la limite pendant cet intervalle d’une seconde sont ignorés. Une valeur de zéro \(0\) signifie qu’il n’existe aucune limite.
|portFlowLimit|Spécifie le nombre maximal de flux qui peuvent être exécutées sur le port.  Une valeur vide ou zéro \(0\) signifie qu’il n’existe aucune limite.
|vmqWeight|Spécifie si la file d’attente de machine virtuelle (VMQ) est activée sur la carte réseau virtuelle. La pondération relative décrit l’affinité de la carte réseau virtuelle à utiliser VMQ. La plage de la valeur est 0 et 100. Spécifiez 0 pour désactiver la VMQ sur la carte réseau virtuelle.
|iovWeight|Spécifie si virtualisation d’e/s de racine unique \(SR-IOV\) est activé sur cette carte réseau virtuelle. La pondération relative définit l’affinité de la carte réseau virtuelle à la fonction virtuelle SR-IOV est attribuée. La plage de la valeur est 0 et 100. Spécifiez 0 pour désactiver la SR-IOV sur la carte réseau virtuelle. 
|iovInterruptModeration|Spécifie la valeur de modération de l’interruption d’une fonction \(SR-IOV\) virtuelle racine unique e/s virtualization attribuée à une carte réseau virtuelle. Les valeurs autorisées sont «default», «ADAPTATIF», «off», «faible», «moyenne» et «élevé».   Si vous avez choisi par défaut, la valeur est déterminée par le paramètre du fournisseur de carte réseau physique.  Si adaptative est choisie, la vitesse de modération de l’interruption est basée sur le modèle de trafic d’exécution. 
|iovQueuePairsRequested|Spécifie le nombre de paires de file d’attente matérielles à allouer à une fonction virtuelle SR-IOV. Si \(RSS\) mise à l’échelle côté réception est requise, et si la carte réseau physique qui est liée au commutateur virtuel prend en charge RSS sur SR-IOV virtuel fonctions, plusieurs paire d’une file d’attente est nécessaire. Valeurs autorisées vont de 1 à 4294967295. 
|QosSettings|Vous pouvez configurer les paramètres suivants de la qualité de service, qui sont facultatives:  <br/><br />**outboundReservedValue:**<br/>Si outboundReservedMode est «absolue» la valeur indique la bande passante, en Mbits/s, garanti que le port virtuel pour la transmission (sortie). Si outboundReservedMode est «poids» la valeur indique la partie de la bande passante garantie pondérée. <br/><br />**outboundMaximumMbps:**  <br/>Indique le nombre maximal autorisé la bande passante côté envoi, en Mbits/s, pour le port virtuel (sortie). <br/><br/>**InboundMaximumMbps:**  <br/>Indique le nombre maximal autorisé côté réception de bande passante pour le port virtuel (entrée) en Mbits/s. |