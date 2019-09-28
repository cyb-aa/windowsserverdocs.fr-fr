---
title: Bitsadmin setcustomheaders
description: Rubrique relative aux commandes Windows pour **Bitsadmin setcustomheaders** -ajouter un en-tête HTTP personnalisé à une requête d’extraction.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ed926410-80d0-46ed-9a90-f752c164bb9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 45e3a5178df69b84618966ca0fcd9cc1e6d0e449
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380641"
---
# <a name="bitsadmin-setcustomheaders"></a>Bitsadmin setcustomheaders



Ajoutez un en-tête HTTP personnalisé à une requête d’extraction.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetCustomHeaders <Job> <Header1> <Header2> <. . .>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|
|Header1 Header2. . .|En-têtes personnalisés pour le travail|

## <a name="remarks"></a>Notes

-   Ce commutateur est utilisé pour ajouter un en-tête HTTP personnalisé à une demande d’extraction envoyée à un serveur HTTP.

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant ajoute un en-tête HTTP personnalisé pour le travail nommé *myDownloadJob*.
```
C:\>bitsadmin / SetCustomHeaders myDownloadJob "Accept-encoding:deflate/gzip"
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)