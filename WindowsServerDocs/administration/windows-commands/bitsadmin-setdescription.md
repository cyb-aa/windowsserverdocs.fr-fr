---
title: bitsadmin setdescription
description: La rubrique commandes Windows pour **Bitsadmin SetDescription**, qui définit la description du travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1e46a5dd-4637-4a2e-b88f-d3f85b177db8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0b62e6b030c23c475418cd6f2c63f04edba1acff
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123016"
---
# <a name="bitsadmin-setdescription"></a>bitsadmin setdescription

Définit la description du travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /setdescription <job> <description>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| le travail | Nom complet ou GUID du travail. |
| description | Texte utilisé pour décrire le travail. |

## <a name="examples"></a>Exemples

L’exemple suivant récupère la description de la tâche nommée *myDownloadJob*.

```
C:\>bitsadmin /setdescription myDownloadJob music_downloads
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)