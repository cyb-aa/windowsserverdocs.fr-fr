---
title: ren
description: Découvrez comment renommer un fichier ou un répertoire à l’aide de la commande ren.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 60398e12-a05d-4524-a73a-0a925943e21d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 21ce37795af3080245633938e96f891f7a485d53
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722418"
---
# <a name="ren"></a>ren

Renomme des fichiers ou des répertoires. Cette commande est identique à la commande **Rename** .



## <a name="syntax"></a>Syntaxe

```
ren [<Drive>:][<Path>]<FileName1> <FileName2>
rename [<Drive>:][<Path>]<FileName1> <FileName2>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[\<Lecteur> :] [\<Chemin>] \<> NomFichier1|Spécifie l’emplacement et le nom du fichier ou de l’ensemble de fichiers que vous souhaitez renommer. *NomFichier1* peut inclure des caractères génériques (**&#42;** et **?**).|
|\<Nomfichier2>|Spécifie le nouveau nom du fichier. Vous pouvez utiliser des caractères génériques pour spécifier de nouveaux noms pour plusieurs fichiers.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes 

- Vous ne pouvez pas spécifier un nouveau lecteur ou chemin d’accès lorsque vous renommez des fichiers.
- Vous ne pouvez pas utiliser la commande **ren** pour renommer des fichiers sur plusieurs lecteurs ou pour déplacer des fichiers vers un autre répertoire.
- Vous pouvez utiliser des caractères génériques (**&#42;** et **?**) dans les deux paramètres de *nom de fichier* . Les caractères qui sont représentés par des caractères génériques dans *NomFichier2* seront identiques aux caractères correspondants dans *NomFichier1*.
- *NomFichier2* doit être un nom de fichier unique. Si *NomFichier2* correspond à un nom de fichier existant, **ren** affiche le message suivant :  
  ```
  Duplicate file name or file not found
  ```

## <a name="examples"></a><a name="BKMK_examples"></a>Illustre

Pour modifier toutes les extensions de nom de fichier. txt du répertoire actif en extensions. doc, tapez :
```
ren *.txt *.doc 
```
Pour modifier le nom d’un répertoire de Chap10 en part10, tapez :
```
ren chap10 part10 
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)