---
title: del
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 346eede2-2085-44f5-9936-6877b5d5a833
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6e569443a56646862c7a2c9fbd2c599cede941a1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378702"
---
# <a name="del"></a>del



Supprime un ou plusieurs fichiers. Cette commande est identique à la commande **Erase** .

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
del [/p] [/f] [/s] [/q] [/a[:]<Attributes>] <Names>
erase [/p] [/f] [/s] [/q] [/a[:]<Attributes>] <Names>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Noms de \<>|Spécifie une liste d’un ou plusieurs fichiers ou répertoires. Les caractères génériques peuvent être utilisés pour supprimer plusieurs fichiers. Si un répertoire est spécifié, tous les fichiers du répertoire sont supprimés.|
|/p|Demande confirmation avant de supprimer le fichier spécifié.|
|/f|Force la suppression des fichiers en lecture seule.|
|/s|Supprime les fichiers spécifiés du répertoire actif et de tous ses sous-répertoires. Affiche les noms des fichiers au fur et à mesure de leur suppression.|
|/q|Spécifie le mode silencieux. Vous n’êtes pas invité à confirmer la suppression.|
|/a [ :]\<les attributs >|Supprime les fichiers en fonction des attributs de fichier suivants :</br>fichiers en lecture seule **r**</br>fichiers masqués **h**</br>**je** ne trouve pas les fichiers indexés</br>fichiers système **s**</br>**fichiers prêts** pour l’archivage</br>points d’analyse **l**</br>-Le préfixe signifie « not »|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

> [!CAUTION]
> Si vous utilisez **del** pour supprimer un fichier de votre disque, vous ne pouvez pas le récupérer.
> -   Si vous utilisez **/p**, **del** affiche le nom d’un fichier et envoie le message suivant :

    `FileName, Delete (Y/N)?`

    To confirm the deletion, press Y. To cancel the deletion and display the next file name (that is, if you specified a group of files), press N. To stop the **del** command, press CTRL+C.
- Si vous désactivez les extensions de commande, **/s** affiche les noms de tous les fichiers qui n’ont pas été trouvés au lieu d’afficher les noms des fichiers en cours de suppression (autrement dit, le comportement est inversé).
- Si vous spécifiez un dossier dans *noms*, tous les fichiers du dossier sont supprimés. Par exemple, la commande suivante supprime tous les fichiers dans le dossier \work :  
  ```
  del \work
  ```  
- Vous pouvez utiliser des caractères génériques **&#42;** (et **?** ) pour supprimer plusieurs fichiers à la fois. Toutefois, pour éviter la suppression accidentelle de fichiers, vous devez utiliser des caractères génériques avec prudence avec la commande **del** . Par exemple, si vous tapez la commande suivante :  
  ```
  del *.*
  ```  
  La commande **del** affiche l’invite suivante :

  `Are you sure (Y/N)?`

  Pour supprimer tous les fichiers du répertoire actif, appuyez sur o, puis sur entrée. Pour annuler la suppression, appuyez sur N, puis sur entrée.

> [!NOTE]
> Avant d’utiliser des caractères génériques avec la commande **del** , utilisez les mêmes caractères génériques avec la commande **dir** pour répertorier tous les fichiers qui seront supprimés.
> -   La commande **del** , avec des paramètres différents, est disponible à partir de la console de récupération.

## <a name="BKMK_examples"></a>Illustre

Pour supprimer tous les fichiers d’un dossier nommé test sur le lecteur C, tapez l’un des éléments suivants :
```
del c:\test
del c:\test\*.*
```
Pour supprimer tous les fichiers avec l’extension de nom de fichier. bat du répertoire actif, tapez :
```
del *.bat
```
Pour supprimer tous les fichiers en lecture seule dans le répertoire actif, tapez :
```
del /a:r *.*
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
