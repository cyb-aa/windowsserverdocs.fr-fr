---
title: Ajout et développement de fonctionnalités
description: Système Insights vous permet de vous permet d’ajouter de nouvelles fonctionnalités prédictives à système Insights, sans nécessiter des mises à jour du système d’exploitation. Cela permet aux développeurs, y compris Microsoft et tiers, pour créer et livrer des fonctionnalités intermédiaire mise en production pour résoudre les scénarios qui que vous intéressent. Nouvelles fonctionnalités peuvent spécifier des données personnalisées pour collecter et analyser, et elles s’intègrent également avec les plans de gestion système Insights existants.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: system-insights
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ''
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 7/31/2018
ms.openlocfilehash: 8caddead774ac69a38906f3c0a0d2eaf005c1d28
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817480"
---
# <a name="adding-and-developing-new-capabilities"></a>Ajout et le développement de nouvelles fonctionnalités

>S'applique à : Windows Server 2019

Système Insights vous permet de vous permet d’ajouter de nouvelles fonctionnalités prédictives à système Insights, sans nécessiter des mises à jour du système d’exploitation. Cela permet aux développeurs, y compris Microsoft et tiers, pour créer et livrer des fonctionnalités intermédiaire mise en production pour résoudre les scénarios qui que vous intéressent. 

Nouvelle fonctionnalité de les intégrer et étendre l’infrastructure système Insights :

- Nouvelles fonctionnalités peuvent **spécifier n’importe quel événement du système ou de compteur de performances**, qui sera collecté, rendus persistants localement et retourné à la fonctionnalité d’analyse lorsque la fonctionnalité est appelée.  
- Nouvelles fonctionnalités peuvent **tirer parti de Windows Admin Center existant et de plans de gestion PowerShell**. Non seulement seront nouvelles fonctionnalités détectable dans les informations système, ils bénéficient également des planifications personnalisées et les actions de mise à jour. 

## <a name="manage-new-capabilities"></a>Gérer les nouvelles fonctionnalités
- [En savoir plus](add-remove-update-capabilities.md) comment ajouter, supprimer et mettre à jour des fonctionnalités à l’aide de PowerShell. 

## <a name="develop-a-capability"></a>Développer une capacité
Utilisez les ressources suivantes pour vous aider à commencer à écrire vos propres fonctions personnalisées :
- [En savoir plus](data-sources.md) sur les sources de données que vous pouvez collecter.
- [Télécharger](https://www.nuget.org/packages/Microsoft.WindowsServer.SystemInsights/) le package NuGet des informations système, qui contient les classes et interfaces que vous devez écrire une fonctionnalité.
- [Visitez](https://aka.ms/systeminsights-api) la documentation API pour en savoir plus sur les classes d’Insights de système et les interfaces. 
- [Utilisez](https://aka.ms/systeminsights-samplecapability) système Insights exemple permet de vous aider à commencer. Cet exemple vous montre comment inscrire une fonctionnalité, spécifier les sources de données à collecter et commencer à analyser les données système.

>[!NOTE]
>Il s’agit d’une fonctionnalité préliminaire. Il est susceptible de changer, car nous ajouter de nouvelles fonctionnalités et incorporer les commentaires.

## <a name="see-also"></a>Voir aussi
Pour en savoir plus sur les informations système, utilisez les ressources suivantes :

- [Vue d’ensemble des informations système](overview.md)
- [Fonctionnalités de présentation](understanding-capabilities.md)
- [Gérer des fonctionnalités](managing-capabilities.md)
- [Ajout, suppression et la mise à jour de fonctionnalités](add-remove-update-capabilities.md)
- [Système Insights Forum aux questions](faq.md)