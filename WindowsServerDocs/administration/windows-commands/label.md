---
title: label
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bbae8bdd-97d4-4566-9118-7c95aa07645f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d0c68fbbf3ea776bbf6cd49fc4fa446d5dd46542
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437910"
---
# <a name="label"></a>label



Crée, modifie ou supprime le nom de volume (autrement dit, le nom) d’un disque. Si utilisée sans paramètres, le **étiquette** commande modifie l’étiquette de volume actuel ou de supprimer l’étiquette existante.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
label [/mp] [<Volume>] [<Label>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/mp|Spécifie que le volume doit être traité comme un nom de volume ou de point de montage.|
|\<Volume>|Spécifie une lettre de lecteur (suivie d’un signe deux-points), point de montage ou nom de volume. Si un nom de volume est spécifié, le **/MP** paramètre n’est pas nécessaire.|
|\<Label>|Spécifie l’étiquette pour le volume.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

- Windows affiche le nom de volume et le numéro de série (le cas échéant) dans le cadre de la liste des répertoires.
- Une étiquette de volume NTFS peut être jusqu'à 32 caractères, y compris les espaces. Étiquettes de volume NTFS conservent et affichent le cas a été utilisé lors de la création de l’étiquette.
- Si vous ne spécifiez pas une valeur pour le **étiquette** paramètre, le **étiquette** commande affiche une sortie au format suivant :  
  ```
  Volume in drive C: xxxxxxxxxxx 
  Volume Serial Number is xxxx-xxxx 
  Volume label (32 characters, ENTER for none)?
  ```  
  Vous pouvez taper une nouvelle étiquette de volume, ou appuyez sur ENTRÉE pour conserver l’étiquette actuelle. Si vous appuyez sur entrée et le volume a une étiquette, le **étiquette** commande vous invite avec le message suivant :  
  ```
  Delete current volume label (Y/N)?
  ```  
  Appuyez sur o pour supprimer l’étiquette, ou appuyez sur N pour conserver l’étiquette.

## <a name="BKMK_examples"></a>Exemples

Pour étiqueter un disque dans le lecteur A qui contient des informations sur les ventes de juillet, tapez :
```
label a:sales-july
```
Pour supprimer l’étiquette actuelle pour le lecteur C, procédez comme suit :
1. À l’invite de commandes, tapez :  
   ```
   Label
   ```  
   Sortie similaire à ce qui suit doit s’afficher :  
   ```
   Volume in drive C: is Main Disk
   Volume Serial Number is 6789-ABCD
   Volume label (32 characters, ENTER for none)?
   ```  
2. Appuyez sur ENTRÉE. L’invite suivante doit s’afficher :  
   ```
   Delete current volume label (Y/N)?
   ```  
3. Appuyez sur o pour supprimer l’étiquette actuelle.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)