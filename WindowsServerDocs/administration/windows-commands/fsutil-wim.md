---
ms.assetid: 6c6ff819-f349-4aea-b0be-1f637f631736
title: fsutil wim
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: c9186721ce4d3a549964e420cbc16d4893a1859d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826040"
---
# <a name="fsutil-wim"></a>fsutil wim
>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10

Fournit des fonctions pour détecter et gérer des fichiers reposant sur une Image WIM de Windows.

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
|enumfiles|Énumère les fichiers WIM soutenu.|
|\<nom du lecteur >|Spécifie le nom du lecteur.|
|\<source de données >|Spécifie la source de données.|
|enumwims|Énumère les fichiers WIM de sauvegarde.|
|queryfile|Requêtes si le fichier est soutenu par WIM et le cas échéant, affiche des détails sur le fichier WIM.|
|\<filename>|Spécifie le nom de fichier.|
|removewim|Supprime un fichier WIM à partir de fichiers de sauvegarde.|




### <a name="examples"></a>Exemples

Pour énumérer les fichiers pour le lecteur C: à partir de la source de données 0, tapez :

```
fsutil wim enumfiles C: 0
```

Pour énumérer les fichiers WIM de sauvegarde pour le lecteur C:, tapez :

```
fsutil wim enumwims C:
```

Pour voir si un fichier est sauvegardé par WIM, tapez :

```
fsutil wim C:\Windows\Notepad.exe
```

Pour supprimer le fichier WIM à partir de la sauvegarde des fichiers pour la source de données 2 et le volume C:, tapez :

```
fsutil wim removewims C: 2
```

### <a name="additional-references"></a>Références supplémentaires
[Clé de la syntaxe de ligne de commande](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)