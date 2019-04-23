---
title: bitsadmin setdisplayname
description: Rubrique de commandes de Windows pour **bitsadmin setdisplayname** -définit le nom complet de la tâche spécifiée.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 13706c53-fb5f-4879-b5ca-82531361d6e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d50cd2785e42b554cee340abc97fe4e4b53adcfc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843670"
---
# <a name="bitsadmin-setdisplayname"></a>bitsadmin setdisplayname



Définit le nom complet de la tâche spécifiée.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetDisplayName <Job> <DisplayName>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|
|DisplayName|Texte utilisé pour le nom complet de la tâche spécifiée.|

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant définit le nom d’affichage pour le travail nommé *myDownloadJob* à *myDownloadJob2*.
```
C:\>bitsadmin /SetDisplayName myDownloadJob "Download Music Job"
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)