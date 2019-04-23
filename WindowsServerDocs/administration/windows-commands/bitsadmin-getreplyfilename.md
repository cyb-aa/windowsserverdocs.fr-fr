---
title: bitsadmin getreplyfilename
description: Rubrique de commandes de Windows pour **bitsadmin getreplyfilename** -Obtient le chemin d’accès du fichier qui contient la réponse du serveur.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 85447184-1732-4816-a365-2e3599551bf8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 46130700e9ac7e2d0076b368712e5dcb3f02ba2f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862150"
---
# <a name="bitsadmin-getreplyfilename"></a>bitsadmin getreplyfilename

Obtient le chemin d’accès du fichier qui contient la réponse du serveur.

**BITS 1.2 et versions antérieures**: Non pris en charge.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetReplyFileName <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|

## <a name="remarks"></a>Notes

Valide uniquement pour les travaux de chargement-réponse.

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant récupère le nom du fichier de réponse pour le travail nommé *myDownloadJob*.
```
C:\>bitsadmin /GetReplyFileName myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)