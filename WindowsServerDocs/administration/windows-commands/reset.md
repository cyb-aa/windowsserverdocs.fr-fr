---
title: Initialisation
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: afbdab44-199c-4e11-884f-e96804965c21
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a8903c300d12a019f8fb4aef6d367131a195d034
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371461"
---
# <a name="reset"></a>Initialisation



Rétablit l’État par défaut de DiskShadow. exe. La **réinitialisation** est particulièrement utile pour séparer les opérations de cliché instantané composé, telles que la **création**, l' **importation**, la **sauvegarde**ou la **restauration**.

## <a name="syntax"></a>Syntaxe

```
reset
```

## <a name="remarks"></a>Notes

-   Lorsque vous utilisez la commande de **réinitialisation** , vous perdez l’État à partir de commandes telles que **Add**, **Set**, **Load**ou **writer**. La **réinitialisation** libère également les interfaces IVssBackupComponent et perd les clichés instantanés non persistants.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)