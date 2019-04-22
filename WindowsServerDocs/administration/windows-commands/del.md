---
title: del
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: a1c2756e53d9f047160ddd037b3868e47d6e3181
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822990"
---
# <a name="del"></a>del



Supprime un ou plusieurs fichiers. Cette commande est identique à la **effacer** commande.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
del [/p] [/f] [/s] [/q] [/a[:]<Attributes>] <Names>
erase [/p] [/f] [/s] [/q] [/a[:]<Attributes>] <Names>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Names>|Spécifie une liste d’un ou plusieurs fichiers ou répertoires. Caractères génériques peuvent être utilisés pour supprimer plusieurs fichiers. Si un répertoire est spécifié, tous les fichiers dans le répertoire seront supprimés.|
|/p|Demande une confirmation avant de supprimer le fichier spécifié.|
|/f|Force la suppression des fichiers en lecture seule.|
|/s|Supprime les fichiers du répertoire actif et tous les sous-répertoires spécifiés. Affiche les noms des fichiers comme ils sont en cours de suppression.|
|/q|Spécifie le mode silencieux. Vous n’êtes pas invité à confirmer l’opération delete.|
|/ a [ ::]\<attributs >|Supprime les fichiers basés sur les attributs de fichier suivants :</br>**r** des fichiers en lecture seule</br>**h** répertorier les fichiers cachés</br>**J’ai** pas les fichiers indexés de contenu</br>**s** fichiers système</br>**un** prêt pour l’archivage des fichiers</br>**l** des points d’analyse</br>-Préfixe ce qui signifie « not »|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

> [!CAUTION]
> Si vous utilisez **del** pour supprimer un fichier à partir de votre disque, vous ne pouvez pas le récupérer.
-   Si vous utilisez **/p**, **del** affiche le nom d’un fichier et envoie le message suivant :

    `FileName, Delete (Y/N)?`

    Pour confirmer la suppression, appuyez sur Y. Pour annuler la suppression et l’affichage le prochain nom de fichier (autrement dit, si vous avez spécifié un groupe de fichiers), appuyez sur N. Pour arrêter la **del** de commande, appuyez sur CTRL + C.
-   Si vous désactivez les extensions de commande, **/s** affiche les noms de tous les fichiers qui n’ont pas été trouvés au lieu d’afficher les noms des fichiers qui sont en cours de suppression (autrement dit, le comportement est inversé).
-   Si vous spécifiez un dossier dans *noms*, tous les fichiers dans le dossier sont supprimés. Par exemple, la commande suivante supprime tous les fichiers du dossier \Work :  
    ```
    del \work
    ```  
-   Vous pouvez utiliser des caractères génériques (**&#42;** et **?**) pour supprimer plusieurs fichiers à la fois. Toutefois, pour éviter de supprimer des fichiers par inadvertance, vous devez utiliser des caractères génériques avec précaution avec le **del** commande. Par exemple, si vous tapez la commande suivante :  
    ```
    del *.*
    ```  
    Le **del** commande affiche l’invite suivante :

    `Are you sure (Y/N)?`

    Pour supprimer tous les fichiers dans le répertoire actif, appuyez sur o et appuyez sur ENTRÉE. Pour annuler la suppression, appuyez sur N puis appuyez sur ENTRÉE.

> [!NOTE]
> Avant d’utiliser des caractères génériques avec la **del** de commande, utilisez les mêmes caractères génériques avec la **dir** commande pour répertorier tous les fichiers qui seront supprimés.
-   Le **del** commande, avec des paramètres différents, est disponible à partir de la Console de récupération.

## <a name="BKMK_examples"></a>Exemples

Pour supprimer tous les fichiers dans un dossier nommé Test sur le lecteur C, tapez une des opérations suivantes :
```
del c:\test
del c:\test\*.*
```
Pour supprimer tous les fichiers portant l’extension de nom de fichier .bat à partir du répertoire actuel, tapez :
```
del *.bak
```
Pour supprimer tous les fichiers en lecture seule dans le répertoire actif, tapez :
```
del /a:r *.*
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)