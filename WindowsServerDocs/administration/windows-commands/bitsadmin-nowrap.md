---
title: bitsadmin nowrap
description: Rubrique de commandes de Windows pour **bitsadmin nowrap** -tronque toutes les lignes de sortie texte extension au-delà du périmètre plus à droite de la fenêtre de commande.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 85a47b90-783a-41e4-96f2-81f26ae8ca93
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4130f606a6b1874e1ea31952160de44d6e09c6b6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822920"
---
# <a name="bitsadmin-nowrap"></a>bitsadmin nowrap

Tronque toutes les lignes de sortie texte extension au-delà du périmètre plus à droite de la fenêtre de commande.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /NoWrap
```

## <a name="remarks"></a>Notes

Par défaut, tous les commutateurs, sauf le **moniteur** commutateur, encapsulez la sortie. Spécifiez le **NoWrap** commutateur avant les autres commutateurs.

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant récupère l’état du travail *myDownloadJob* et n’encapsule pas la sortie
```
C:\>bitsadmin /NoWrap /GetState myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)