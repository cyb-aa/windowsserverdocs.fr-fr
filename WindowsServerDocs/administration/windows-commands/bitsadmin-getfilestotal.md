---
title: bitsadmin getfilestotal
description: Rubrique de commandes de Windows pour **bitsadmin getfilestotal** -récupère le nombre de fichiers dans le travail spécifié.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 21240cfa1f0fa1f8e8fc3d2acf83f5df80812816
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857460"
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
|Tâche|Nom d’affichage ou le GUID du travail|

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant récupère le nombre de fichiers inclus dans la tâche nommée *myDownloadJob*.
```
C:\>bitsadmin /GetFilesTotal myDownloadJob
```

##

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md) Voir aussi