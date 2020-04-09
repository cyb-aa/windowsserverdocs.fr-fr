---
title: Bitsadmin util et REPAIRSERVICE
description: La rubrique commandes Windows pour Bitsadmin util et REPAIRSERVICE, qui corrige les problèmes connus dans les différentes versions du service BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ac7baeb-4340-4186-bfcb-66478195378d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aaaa6edab22031dc53d266984bb669634e3bb362
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848892"
---
# <a name="bitsadmin-util-and-repairservice"></a>Bitsadmin util et REPAIRSERVICE

Si le service BITS ne parvient pas à démarrer, utilisez ce commutateur pour résoudre les problèmes connus dans différentes versions de BITS.

**BITSAdmin 1,5 et versions antérieures :**  pas pris en charge.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /Util /RepairService [/Force]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Force|Facultatif : supprime et recrée le service.|

## <a name="remarks"></a>Notes

Ce commutateur résout les erreurs liées à une configuration de service incorrecte et aux dépendances sur les services Windows (tels que LANManworkstation) et le répertoire réseau. Ce commutateur génère une sortie qui indique si les problèmes ont été résolus.

> [!NOTE]
> Si BITS recrée le service, la chaîne de description du service peut être définie sur anglais dans un système localisé.

> [!IMPORTANT]
> Cette commande n’est pas prise en charge sur Windows Vista.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant répare la configuration du service BITS.
```
C:\>bitsadmin /Util /RepairService
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)