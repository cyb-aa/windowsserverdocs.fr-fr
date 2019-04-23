---
title: bitsadmin getproxybypasslist
description: Rubrique de commandes de Windows pour **bitsadmin getproxybypasslist** -récupère la liste de contournement proxy pour le travail spécifié.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 50959be3-7014-4bc9-9a7b-68f1ff94a94a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 020b8fc0019eb103a0e469258be8705b80dd45de
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854110"
---
# <a name="bitsadmin-getproxybypasslist"></a>bitsadmin getproxybypasslist

Récupère la liste de contournement proxy pour le travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetProxyBypassList <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|

## <a name="remarks"></a>Notes

La liste de contournement contient les noms d’hôtes ou adresses IP ou les deux, qui ne doivent ne pas être acheminé via un proxy. La liste peut contenir «\<local > » pour faire référence à tous les serveurs sur le même réseau local. La liste peut être point-virgule ou délimitées par des espaces.

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant récupère la liste de contournement proxy pour le travail nommé *myDownloadJob*.
```
C:\>bitsadmin /GetProxyBypassList myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)