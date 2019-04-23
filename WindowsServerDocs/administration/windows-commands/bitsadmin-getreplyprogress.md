---
title: bitsadmin getreplyprogress
description: Rubrique de commandes de Windows pour **bitsadmin getreplyprogress** -récupère la taille et la progression de la réponse du serveur.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7f7cb0b4-ad95-44fd-a35d-0ddf5fc0b0d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aafecfb5873392ef86e6f7cceb139091b15e3b99
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852930"
---
# <a name="bitsadmin-getreplyprogress"></a>bitsadmin getreplyprogress

Récupère la taille et la progression de la réponse du serveur.

**BITS 1.2 et versions antérieures**: Non pris en charge.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetReplyProgress <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|

## <a name="remarks"></a>Notes

Valide uniquement pour les travaux de chargement-réponse.

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant récupère la progression de la réponse pour le travail nommé *myDownloadJob*.
```
C:\>bitsadmin /GetReplyProgress myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)