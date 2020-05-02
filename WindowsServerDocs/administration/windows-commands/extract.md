---
title: extract
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 20dab03e-f6e1-4eb8-b8a1-fd6f1d97ee83
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1cca89a356530e49fbf2b0610ff3ced1c5733847
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725653"
---
# <a name="extract"></a>extract



## <a name="syntax"></a>Syntaxe

```
EXTRACT [/Y] [/A] [/D | /E] [/L dir] cabinet [filename ...]
EXTRACT [/Y] source [newname]
EXTRACT [/Y] /C source destination
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|CAB|Le fichier contient deux fichiers ou plus.|
|filename|Nom du fichier à extraire de l’armoire. Les caractères génériques et les noms de fichiers multiples (séparés par des espaces) peuvent être utilisés.|
|source|Fichier compressé (une armoire avec un seul fichier).|
|newname|Nouveau nom de fichier pour fournir le fichier extrait. S’il n’est pas fourni, le nom d’origine est utilisé.|
|/A|Traiter toutes les armoires. Suit la chaîne cab à partir du premier fichier CAB mentionné.|
|/C|Copiez le fichier source dans la destination (pour effectuer une copie à partir de disques DMF).|
|/D|Affichez le répertoire CAB (utilisez avec un nom de fichier pour éviter l’extraction).|
|/E|Extraire (utilisez à la place de *.* pour extraire tous les fichiers).|
|/L Rép|Emplacement dans lequel placer les fichiers extraits (le répertoire par défaut est le répertoire actif).|
|/Y|Ne pas demander confirmation avant de remplacer un fichier existant.|

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)