---
title: label
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: e66a2d9a7d28462b287084e3f8b129ffc03800bd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374791"
---
# <a name="label"></a>label



Crée, modifie ou supprime l’étiquette de volume (autrement dit, le nom) d’un disque. En cas d’utilisation sans paramètre, la commande **label** modifie l’étiquette de volume en cours ou supprime l’étiquette existante.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
label [/mp] [<Volume>] [<Label>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/mp|Spécifie que le volume doit être traité comme un point de montage ou un nom de volume.|
|> du volume \<|Spécifie une lettre de lecteur (suivie d’un signe deux-points), d’un point de montage ou d’un nom de volume. Si un nom de volume est spécifié, le paramètre **/MP** n’est pas nécessaire.|
|Étiquette \<>|Spécifie l’étiquette du volume.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

- Windows affiche le nom de volume et le numéro de série (le cas échéant) dans le cadre de la liste des répertoires.
- Un nom de volume NTFS peut comporter jusqu’à 32 caractères, y compris les espaces. Les étiquettes de volume NTFS conservent et affichent le cas qui a été utilisé lors de la création de l’étiquette.
- Si vous ne spécifiez pas de valeur pour le paramètre **label** , la commande **label** affiche la sortie dans le format suivant :  
  ```
  Volume in drive C: xxxxxxxxxxx 
  Volume Serial Number is xxxx-xxxx 
  Volume label (32 characters, ENTER for none)?
  ```  
  Vous pouvez taper un nouveau nom de volume ou appuyer sur entrée pour conserver l’étiquette actuelle. Si vous appuyez sur entrée et que le volume contient actuellement une étiquette, la commande **étiquette** vous invite à entrer le message suivant :  
  ```
  Delete current volume label (Y/N)?
  ```  
  Appuyez sur Y pour supprimer l’étiquette ou sur N pour conserver l’étiquette.

## <a name="BKMK_examples"></a>Illustre

Pour étiqueter un disque dans le lecteur A qui contient des informations de ventes pour le juillet, tapez :
```
label a:sales-july
```
Pour supprimer l’étiquette actuelle pour le lecteur C, procédez comme suit :
1. À l’invite de commandes, tapez :  
   ```
   Label
   ```  
   Une sortie similaire à ce qui suit doit s’afficher :  
   ```
   Volume in drive C: is Main Disk
   Volume Serial Number is 6789-ABCD
   Volume label (32 characters, ENTER for none)?
   ```  
2. Appuyez sur ENTRÉE. L’invite suivante doit s’afficher :  
   ```
   Delete current volume label (Y/N)?
   ```  
3. Appuyez sur Y pour supprimer l’étiquette actuelle.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)