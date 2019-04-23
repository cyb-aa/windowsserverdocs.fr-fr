---
title: version et bitsadmin util
description: Rubrique de commandes de Windows pour **bitsadmin util et version** -affiche la version du service BITS.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 98f17328-dfbd-4cbb-93c1-b8d424bc3f0a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2e768ec5ae43fc17c480b9deede698cca01c6291
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882870"
---
# <a name="bitsadmin-util-and-version"></a>version et bitsadmin util

Affiche la version du service BITS (par exemple, 2.0).

**BITSAdmin 1.5 et versions antérieur**: Non pris en charge.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /Util /Version [/Verbose]
```

## <a name="remarks"></a>Notes

Le **Verbose** commutateur effectue les opérations suivantes :
-   Affiche la version de fichier pour chaque DLL associée de BITS
-   Vérifie que le Service BITS peut être démarré.
-   Affiche les valeurs de stratégie de groupe (Windows Vista uniquement)

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant, la version du Service BITS.
```
C:\>bitsadmin /Util /Version
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)