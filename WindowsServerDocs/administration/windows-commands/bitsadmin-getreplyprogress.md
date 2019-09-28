---
title: bitsadmin getreplyprogress
description: La rubrique commandes Windows pour **Bitsadmin getreplyprogress** -récupère la taille et la progression de la réponse du serveur.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: c791fe98271b497e5ecf48338ab3bbb0cc50de98
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381238"
---
# <a name="bitsadmin-getreplyprogress"></a>bitsadmin getreplyprogress

Récupère la taille et la progression de la réponse du serveur.

**BITS 1,2 et versions antérieures**: Non pris en charge.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetReplyProgress <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|

## <a name="remarks"></a>Notes

Valide uniquement pour les travaux de chargement-réponse.

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant récupère la progression de la réponse pour la tâche nommée *myDownloadJob*.
```
C:\>bitsadmin /GetReplyProgress myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)