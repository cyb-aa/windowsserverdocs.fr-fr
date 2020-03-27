---
title: Technologies de déchargement et d'optimisation du réseau
description: Cette rubrique fournit une vue d’ensemble des technologies de déchargement et d’optimisation de Windows Server 2016, et inclut des liens vers des conseils supplémentaires sur ces technologies.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/20/2018
ms.openlocfilehash: 04e924190170bbd3c841b452a27dc586bb69024e
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316865"
---
# <a name="network-offload-and-optimization-technologies"></a>Technologies de déchargement et d'optimisation du réseau

Dans cette rubrique, nous vous proposons une vue d’ensemble des différentes fonctionnalités de déchargement et d’optimisation du réseau disponibles dans Windows Server 2016 et voyons comment elles contribuent à améliorer l’efficacité de la mise en réseau. Ces technologies incluent les fonctionnalités et technologies intégrées, ainsi que les fonctionnalités et technologies intégrées aux logiciels et matériels (SH), ainsi que les fonctionnalités et technologies de matériel uniquement (HO).

Les trois catégories de fonctionnalités de mise en réseau disponibles dans Windows Server 2016 sont les suivantes : 

1.  [Logiciels uniquement (ainsi) fonctionnalités et technologies](hpn-software-only-features.md): ces fonctionnalités sont implémentées dans le cadre du système d’exploitation et sont indépendantes de la ou des cartes réseau sous-jacentes. Parfois, ces fonctionnalités nécessitent un paramétrage de la carte réseau pour un fonctionnement optimal. Les fonctionnalités Hyper-v, telles que vmQoS, les listes de contrôle d’accès et les fonctionnalités non-Hyper-V, telles que l’Association de cartes réseau, sont des exemples.   

2.  [Fonctionnalités et technologies intégrées des logiciels et matériels (SH)](hpn-software-hardware-features.md): ces fonctionnalités ont des composants logiciels et matériels. Le logiciel est étroitement lié aux capacités matérielles requises pour que la fonctionnalité fonctionne. Citons par exemple VMMQ, les ordinateurs virtuels, le déchargement de la somme de contrôle IPv4 côté envoi et RSS.   

3.  [Fonctionnalités et technologies de matériel uniquement (Ho)](hpn-hardware-only-features.md): ces accélérations matérielles améliorent les performances de mise en réseau conjointement avec le logiciel, mais ne font pas partie intégrante d’une fonctionnalité logicielle. Citons par exemple la modération des interruptions, le contrôle du trafic et le déchargement de la somme de contrôle IPv4 côté réception. 

4. [Propriétés avancées de la carte réseau](hpn-nic-advanced-properties.md): vous pouvez gérer les cartes réseau et toutes les fonctionnalités via Windows PowerShell à l’aide de l’applet de commande NetAdapter.  Vous pouvez également gérer des cartes réseau et toutes les fonctionnalités à l’aide du panneau de configuration réseau (ncpa. cpl). 

>[!TIP]
>- Les fonctionnalités et les technologies sont donc disponibles dans toutes les architectures matérielles, quelle que soit la vitesse de la carte réseau ou les fonctionnalités de la carte réseau.
>
>- Les fonctionnalités SH et HO sont disponibles uniquement lorsque votre carte réseau prend en charge les fonctionnalités ou les technologies.

---