---
title: bitsadmin makecustomheaderswriteonly
description: Rubrique de référence pour la commande Bitsadmin makecustomheaderswriteonly, qui rend les en-têtes HTTP personnalisés d’un travail en écriture seule.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 2aeab7e0ee7797b3e0be7be1156920f3bafc84dc
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717402"
---
# <a name="bitsadmin-makecustomheaderswriteonly"></a>bitsadmin makecustomheaderswriteonly

Rendre les en-têtes HTTP personnalisés d’un travail en écriture seule.

> [!IMPORTANT]
> Cette action ne peut pas être annulée.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /makecustomheaderswriteonly <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| travail | Nom complet ou GUID du travail. |

## <a name="examples"></a>Exemples

Pour créer des en-têtes HTTP personnalisés en écriture seule pour le travail nommé *myDownloadJob*:

```
bitsadmin /makecustomheaderswriteonly myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
