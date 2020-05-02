---
ms.assetid: 6c6ff819-f349-4aea-b0be-1f637f631736
title: Fsutil Wim
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 12a9965515ef26e0cbccb2d20d25f66b54b23b8a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720061"
---
# <a name="fsutil-wim"></a>Fsutil Wim
> S'applique à : Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows 10

Fournit des fonctions permettant de détecter et de gérer les fichiers d’image Windows (WIM).

## <a name="syntax"></a>Syntaxe

```
fsutil wim [enumfiles] <drive name> <data source>
fsutil wim [enumwims] <drive name>
fsutil wim [queryfile] <filename>
fsutil wim [removewim] <drive name> <data source>
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------------|---------------|
|EnumFiles|Énumère les fichiers WIM sauvegardés.|
|\<nom du lecteur>|Spécifie le nom du lecteur.|
|\<> de la source de données|Spécifie la source de données.|
|enumwims|Énumère les fichiers WIM de sauvegarde.|
|queryfile|Interroge si le fichier est sauvegardé par WIM et, le cas échéant, affiche des détails sur le fichier WIM.|
|\<nom de fichier>|Spécifie le nom de fichier.|
|removewim|Supprime un fichier WIM des fichiers de sauvegarde.|




### <a name="examples"></a>Exemples

Pour énumérer les fichiers du lecteur C : à partir de la source de données 0, tapez :

```
fsutil wim enumfiles C: 0
```

Pour énumérer les fichiers WIM de sauvegarde pour le lecteur C :, tapez :

```
fsutil wim enumwims C:
```

Pour voir si un fichier est sauvegardé par WIM, tapez :

```
fsutil wim C:\Windows\Notepad.exe
```

Pour supprimer le fichier WIM des fichiers de sauvegarde pour le volume C : et la source de données 2, tapez :

```
fsutil wim removewims C: 2
```

### <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

[Fsutil](Fsutil.md)