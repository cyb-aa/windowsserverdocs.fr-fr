---
title: cache Bitsadmin et deleteURL
description: Rubrique de référence pour le cache Bitsadmin et la commande deleteURL, qui supprime toutes les entrées de cache pour l’URL donnée.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e108b76b-fae9-4c16-bf4c-d74c9f025953
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 075c48e5c8c205cbbf3fe476260ec7909edcc3e6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718446"
---
# <a name="bitsadmin-cache-and-deleteurl"></a>cache Bitsadmin et deleteURL

Supprime toutes les entrées de cache pour l’URL donnée.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /deleteURL URL
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| URL | Uniform Resource Locator qui identifie un fichier distant. |

## <a name="examples"></a>Exemples

Pour supprimer toutes les entrées de `https://www.contoso.com/en/us/default.aspx`cache pour :

```
bitsadmin /deleteURL https://www.contoso.com/en/us/default.aspx 
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande de cache Bitsadmin](bitsadmin-cache.md)
