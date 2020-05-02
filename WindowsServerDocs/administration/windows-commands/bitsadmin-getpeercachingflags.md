---
title: bitsadmin getpeercachingflags
description: Rubrique de référence pour la commande Bitsadmin getpeercachingflags, qui récupère des indicateurs qui déterminent si les fichiers du travail peuvent être mis en cache et desservis à des homologues, et si BITS peut télécharger du contenu pour le travail à partir d’homologues.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3c3c9f28-4c04-4c49-a23a-dee5bbcc8981
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f30eead56958af3cd0fb0d91f6ee2bf9f79fdc4e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717689"
---
# <a name="bitsadmin-getpeercachingflags"></a>bitsadmin getpeercachingflags

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Récupère les indicateurs qui déterminent si les fichiers du travail peuvent être mis en cache et desservis aux homologues, et si BITS peut télécharger du contenu pour le travail à partir de pairs.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /getpeercachingflags <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| travail | Nom complet ou GUID du travail. |

## <a name="examples"></a>Exemples

Pour récupérer les indicateurs du travail nommé *myDownloadJob*:

```
bitsadmin /getpeercachingflags myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
