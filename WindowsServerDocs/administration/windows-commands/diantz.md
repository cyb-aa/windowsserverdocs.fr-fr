---
title: diantz
description: Rubrique de référence pour la commande diantz, qui empaquette les fichiers existants dans un fichier cabinet (. cab).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 218ed5d7-1203-4d68-ad9b-65cdd022d54f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e45c0c4f71bc7faf6d5de0fa198ac872f6ff2597
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82992501"
---
# <a name="diantz"></a>diantz

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Empaquetez les fichiers existants dans un fichier cabinet (. cab). Cette commande effectue les mêmes actions que la [commande MAKECAB](makecab.md)mise à jour.

## <a name="syntax"></a>Syntaxe

```
diantz [/v[n]] [/d var=<value> ...] [/l <dir>] <source> [<destination>]
diantz [/v[<n>]] [/d var=<value> ...] /f <directives_file> [...]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| `<source>` | Fichier à compresser. |
| `<destination>` | Nom de fichier pour fournir un fichier compressé. En cas d’omission, le dernier caractère du nom du fichier source est remplacé par un trait de soulignement (_) et utilisé comme destination. |
| /f `<directives_file>` | Un fichier avec des directives **diantz** (peut être répété). |
| /d var =`<value>` | Définit la variable avec la valeur spécifiée. |
| /l`<dir>` | Emplacement de destination de la destination (le répertoire par défaut est le répertoire actif). |
| /v [`<n>`] | Définissez le niveau de détail du débogage (0 = aucun,..., 3 = complet). |
| /? | Affiche l'aide à l'invite de commandes. |

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [Format du fichier CAB Microsoft](https://docs.microsoft.com/previous-versions/bb417343(v=msdn.10))
