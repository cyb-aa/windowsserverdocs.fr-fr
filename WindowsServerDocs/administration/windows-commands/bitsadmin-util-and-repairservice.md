---
title: REPAIRSERVICE et bitsadmin util
description: Rubrique de commandes de Windows pour **bitsadmin util et repairservice** -commande utilisée pour résoudre des problèmes connus avec diverses versions du service BITS.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2ac7baeb-4340-4186-bfcb-66478195378d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bc5101378a389c865f5753146b711be0d15c6785
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852090"
---
# <a name="bitsadmin-util-and-repairservice"></a>REPAIRSERVICE et bitsadmin util

Si BITS ne démarre pas, utilisez ce commutateur pour résoudre les problèmes connus avec différentes versions de BITS.

**BITSAdmin 1.5 et versions antérieur :** ne pas pris en charge.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /Util /RepairService [/Force]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Force|Facultatif : supprime et recrée le service.|

## <a name="remarks"></a>Notes

Ce commutateur résout la configuration de service associé à incorrect erreurs et les dépendances sur les services Windows (par exemple, LANManworkstation) et au répertoire réseau. Ce commutateur génère une sortie qui indique si les problèmes qui ont été résolus.

> [!NOTE]
> Si BITS recrée le service, la chaîne de description de service peut être définie sur anglais dans un système localisé.

> [!IMPORTANT]
> Cette commande n’est pas pris en charge sur Windows Vista.

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant répare la configuration du Service BITS.
```
C:\>bitsadmin /Util /RepairService
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)