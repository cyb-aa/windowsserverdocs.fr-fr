---
title: Bitsadmin util et version
description: La rubrique commandes Windows pour **Bitsadmin util et version** -affiche la version du service bits.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 495ef17bbf6f39f20f6729b64de4b4bec0f9a3c2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380201"
---
# <a name="bitsadmin-util-and-version"></a>Bitsadmin util et version

Affiche la version du service BITS (par exemple, 2,0).

**BITSAdmin 1,5 et versions antérieures**: Non pris en charge.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /Util /Version [/Verbose]
```

## <a name="remarks"></a>Notes

Le commutateur **Verbose** effectue les opérations suivantes :
-   Affiche la version de fichier pour chaque DLL liée à BITS
-   Vérifie que le service BITS peut être démarré
-   Affiche les valeurs de stratégie de groupe BITS (Windows Vista uniquement)

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant illustre la version du service BITS.
```
C:\>bitsadmin /Util /Version
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)