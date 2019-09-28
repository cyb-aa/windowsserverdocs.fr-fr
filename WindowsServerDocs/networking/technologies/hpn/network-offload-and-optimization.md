---
title: Technologies de déchargement et d'optimisation du réseau
description: Cette rubrique fournit une vue d’ensemble des technologies de déchargement et d’optimisation de Windows Server 2016, et inclut des liens vers des conseils supplémentaires sur ces technologies.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/20/2018
ms.openlocfilehash: 57e8a61bc54be05a32daa7eabc55a09553cee28a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405691"
---
# <a name="network-offload-and-optimization-technologies"></a>Technologies de déchargement et d'optimisation du réseau

Dans cette rubrique, nous vous proposons une vue d’ensemble des différentes fonctionnalités de déchargement et d’optimisation du réseau disponibles dans Windows Server 2016 et voyons comment elles contribuent à améliorer l’efficacité de la mise en réseau. Ces technologies incluent les fonctionnalités et technologies intégrées, ainsi que les fonctionnalités et technologies intégrées aux logiciels et matériels (SH), ainsi que les fonctionnalités et technologies de matériel uniquement (HO).

Les trois catégories de fonctionnalités de mise en réseau disponibles dans Windows Server 2016 sont les suivantes : 

1.  [Fonctionnalités et technologies du logiciel uniquement (SO)](hpn-software-only-features.md): Ces fonctionnalités sont implémentées dans le cadre du système d’exploitation et sont indépendantes de la ou des cartes réseau sous-jacentes. Parfois, ces fonctionnalités nécessitent un paramétrage de la carte réseau pour un fonctionnement optimal. Les fonctionnalités Hyper-v, telles que vmQoS, les listes de contrôle d’accès et les fonctionnalités non-Hyper-V, telles que l’Association de cartes réseau, sont des exemples.   

2.  [Fonctionnalités et technologies intégrées aux logiciels et matériels (SH)](hpn-software-hardware-features.md): Ces fonctionnalités comportent des composants logiciels et matériels. Le logiciel est étroitement lié aux capacités matérielles requises pour que la fonctionnalité fonctionne. Citons par exemple VMMQ, les ordinateurs virtuels, le déchargement de la somme de contrôle IPv4 côté envoi et RSS.   

3.  [Fonctionnalités et technologies matérielles uniquement (Ho)](hpn-hardware-only-features.md): Ces accélérations matérielles améliorent les performances de mise en réseau conjointement avec le logiciel, mais ne font pas partie intégrante d’une fonctionnalité logicielle. Citons par exemple la modération des interruptions, le contrôle du trafic et le déchargement de la somme de contrôle IPv4 côté réception. 

4. [Propriétés avancées de la carte réseau](hpn-nic-advanced-properties.md): Vous pouvez gérer les cartes réseau et toutes les fonctionnalités via Windows PowerShell à l’aide de l’applet de commande NetAdapter.  Vous pouvez également gérer des cartes réseau et toutes les fonctionnalités à l’aide du panneau de configuration réseau (ncpa. cpl). 

>[!TIP]
>- Les fonctionnalités et les technologies sont donc disponibles dans toutes les architectures matérielles, quelle que soit la vitesse de la carte réseau ou les fonctionnalités de la carte réseau.
>
>- Les fonctionnalités SH et HO sont disponibles uniquement lorsque votre carte réseau prend en charge les fonctionnalités ou les technologies.

---