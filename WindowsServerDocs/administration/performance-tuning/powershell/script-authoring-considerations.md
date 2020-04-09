---
title: Considérations sur les performances des scripts PowerShell
description: Écriture de scripts pour les performances dans PowerShell
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: jasonsh
author: lzybkr
ms.date: 10/16/2017
ms.openlocfilehash: f22a4f1ba5c0f048e2aa01c744feb3b2b83007a0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851922"
---
# <a name="powershell-scripting-performance-considerations"></a>Considérations sur les performances des scripts PowerShell

Les scripts PowerShell qui exploitent directement .NET et évitent le pipeline ont tendance à être plus rapides que idiomatique PowerShell. Idiomatique PowerShell utilise généralement des applets de commande et des fonctions PowerShell de manière intensive, en tirant souvent parti du pipeline et en le déplaçant dans .NET uniquement lorsque cela est nécessaire.

>[!Note] 
> La plupart des techniques décrites ici ne sont pas idiomatique PowerShell et peuvent réduire la lisibilité d’un script PowerShell. Les auteurs de scripts sont invités à utiliser idiomatique PowerShell à moins que les performances ne le définissent autrement.

## <a name="suppressing-output"></a>Suppression de la sortie

Il existe de nombreuses façons d’éviter d’écrire des objets dans le pipeline :

```PowerShell
$null = $arrayList.Add($item)
[void]$arrayList.Add($item)
```

L’assignation à `$null` ou à la conversion en `[void]` est à peu près équivalente et doit généralement être préférable là où les performances sont importantes.

```PowerShell
$arrayList.Add($item) > $null
```

La redirection de fichiers vers `$null` est presque aussi efficace que les alternatives précédentes, la plupart des scripts ne remarqueront jamais la différence.
En fonction du scénario, la redirection de fichiers introduit un peu de surcharge.

```PowerShell
$arrayList.Add($item) | Out-Null
```

La canalisation vers `Out-Null` a une surcharge importante par rapport aux alternatives.
Elle doit être évitée dans le code sensible aux performances.

```PowerShell
$null = . {
    $arrayList.Add($item)
    $arrayList.Add(42)
}
```

L’introduction d’un bloc de script et son appel (à l’aide de l’approvisionnement en points ou autrement), puis l’assignation du résultat à `$null` est une technique pratique pour supprimer la sortie d’un grand bloc de script.
Cette technique s’exécute approximativement, de même que le canalisation à `Out-Null` et doit être évitée dans les scripts sensibles aux performances.
La surcharge supplémentaire de cet exemple provient de la création et de l’appel d’un bloc de script qui était auparavant un script inline.


## <a name="array-addition"></a>Ajout de tableau

La génération d’une liste d’éléments est souvent effectuée à l’aide d’un tableau avec l’opérateur d’addition :

```PowerShell
$results = @()
$results += Do-Something
$results += Do-SomethingElse
$results
```

Cela peut être très inefficent, car les tableaux sont immuables.
Chaque ajout au tableau crée en fait un nouveau tableau suffisamment grand pour contenir tous les éléments des opérandes de gauche et de droite, puis copie les éléments des deux opérandes dans le nouveau tableau.
Pour les petits regroupements, cette surcharge peut ne pas être importante.
Pour les collections volumineuses, cela peut être un problème.

Il existe deux alternatives.
Si vous n’avez pas besoin d’un tableau, envisagez plutôt d’utiliser une ArrayList :

```PowerShell
$results = [System.Collections.ArrayList]::new()
$results.AddRange((Do-Something))
$results.AddRange((Do-SomethingElse))
$results
```

Si vous avez besoin d’un tableau, vous pouvez utiliser vos propres `ArrayList` et appeler simplement `ArrayList.ToArray` quand vous souhaitez le tableau.
Vous pouvez également laisser PowerShell créer les `ArrayList` et `Array` pour vous :

```PowerShell
$results = @(
    Do-Something
    Do-SomethingElse
)
```

Dans cet exemple, PowerShell crée un `ArrayList` pour stocker les résultats écrits dans le pipeline à l’intérieur de l’expression de tableau.
Juste avant d’assigner à `$results`, PowerShell convertit le `ArrayList` en `object[]`.

## <a name="processing-large-files"></a>Traitement des rapport sur les fichiers volumineux

Le idiomatique moyen de traiter un fichier dans PowerShell peut ressembler à ceci :

```PowerShell
Get-Content $path | Where-Object { $_.Length -gt 10 }
```

Cela peut être presque un ordre de grandeur plus lent que l’utilisation directe des API .NET :

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

## <a name="avoid-write-host"></a>Éviter l’écriture-hôte

Il est généralement considéré comme une mauvaise pratique d’écrire la sortie directement dans la console, mais dans la mesure du logique, de nombreux scripts utilisent `Write-Host`.

Si vous devez écrire de nombreux messages dans la console, `Write-Host` peut être un ordre de grandeur plus lent que `[Console]::WriteLine()`. Toutefois, sachez que `[Console]::WriteLine()` n’est qu’une alternative appropriée pour des hôtes spécifiques comme PowerShell. exe ou powershell_ise. exe. il n’est pas garanti qu’il fonctionne sur tous les ordinateurs hôtes.

Au lieu d’utiliser `Write-Host`, envisagez d’utiliser [Write-Output](/powershell/module/Microsoft.PowerShell.Utility/Write-Output?view=powershell-5.1).

