---
title: bitsadmin setreplyfilename
description: Rubrique de commandes de Windows pour **bitsadmin setreplyfilename** -spécifier le chemin d’accès du fichier qui contient la réponse du serveur.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c26d3342-0533-40b1-a13e-e09678232b25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b86d4137f661e9953d6d397b2fbc890393bbd8a0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852870"
---
# <a name="bitsadmin-setreplyfilename"></a>bitsadmin setreplyfilename

Spécifiez le chemin d’accès du fichier qui contient la réponse du serveur.

**BITS 1.2 et versions antérieures**: Non pris en charge.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetReplyFileName <Job> <Path>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|
|Path|Emplacement pour placer la réponse du serveur|

## <a name="remarks"></a>Notes

Valide uniquement pour les travaux de chargement-réponse.

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant définit la rechercheSi de nom de fichier de réponse de la tâche nommée *myDownloadJob*.
```
C:\>bitsadmin /SetReplyFileName myDownloadJob c:\reply
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)