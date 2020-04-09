---
title: Bitsadmin util et version
description: La rubrique commandes Windows pour Bitsadmin util et version, qui affiche la version du service BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 98f17328-dfbd-4cbb-93c1-b8d424bc3f0a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 087cc1033166ab93e7496caaa7335433cafd6249
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848832"
---
# <a name="bitsadmin-util-and-version"></a>Bitsadmin util et version

Affiche la version du service BITS (par exemple, 2,0).

**BITSAdmin 1,5 et versions antérieures**: non pris en charge.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /Util /Version [/Verbose]
```

## <a name="remarks"></a>Notes

Le commutateur **Verbose** effectue les opérations suivantes :
-   Affiche la version de fichier pour chaque DLL liée à BITS
-   Vérifie que le service BITS peut être démarré
-   Affiche les valeurs de stratégie de groupe BITS (Windows Vista uniquement)

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant illustre la version du service BITS.
```
C:\>bitsadmin /Util /Version
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)