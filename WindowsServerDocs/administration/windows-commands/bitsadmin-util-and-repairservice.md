---
title: Bitsadmin util et REPAIRSERVICE
description: La rubrique commandes Windows pour **Bitsadmin util et REPAIRSERVICE**, qui corrige les problèmes connus dans les différentes versions du service bits.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ac7baeb-4340-4186-bfcb-66478195378d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 164a402e7cbfc0a9223a97f4246eac84f0797aed
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122515"
---
# <a name="bitsadmin-util-and-repairservice"></a>Bitsadmin util et REPAIRSERVICE

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
| /Force | Ce paramètre est facultatif. Supprime et crée à nouveau le service.|

> [!NOTE]
> Si le service BITS crée à nouveau le service, la chaîne de description du service peut être définie sur l’anglais même dans un système localisé.

## <a name="examples"></a>Exemples

L’exemple suivant répare la configuration du service BITS.

```
C:\>bitsadmin /util /repairservice
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)