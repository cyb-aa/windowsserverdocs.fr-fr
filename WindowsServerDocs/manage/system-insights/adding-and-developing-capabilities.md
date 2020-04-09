---
title: Ajout et développement de fonctionnalités
description: System Insights vous permet d’ajouter de nouvelles fonctionnalités prédictives à System Insights, sans nécessiter de mise à jour du système d’exploitation. Cela permet aux développeurs, y compris à Microsoft et à des tiers, de créer et de fournir de nouvelles fonctionnalités pour répondre aux scénarios qui vous intéressent. Les nouvelles fonctionnalités peuvent spécifier des données personnalisées à collecter et à analyser, et elles s’intègrent également aux plans de gestion System Insights existants.
ms.prod: windows-server
ms.technology: system-insights
ms.topic: article
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 7/31/2018
ms.openlocfilehash: 0dd4e24197d5a8c438d70a849e435ce28792dfce
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858432"
---
# <a name="adding-and-developing-new-capabilities"></a>Ajout et développement de nouvelles fonctionnalités

>S'applique à : Windows Server 2019

System Insights vous permet d’ajouter de nouvelles fonctionnalités prédictives à System Insights, sans nécessiter de mise à jour du système d’exploitation. Cela permet aux développeurs, y compris à Microsoft et à des tiers, de créer et de fournir de nouvelles fonctionnalités pour répondre aux scénarios qui vous intéressent. 

Toute nouvelle fonctionnalité peut s’intégrer à l’infrastructure System Insights existante et l’étendre :

- Les nouvelles fonctionnalités peuvent **spécifier n’importe quel compteur de performances ou événement système**, qui sera collecté, conservé localement et retournés à la fonctionnalité d’analyse lorsque la fonctionnalité est appelée.  
- Les nouvelles fonctionnalités peuvent **tirer parti du centre d’administration Windows et des plans de gestion PowerShell existants**. Non seulement les nouvelles fonctionnalités seront détectables dans System Insights, mais elles bénéficieront également de planifications personnalisées et d’actions de correction. 

## <a name="manage-new-capabilities"></a>Gérer les nouvelles fonctionnalités
- [Découvrez](add-remove-update-capabilities.md) comment ajouter, supprimer et mettre à jour des fonctionnalités à l’aide de PowerShell. 

## <a name="develop-a-capability"></a>Développer une fonctionnalité
Utilisez les ressources suivantes pour vous aider à commencer à écrire vos propres fonctionnalités personnalisées :
- [En savoir plus](data-sources.md) sur les sources de données que vous pouvez collecter.
- [Téléchargez](https://www.nuget.org/packages/Microsoft.WindowsServer.SystemInsights/) le package NuGet System Insights, qui contient les classes et les interfaces dont vous avez besoin pour écrire une fonctionnalité.
- [Consultez](https://aka.ms/systeminsights-api) la documentation de l’API pour en savoir plus sur les classes et interfaces System Insights. 
- [Utilisez](https://aka.ms/systeminsights-samplecapability) l’exemple de fonctionnalité System Insights pour vous aider à commencer. Cela vous montre comment inscrire une fonctionnalité, spécifier les sources de données à collecter et démarrer l’analyse des données système.

>[!NOTE]
>Il s’agit d’une fonctionnalité préliminaire. Elle est sujette à modification, à mesure que nous ajoutons de nouvelles fonctionnalités et incorporons des commentaires.

## <a name="see-also"></a>Voir aussi
Pour en savoir plus sur System Insights, utilisez les ressources suivantes :

- [Vue d’ensemble de System Insights](overview.md)
- [Présentation des fonctionnalités](understanding-capabilities.md)
- [Gestion des fonctionnalités](managing-capabilities.md)
- [Ajout, suppression et mise à jour de fonctionnalités](add-remove-update-capabilities.md)
- [FAQ System Insights](faq.md)