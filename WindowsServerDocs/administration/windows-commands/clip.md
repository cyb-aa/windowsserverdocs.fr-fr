---
title: clip
description: Rubrique de référence pour la commande clip, qui redirige la sortie de commande de la ligne de commande vers le presse-papiers Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 85322d85-3376-4806-845b-93ac77fe27bf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 61c905e3dcce52f3a3d35adeac55fc5df574f664
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82712788"
---
# <a name="clip"></a>clip

Redirige la sortie de commande de la ligne de commande vers le presse-papiers Windows. Vous pouvez utiliser cette commande pour copier des données directement dans une application qui peut recevoir du texte à partir du presse-papiers. Vous pouvez également coller cette sortie de texte dans d’autres programmes.

## <a name="syntax"></a>Syntaxe

```
<command> | clip
clip < <filename>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| `<command>` | Spécifie une commande dont vous souhaitez envoyer la sortie dans le presse-papiers Windows. |
| `<filename>` | Spécifie un fichier dont vous souhaitez envoyer le contenu dans le presse-papiers Windows. |
| /? | Affiche l'aide à l'invite de commandes. |

## <a name="examples"></a>Exemples

Pour copier la liste de répertoires active dans le presse-papiers Windows, tapez :

```
dir | clip
```

Pour copier la sortie d’un programme appelé *Generic. awk* dans le presse-papiers Windows, tapez :

```
awk -f generic.awk input.txt | clip
```

Pour copier le contenu d’un fichier nommé *Readme. txt* dans le presse-papiers Windows, tapez :

```
clip < readme.txt
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)