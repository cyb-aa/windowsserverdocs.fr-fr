---
title: ren
description: Découvrez comment renommer un fichier ou un répertoire à l’aide de la commande ren.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 2ba3f6a13dc03c0b6a5561be9f0f692546a25149
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384580"
---
# <a name="ren"></a>ren

Renomme des fichiers ou des répertoires. Cette commande est identique à la commande **Rename** .

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
ren [<Drive>:][<Path>]<FileName1> <FileName2>
rename [<Drive>:][<Path>]<FileName1> <FileName2>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[\<> de lecteur :] [\<Path >]\<Nomfichier1 >|Spécifie l’emplacement et le nom du fichier ou de l’ensemble de fichiers que vous souhaitez renommer. *NomFichier1* peut inclure des caractères génériques **&#42;** (et **?** ).|
|\<Nom_de_fichier2 >|Spécifie le nouveau nom du fichier. Vous pouvez utiliser des caractères génériques pour spécifier de nouveaux noms pour plusieurs fichiers.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

- Vous ne pouvez pas spécifier un nouveau lecteur ou chemin d’accès lorsque vous renommez des fichiers.
- Vous ne pouvez pas utiliser la commande **ren** pour renommer des fichiers sur plusieurs lecteurs ou pour déplacer des fichiers vers un autre répertoire.
- Vous pouvez utiliser des caractères génériques **&#42;** (et **?** ) dans les deux paramètres de *nom de fichier* . Les caractères qui sont représentés par des caractères génériques dans *NomFichier2* seront identiques aux caractères correspondants dans *NomFichier1*.
- *NomFichier2* doit être un nom de fichier unique. Si *NomFichier2* correspond à un nom de fichier existant, **ren** affiche le message suivant :  
  ```
  Duplicate file name or file not found
  ```

## <a name="BKMK_examples"></a>Illustre

Pour modifier toutes les extensions de nom de fichier. txt du répertoire actif en extensions. doc, tapez :
```
ren *.txt *.doc 
```
Pour modifier le nom d’un répertoire de Chap10 en part10, tapez :
```
ren chap10 part10 
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)