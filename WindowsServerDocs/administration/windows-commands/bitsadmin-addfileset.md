---
title: bitsadmin addfileset
description: Rubrique de commandes de Windows pour **bitsadmin addfileset** -ajoute un ou plusieurs fichiers pour le travail spécifié.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: f8f6ff32dfa6042272c68647477d77183ce9cb76
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889440"
---
# <a name="bitsadmin-addfileset"></a>bitsadmin addfileset

Ajoute un ou plusieurs fichiers pour le travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /addfileset <Job> <TextFile>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|
|TextFile|Un fichier texte, chaque ligne contenant un référentiel distant et un nom de fichier local.</br>Remarque: Les noms sont délimitées par des espaces. Les lignes qui commencent par un caractère # sont traitées comme un commentaire.|

## <a name="BKMK_examples"></a>Exemples

```
C:\>bitsadmin /addfileset files.txt
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)