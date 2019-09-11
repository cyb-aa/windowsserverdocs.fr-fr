---
title: Mise en réseau hautes performances
description: Cette rubrique fournit une vue d’ensemble des technologies de déchargement et d’optimisation de Windows Server 2016, et inclut des liens vers des conseils supplémentaires sur ces technologies.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/12/2018
ms.openlocfilehash: 3a561096da49efc4c217c0736d888674b2acf202
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871951"
---
## <a name="hardware-only-ho-features-and-technologies"></a>Fonctionnalités et technologies matérielles uniquement (HO)

Ces accélérations matérielles améliorent les performances de mise en réseau conjointement avec le logiciel, mais ne font pas partie intégrante d’une fonctionnalité logicielle. Citons par exemple la modération des interruptions, le contrôle du trafic et le déchargement de la somme de contrôle IPv4 côté réception.

>[!TIP]
>Les fonctionnalités SH et HO sont disponibles si la carte réseau installée la prend en charge. Les descriptions de fonctionnalités ci-dessous expliquent comment savoir si votre carte réseau prend en charge la fonctionnalité.

### <a name="address-checksum-offload"></a>Déchargement de somme de contrôle d’adresse

Les déchargements de somme de contrôle d’adresse sont une fonctionnalité de carte réseau qui décharge le calcul des sommes de contrôle d’adresse (IP, TCP, UDP) sur le matériel de carte réseau pour l’envoi et la réception.

Sur le chemin de réception, le déchargement de la somme de contrôle calcule les sommes de contrôle dans les en-têtes IP, TCP et UDP (selon le cas) et indique au système d’exploitation si les sommes de contrôle ont réussi, échoué ou n’ont pas été vérifiées. Si la carte d’interface réseau déclare que les sommes de contrôle sont valides, le système d’exploitation accepte le paquet non contesté. Si la carte d’interface réseau déclare que les sommes de contrôle ne sont pas valides ou ne sont pas activées, la pile IP/TCP/UDP calcule de nouveau les sommes de contrôle en interne. Si la somme de contrôle calculée échoue, le paquet est rejeté.

Dans le chemin d’envoi, le déchargement de la somme de contrôle calcule et insère les sommes de contrôle dans l’en-tête IP, TCP ou UDP, le cas échéant.

La désactivation des déchargements de somme de contrôle sur le chemin d’envoi ne désactive pas le calcul et l’insertion des paquets envoyés au pilote de miniport à l’aide de la fonctionnalité de déchargement d’envoi volumineux (LSO).  Pour désactiver tous les calculs de déchargement de la somme de contrôle, l’utilisateur doit également désactiver LSO.

_**Gérer les déchargements de somme de contrôle d’adresse**_

Dans les propriétés avancées, il existe plusieurs propriétés distinctes :

-   Déchargement de somme de contrôle IPv4

-   Déchargement de la somme de contrôle TCP (IPv4)

-   Déchargement de la somme de contrôle TCP (IPv6)

-   Déchargement de la somme de contrôle UDP (IPv4)

-   Déchargement de la somme de contrôle UDP (IPv6)

Par défaut, tous les sont toujours activés. Nous vous recommandons d’activer toujours tous ces déchargements.

Les déchargements de somme de contrôle peuvent être gérés à l’aide des applets de commande Enable-NetAdapterChecksumOffload et Disable-NetAdapterChecksumOffload. Par exemple, l’applet de commande suivante active les calculs de la somme de contrôle TCP (IPv4) et UDP (IPv4) :

```PowerShell
Enable-NetAdapterChecksumOffload –Name * -TcpIPv4 -UdpIPv4
```

_**Conseils sur l’utilisation des déchargements de somme de contrôle d’adresse**_

Les déchargements de somme de contrôle d’adresse doivent toujours être activés quelle que soit la charge de travail ou les circonstances. Cette fonctionnalité de base de toutes les technologies de déchargement améliore toujours les performances de votre réseau. Le déchargement de la somme de contrôle est également requis pour les autres déchargements sans État qui fonctionnent, notamment la mise à l’échelle côté réception (RSS), la fusion de segment de réception (RSC) et le déchargement d’envoi volumineux (LSO).

### <a name="interrupt-moderation-im"></a>Modération des interruptions (IM)

IM met en mémoire tampon plusieurs paquets reçus avant d’interrompre le système d’exploitation. Lorsqu’une carte réseau reçoit un paquet, elle démarre un minuteur. Lorsque la mémoire tampon est saturée ou que le minuteur expire, selon la première situation, la carte d’interface réseau interrompt le système d’exploitation. 

De nombreuses cartes réseau prennent en charge plus que la fonction on/off pour la modération des interruptions. La plupart des cartes réseau prennent en charge les concepts d’un taux faible, moyen et élevé pour la messagerie instantanée. Les différents taux représentent des minuteries plus courtes ou plus longues et des ajustements de taille de mémoire tampon appropriés afin de réduire la latence (faible modération des interruptions) ou de réduire les interruptions (modération d’interruptions élevées).

Il y a un équilibre entre la réduction des interruptions et la retardation excessive de la remise des paquets. En règle générale, le traitement des paquets est plus efficace avec la modération des interruptions activée. Les applications hautes performances ou à faible latence peuvent avoir besoin d’évaluer l’impact de la désactivation ou de la réduction de la modération des interruptions.

### <a name="jumbo-frames"></a>Trames Jumbo

Les trames Jumbo sont une fonctionnalité de réseau et de carte réseau qui permet à une application d’envoyer des trames dont la taille est supérieure à la valeur par défaut de 1500 octets. En général, la limite sur les trames Jumbo est d’environ 9000 octets, mais elle peut être plus petite.

Aucune modification n’a été apportée à la prise en charge des trames Jumbo dans Windows Server 2012 R2.

Dans Windows Server 2016, il existe un nouveau déchargement : MTU_for_HNV. Ce nouveau déchargement fonctionne avec les paramètres de frame Jumbo pour garantir que le trafic encapsulé ne nécessite pas de segmentation entre l’hôte et le commutateur adjacent. Cette nouvelle fonctionnalité de la pile SDN permet à la carte réseau de calculer automatiquement la MTU à publier et la MTU à utiliser sur le câble. Ces valeurs pour MTU sont différentes si un déchargement de HNV est en cours d’utilisation. (Dans le tableau de compatibilité des fonctionnalités, le tableau 1, MTU_for_HNV aurait les mêmes interactions que les déchargements de HNVv2, car il est directement lié aux déchargements de HNVv2.)

### <a name="large-send-offload-lso"></a>Déchargement d’envoi important (LSO, Large Send Offload)

LSO permet à une application de transmettre un grand bloc de données à la carte réseau, et la carte réseau décompose les données en paquets qui tiennent dans l’unité de transfert maximale (MTU) du réseau.

### <a name="receive-segment-coalescing-rsc"></a>Receive Segment Coalescing (RSC)

La fusion de segment de réception, également connue sous le nom de déchargement de réception volumineux, est une fonctionnalité de carte réseau qui prend en charge les paquets qui font partie du même flux qui arrivent entre les interruptions du réseau et les redirige en un seul paquet avant de les transmettre au système d’exploitation. RSC n’est pas disponible sur les cartes réseau qui sont liées au commutateur virtuel Hyper-V. Pour plus d’informations, voir [Receive segment fusion (RSC)](https://docs.microsoft.com/windows-server/networking/technologies/hpn/rsc-in-the-vswitch).