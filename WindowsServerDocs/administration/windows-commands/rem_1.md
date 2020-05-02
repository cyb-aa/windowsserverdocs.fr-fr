---
title: rem
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7245aedb-ba6f-49bb-9f77-848c4853c68f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4362eb37ff415d747962b86f66afe0283220b02b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722448"
---
# <a name="rem"></a>rem



Fournit un moyen d’ajouter des commentaires à un script.

## <a name="syntax"></a>Syntaxe

```
rem
```

## <a name="examples"></a>Exemples

Dans cet exemple de script, **REM** est utilisé pour fournir un commentaire sur ce que fait le script :
```
rem The commands in this script set up 3 drives.
rem The first drive is a primary partition and is
rem assigned the letter D. The second and third drives
rem are logical partitions, and are assigned letters
rem E and F.
create partition primary size=2048
assign d:
create partition extended
create partition logical size=2048
assign e:
create partition logical
assign f:
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

