---
title: Considérations relatives à la création de modules PowerShell
description: Considérations relatives à la création de modules PowerShell
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: JasonSh
author: lzybkr
ms.date: 10/16/2017
ms.openlocfilehash: 8945339e7a7950d3cd722ab2af629b45e7f6dd5d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370360"
---
# <a name="powershell-module-authoring-considerations"></a>Considérations relatives à la création de modules PowerShell

Ce document contient des instructions relatives à la création d’un module pour des performances optimales.

## <a name="module-manifest-authoring"></a>Création de manifeste de module

Un manifeste de module qui n’utilise pas les indications suivantes peut avoir un impact notable sur les performances générales de PowerShell, même si le module n’est pas utilisé dans une session.

La découverte automatique des commandes analyse chaque module pour déterminer les commandes exportées par le module et cette analyse peut être coûteuse.
Les résultats de l’analyse des modules sont mis en cache par utilisateur, mais le cache n’est pas disponible lors de la première exécution, qui est un scénario classique avec des conteneurs.
Pendant l’analyse des modules, si les commandes exportées peuvent être entièrement déterminées à partir du manifeste, une analyse plus coûteuse du module peut être évitée.

### <a name="guidelines"></a>Recommandations

* Dans le manifeste de module, n’utilisez pas de caractères génériques dans les entrées `AliasesToExport`, `CmdletsToExport` et `FunctionsToExport`.

* Si le module n’exporte pas de commandes d’un type particulier, spécifiez-le explicitement dans le manifeste en spécifiant `@()`.
Une entrée manquante ou `$null` équivaut à spécifier le caractère générique `*`.

Les éléments suivants doivent être évités dans la mesure du possible :

```PowerShell
@{
    FunctionsToExport = '*'

    # Also avoid omitting an entry, it is equivalent to using a wildcard
    # CmdletsToExport = '*'
    # AliasesToExport = '*'
}
```

Au lieu de cela, utilisez :

```PowerShell
@{
    FunctionsToExport = 'Format-Hex', 'Format-Octal'
    CmdletsToExport = @()  # Specify an empty array, not $null
    AliasesToExport = @()  # Also ensure all three entries are present
}
```

## <a name="avoid-cdxml"></a>Éviter le CDXML

Lorsque vous décidez comment implémenter votre module, il existe trois choix principaux :

* Binaire (généralement C#)
* Script (PowerShell)
* CDXML (fichier XML avec encapsulation CIM)

Si la vitesse de chargement de votre module est importante, CDXML est beaucoup plus lent qu’un module binaire.

Un module binaire charge le plus rapidement, car il est compilé à l’avance et peut utiliser NGen pour compiler juste une fois par ordinateur.

Un module de script se charge généralement un peu plus lentement qu’un module binaire, car PowerShell doit analyser le script avant de le compiler et de l’exécuter.

Un module CDXML est généralement beaucoup plus lent qu’un module de script, car il doit d’abord analyser un fichier XML qui génère alors un certain nombre de scripts PowerShell qui sont ensuite analysés et compilés.

