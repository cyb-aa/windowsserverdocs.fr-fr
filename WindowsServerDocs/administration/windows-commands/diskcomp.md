---
title: diskcomp
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4f56f534-a356-4daa-8b4f-38e089341e42
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3945311873dff763a8e9e8cdd3f766c1ed4580b1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837530"
---
# <a name="diskcomp"></a>diskcomp



Compare le contenu de deux disquettes. Si utilisée sans paramètres, **diskcomp** utilise le lecteur en cours pour comparer les deux disques. Pour obtenir des exemples montrant comment utiliser cette commande, consultez [exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
diskcomp [<Drive1>: [<Drive2>:]]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Drive1>|Spécifie le lecteur qui contient l’une des disquettes.|
|\<Drive2>|Spécifie le lecteur qui contient l’autre disquette.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   À l’aide de disques

    Le **diskcomp** commande fonctionne uniquement avec les disquettes. Vous ne pouvez pas utiliser **diskcomp** avec un disque dur. Si vous spécifiez un lecteur de disque dur *lecteur1* ou *lecteur2*, **diskcomp** affiche le message d’erreur suivant :  
    ```
    Invalid drive specification
    Specified drive does not exist
    or is nonremovable
    ```  
-   Comparaison de disques

    Si toutes les pistes sur les deux disques comparés sont les mêmes, **diskcomp** affiche le message suivant :  
    ```
    Compare OK
    ```  
    Si les pistes ne sont pas identiques, **diskcomp** affiche un message semblable au suivant :  
    ```
    Compare error on
    side 1, track 2
    ```  
    Lorsque **diskcomp** une fois la comparaison, il affiche le message suivant :  
    ```
    Compare another diskette (Y/N)?
    ```  
    Si vous appuyez sur O, **diskcomp** vous invite à insérer le disque de la comparaison suivante. Si vous appuyez sur N, **diskcomp** arrête la comparaison.

    Lorsque **diskcomp** rend la comparaison, il ignore le nombre de volume d’un disque.
-   En omettant les paramètres de lecteur

    Si vous omettez le *lecteur2* paramètre, **diskcomp** utilise le lecteur actuel pour *lecteur2*. Si vous omettez les deux paramètres de lecteur, **diskcomp** utilise le lecteur en cours pour les deux. Si le lecteur actuel est identique à *lecteur1*, **diskcomp** vous invite à permuter des disques en fonction des besoins.
-   À l’aide d’un lecteur

    Si vous spécifiez le même lecteur de disquette pour *lecteur1* et *lecteur2*, **diskcomp** les compare à l’aide d’un seul lecteur et vous invite à insérer les disques en fonction des besoins. Vous devrez peut-être permuter les disques de plusieurs fois, en fonction de la capacité des disques et la quantité de mémoire disponible.
-   Comparaison des différents types de disques

    **Diskcomp** ne peut pas comparer un disque recto avec un disque recto-verso, ni un disque à haute densité avec un disque double densité. Si le disque dans *lecteur1* n’est pas du même type que le disque dans *lecteur2*, **diskcomp** affiche le message suivant :  
    ```
    Drive types or diskette types not compatible
    ```  
-   À l’aide de **diskcomp** avec les réseaux et des lecteurs redirigés

    **Diskcomp** ne fonctionne pas sur un lecteur réseau ou sur un lecteur créé par le **subst** commande. Si vous essayez d’utiliser **diskcomp** avec un lecteur d’une de ces types, **diskcomp** affiche le message d’erreur suivant :  
    ```
    Invalid drive specification
    ```  
-   Comparaison d’un disque d’origine avec une copie

    Lorsque vous utilisez **diskcomp** avec un disque que vous avez apportées à l’aide de **copie**, **diskcomp** peut afficher un message semblable au suivant :  
    ```
    Compare error on 
    side 0, track 0
    ```  
    Ce type d’erreur peut se produire même si les fichiers sur les disques sont identiques. Bien que **copie** les doublons d’informations, il ne pas nécessairement le placer dans le même emplacement sur le disque de destination.
-   Présentation de **diskcomp** les codes de sortie

    Le tableau suivant décrit chaque code de sortie.  
    |Code de sortie|Description|
    |---------|-----------|
    |0|Les disques sont identiques|
    |1|Différences ont été trouvées.|
    |3|Erreur matérielle s’est produite|
    |4|Erreur d’initialisation s’est produite|

    Pour traiter des codes de sortie qui sont retournés par **diskcomp**, vous pouvez utiliser la variable d’environnement ERRORLEVEL sur le **si** ligne de commande dans un programme de traitement par lots.

## <a name="BKMK_examples"></a>Exemples

Si votre ordinateur n'a qu’un seul lecteur de disquette (par exemple, le lecteur A), et que vous souhaitez comparer deux disques, tapez :
```
diskcomp a: a:
```
**Diskcomp** vous invite à insérer chaque disque, en fonction des besoins.

L’exemple suivant illustre comment traiter un **diskcomp** quitter le code dans un programme de traitement par lots qui utilise la variable d’environnement ERRORLEVEL sur le **si** ligne de commande :
```
rem Checkout.bat compares the disks in drive A and B 
echo off 
diskcomp a: b: 
if errorlevel 4 goto ini_error 
if errorlevel 3 goto hard_error 
if errorlevel 1 goto no_compare
if errorlevel 0 goto compare_ok 
:ini_error 
echo ERROR: Insufficient memory or command invalid 
goto exit 
:hard_error 
echo ERROR: An irrecoverable error occurred 
goto exit 
:break 
echo "You just pressed CTRL+C" to stop the comparison 
goto exit 
:no_compare 
echo Disks are not the same 
goto exit 
:compare_ok 
echo The comparison was successful; the disks are the same 
goto exit 
:exit
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
