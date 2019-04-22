---
title: Bitsadmin getcustomheaders
description: Rubrique de commandes de Windows pour **bitsadmin getcustomheaders** -récupère les en-têtes HTTP personnalisés à partir de la tâche.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1f0d38d3-e865-4474-81e8-773d65c3d1cc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2f5959541f0e3190e26bbb298a9cd7c63ab32cae
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812090"
---
# <a name="bitsadmin-getcustomheaders"></a>Bitsadmin getcustomheaders



Récupère les en-têtes HTTP personnalisés à partir de la tâche.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetCustomHeaders <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant obtient les en-têtes personnalisés pour le travail nommé *myDownloadJob*.
```
C:\>bitsadmin /GetCustomHeaders myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)