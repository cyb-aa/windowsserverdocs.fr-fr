---
title: Ajout, suppression et mise à jour de fonctionnalités
description: Système Insights vous permet de créer de nouvelles fonctionnalités qui tirent parti de la collecte de données existant et les fonctionnalités de gestion. Il est important que vous avez également la prise en charge de plateforme pour gérer l’ajout, suppression et les mises à jour de ces fonctionnalités. Cette rubrique décrit les fonctionnalités de haut niveau pour ajouter, supprimer et mettre à jour des fonctionnalités dans les informations système.
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
ms.openlocfilehash: 07fb036d1c4aa4a63107594ec1f81cb5be1c7724
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880160"
---
# <a name="adding-removing-and-updating-capabilities"></a>Ajout, suppression et mise à jour de fonctionnalités

>S'applique à : Windows Server 2019

Système Insights vous permet de créer de nouvelles fonctionnalités qui tirent parti de la collecte de données existant et les fonctionnalités de gestion. Une fois que ces fonctionnalités sont créées, toutefois, il est également important que vous avez également la prise en charge de plateforme pour gérer l’ajout, suppression et les mises à jour de ces fonctionnalités. 

Cette rubrique décrit les fonctionnalités de haut niveau pour ajouter, supprimer et mettre à jour des fonctionnalités dans les informations système. 

## <a name="adding-a-capability"></a>Ajout d’une fonctionnalité
Système Insights permet de vous permet d’ajouter de nouvelles fonctionnalités à tout moment à l’aide de la **Add-InsightsCapability** applet de commande. Le **Add-InsightsCapability** , vous devez spécifier un nom de fonctionnalité et de la bibliothèque de fonctionnalité. La bibliothèque de fonctionnalité contient la description de la fonctionnalité, sources de données et la logique de prédiction.

```PowerShell
Add-InsightsCapability -Name "Sample capability" -Library "C:\SampleCapability.dll"
```

Après l’ajout d’une fonctionnalité à système Insights, vous pouvez immédiatement appeler et gérer la fonctionnalité à l’aide de PowerShell ou Windows Admin Center. 

## <a name="updating-a-capability"></a>Une fonctionnalité de mise à jour
Système Insights vous permet également de mettre à jour une fonctionnalité à l’aide de la **InsightsCapability de mise à jour** applet de commande.

```PowerShell
Update-InsightsCapability -Name "Sample capability" -Library "C:\SampleCapabilityv2.dll"
```

La mise à jour d’une fonctionnalité vous permet de spécifier une nouvelle bibliothèque de capacité, ce qui vous permet de modifier la description de la fonctionnalité, les sources de données et la logique de prédiction associée à cette fonctionnalité. Plus important encore, la mise à jour d’une fonctionnalité conserve toutes les informations de configuration et historiques sur cette fonctionnalité, y compris les calendriers personnalisés, les actions et les résultats de prédiction historique. 

## <a name="removing-a-capability"></a>Suppression d’une fonctionnalité
Vous pouvez également supprimer des fonctionnalités dans les informations système à l’aide de la **Remove-InsightsCapability** applet de commande. 

```PowerShell
Remove-InsightsCapability -Name "Sample capability" 
```
>[!NOTE]
>La valeur par défaut de capacités de prévision ne peut pas être supprimé.

Suppression définitive d’une fonctionnalité supprime la fonctionnalité et toutes les informations, notamment la planification, les actions correctives et les derniers résultats de prédiction. 

>[!TIP]
>Envisagez de désactiver une fonctionnalité plutôt que de le supprimer si vous êtes inquiet de la suppression définitive de toutes les informations associées à la fonctionnalité. 

## <a name="see-also"></a>Voir aussi
Pour en savoir plus sur les informations système, utilisez les ressources suivantes :

- [Vue d’ensemble des informations système](overview.md)
- [Fonctionnalités de présentation](understanding-capabilities.md)
- [Gérer des fonctionnalités](managing-capabilities.md)
- [Ajout et le développement de fonctionnalités](adding-and-developing-capabilities.md)
- [Système Insights Forum aux questions](faq.md)