---
title: bitsadmin setdisplayname
description: La rubrique commandes Windows pour **Bitsadmin SetDisplayName**, qui définit le nom complet du travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 13706c53-fb5f-4879-b5ca-82531361d6e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0b1086903dd130392800f325c451bb4750fbf8fa
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123003"
---
# <a name="bitsadmin-setdisplayname"></a>bitsadmin setdisplayname

Définit le nom d’affichage du travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /setdisplayname <job> <display_name>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| le travail | Nom complet ou GUID du travail. |
| display_name | Texte utilisé comme nom affiché pour le travail spécifique. |

## <a name="examples"></a>Exemples

L’exemple suivant définit le nom complet du travail sur *myDownloadJob*.

```
C:\>bitsadmin /setdisplayname myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)