---
title: bitsadmin setdescription
description: Rubrique de référence pour la commande Bitsadmin SetDescription, qui définit la description du travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1e46a5dd-4637-4a2e-b88f-d3f85b177db8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dc76da7cbe348461a79984b8061767711e090da7
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719310"
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
| travail | Nom complet ou GUID du travail. |
| description | Texte utilisé pour décrire le travail. |

## <a name="examples"></a>Exemples

Pour récupérer la description de la tâche nommée *myDownloadJob*:

```
bitsadmin /setdescription myDownloadJob music_downloads
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
