---
title: Considérations sur les performances script PowerShell
description: Écriture de scripts pour des performances dans PowerShell
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: JasonSh
author: lzybkr
ms.date: 10/16/2017
ms.openlocfilehash: df406c7e382907ca32006ec4dae6537a140a91cf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830370"
---
# <a name="powershell-scripting-performance-considerations"></a>Considérations sur les performances script PowerShell

Les scripts PowerShell qui exploitent des .NET directement et éviter le pipeline ont tendance à être plus rapide que PowerShell idiomatique. PowerShell IDIOMATIQUE utilise généralement des applets de commande et PowerShell fonctionne très souvent en exploitant le pipeline et en descendant dans .NET uniquement lorsque cela est nécessaire.

>[!Note] 
> La plupart des techniques décrites ici ne sont pas PowerShell IDIOMATIQUE et peuvent réduire la lisibilité d’un script PowerShell. Les auteurs de script sont recommandés d’utiliser PowerShell IDIOMATIQUE, sauf si les performances en décide autrement.

## <a name="suppressing-output"></a>Suppression de sortie

Il existe de nombreuses façons d’éviter d’écrire des objets dans le pipeline :

```PowerShell
$null = $arrayList.Add($item)
[void]$arrayList.Add($item)
```

Assignation à `$null` ou effectuer un cast à `[void]` sont à peu près équivalente et doivent généralement être préféré où les performances est importante.

```PowerShell
$arrayList.Add($item) > $null
```

Fichier de la redirection vers `$null` est presque aussi bien que les alternatives précédents, plus les scripts ne seraient jamais Notez la différence.
Selon le scénario, la redirection de fichiers présente cependant un peu de surcharge.

```PowerShell
$arrayList.Add($item) | Out-Null
```

Dirige vers `Out-Null` entraîne une surcharge importante par rapport aux solutions concurrentes.
Elle doit éviter de code sensible des performances.

```PowerShell
$null = . {
    $arrayList.Add($item)
    $arrayList.Add(42)
}
```

Présentation d’un bloc de script et l’appeler (à l’aide du point d’approvisionnement ou autre), puis assigne le résultat à `$null` est une technique pratique permettant de supprimer la sortie d’un grand bloc de script.
Cette technique s’exécute à peu près ainsi que des pipelines à `Out-Null` et doivent être évitées dans le script sensible des performances.
La surcharge supplémentaire dans cet exemple provient de la création d’et en appelant un bloc de script qui a été précédemment script inline.


## <a name="array-addition"></a>Ajout de tableau

Génération d’une liste d’éléments est souvent effectuée à l’aide d’un tableau avec l’opérateur d’addition :

```PowerShell
$results = @()
$results += Do-Something
$results += Do-SomethingElse
$results
```

Cela peut être très inefficent car les tableaux sont immuables.
Chaque ajout au tableau réellement crée un tableau assez grand pour contenir tous les éléments des deux opérandes gauche et droit, puis copie les éléments des deux opérandes dans le nouveau tableau.
Pour les petites collections, cette surcharge n’est pas important.
Pour les grandes collections, il peut s’agir sans aucun doute un problème.

Il existe deux alternatives.
Si vous ne nécessitent en fait un tableau, pensez à la place à l’aide d’une liste de tableaux :

```PowerShell
$results = [System.Collections.ArrayList]::new()
$results.AddRange((Do-Something))
$results.AddRange((Do-SomethingElse))
$results
```

Si vous avez besoin d’un tableau, vous pouvez utiliser votre propre `ArrayList` et simplement appeler `ArrayList.ToArray` lorsque vous souhaitez que le tableau.
Vous pouvez également laisser PowerShell créer le `ArrayList` et `Array` pour vous :

```PowerShell
$results = @(
    Do-Something
    Do-SomethingElse
)
```

Dans cet exemple, le script PowerShell crée un `ArrayList` pour stocker les résultats écrits dans le pipeline à l’intérieur de l’expression de tableau.
Avant d’affecter à `$results`, PowerShell convertit le `ArrayList` à un `object[]`.

## <a name="processing-large-files"></a>Traitement des fichiers volumineux

La méthode IDIOMATIQUE pour traiter un fichier dans PowerShell peut ressembler à :

```PowerShell
Get-Content $path | Where-Object { $_.Length -gt 10 }
```

Cela peut être presque un ordre de grandeur plus lente que l’utilisation des API .NET directement :

```PowerShell
try
{
    $stream = [System.IO.StreamReader]::new($path)
    while ($line = $stream.ReadLine())
    {
        if ($line.Length -gt 10)
        {
            $line
        }
    }
}
finally
{
    $stream.Dispose()
}
```

## <a name="avoid-write-host"></a>Éviter de Write-Host

Il est généralement considéré comme une mauvaise pratique pour écrire la sortie directement dans la console, mais lorsqu’il est judicieux, nombreux scripts utilisent `Write-Host`.

Si vous devez écrire plusieurs messages dans la console, `Write-Host` peut être un ordre de grandeur plus lent que `[Console]::WriteLine()`. Toutefois, n’oubliez pas que `[Console]::WriteLine()` est uniquement une alternative appropriée pour des hôtes spécifiques tels que powershell.exe ou powershell_ise.exe : il n’est pas garanti fonctionne dans tous les ordinateurs hôtes.

Au lieu d’utiliser `Write-Host`, envisagez d’utiliser [Write-Output](/powershell/module/Microsoft.PowerShell.Utility/Write-Output?view=powershell-5.1).

