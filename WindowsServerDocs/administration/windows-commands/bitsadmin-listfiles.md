---
title: bitsadmin listfiles
description: Rubrique de commandes de Windows pour **bitsadmin listfiles** -répertorie les fichiers dans le travail spécifié.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ad0d1eaa-3bd8-45e5-8f72-4da7366f0d59
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4f0f86a7e176c601c51dbdf403baf51f70e53dc4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852970"
---
# <a name="bitsadmin-listfiles"></a>bitsadmin listfiles



Répertorie les fichiers dans le travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /ListFiles <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant récupère la liste des fichiers pour le travail nommé *myDownloadJob*.
```
C:\>bitsadmin /GetNotifyFlags myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)