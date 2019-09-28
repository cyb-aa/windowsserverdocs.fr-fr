---
title: Bitsadmin util et REPAIRSERVICE
description: Rubrique relative aux commandes Windows pour **Bitsadmin util et REPAIRSERVICE** -Command permettant de résoudre les problèmes connus liés aux différentes versions du service bits.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 0ab06ac9c784cfa438eb285c28f0e661cf4b8302
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380280"
---
# <a name="bitsadmin-util-and-repairservice"></a>Bitsadmin util et REPAIRSERVICE

Si le service BITS ne parvient pas à démarrer, utilisez ce commutateur pour résoudre les problèmes connus liés aux différentes versions de BITS.

**BITSAdmin 1,5 et versions antérieures :**  Not pris en charge.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /Util /RepairService [/Force]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Force|Facultatif : supprime et recrée le service.|

## <a name="remarks"></a>Notes

Ce commutateur résout les erreurs liées à une configuration de service incorrecte et aux dépendances sur les services Windows (tels que LANManworkstation) et le répertoire réseau. Ce commutateur génère une sortie qui indique si les problèmes ont été résolus.

> [!NOTE]
> Si BITS recrée le service, la chaîne de description du service peut être définie sur anglais dans un système localisé.

> [!IMPORTANT]
> Cette commande n’est pas prise en charge sur Windows Vista.

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant répare la configuration du service BITS.
```
C:\>bitsadmin /Util /RepairService
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)