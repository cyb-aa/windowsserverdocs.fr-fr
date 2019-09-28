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
ms.openlocfilehash: fc79b70e8dedb9ecad5e8c6e89f51ece3279faa4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376660"
---
# <a name="fsutil-wim"></a>Fsutil Wim
>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10

Fournit des fonctions permettant de détecter et de gérer les fichiers d’image Windows (WIM).

## <a name="syntax"></a>Syntaxe

```
fsutil wim [enumfiles] <drive name> <data source>
fsutil wim [enumwims] <drive name>
fsutil wim [queryfile] <filename>
fsutil wim [removewim] <drive name> <data source>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------------|---------------|
|EnumFiles|Énumère les fichiers WIM sauvegardés.|
|nom de la @no__t 0drive >|Spécifie le nom du lecteur.|
|> Source @no__t 0data|Spécifie la source de données.|
|enumwims|Énumère les fichiers WIM de sauvegarde.|
|queryfile|Interroge si le fichier est sauvegardé par WIM et, le cas échéant, affiche des détails sur le fichier WIM.|
|@no__t 0filename >|Spécifie le nom de fichier.|
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
[Clé de syntaxe de ligne de commande](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)