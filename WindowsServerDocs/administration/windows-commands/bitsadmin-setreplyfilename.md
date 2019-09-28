---
title: bitsadmin setreplyfilename
description: Rubrique commandes Windows pour **Bitsadmin setreplyfilename** -spécifiez le chemin d’accès du fichier qui contient la réponse du serveur.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: a490b5bc565549d096b6f43f42758f77570fcb26
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380422"
---
# <a name="bitsadmin-setreplyfilename"></a>bitsadmin setreplyfilename

Spécifiez le chemin d’accès du fichier qui contient la réponse du serveur.

**BITS 1,2 et versions antérieures**: Non pris en charge.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetReplyFileName <Job> <Path>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|
|Path|Emplacement où la réponse du serveur doit être placée|

## <a name="remarks"></a>Notes

Valide uniquement pour les travaux de chargement-réponse.

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant définit le nom de fichier de réponse pathfor la tâche nommée *myDownloadJob*.
```
C:\>bitsadmin /SetReplyFileName myDownloadJob c:\reply
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)