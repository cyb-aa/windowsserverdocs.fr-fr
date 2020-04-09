---
title: réinitialiser
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: afbdab44-199c-4e11-884f-e96804965c21
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5c27ddd93d06670a30f797bd58dd396a9e7ce70a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80835782"
---
# <a name="reset"></a>réinitialiser



Rétablit l’État par défaut de DiskShadow. exe. La **réinitialisation** est particulièrement utile pour séparer les opérations de cliché instantané composé, telles que la **création**, l' **importation**, la **sauvegarde**ou la **restauration**.

## <a name="syntax"></a>Syntaxe

```
reset
```

## <a name="remarks"></a>Notes

-   Lorsque vous utilisez la commande de **réinitialisation** , vous perdez l’État à partir de commandes telles que **Add**, **Set**, **Load**ou **writer**. La **réinitialisation** libère également les interfaces IVssBackupComponent et perd les clichés instantanés non persistants.

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)