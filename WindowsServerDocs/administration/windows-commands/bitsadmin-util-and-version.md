---
title: Bitsadmin util et version
description: La rubrique commandes Windows pour **Bitsadmin util et version**, qui affiche la version du service bits.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 98f17328-dfbd-4cbb-93c1-b8d424bc3f0a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2c2518eb7a8f15d9a592ed9a77dd67a6f8d8afac
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122478"
---
# <a name="bitsadmin-util-and-version"></a>Bitsadmin util et version

Affiche la version du service BITS (par exemple, 2,0).

> [!NOTE]
> Cette commande n’est pas prise en charge par BITS 1,5 et versions antérieures.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /util /version [/verbose]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| /verbose | Utilisez ce commutateur pour afficher la version de fichier pour chaque DLL liée à BITS et pour vérifier si le service BITS peut démarrer.|

## <a name="examples"></a>Exemples

L’exemple suivant illustre la version du service BITS.

```
C:\>bitsadmin /util /version
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)