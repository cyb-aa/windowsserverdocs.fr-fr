---
title: extraire
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 20dab03e-f6e1-4eb8-b8a1-fd6f1d97ee83
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9113c34b61b98fb738bc0aff03193ab73b1abbd7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882310"
---
# <a name="extract"></a>extraire



## <a name="syntax"></a>Syntaxe

```
EXTRACT [/Y] [/A] [/D | /E] [/L dir] cabinet [filename ...]
EXTRACT [/Y] source [newname]
EXTRACT [/Y] /C source destination
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|CAB|Fichier contient deux ou plusieurs fichiers.|
|filename|Nom du fichier à extraire le fichier CAB. Les caractères génériques et plusieurs noms de fichiers (séparés par des espaces) peuvent être utilisés.|
|source|Fichier compressé (fichier CAB avec un seul fichier).|
|NewName|Nouveau nom de fichier pour le fichier extrait. Si ne pas fourni, le nom d’origine est utilisé.|
|/A|Traiter tous les fichiers CAB. Suit la chaîne d’installation commençant dans le premier fichier CAB mentionné.|
|/C|Copier le fichier source vers la destination (pour copier à partir de disques DMF).|
|/D|Afficher le répertoire CAB (utiliser avec le nom de fichier pour éviter l’extraction).|
|/E|Extraire (utiliser au lieu de *.* pour extraire tous les fichiers).|
|/ L dir|Emplacement des fichiers extraits (valeur par défaut est le répertoire actif).|
|/Y|Ne pas demander avant de remplacer un fichier existant.|

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)