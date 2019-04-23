---
title: Bitsadmin getproxylist - récupère la liste de proxy pour le travail spécifié.
description: Rubrique de commandes de Windows pour **bitsadmin getproxylist** -récupère la liste de proxy pour le travail spécifié.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: e8c3ffb1e425552cda5b14a00287817ace77a90f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840510"
---
# <a name="bitsadmin-getproxylist"></a>bitsadmin getproxylist

Récupère la liste de proxy pour le travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetProxyList <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|

## <a name="remarks"></a>Notes

La liste de proxy est la liste des serveurs proxy à utiliser. La liste est délimitée par des virgules.

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant récupère la liste de proxy pour le travail nommé *myDownloadJob*.
```
C:\>bitsadmin /GetProxyList myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)