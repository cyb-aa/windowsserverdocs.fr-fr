---
title: Ajout, suppression et mise à jour de fonctionnalités
description: System Insights vous permet de créer de nouvelles fonctionnalités qui tirent parti des fonctionnalités existantes de collecte et de gestion des données. Il est important que vous disposiez également de la prise en charge de la plateforme pour gérer l’ajout, la suppression et les mises à jour de ces fonctionnalités. Cette rubrique décrit les fonctionnalités de haut niveau pour ajouter, supprimer et mettre à jour des fonctionnalités dans System Insights.
ms.prod: windows-server
ms.technology: system-insights
ms.topic: article
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 7/31/2018
ms.openlocfilehash: 0a760f25de79bc89b2aa67aec6bb1e3a493c1310
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856262"
---
# <a name="adding-removing-and-updating-capabilities"></a>Ajout, suppression et mise à jour de fonctionnalités

>S'applique à : Windows Server 2019

System Insights vous permet de créer de nouvelles fonctionnalités qui tirent parti des fonctionnalités existantes de collecte et de gestion des données. Toutefois, une fois ces fonctionnalités créées, il est tout aussi important que vous disposiez également de la prise en charge de la plateforme pour gérer l’ajout, la suppression et les mises à jour de ces fonctionnalités. 

Cette rubrique décrit les fonctionnalités de haut niveau pour ajouter, supprimer et mettre à jour des fonctionnalités dans System Insights. 

## <a name="adding-a-capability"></a>Ajout d’une fonctionnalité
System Insights vous permet d’ajouter de nouvelles fonctionnalités à tout moment à l’aide de l’applet de commande **Add-InsightsCapability** . La fonction **Add-InsightsCapability** vous oblige à spécifier un nom de fonctionnalité et la bibliothèque de fonctionnalités. La bibliothèque de fonctionnalités contient la description de la fonctionnalité, les sources de données et la logique de prédiction.

```PowerShell
Add-InsightsCapability -Name Sample capability -Library C:\SampleCapability.dll
```

Une fois qu’une fonctionnalité a été ajoutée à System Insights, vous pouvez immédiatement appeler et gérer la fonctionnalité à l’aide de PowerShell ou du centre d’administration Windows. 

## <a name="updating-a-capability"></a>Mise à jour d’une fonctionnalité
System Insights vous permet également de mettre à jour une fonctionnalité à l’aide de l’applet de commande **Update-InsightsCapability** .

```PowerShell
Update-InsightsCapability -Name Sample capability -Library C:\SampleCapabilityv2.dll
```

La mise à jour d’une fonctionnalité vous permet de spécifier une nouvelle bibliothèque de fonctionnalités, ce qui vous permet de modifier la description de la fonctionnalité, les sources de données et la logique de prédiction associée à cette fonctionnalité. Plus important encore, la mise à jour d’une fonctionnalité permet de conserver toutes les informations de configuration et d’historique sur cette fonctionnalité, y compris les planifications personnalisées, les actions et les résultats de prédictions historiques. 

## <a name="removing-a-capability"></a>Suppression d’une fonctionnalité
Vous pouvez également supprimer des fonctionnalités dans System Insights à l’aide de l’applet de commande **Remove-InsightsCapability** . 

```PowerShell
Remove-InsightsCapability -Name Sample capability 
```
>[!NOTE]
>Les fonctionnalités de prévision par défaut ne peuvent pas être supprimées.

La suppression d’une fonctionnalité supprime définitivement la fonctionnalité et toutes les informations associées, y compris la planification, toutes les actions de correction et les résultats de prédiction précédents. 

>[!TIP]
>Envisagez de désactiver une fonctionnalité plutôt que de la supprimer si vous vous inquiétez de supprimer définitivement toutes les informations associées à la fonctionnalité. 

## <a name="see-also"></a>Voir aussi
Pour en savoir plus sur System Insights, utilisez les ressources suivantes :

- [Vue d’ensemble de System Insights](overview.md)
- [Présentation des fonctionnalités](understanding-capabilities.md)
- [Gestion des fonctionnalités](managing-capabilities.md)
- [Ajout et développement de fonctionnalités](adding-and-developing-capabilities.md)
- [FAQ System Insights](faq.md)