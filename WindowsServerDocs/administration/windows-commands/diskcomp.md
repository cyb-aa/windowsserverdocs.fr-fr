---
title: diskcomp
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: ca5ea0f4587b21b2a274c772aab239668b7868b4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377871"
---
# <a name="diskcomp"></a>diskcomp



Compare le contenu de deux disquettes. En cas d’utilisation sans paramètre, **diskcomp** utilise le lecteur actuel pour comparer les deux disques. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
diskcomp [<Drive1>: [<Drive2>:]]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Lecteur1 >|Spécifie le lecteur contenant l’une des disquettes.|
|\<lecteur2 >|Spécifie le lecteur contenant l’autre disquette.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

- Utilisation de disques

  La commande **diskcomp** fonctionne uniquement avec les disquettes. Vous ne pouvez pas utiliser **diskcomp** avec un disque dur. Si vous spécifiez un disque dur pour *lecteur1* ou *lecteur2*, **diskcomp** affiche le message d’erreur suivant :  
  ```
  Invalid drive specification
  Specified drive does not exist
  or is nonremovable
  ```  
- Comparaison des disques

  Si toutes les pistes sur les deux disques comparées sont identiques, **diskcomp** affiche le message suivant :  
  ```
  Compare OK
  ```  
  Si les pistes ne sont pas les mêmes, **diskcomp** affiche un message similaire à ce qui suit :  
  ```
  Compare error on
  side 1, track 2
  ```  
  Lorsque **diskcomp** termine la comparaison, il affiche le message suivant :  
  ```
  Compare another diskette (Y/N)?
  ```  
  Si vous appuyez sur Y, **diskcomp** vous invite à insérer le disque pour la comparaison suivante. Si vous appuyez sur N, **diskcomp** arrête la comparaison.

  Lorsque **diskcomp** effectue la comparaison, il ignore le numéro de volume d’un disque.
- Omission des paramètres de lecteur

  Si vous omettez le paramètre *lecteur2* , **diskcomp** utilise le lecteur actif pour *lecteur2*. Si vous omettez les deux paramètres de lecteur, **diskcomp** utilise le lecteur actif pour les deux. Si le lecteur actuel est le même que *lecteur1*, **diskcomp** vous invite à échanger des disques si nécessaire.
- Utilisation d’un lecteur

  Si vous spécifiez le même lecteur de disquette pour *lecteur1* et *lecteur2*, **diskcomp** les compare à l’aide d’un lecteur et vous invite à insérer les disques en fonction des besoins. Vous devrez peut-être permuter les disques plusieurs fois, en fonction de la capacité des disques et de la quantité de mémoire disponible.
- Comparaison de différents types de disques

  **Diskcomp** ne peut pas comparer un disque à une seule face et un disque à double face, ni un disque à haute densité avec un disque à double densité. Si le disque dans *lecteur1* n’est pas du même type que le disque dans *lecteur2*, **diskcomp** affiche le message suivant :  
  ```
  Drive types or diskette types not compatible
  ```  
- Utilisation de **diskcomp** avec des réseaux et des lecteurs redirigés

  **Diskcomp** ne fonctionne pas sur un lecteur réseau ou sur un lecteur créé par la commande **subst** . Si vous tentez d’utiliser **diskcomp** avec un lecteur de l’un de ces types, **diskcomp** affiche le message d’erreur suivant :  
  ```
  Invalid drive specification
  ```  
- Comparaison d’un disque d’origine avec une copie

  Quand vous utilisez **diskcomp** avec un disque que vous avez créé à l’aide de la commande **copier**, **diskcomp** peut afficher un message similaire à ce qui suit :  
  ```
  Compare error on 
  side 0, track 0
  ```  
  Ce type d’erreur peut se produire même si les fichiers sur les disques sont identiques. Bien que **copie** les informations de doublons, elles ne sont pas nécessairement placées au même emplacement sur le disque de destination.
- Fonctionnement des codes de sortie de **diskcomp**

  Le tableau suivant décrit chaque code de sortie.  

  |Code de sortie|Description|
  |---------|-----------|
  |0|Les disques sont identiques|
  |1|Des différences ont été trouvées|
  |3|Une erreur matérielle s’est produite|
  |4|Une erreur d’initialisation s’est produite|

  Pour traiter les codes de sortie retournés par **diskcomp**, vous pouvez utiliser la variable d’environnement errorlevel sur la ligne de commande **If** dans un programme de traitement par lots.

## <a name="BKMK_examples"></a>Illustre

Si votre ordinateur ne possède qu’un seul lecteur de disquette (par exemple, le lecteur A) et que vous souhaitez comparer deux disques, tapez :
```
diskcomp a: a:
```
**Diskcomp** vous invite à insérer chaque disque, si nécessaire.

L’exemple suivant illustre le traitement d’un code de sortie **diskcomp** dans un programme de traitement par lots qui utilise la variable d’environnement errorlevel sur la ligne de commande **If** :
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

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
