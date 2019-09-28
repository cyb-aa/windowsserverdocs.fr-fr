---
title: 'Bitsadmin getproxylist : récupère la liste de proxy pour le travail spécifié.'
description: La rubrique commandes Windows pour **Bitsadmin getproxylist** -récupère la liste de proxy pour le travail spécifié.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eebfa727-d8f1-4ae3-9382-6d8ffe8c3df3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6f176d268c816725b183da0a948afcb25272b2fb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381313"
---
# <a name="bitsadmin-getproxylist"></a>bitsadmin getproxylist

Récupère la liste de proxys pour le travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetProxyList <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|

## <a name="remarks"></a>Notes

La liste de proxys est la liste des serveurs proxy à utiliser. La liste est délimitée par des virgules.

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant récupère la liste de proxy pour la tâche nommée *myDownloadJob*.
```
C:\>bitsadmin /GetProxyList myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)