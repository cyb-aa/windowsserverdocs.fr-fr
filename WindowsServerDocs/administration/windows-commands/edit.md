---
title: modifier
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4e0ff2e8-3518-47c1-8c69-5e93f895fa0e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c77941d39118554b6b59e436e63d67a4a1f7c8cb
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720860"
---
# <a name="edit"></a>modifier



Démarre l’éditeur MS-DOS, qui crée et modifie les fichiers texte ASCII.



## <a name="syntax"></a>Syntaxe

```
edit [/b] [/h] [/r] [/s] [/<NNN>] [[<Drive>:][<Path>]<FileName> [<FileName2> [...]]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[\<Lecteur> :] [<Path>]<FileName> [<FileName2> [...]]|Spécifie l’emplacement et le nom d’un ou plusieurs fichiers texte ASCII. Si le fichier n’existe pas, l’éditeur MS-DOS le crée. Si le fichier existe, l’éditeur MS-DOS l’ouvre et affiche son contenu à l’écran. Le *nom de fichier* peut contenir des caractères génériques (**&#42;** et **?**). Séparez les noms de fichiers multiples par des espaces.|
|/b|Force le mode monochrome, de sorte que l’éditeur MS-DOS s’affiche en noir et blanc.|
|/h|Affiche le nombre maximal de lignes possibles pour l’analyse en cours.|
|/r|Charge le ou les fichiers en mode lecture seule.|
|/s|Force l’utilisation de noms de fichiers courts.|
|\<NNN>|Charge le ou les fichiers binaires, en encapsulant les lignes dans *nnn* caractères de largeur.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes 

-   Pour obtenir de l’aide supplémentaire, ouvrez l’éditeur MS-DOS, puis appuyez sur la touche F1.
-   Certaines analyses ne prennent pas en charge l’affichage des touches de raccourci par défaut. Si votre moniteur n’affiche pas de touches de raccourci, utilisez **/b**.

## <a name="examples"></a>Exemples

Pour ouvrir l’éditeur MS-DOS, tapez :
```
edit
```
Pour créer et modifier un fichier nommé newtextfile. txt dans le répertoire actif, tapez :
```
edit newtextfile.txt
```