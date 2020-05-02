---
title: bitsadmin setdisplayname
description: Rubrique de référence pour la commande Bitsadmin SetDisplayName, qui définit le nom complet du travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 13706c53-fb5f-4879-b5ca-82531361d6e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 382cb2f20f0374c2d2787c4c3d88670b4f7260cd
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719386"
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
| travail | Nom complet ou GUID du travail. |
| display_name | Texte utilisé comme nom affiché pour le travail spécifique. |

## <a name="examples"></a>Exemples

Pour définir le nom complet du travail sur *myDownloadJob*:

```
bitsadmin /setdisplayname myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
