---
title: bitsadmin addfileset
description: Rubrique relative aux commandes Windows pour **Bitsadmin ADDFILESET** -ajoute un ou plusieurs fichiers au travail spécifié.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 75466994-262f-4724-b14d-f813c5397675
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 464f2da151d5a7bfffde286e52d9158560d48dcc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381992"
---
# <a name="bitsadmin-addfileset"></a>bitsadmin addfileset

Ajoute un ou plusieurs fichiers au travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /addfileset <Job> <TextFile>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|
|TextFile|Un fichier texte, chaque ligne contenant un nom de fichier distant et un nom de fichier local.</br>Remarque : Les noms sont délimités par des espaces. Les lignes qui commencent par un caractère # sont traitées comme un commentaire.|

## <a name="BKMK_examples"></a>Illustre

```
C:\>bitsadmin /addfileset files.txt
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)