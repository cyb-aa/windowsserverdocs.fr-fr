---
title: bitsadmin getproxybypasslist
description: Rubrique de référence pour la commande Bitsadmin getproxybypasslist, qui récupère la liste de contournement du proxy pour le travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 50959be3-7014-4bc9-9a7b-68f1ff94a94a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3b4f37d09521c28d55104975ed754a10e8df011e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717665"
---
# <a name="bitsadmin-getproxybypasslist"></a>bitsadmin getproxybypasslist

Récupère la liste de contournement du proxy pour le travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /getproxybypasslist <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| travail | Nom complet ou GUID du travail. |

### <a name="remarks"></a>Notes 

La liste de contournement contient les noms d’hôte ou les adresses IP, ou les deux, qui ne doivent pas être routés via un proxy. La liste peut contenir `<local>` pour faire référence à tous les serveurs sur le même réseau local. La liste peut être un point-virgule (;) ou délimités par des espaces.

## <a name="examples"></a>Exemples

Pour récupérer la liste de contournement du proxy pour le travail nommé *myDownloadJob*:

```
bitsadmin /getproxybypasslist myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
