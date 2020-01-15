---
title: bitsadmin getfilestotal
description: La rubrique commandes Windows pour **Bitsadmin getfilestotal** -récupère le nombre de fichiers dans le travail spécifié.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c5de113e-f29c-4cd3-9392-0e300018d516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e5c28d8e970bd7db896073bf8cddb168ffe9deff
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75946845"
---
# <a name="bitsadmin-getfilestotal"></a>bitsadmin getfilestotal



Récupère le nombre de fichiers dans le travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetFilesTotal <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant récupère le nombre de fichiers inclus dans le travail nommé *myDownloadJob*.
```
C:\>bitsadmin /GetFilesTotal myDownloadJob
```

## <a name="see-also"></a>Articles associés

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
