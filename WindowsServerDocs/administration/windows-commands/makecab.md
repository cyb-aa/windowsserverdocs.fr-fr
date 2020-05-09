---
title: makecab
description: Rubrique de référence pour la commande MAKECAB, qui empaquette les fichiers existants dans un fichier cabinet (. cab).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4da95297-c593-427b-9f76-2f389c46cbf4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f3d5e778ee78d812a3ec8c3683b01e0b304a127e
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82992340"
---
# <a name="makecab"></a>makecab

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Empaquetez les fichiers existants dans un fichier cabinet (. cab). Cette commande effectue les mêmes actions que la commande **diantz** .

## <a name="syntax"></a>Syntaxe

```
makecab [/v[n]] [/d var=<value> ...] [/l <dir>] <source> [<destination>]
makecab [/v[<n>]] [/d var=<value> ...] /f <directives_file> [...]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| `<source>` | Fichier à compresser. |
| `<destination>` | Nom de fichier pour fournir un fichier compressé. En cas d’omission, le dernier caractère du nom du fichier source est remplacé par un trait de soulignement (_) et utilisé comme destination. |
| /f `<directives_file>` | Un fichier avec des directives **makecab** (peut être répété). |
| /d var =`<value>` | Définit la variable avec la valeur spécifiée. |
| /l`<dir>` | Emplacement de destination de la destination (le répertoire par défaut est le répertoire actif). |
| /v [`<n>`] | Définissez le niveau de détail du débogage (0 = aucun,..., 3 = complet). |
| /? | Affiche l'aide à l'invite de commandes. |

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [Format du fichier CAB Microsoft](https://docs.microsoft.com/previous-versions/bb417343(v=msdn.10))
