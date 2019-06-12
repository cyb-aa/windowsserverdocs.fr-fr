---
title: bitsadmin replaceremoteprefix
description: Rubrique de commandes de Windows pour **bitsadmin replaceremoteprefix** -tous les fichiers dans le travail à distance dont l’URL commence par *OldPrefix* sont modifiés pour utiliser *NewPrefix*.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d0e0abb1-bdb4-4c74-abbc-16c809f5fd81
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 25c0f997ea0b9f97051baa291bdf87c84b6b1cbb
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811299"
---
# <a name="bitsadmin-replaceremoteprefix"></a>bitsadmin replaceremoteprefix

Tous les fichiers dans le travail à distance dont l’URL commence par *OldPrefix* sont modifiés pour utiliser *NewPrefix*.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /ReplaceRemotePrefix <Job> <OldPrefix> <NewPrefix
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|
|OldPrefix|Préfixe d’URL existant|
|NewPrefix|Nouveau préfixe d’URL|

## <a name="examples"></a>Exemples

L’exemple suivant modifie tous les fichiers de travail nommé *myDownloadJob* dont la propriété URL distante commence par *http://stageserver* à *http://prodserver* .

```
C:\>bitsadmin /ReplaceRemotePrefix myDownloadJob http://stageserver http://prodserver
```

## <a name="additional-information"></a>Informations complémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)