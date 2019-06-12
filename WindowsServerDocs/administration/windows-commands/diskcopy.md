---
title: diskcopy
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5fd21efa-52cc-4e70-a7fe-35125a435106
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/07/2018
ms.openlocfilehash: aadb3a77cda7f1403cd2f04ced12c17617f046df
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439572"
---
# <a name="diskcopy"></a>diskcopy



Copie le contenu de la disquette dans le lecteur source vers une disquette formatée ou non dans le lecteur de destination. Si utilisée sans paramètres, **diskcopy** utilise le lecteur actuel pour le disque source et le disque de destination.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

> [!NOTE]
> Cette commande n’est pas incluse dans Windows 10.

## <a name="syntax"></a>Syntaxe

```
diskcopy [<Drive1>: [<Drive2>:]] [/v]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Drive1>|Spécifie le lecteur qui contient le disque source.|
|\<Drive2>|Spécifie le lecteur qui contient le disque de destination.|
|/v|Vérifie que les informations sont correctement copiées. Cette option ralentit le processus de copie.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   À l’aide de disques

    **Diskcopy** fonctionne uniquement avec des disques amovibles, tels que des disquettes, qui doit être du même type. Vous ne pouvez pas utiliser **diskcopy** avec un disque dur. Si vous spécifiez un lecteur de disque dur *lecteur1* ou *lecteur2*, **diskcopy** affiche le message d’erreur suivant :  
    ```
    Invalid drive specification
    Specified drive does not exist or is nonremovable
    ```  
    Le **diskcopy** commande vous invite à insérer la source et destination des disques et attend que vous appuyiez sur n’importe quelle touche du clavier avant de continuer.

    Après la copie du disque, **diskcopy** affiche le message suivant :  
    ```
    Copy another diskette (Y/N)?
    ```  
    Si vous appuyez sur O, **diskcopy** vous invite à insérer les disques source et destination pour la prochaine opération de copie. Pour arrêter la **diskcopy** traiter, appuyez sur **N**.

    Si vous copiez vers une disquette non formatée dans *lecteur2*, **diskcopy** formate le disque avec le même nombre de côtés et secteurs par piste de la disquette dans *lecteur1*. **Diskcopy** affiche le message suivant pendant qu’il formate le disque et copie les fichiers :  
    ```
    Formatting while copying
    ```  
-   Numéros de série de disque

    Si le disque source a un numéro de série du volume **diskcopy** crée un nouveau numéro de série du volume pour le disque de destination et affiche le nombre lorsque l’opération de copie est terminée.
-   En omettant les paramètres de lecteur

    Si vous omettez le *lecteur2* paramètre, **diskcopy** utilise le lecteur actuel en tant que le lecteur de destination. Si vous omettez les deux paramètres de lecteur, **diskcopy** utilise le lecteur en cours pour les deux. Si le lecteur actuel est identique à *lecteur1*, **diskcopy** vous invite à permuter des disques en fonction des besoins.
-   À l’aide d’un seul lecteur pour la copie

    Exécutez **diskcopy** à partir d’un lecteur autre que le lecteur de disquette, par exemple le C le lecteur. Si disquette *lecteur1* et disquette *lecteur2* sont les mêmes, **diskcopy** vous invite à permuter des disques. Si les disques contiennent plus d’informations que la mémoire disponible peut contenir, **diskcopy** ne peut pas lire toutes les informations à la fois. **Diskcopy** lit à partir du disque source, écrit sur le disque de destination, et vous invite à réinsérer le disque source. Ce processus se poursuit jusqu'à ce que vous avez copié la totalité du disque.
-   Éviter la fragmentation du disque

    La fragmentation est la présence de petites zones d’espace disque inutilisé entre les fichiers existants sur un disque. Un disque source fragmenté peut ralentir le processus de recherche, de lecture ou d’écriture de fichiers.

    Étant donné que **diskcopy** effectue une copie exacte du disque source sur le disque de destination, toute fragmentation sur le disque source est transférée vers le disque de destination. Pour éviter de transférer la fragmentation d’un disque vers un autre, utilisez **copie** ou **xcopy** pour copier votre disque. Étant donné que **copie** et **xcopy** copie des fichiers de manière séquentielle, le nouveau disque n’est pas fragmentée.

> [!NOTE]
> Vous ne pouvez pas utiliser **xcopy** pour copier un disque de démarrage.
> -   Présentation de **diskcopy** les codes de sortie

    The following table explains each exit code.  
    |Code de sortie|Description|
    |---------|-----------|
    |0|Opération de copie a réussi|
    |1|Erreur de lecture/écriture non fatale s’est produite|
    |3|Erreur matérielle irrécupérable s’est produite|
    |4|Erreur d’initialisation s’est produite|

    To process the exit codes that are returned by **diskcomp**, you can use the *ERRORLEVEL* environment variable on the **if** command line in a batch program.

## <a name="BKMK_examples"></a>Exemples

Pour copier le disque dans le lecteur B sur le disque dans le lecteur A, tapez :
```
diskcopy b: a:
```
Pour utiliser le lecteur de disquette A pour copier une disquette à l’autre, tout d’abord basculer vers le lecteur C et tapez :

diskcopy a: a:

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)