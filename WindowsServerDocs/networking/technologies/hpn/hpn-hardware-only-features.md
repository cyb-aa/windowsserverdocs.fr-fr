---
title: Mise en réseau hautes performances
description: Cette rubrique fournit une vue d’ensemble des Technologies d’optimisation dans Windows Server 2016 et le déchargement et inclut des liens vers des conseils supplémentaires sur ces technologies.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/12/2018
ms.openlocfilehash: 09e18a41452baa0add8e055ceb22d2f5c2ad886e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815830"
---
## <a name="hardware-only-ho-features-and-technologies"></a>Fonctionnalités et technologies matérielles uniquement (HO)

Ces accélérations de matériel améliorent les performances de mise en réseau conjointement avec le logiciel, mais ne sont pas étroitement partie de tout logiciel. Par exemple : la modération de l’interruption, de contrôle de flux et de déchargement de somme de contrôle côté réception IPv4.

>[!TIP]
>Fonctionnalités SH et HO sont disponibles si la carte réseau installée le prend en charge. Les descriptions de fonctionnalité ci-dessous décrit comment savoir si votre carte réseau prend en charge la fonctionnalité.

### <a name="address-checksum-offload"></a>Déchargement de somme de contrôle d’adresse

Déchargements de somme de contrôle d’adresse sont une fonctionnalité de carte réseau qui permet de décharger le calcul de sommes de contrôle adresse (IP, TCP, UDP) pour le matériel de carte réseau pour les deux envoyer et recevoir.

Sur le chemin d’accès de la réception, le déchargement de la somme de contrôle calcule les sommes de contrôle dans les en-têtes IP, TCP et UDP (le cas échéant) et indique au système d’exploitation si les sommes de contrôle a réussi, a échoué ou désactivée. Si la carte réseau affirme que les sommes de contrôle sont valides, le système d’exploitation accepte le paquet non. Si la carte réseau déclare que les sommes de contrôle sont non valides ou non vérifié, la pile IP/TCP/UDP calcule en interne les sommes de contrôle à nouveau. Si la somme de contrôle calculée échoue, le paquet abandonnée.

Sur le chemin d’accès de l’envoi, le déchargement de la somme de contrôle calcule et insère les sommes de contrôle dans l’en-tête IP, TCP ou UDP comme il convient.

La désactivation des déchargements de somme de contrôle sur le chemin d’accès de l’envoi ne désactive pas le calcul de somme de contrôle et d’insertion pour les paquets envoyés au pilote de miniport à l’aide de la fonctionnalité Envoyer déchargement important (LSO).  Pour désactiver tous les calculs de déchargement de la somme de contrôle, l’utilisateur doit également désactiver LSO.

_**Gérer les déchargements de somme de contrôle d’adresse**_

Dans les propriétés avancées, il existe plusieurs propriétés distinctes :

-   Déchargement de la somme IPv4

-   Déchargement de somme de contrôle TCP (IPv4)

-   Déchargement de somme de contrôle TCP (IPv6)

-   Déchargement de la somme de contrôle UDP (IPv4)

-   Déchargement de somme de contrôle UDP (IPv6)

Par défaut, ceux-ci sont tout toujours activées. Nous vous recommandons de toujours l’activation de toutes ces déchargements.

Les déchargements de somme de contrôle peuvent être gérés à l’aide des applets de commande Enable-NetAdapterChecksumOffload et Disable-NetAdapterChecksumOffload. Par exemple, l’applet de commande suivante active les TCP (IPv4) et les calculs de somme de contrôle UDP (IPv4) :

```PowerShell
Enable-NetAdapterChecksumOffload –Name * -TcpIPv4 -UdpIPv4
```

_**Conseils sur l’utilisation adresse décharge de somme de contrôle**_

Adresse décharge de somme de contrôle doit toujours être activé indépendamment de la charge de travail ou de circonstance. Cela décharger la plupart des base de toutes les technologies toujours améliorent les performances de votre réseau. Déchargement de la somme de contrôle est également requis pour les autres déchargements sans état fonctionne, y compris du trafic entrant (RSS), de segment RSC (receive coalescing) et de déchargement d’envoi important (LSO).

### <a name="interrupt-moderation-im"></a>Modération (IM) de l’interruption

Messagerie instantanée met en mémoire tampon plusieurs paquets reçus avant d’interrompre le système d’exploitation. Lorsqu’une carte réseau reçoit un paquet, il démarre un minuteur. Lorsque la mémoire tampon est plein, ou le minuteur expire, selon la première, la carte réseau interrompt le système d’exploitation. 

Nombre de cartes réseau prise en charge plus de simplement activé/désactivé de modération de l’interruption. La plupart des cartes réseau prennent en charge les concepts d’un taux élevé, moyen et faible pour la messagerie instantanée. Les différents taux représentent les minuteurs courtes ou plus longues et réglages de taille de mémoire tampon appropriée pour réduire la latence (modération de l’interruption de faible) ou de réduire les interruptions (modération d’interruption élevé).

Il existe un équilibre trouver le juste équilibre entre la réduction des interruptions et excessivement retarder la remise de paquets. En règle générale, le traitement des paquets est plus efficace avec modération de l’interruption est activée. Hautes performances ou les applications à faible latence deviez évaluer l’impact de la désactivation ou la réduction de modération de l’interruption.

### <a name="jumbo-frames"></a>Trames Jumbo

Les trames Jumbo est une fonctionnalité de carte réseau et de réseau qui permet à une application à envoyer des trames sont beaucoup plus volumineux que la valeur par défaut de 1 500 octets. En général, la limite sur les trames jumbo est d’environ 9 000 octets, mais peut être plus petite.

Il n’y avait aucune modification à la prise en charge de trames jumbo dans Windows Server 2012 R2.

Dans Windows Server 2016, qu'il existe un nouveau décharger : MTU_for_HNV. Cet nouvelle déchargement fonctionne avec les paramètres de trame étendue pour garantir la segmentation entre l’hôte et le commutateur adjacent ne nécessite pas le trafic encapsulé. Cette nouvelle fonctionnalité de la pile SDN a la carte réseau de calculer automatiquement quelles maximale à publier et les utiliser sur le câble. Ces valeurs pour MTU sont différentes si le déchargement de n’importe quel HNV est en cours d’utilisation. (Dans le tableau de compatibilité de fonctionnalité, le tableau 1, MTU_for_HNV aurait les mêmes interactions comme permet de décharger le HNVv2 déchargements ont, car il est directement lié à la HNVv2.)

### <a name="large-send-offload-lso"></a>Déchargement d’envoi important (LSO, Large Send Offload)

LSO permet à une application passer un grand bloc de données à la carte réseau et les sauts de carte réseau les données en paquets qui tiennent dans le transfert unité maximale (MTU) du réseau.

### <a name="receive-segment-coalescing-rsc"></a>Receive Segment Coalescing (RSC)

Receive Segment Coalescing, également appelé volumineux réception déchargement, est une fonctionnalité de carte réseau qui prend les paquets qui font partie du même flux qui arrive entre les interruptions réseau et les regroupe dans un paquet unique avant de les remettre au système d’exploitation. RSC n’est pas disponible sur les cartes réseau qui est liés au commutateur virtuel Hyper-V. Pour plus d’informations, consultez [fusion de Segment de réception (RSC)](https://docs.microsoft.com/windows-server/networking/technologies/hpn/rsc-in-the-vswitch).