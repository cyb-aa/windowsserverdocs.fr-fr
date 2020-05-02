---
title: reset
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: afbdab44-199c-4e11-884f-e96804965c21
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0701324ad1ee94cc645c7519d81fef7357b6a34a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722354"
---
# <a name="reset"></a>reset



Rétablit l’État par défaut de DiskShadow. exe. La **réinitialisation** est particulièrement utile pour séparer les opérations de cliché instantané composé, telles que la **création**, l' **importation**, la **sauvegarde**ou la **restauration**.

## <a name="syntax"></a>Syntaxe

```
reset
```

## <a name="remarks"></a>Notes

-   Lorsque vous utilisez la commande de **réinitialisation** , vous perdez l’État à partir de commandes telles que **Add**, **Set**, **Load**ou **writer**. La **réinitialisation** libère également les interfaces IVssBackupComponent et perd les clichés instantanés non persistants.

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)