---
title: Technologies de déchargement et d'optimisation du réseau
description: Cette rubrique fournit une vue d’ensemble des Technologies d’optimisation dans Windows Server 2016 et le déchargement et inclut des liens vers des conseils supplémentaires sur ces technologies.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/20/2018
ms.openlocfilehash: 9cd63ae557eab6e8e4f69fedd20619d4e7af9184
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847850"
---
# <a name="network-offload-and-optimization-technologies"></a>Technologies de déchargement et d'optimisation du réseau

Dans cette rubrique, nous vous donner une vue d’ensemble du réseau différentes déchargement et optimisation des fonctionnalités disponibles dans Windows Server 2016 et voir comment ils contribuer à rendre la mise en réseau plus efficace. Ces technologies incluent des fonctionnalités de logiciel uniquement (OA) et de technologies, logiciel et matériel (SH) intégré des fonctionnalités et les technologies et les fonctionnalités du matériel uniquement (no) et les technologies.

Les trois catégories de fonctionnalités de mise en réseau disponibles dans Windows Server 2016 sont : 

1.  [Logiciels (OA) fonctionnalités et technologies](hpn-software-only-features.md): Ces fonctionnalités sont implémentées en tant que partie du système d’exploitation et sont indépendantes de la carte sous-jacente. Parfois, ces fonctionnalités nécessite quelques réglages de la carte réseau pour un fonctionnement optimal. Exemples de ces fonctionnalités de Hyper-v comme vmQoS, les ACL et les fonctionnalités Hyper-V, comme l’association de cartes réseau.   

2.  [Logiciel et matériel (SH) intégré des fonctionnalités et technologies](hpn-software-hardware-features.md): Ces fonctionnalités ont des composants logiciels et matériels. Le logiciel est étroitement lié à des capacités matérielles qui sont requises pour la fonctionnalité soit opérationnelle. Par exemple : VMMQ VMQ, déchargement de somme de contrôle côté envoi IPv4 et RSS.   

3.  [Matériel uniquement (no) fonctionnalités et technologies](hpn-hardware-only-features.md): Ces accélérations de matériel améliorent les performances de mise en réseau conjointement avec le logiciel, mais ne sont pas étroitement partie de tout logiciel. Par exemple : la modération de l’interruption, de contrôle de flux et de déchargement de somme de contrôle côté réception IPv4. 

4. [Les propriétés avancées de carte réseau](hpn-nic-advanced-properties.md): Vous pouvez gérer des cartes réseau et toutes les fonctionnalités par le biais de PowerShell de Windows à l’aide de l’applet de commande NetAdapter.  Vous pouvez également gérer des cartes réseau et toutes les fonctionnalités à l’aide du Panneau de configuration réseau (ncpa.cpl). 

>[!TIP]
>- Par conséquent, fonctionnalités et technologies sont disponibles dans toutes les architectures matérielles, quelle que soit la vitesse de la carte réseau ou les fonctionnalités de la carte réseau.
>
>- SH et HO fonctionnalités sont disponibles uniquement lorsque votre carte réseau prend en charge les fonctionnalités ou les technologies.

---