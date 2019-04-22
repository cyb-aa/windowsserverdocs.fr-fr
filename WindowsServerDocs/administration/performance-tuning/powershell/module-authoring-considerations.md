---
title: Module PowerShell de la création des considérations relatives à la
description: Module PowerShell de la création des considérations relatives à la
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: JasonSh
author: lzybkr
ms.date: 10/16/2017
ms.openlocfilehash: 37dd860019b91daf70947dba93d20274048487a0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818720"
---
# <a name="powershell-module-authoring-considerations"></a>Module PowerShell de la création des considérations relatives à la

Ce document inclut des recommandations relatives à la façon dont un module a été créé pour de meilleures performances.

## <a name="module-manifest-authoring"></a>Création du manifeste module

Un manifeste de module qui n’utilise pas les instructions suivantes peut avoir un impact perceptible sur les performances générales de PowerShell même si le module n’est pas utilisé dans une session.

La détection automatique de la commande analyse chaque module pour déterminer les commandes exporte le module et cette analyse peut s’avérer coûteuse.
Les résultats d’analyse de module sont mises en cache par utilisateur, mais le cache n’est pas disponible sur la première exécution, qui est un scénario classique avec des conteneurs.
Pendant l’analyse de module, si les commandes exportées peuvent être entièrement déterminés à partir du manifeste, une analyse plus coûteuse du module peut être évitée.

### <a name="guidelines"></a>Recommandations

* Dans le manifeste de module, n’utilisez pas de caractères génériques dans le `AliasesToExport`, `CmdletsToExport`, et `FunctionsToExport` entrées.

* Si le module n’exporte pas les commandes d’un type particulier, spécifiez ce explicitement dans le manifeste en spécifiant `@()`.
Absence d’un ou `$null` entrée équivaut à spécifier le caractère générique `*`.

Les éléments suivants doivent être évités autant que possible :

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

## <a name="avoid-cdxml"></a>Avoid CDXML

Lorsque vous décidez comment implémenter votre module, il existe trois principales options :

* Binaire (généralement C#)
* Script (PowerShell)
* CDXML (un fichier XML d’habillage CIM)

Si la vitesse de chargement de votre module est importante, CDXML est à peu près d’un ordre de grandeur plus lent que d’un module binaire.

Un module binaire se charge plus rapidement, car il est compilé à l’avance et que vous pouvez utiliser NGen à la compilation JIT en une fois par ordinateur.

Un module de script charge généralement un peu plus lentement que d’un module binaire, car PowerShell doit analyser le script avant de compiler et l’exécuter.

Un module CDXML est généralement beaucoup plus lent qu’un module de script, car il doit tout d’abord analyser un fichier XML qui génère ensuite un peu de script PowerShell qui est ensuite analysé et compilé.

