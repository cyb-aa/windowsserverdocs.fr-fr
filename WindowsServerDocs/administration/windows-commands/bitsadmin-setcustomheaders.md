---
title: Bitsadmin setcustomheaders
description: Rubrique de commandes de Windows pour **bitsadmin setcustomheaders** -ajouter un en-tête HTTP personnalisé à une demande GET.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6d90ac2d23b852ae0c2114e7cd5a9c9e6382ce8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853850"
---
# <a name="bitsadmin-setcustomheaders"></a>Bitsadmin setcustomheaders



Ajouter un en-tête HTTP personnalisé à une demande GET.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetCustomHeaders <Job> <Header1> <Header2> <. . .>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|
|En-tête2 Header1. . .|Les en-têtes personnalisés pour le travail|

## <a name="remarks"></a>Notes

-   Ce commutateur est utilisé pour ajouter un en-tête HTTP personnalisé à une demande GET envoyée à un serveur HTTP.

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant ajoute un en-tête HTTP personnalisé pour le travail nommé *myDownloadJob*.
```
C:\>bitsadmin / SetCustomHeaders myDownloadJob "Accept-encoding:deflate/gzip"
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)