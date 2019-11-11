---
title: Services Bureau à distance - Prendre en charge différents types d’utilisateurs
description: Décrit les différents types d’utilisateurs des services Bureau à distance.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: da522a18-c33f-468e-b9d6-3ad7d3cfba26
author: spatnaik
ms.author: spatnaik
ms.date: 11/06/2019
manager: scottman
ms.openlocfilehash: dc83d76dbfaded9dadae7c16ae9e1c8df7958a7d
ms.sourcegitcommit: d25e47a54a120b9bc26ff43597518fca58c1cc5b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/07/2019
ms.locfileid: "73753822"
---
# <a name="remote-desktop-services---cater-to-different-kinds-of-users"></a>Services Bureau à distance - Prendre en charge différents types d’utilisateurs

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016

Les services Bureau à distance prennent en charge différents types de charges de travail. Adaptez votre déploiement en fonction du besoin attendu de chaque type d’utilisateur.

## <a name="types-of-users"></a>Types d’utilisateurs

### <a name="knowledge-user"></a>Utilisateur de connaissances

Les utilisateurs de connaissances utilisent des applications de productivité légères telles que Microsoft Word, Excel, Outlook et le navigateur Microsoft Edge.

Pour les utilisateurs de connaissances, nous recommandons de ne pas avoir plus de quatre utilisateurs par processeur virtuel (vCPU).

### <a name="professional-user"></a>Utilisateur professionnel

Les utilisateurs professionnels utilisent des navigateurs Internet et des applications de productivité en plus de prendre en charge des charges de travail plus intensives, telles que le développement de logiciels et la création de contenu multimédia.

Pour les utilisateurs professionnels, nous recommandons de ne pas avoir plus de deux utilisateurs par vCPU.

### <a name="power-user"></a>Utilisateur avec pouvoir

Les utilisateurs avec pouvoir utilisent des applications d’ingénierie et graphiques telles que la conception assistée par ordinateur (CAO) et Adobe Photoshop. Les GPU sont souvent un bon choix pour les utilisateurs qui utilisent régulièrement des programmes gourmands en affichage graphique pour le rendu vidéo, la conception 3D et les simulations.

Pour les utilisateurs avec pouvoir, nous recommandons de ne pas avoir plus de d’un utilisateur par vCPU.

Pour en savoir plus sur l’accélération graphique, consultez [Choisissez votre technologie de rendu des éléments graphiques](rds-graphics-virtualization.md).

Azure propose d’autres options de déploiement de l’accélération graphique et plusieurs tailles de machine virtuelle avec GPU. Pour en savoir plus, consultez [Tailles de machine virtuelle à GPU optimisé](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu).

## <a name="test-workload"></a>Charge de travail de test

Nous vous recommandons d’effectuer un test de charge des déploiements avec des tests de contrainte et des simulations d’utilisation dans la vie réelle. Vous pouvez utiliser des outils de simulation pour effectuer un test de charge de votre déploiement. Assurez-vous que le système est suffisamment réactif et résilient pour répondre aux besoins de l’utilisateur, et n’oubliez pas de faire varier la taille de la charge pour éviter les surprises.
