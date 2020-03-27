---
title: Fonctionnalités et technologies logicielles uniquement (SO)
description: Ces fonctionnalités sont implémentées dans le cadre du système d’exploitation et sont indépendantes de la ou des cartes réseau sous-jacentes. Parfois, ces fonctionnalités nécessitent un paramétrage de la carte réseau pour un fonctionnement optimal. Les fonctionnalités Hyper-v telles que la qualité de service (vmQoS) de l’ordinateur virtuel, les listes de Access Control (ACL) et les fonctionnalités non-Hyper-V, telles que l’Association de cartes réseau, sont des exemples.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/20/2018
ms.openlocfilehash: a83a36ce7a47f0ebde35bf93bdca20796dd37a28
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316880"
---
# <a name="software-only-so-features-and-technologies"></a>Fonctionnalités et technologies logicielles uniquement (SO)
Les fonctionnalités logicielles uniquement sont implémentées dans le cadre du système d’exploitation et sont indépendantes de la ou des cartes réseau sous-jacentes. Parfois, ces fonctionnalités nécessitent un paramétrage de la carte réseau pour un fonctionnement optimal. Les fonctionnalités Hyper-v telles que la qualité de service (vmQoS) de l’ordinateur virtuel, les listes de Access Control (ACL) et les fonctionnalités non-Hyper-V, telles que l’Association de cartes réseau, sont des exemples.

## <a name="access-control-lists-acls"></a>Listes de Access Control (ACL)

Fonctionnalité Hyper-V et SDNv1 pour la gestion de la sécurité d’une machine virtuelle. Cette fonctionnalité s’applique à la pile Hyper-V non virtualisée et à la pile HVNv1. Vous pouvez gérer les ACL du commutateur Hyper-V via les applets de commande PowerShell [Add-VMNetworkAdapterAcl](https://docs.microsoft.com/powershell/module/hyper-v/add-vmnetworkadapteracl?view=win10-ps) et [Remove-VMNetworkAdapterAcl](https://docs.microsoft.com/powershell/module/hyper-v/remove-vmnetworkadapteracl?view=win10-ps) .

## <a name="extended-acls"></a>ACL étendues

Les listes de contrôle d’accès étendues du commutateur virtuel Hyper-V vous permettent de configurer les listes de contrôle d’accès étendues du commutateur virtuel Hyper-V pour fournir une protection par pare-feu et appliquer des stratégies de sécurité pour les machines virtuelles du locataire dans les centres Étant donné que les listes de contrôle d’accès de port sont configurées sur le commutateur virtuel Hyper-V plutôt que sur les machines virtuelles, l’administrateur peut gérer les stratégies de sécurité pour tous les locataires dans un environnement multi-locataire.

Vous pouvez gérer les listes de contrôle d’accès étendues du commutateur Hyper-V via les applets de commande PowerShell [Add-VMNetworkAdapterExtendedAcl](https://docs.microsoft.com/powershell/module/hyper-v/add-vmnetworkadapterextendedacl?view=win10-ps) et [Remove-VMNetworkAdapterExtendedAcl](https://docs.microsoft.com/powershell/module/hyper-v/remove-vmnetworkadapteracl?view=win10-ps) .

>[!TIP] 
>Cette fonctionnalité s’applique à la pile HNVv1. Pour les listes de contrôle d’accès dans la pile SDN, reportez-vous aux ACL Software Defined Networking SDN) ci-dessous.

Pour plus d’informations sur les listes de Access Control de ports étendus dans cette bibliothèque, consultez [créer des stratégies de sécurité avec des listes de Access Control de ports étendus](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/Create-Security-Policies-with-Extended-Port-Access-Control-Lists).

## <a name="nic-teaming"></a>Association de cartes réseau

L’Association de cartes réseau, également appelée liaison de carte réseau, est l’agrégation de plusieurs ports NIC dans une entité que l’hôte perçoit comme un port de carte réseau unique. L’Association de cartes réseau protège contre la défaillance d’un seul port de carte réseau (ou le câble qui lui est connecté). Il regroupe également le trafic réseau pour un débit plus rapide. Pour plus d’informations, consultez [Association de cartes réseau](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming).

Avec Windows Server 2016, vous disposez de deux méthodes pour effectuer une association :

1.  Solution d’association Windows Server 2012

2.  Windows Server 2016 Switch Embedded Teaming (SET)


## <a name="rsc-in-the-vswitch"></a>RSC dans le vSwitch

La fusion de segment de réception (RSC) dans le vSwitch est une fonctionnalité qui utilise des paquets qui font partie du même flux et qui arrivent entre les interruptions du réseau, et les fusionne en un seul paquet avant de les transmettre au système d’exploitation. Le commutateur virtuel dans Windows Server 2019 dispose de cette fonctionnalité. Pour plus d’informations sur cette fonctionnalité, consultez [réception de la fusion de segments dans le vswitch](https://docs.microsoft.com/windows-server/networking/technologies/hpn/rsc-in-the-vswitch).

## <a name="software-defined-networking-sdn-acls"></a>Listes de contrôle d’accès réseau à définition logicielle (SDN)

L’extension SDN dans Windows Server 2016 a amélioré les méthodes de prise en charge des listes de contrôle d’accès. Dans la pile Windows Server 2016 SDN v2, les ACL SDN sont utilisées à la place des ACL et des ACL étendus. Vous pouvez utiliser le contrôleur de réseau pour gérer les listes de contrôle d’accès SDN. 

## <a name="sdn-quality-of-service-qos"></a>Qualité de service (QoS) des réseaux à définition logicielle

L’extension SDN dans Windows Server 2016 a amélioré les méthodes de contrôle de la bande passante (réservations de sortie, limites de sortie et limites d’entrée) à 5 tuples. En règle générale, ces stratégies sont appliquées au niveau carte réseau virtuelle ou carte réseau, mais vous pouvez les rendre plus spécifiques. Dans la pile Windows Server 2016 SDN v2, la QoS SDN est utilisée à la place de vmQoS. Vous pouvez utiliser le contrôleur de réseau pour gérer SDN QoS.

## <a name="switch-embedded-teaming-set"></a>Basculer l’Association incorporée (SET)

SET est une autre solution d’association de cartes réseau que vous pouvez utiliser dans les environnements qui incluent Hyper-V et la pile SDN (Software Defined Networking) dans Windows Server 2016. SET intègre certaines fonctionnalités d’association de cartes réseau dans le commutateur virtuel Hyper-V. Pour plus d’informations sur l’Association incorporée de commutateurs dans cette bibliothèque, consultez [accès direct à la mémoire à distance (RDMA) et Switch Embedded Teaming (Set)](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming).

## <a name="virtual-receive-side-scaling-vrss"></a>Mise à l'échelle côté réception virtuelle (vRSS, Virtual Receive Side Scaling)

Le logiciel vRSS est utilisé pour répartir le trafic entrant destiné à une machine virtuelle sur plusieurs processeurs logiques de la machine virtuelle. Le logiciel vRSS donne à la machine virtuelle la possibilité de gérer plus de trafic réseau qu’une seule LP pouvant être gérée. Pour plus d’informations, voir la rubrique relative à [la mise à l’échelle côté réception virtuelle (vRSS)](https://docs.microsoft.com/windows-server/networking/technologies/vrss/vrss-top).

## <a name="virtual-machine-quality-of-service-vmqos"></a>Qualité de service (vmQoS) de la machine virtuelle

La qualité de service de la machine virtuelle est une fonctionnalité Hyper-V qui permet au commutateur de définir des limites sur le trafic généré par chaque machine virtuelle. Il permet également à une machine virtuelle de réserver une quantité de bande passante sur la connexion réseau externe afin qu’une machine virtuelle ne puisse pas priver une autre machine virtuelle pour la bande passante. Dans la pile Windows Server 2016 SDN v2, SDN QoS remplace vmQoS.

vmQoS peut définir des limites de sortie et des réservations de sortie. Vous devez déterminer le mode de réservation de sortie (pondération relative ou bande passante absolue) avant de créer le commutateur Hyper-V.

-  Déterminez le mode de réservation de sortie avec le paramètre – MinimumBandwidthMode de l’applet de commande PowerShell New-VMSwitch.

-  Définissez la valeur de la limite de sortie avec le paramètre – MaximumBandwidth sur l’applet de commande PowerShell Set-VMNetworkAdapter.

-  Définissez la valeur de la réservation de sortie avec l’un des paramètres suivants de l’applet de commande PowerShell Set VMNetworkAdapter :

   -  Si le paramètre – MinimumBandwidthMode sur l’applet de commande New-VMSwitch est absolu, définissez le paramètre – MinimumBandwidthAbsolute sur l’applet de commande Set VMNetworkAdapter.

   -  Si le paramètre – MinimumBandwidthMode sur l’applet de commande New-VMSwitch est Weight, définissez le paramètre – MinimumBandwidthWeight sur l’applet de commande Set VMNetworkAdapter.

En raison des limitations de l’algorithme utilisé pour cette fonctionnalité, nous recommandons que la pondération la plus élevée ou la bande passante absolue ne soit pas supérieure à 20 fois la pondération la plus faible ou la bande passante absolue. Si vous avez besoin de davantage de contrôle, envisagez d’utiliser la pile SDN et la fonctionnalité SDN-QoS.


---