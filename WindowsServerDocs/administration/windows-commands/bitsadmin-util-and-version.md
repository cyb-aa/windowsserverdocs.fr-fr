---
title: bitsadmin util and version
description: Rubrique de référence pour la commande Bitsadmin util et version, qui affiche la version du service BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 98f17328-dfbd-4cbb-93c1-b8d424bc3f0a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 20c3db6e6fcd5ef3d00287f36c9f9624ab5224dd
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82707593"
---
# <a name="bitsadmin-util-and-version"></a>bitsadmin util and version

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

Pour afficher la version du service BITS.

```
bitsadmin /util /version
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin util](bitsadmin-util.md)

- [commande Bitsadmin](bitsadmin.md)
