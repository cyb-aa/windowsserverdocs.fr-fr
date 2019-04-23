---
title: Bitsadmin setvalidationstate
description: Rubrique de commandes de Windows pour **bitsadmin setvalidationstate** -définit l’état de validation du contenu du fichier donné au sein du travail.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e8fc8e8c-171c-4681-8057-6986b018e576
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 647389aaac06d1eb109052548c1b24f7579bde2f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851240"
---
# <a name="bitsadmin-setvalidationstate"></a>Bitsadmin setvalidationstate



Définit l’état de validation du contenu du fichier donné au sein du travail.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetValidationState <Job> <file index> <true|false> 
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|
|Fichier d’index|Démarre à 0|
|True|False|A la valeur TRUE si le contenu du fichier est valide, sinon la valeur FALSE|

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant définit l’état de validation du contenu du fichier 2 sur TRUE pour le travail nommé *myJob*.
```
C:\>bitsadmin /SetValidationState myJob 2 TRUE 
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)