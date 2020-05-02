---
title: bitsadmin util and repairservice
description: Rubrique de référence pour la commande Bitsadmin util et REPAIRSERVICE, qui résout les problèmes connus dans les différentes versions du service BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ac7baeb-4340-4186-bfcb-66478195378d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0104a3f2ace972821151bf5083f9b0795e427ff1
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82707652"
---
# <a name="bitsadmin-util-and-repairservice"></a>bitsadmin util and repairservice

Si le service BITS ne démarre pas, ce commutateur tente de résoudre les erreurs liées à une configuration de service incorrecte et aux dépendances sur les services Windows (tels que LANManworkstation) et le répertoire réseau. Ce commutateur génère également une sortie qui indique si les problèmes qui ont été résolus.

> [!NOTE]
> Cette commande n’est pas prise en charge par BITS 1,5 et versions antérieures.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /util /repairservice [/force]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| /Force | facultatif. Supprime et crée à nouveau le service.|

> [!NOTE]
> Si le service BITS crée à nouveau le service, la chaîne de description du service peut être définie sur l’anglais même dans un système localisé.

## <a name="examples"></a>Exemples

Pour réparer la configuration du service BITS :

```
bitsadmin /util /repairservice
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin util](bitsadmin-util.md)

- [commande Bitsadmin](bitsadmin.md)
