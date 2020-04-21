---
title: Configurer la qualité de service (QoS) pour une carte réseau de machine virtuelle cliente
description: Quand vous configurez la qualité de service pour une carte réseau de machine virtuelle cliente, vous avez le choix entre le pont de centre de données \(DCB\)ou le réseau à définition logicielle \(SDN\) QoS.
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 6d783ff6-7dd5-496c-9ed9-5c36612c6859
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/23/2018
ms.openlocfilehash: 76a37bde8ca89fe6808a12aff51185d179f5d523
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854552"
---
# <a name="configure-quality-of-service-qos-for-a-tenant-vm-network-adapter"></a>Configurer la qualité de service (QoS) pour une carte réseau de machine virtuelle cliente

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Quand vous configurez la qualité de service pour une carte réseau de machine virtuelle cliente, vous avez le choix entre le pont de centre de données \(DCB\)ou le réseau à définition logicielle \(SDN\) QoS.

1.    **DCB**. Vous pouvez configurer DCB à l’aide des applets de commande Windows PowerShell NetQoS. Pour obtenir un exemple, consultez la section « Activer le pontage du centre de données » dans la rubrique [accès direct à la mémoire à distance (RDMA) et Switch Embedded Teaming (Set)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).

2.    **QoS SDN**. Vous pouvez activer SDN QoS à l’aide du contrôleur de réseau, qui peut être configuré pour limiter la bande passante sur une interface virtuelle afin d’empêcher une machine virtuelle à trafic élevé de bloquer d’autres utilisateurs.  Vous pouvez également configurer la qualité de service SDN pour réserver une quantité spécifique de bande passante pour une machine virtuelle afin de vous assurer que la machine virtuelle est accessible quelle que soit la quantité de trafic réseau.  

Appliquez tous les paramètres de QoS SDN via les paramètres de port des propriétés de l’interface réseau. Pour plus d’informations, reportez-vous au tableau ci-dessous.

|Nom d’élément|Description|
|------------|-----------| 
|macSpoofing| Permet aux machines virtuelles de modifier le contrôle d’accès au média source \(adresse MAC\) des paquets sortants à une adresse MAC non affectée à la machine virtuelle.<p>Valeurs autorisées :<ul><li>Activé : utilisez une autre adresse MAC.</li><li>Désactivé : Utilisez uniquement l’adresse MAC qui lui est assignée.</li></ul>|
|arpGuard| Permet à ARP Guard uniquement les adresses spécifiées dans ArpFilter de traverser le port.<p>Valeurs autorisées :<ul><li>Activé-autorisé</li><li>Désactivé : non autorisé</li></ul>|
|dhcpGuard| Autorise ou supprime tous les messages DHCP d’une machine virtuelle qui prétend être un serveur DHCP. <p>Valeurs autorisées :<ul><li>Activé : supprime les messages DHCP, car le serveur DHCP virtualisé est considéré comme non fiable.</li><li>Désactivé : autorise la réception du message, car le serveur DHCP virtualisé est considéré comme digne de confiance.</li></ul>|
|stormLimit| Nombre de paquets (diffusion, multidiffusion et monodiffusion inconnue) par seconde qu’une machine virtuelle est autorisée à envoyer via la carte réseau virtuelle. Les paquets au-delà de la limite au cours de cet intervalle d’une seconde sont supprimés. La valeur zéro \(0\) signifie qu’il n’y a aucune limite..|
|portFlowLimit| Nombre maximal de flux autorisés à s’exécuter pour le port. Une valeur vide ou zéro \(0\) signifie qu’il n’y a aucune limite. |
|vmqWeight| Le poids relatif décrit l’affinité de la carte réseau virtuelle pour utiliser la file d’attente d’ordinateurs virtuels. La plage de valeurs est comprise entre 0 et 100.<p>Valeurs autorisées :<ul><li>0 – désactive l’ordinateurs virtuels sur la carte réseau virtuelle.</li><li>1-100 – active l’ordinateurs virtuels sur la carte réseau virtuelle.</li></ul>|
|iovWeight| Le poids relatif définit l’affinité de la carte réseau virtuelle sur la virtualisation d’e/s d’une racine unique assignée \(fonction virtuelle SR-IOV\). <p>Valeurs autorisées :<ul><li>0 – désactive SR-IOV sur la carte réseau virtuelle.</li><li>1-100 – active SR-IOV sur la carte réseau virtuelle.</li></ul>|
|iovInterruptModeration|<p>Valeurs autorisées :<ul><li>par défaut : le paramètre du fournisseur de la carte réseau physique détermine la valeur.</li><li>adaptatif </li><li>le </li><li>entrée</li><li>moyenne</li><li>rapide</li></ul><p>Si vous choisissez l’option **par défaut**, le paramètre du fournisseur de la carte réseau physique détermine la valeur.  Si vous choisissez, **Adaptive**, le modèle de trafic du runtime détermine le taux de modération des interruptions.|
|iovQueuePairsRequested| Nombre de paires de files d’attente matérielles allouées à une fonction virtuelle SR-IOV. Si la mise à l’échelle côté réception \(\) RSS est requise et si la carte réseau physique qui crée une liaison avec le commutateur virtuel prend en charge RSS sur les fonctions virtuelles SR-IOV, plusieurs paires de files d’attente sont requises. <p>Valeurs autorisées : 1 à 4294967295.|
|QosSettings| Configurez les paramètres de QoS suivants, qui sont tous facultatifs : <ul><li>**outboundReservedValue** : si outboundReservedMode est « Absolute », la valeur indique la bande passante, en Mbits/s, garantie au port virtuel pour la transmission (sortie). Si outboundReservedMode a la valeur « Weight », la valeur indique la partie pondérée de la bande passante garantie.</li><li>**outboundMaximumMbps** : indique la bande passante côté envoi autorisée maximale, en Mbits/s, pour le port virtuel (sortie).</li><li>**InboundMaximumMbps** : indique la bande passante maximale autorisée côté réception pour le port virtuel (entrée) en Mbits/s.</li></ul> |

---