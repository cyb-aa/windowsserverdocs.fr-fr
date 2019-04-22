---
title: Fonctionnalités et technologies logicielles uniquement (SO)
description: 'Ces fonctionnalités sont implémentées en tant que partie du système d’exploitation et sont indépendantes de la carte sous-jacente. Parfois, ces fonctionnalités nécessitent quelques réglages de la carte réseau pour un fonctionnement optimal. Par exemple : fonctionnalités de Hyper-v telles que la Machine virtuelle qualité de Service (vmQoS), de listes de contrôle d’accès (ACL) et de fonctionnalités Hyper-V, comme l’association de cartes réseau.'
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/20/2018
ms.openlocfilehash: 504bc92971e778b468812dc4064fa6f0afff87ad
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823780"
---
# <a name="software-only-so-features-and-technologies"></a>Fonctionnalités et technologies logicielles uniquement (SO)
Fonctionnalités du logiciel uniquement sont implémentées en tant que partie du système d’exploitation et sont indépendantes des cartes réseau sous-jacent. Parfois, ces fonctionnalités nécessitent quelques réglages de la carte réseau pour un fonctionnement optimal. Par exemple : fonctionnalités de Hyper-v telles que la Machine virtuelle qualité de Service (vmQoS), de listes de contrôle d’accès (ACL) et de fonctionnalités Hyper-V, comme l’association de cartes réseau.

## <a name="access-control-lists-acls"></a>Listes de contrôle d’accès (ACL)

Une fonctionnalité Hyper-V et SDNv1 pour la gestion de la sécurité pour une machine virtuelle. Cette fonctionnalité s’applique à la pile de Hyper-V non virtualisé et la pile HVNv1. Vous pouvez gérer Hyper-V basculer ACL via [Add-VMNetworkAdapterAcl](https://docs.microsoft.com/powershell/module/hyper-v/add-vmnetworkadapteracl?view=win10-ps) et [Remove-VMNetworkAdapterAcl](https://docs.microsoft.com/powershell/module/hyper-v/remove-vmnetworkadapteracl?view=win10-ps) applets de commande PowerShell.

## <a name="extended-acls"></a>ACL étendues

Étendues des ACL du commutateur virtuel Hyper-V permettent de configurer l’Hyper-V Virtual Switch étendue ACL de Port pour fournir une protection de pare-feu et d’appliquer des stratégies de sécurité pour les machines virtuelles clientes dans les centres de données. Étant donné que l’ACL de port sont configurés sur le commutateur virtuel Hyper-V plutôt que dans les machines virtuelles, l’administrateur peut gérer les stratégies de sécurité pour tous les clients dans un environnement partagé.

Vous pouvez gérer le commutateur Hyper-V étendu ACL via le [Add-VMNetworkAdapterExtendedAcl](https://docs.microsoft.com/powershell/module/hyper-v/add-vmnetworkadapterextendedacl?view=win10-ps) et [Remove-VMNetworkAdapterExtendedAcl](https://docs.microsoft.com/powershell/module/hyper-v/remove-vmnetworkadapteracl?view=win10-ps) applets de commande PowerShell.

>[!TIP] 
>Cette fonctionnalité s’applique à la pile HNVv1. Pour les ACL dans la pile SDN, reportez-vous à SDN Software Defined Networking) ACL ci-dessous.

Pour plus d’informations sur l’étendue Port Access Control Lists dans cette bibliothèque, consultez [créer des stratégies de sécurité avec étendue Port Access Control Lists](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/Create-Security-Policies-with-Extended-Port-Access-Control-Lists).

## <a name="nic-teaming"></a>Association de cartes réseau

Association de cartes réseau, également appelée liaison, de la carte réseau est l’agrégation de plusieurs ports de carte réseau dans une entité de que l’hôte perçoit comme un seul port de carte réseau. Association de cartes réseau protège contre la défaillance d’un seul port de carte réseau (ou le câble connecté à celui-ci). Il regroupe également le trafic réseau pour un débit plus rapide. Pour plus d’informations, consultez [association de cartes réseau](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming).

Avec Windows Server 2016, vous avez deux façons de réaliser l’association de cartes :

1.  Solution d’association Windows Server 2012

2.  Windows Server 2016 Switch Embedded association (SET)


## <a name="rsc-in-the-vswitch"></a>RSC dans le commutateur virtuel

Réception Segment fusion (RSC) dans le commutateur virtuel est une fonctionnalité qui prend les paquets qui font partie du même flux et arrivent entre les interruptions réseau et les regroupe dans un paquet unique avant de les remettre au système d’exploitation. Le commutateur virtuel dans Windows Server 2019 possède cette fonctionnalité. Pour plus d’informations sur cette fonctionnalité, consultez [réception Segment Coalescing dans le commutateur virtuel](https://docs.microsoft.com/windows-server/networking/technologies/hpn/rsc-in-the-vswitch).

## <a name="software-defined-networking-sdn-acls"></a>Software Defined ACL de mise en réseau (SDN)

L’extension de SDN dans Windows Server 2016 améliorée des façons pour prendre en charge les ACL. Dans la pile v2 SDN de Windows Server 2016, SDN ACL sont utilisés au lieu des ACL et les ACL étendues. Vous pouvez utiliser le contrôleur de réseau pour gérer les ACL de SDN. 

## <a name="sdn-quality-of-service-qos"></a>SDN la qualité de Service (QoS)

L’extension SDN dans Windows Server 2016 amélioré des moyens de fournir un contrôle de bande passante (réservations de sortie, les limites de sortie et les limites entrée) sur une base de 5-tuple. En règle générale, ces stratégies sont appliquées au niveau de la carte réseau virtuelle ou réseau d’ordinateur virtuel, mais vous pouvez les rendre plus spécifiques. Dans la pile v2 SDN de Windows Server 2016, SDN QoS est utilisée au lieu de vmQoS. Vous pouvez utiliser le contrôleur de réseau pour gérer QoS des SDN.

## <a name="switch-embedded-teaming-set"></a>Commutateur incorporé association (SET)

JEU est une autre solution association de cartes réseau que vous pouvez utiliser dans les environnements qui incluent Hyper-V et la pile de mise en réseau SDN (Software Defined) dans Windows Server 2016. JEU intègre des fonctionnalités d’association de cartes réseau dans le commutateur virtuel Hyper-V. Pour plus d’informations sur le Switch Embedded Teaming dans cette bibliothèque, consultez [accès de mémoire Direct à distance (RDMA) et l’association de cartes SET (Switch Embedded)](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming).

## <a name="virtual-receive-side-scaling-vrss"></a>Mise à l'échelle côté réception virtuelle (vRSS, Virtual Receive Side Scaling)

Logiciel vRSS est utilisé pour répartir le trafic entrant destiné à une machine virtuelle entre plusieurs processeurs logiques (LPs) de la machine virtuelle. Logiciel vRSS donne la machine virtuelle la possibilité de gérer davantage de trafic réseau à un seul LP serait en mesure de gérer. Pour plus d’informations, consultez [virtuel l’échelle côté réception (vRSS)](https://docs.microsoft.com/windows-server/networking/technologies/vrss/vrss-top).

## <a name="virtual-machine-quality-of-service-vmqos"></a>Qualité de Service d’ordinateurs virtuels (vmQoS)

Machine virtuelle qualité de Service est une fonctionnalité de Hyper-V qui permet le commutateur définir des limites sur le trafic généré par chaque machine virtuelle. Il permet également d’une machine virtuelle réserver une quantité de bande passante sur la connexion réseau externe afin qu’une seule machine virtuelle ne peut pas retarde l’exécution d’une autre machine virtuelle pour la bande passante. Dans la pile v2 SDN de Windows Server 2016, SDN QoS remplace vmQoS.

vmQoS peut définir des limites de sortie et les réservations de sortie. Vous devez déterminer le mode de réservation de sortie (poids relatif ou absolu de bande passante) avant de créer le commutateur Hyper-V.

-  Déterminer le mode de réservation de sortie avec le paramètre – MinimumBandwidthMode de l’applet de commande New-VMSwitch PowerShell.

-  Définissez la valeur de la limite de sortie avec le paramètre – MaximumBandwidth sur l’applet de commande Set-VMNetworkAdapter PowerShell.

-  Définissez la valeur de la réservation de sortie avec un des paramètres suivants de l’applet de commande PowerShell de VMNetworkAdapter définir :

   -  Si le paramètre – MinimumBandwidthMode sur l’applet de commande New-VMSwitch est absolue, définissez le paramètre – MinimumBandwidthAbsolute sur l’applet de commande VMNetworkAdapter définie.

   -  Si le paramètre – MinimumBandwidthMode sur l’applet de commande New-VMSwitch est poids, définissez le paramètre – MinimumBandwidthWeight sur l’applet de commande VMNetworkAdapter définie.

En raison des limitations dans l’algorithme utilisé pour cette fonctionnalité, nous recommandons que le poids le plus élevé ou la bande passante absolue ne pas plus de 20 fois le poids le plus bas ou bande passante absolue. Si davantage de contrôle est nécessaire, envisagez d’utiliser la pile SDN et la fonctionnalité QoS des SDN.


---