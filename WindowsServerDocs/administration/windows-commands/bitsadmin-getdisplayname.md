---
title: bitsadmin getdisplayname
description: Rubrique de commandes de Windows pour **bitsadmin getdisplayname** -récupère le nom complet de la tâche spécifiée.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e5c0e76c-4cc6-42d8-ac30-30bf3dc11b9b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c1ef16f54b7b825e4293a3870d8181985b83843b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857600"
---
# <a name="bitsadmin-getdisplayname"></a>bitsadmin getdisplayname



Récupère le nom complet de la tâche spécifiée.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetDisplayName <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant récupère le nom d’affichage pour le travail nommé *myDownloadJob*.
```
C:\>bitsadmin /GetDisplayName myDownloadJob
```
Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)