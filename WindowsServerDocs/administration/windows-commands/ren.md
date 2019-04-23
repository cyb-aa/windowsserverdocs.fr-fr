---
title: ren
description: Découvrez comment renommer un fichier ou répertoire avec la commande ren.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 60398e12-a05d-4524-a73a-0a925943e21d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: c239dd1f1f8d03d761e45505634da10f19ed08cf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842020"
---
# <a name="ren"></a>ren

Renomme les fichiers ou répertoires. Cette commande est identique à la **renommer** commande.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
ren [<Drive>:][<Path>]<FileName1> <FileName2>
rename [<Drive>:][<Path>]<FileName1> <FileName2>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[\<Drive>:][\<Path>]\<FileName1>|Spécifie l’emplacement et le nom du fichier ou un ensemble de fichiers que vous souhaitez renommer. *Nom_fichier1* peut inclure des caractères génériques (**&#42;** et **?**).|
|\<FileName2>|Spécifie le nouveau nom pour le fichier. Vous pouvez utiliser des caractères génériques pour spécifier de nouveaux noms pour plusieurs fichiers.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Vous ne pouvez pas spécifier un nouveau lecteur ou chemin d’accès lorsque vous renommez des fichiers.
-   Vous ne pouvez pas utiliser le **ren** commande pour renommer des fichiers sur disques ou pour déplacer les fichiers vers un autre répertoire.
-   Vous pouvez utiliser des caractères génériques (**&#42;** et **?**) dans le *FileName* paramètre. Les caractères qui sont représentés par des caractères génériques dans *Nom_fichier2* sera identique pour les caractères correspondants dans *Nom_fichier1*.
-   *Nom_fichier2* doit être un nom de fichier unique. Si *Nom_fichier2* correspond à un nom de fichier existant, **ren** affiche le message suivant :  
    ```
    Duplicate file name or file not found
    ```

## <a name="BKMK_examples"></a>Exemples

Pour modifier toutes les extensions de nom de fichier .txt dans le répertoire actif aux extensions .doc, tapez :
```
ren *.txt *.doc 
```
Pour modifier le nom d’un répertoire à partir de Chap10 Chap10, tapez :
```
ren chap10 part10 
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)