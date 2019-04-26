---
title: Configurer la qualité de Service (QoS) pour une carte réseau de machine virtuelle locataire
description: Lorsque vous configurez la qualité de service pour une carte réseau de machine virtuelle locataire, vous avez le choix entre Data Center Bridging \(DCB\)ou Sdn \(SDN\) QoS.
manager: dougkim
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
ms.date: 08/23/2018
ms.openlocfilehash: 0b9ce318c3d249b23d7560e0b6bb90a83e60d64d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880600"
---
# <a name="configure-quality-of-service-qos-for-a-tenant-vm-network-adapter"></a>Configurer la qualité de Service (QoS) pour une carte réseau de machine virtuelle locataire

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Lorsque vous configurez la qualité de service pour une carte réseau de machine virtuelle locataire, vous avez le choix entre Data Center Bridging \(DCB\)ou Sdn \(SDN\) QoS.

1.  **DCB**. Vous pouvez configurer DCB en utilisant les applets de commande Windows PowerShell NetQoS. Pour obtenir un exemple, consultez la section « Activer Data Center Bridging » dans la rubrique [accès de mémoire Direct à distance (RDMA) et l’association de cartes SET (Switch Embedded)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).

2.  **QoS des SDN**. Vous pouvez activer QoS des SDN à l’aide du contrôleur de réseau, ce qui peut être définie pour limiter la bande passante sur une interface virtuelle pour empêcher une machine virtuelle à trafic élevé de bloquer d’autres utilisateurs.  Vous pouvez également configurer la QoS des SDN pour réserver une certaine quantité de bande passante pour une machine virtuelle pour vous assurer que la machine virtuelle est accessible indépendamment de la quantité de trafic réseau.  

Appliquer tous les paramètres QoS des SDN via les paramètres de Port des propriétés d’Interface réseau. Reportez-vous au tableau ci-dessous pour plus d’informations.

|Nom d’élément|Description|
|------------|-----------| 
|macSpoofing| Permet aux machines virtuelles modifier le contrôle d’accès source media \(MAC\) adresse dans les paquets sortants pour une adresse MAC ne pas affectée à la machine virtuelle.<p>Valeurs autorisées :<ul><li>Activé – Utilisez une adresse MAC différente.</li><li>Désactivé : utiliser uniquement l’adresse MAC qui lui est assignée.</li></ul>|
|arpGuard| Permet de guard ARP seules les adresses spécifiés dans ArpFilter à passer par le port.<p>Valeurs autorisées :<ul><li>Activé : autorisé</li><li>Désactivé : ne pas autorisé</li></ul>|
|dhcpGuard| Autorise ou supprime tous les messages DHCP à partir d’une machine virtuelle qui prétend être un serveur DHCP. <p>Valeurs autorisées :<ul><li>Activé – supprime les messages DHCP étant donné que le serveur DHCP virtualisé est considéré comme non fiables.</li><li>Désactivé : permet au message d’être reçu parce que le serveur DHCP virtualisé est considéré comme digne de confiance.</li></ul>|
|stormLimit| Le nombre de paquets (monodiffusion de diffusion, de multidiffusion et inconnue) par seconde, une machine virtuelle est autorisé à envoyer via la carte réseau virtuelle. Paquets au-delà de la limite pendant cet intervalle d’une seconde être perdues. Une valeur égale à zéro \(0\) signifie qu’il n’existe aucune limite...|
|portFlowLimit| Le nombre maximal de flux autorisés à exécuter pour le port. Une valeur vide ou zéro \(0\) signifie qu’il n’existe aucune limite. |
|vmqWeight| La pondération relative décrit l’affinité de la carte réseau virtuelle à utiliser virtual machine queue (VMQ). La plage de la valeur est 0 et 100.<p>Valeurs autorisées :<ul><li>0 – désactive la VMQ sur la carte réseau virtuelle.</li><li>1-100 – Active le VMQ sur la carte réseau virtuelle.</li></ul>|
|iovWeight| La pondération relative définit l’affinité de la carte réseau virtuelle pour la virtualisation d’e/s de racine unique attribuée \(SR-IOV\) fonction virtuelle. <p>Valeurs autorisées :<ul><li>0 – désactive SR-IOV sur la carte réseau virtuelle.</li><li>1-100 – permet de SR-IOV sur la carte réseau virtuelle.</li></ul>|
|iovInterruptModeration|<p>Valeurs autorisées :<ul><li>par défaut – paramètre du fournisseur de carte réseau physique détermine la valeur.</li><li>Adaptive- </li><li>le </li><li>Faible</li><li>moyenne</li><li>Haute</li></ul><p>Si vous choisissez **par défaut**, paramètre du fournisseur de carte réseau physique détermine la valeur.  Si vous choisissez, **adaptive**, le trafic de l’exécution de modèle détermine la vitesse de modération d’interruption.|
|iovQueuePairsRequested| Le nombre de paires de file d’attente de matériel allouée à une fonction virtuelle SR-IOV. Si le côté réception mise à l’échelle \(RSS\) est requis et si la carte réseau physique qui se lie au commutateur virtuel prend en charge RSS sur les fonctions virtuelles SR-IOV, plusieurs paires de file d’attente est requise. <p>Valeurs autorisées : 1 et 4294967295.|
|QosSettings| Configurez les paramètres de qualité de service suivants, qui sont tous facultatifs : <ul><li>**outboundReservedValue** : si outboundReservedMode est « absolue » la valeur indique la bande passante, en Mbits/s, garantie au port virtuel pour la transmission (en sortie). Si outboundReservedMode est « poids » la valeur indique la quantité pondérée de la bande passante garantie.</li><li>**outboundMaximumMbps** -indique la valeur maximale autorisée de la bande passante du côté envoi, en Mbits/s, pour le port virtuel (en sortie).</li><li>**InboundMaximumMbps** -indique la valeur maximale autorisée pour le port virtuel (entrée) en Mbits/s de bande passante côté réception.</li></ul> |

---