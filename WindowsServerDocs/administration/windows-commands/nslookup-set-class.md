---
title: nslookup set class
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ed826400-40da-42b6-b7f0-95db73790723
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5e02a6f473a7c2dc6d6b66eb5b23fded930bc817
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856350"
---
# <a name="nslookup-set-class"></a>nslookup set class



Modifie la classe de requête. La classe spécifie le groupe de protocole des informations.

## <a name="syntax"></a>Syntaxe

```
set class=<Class>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Class>|La classe par défaut est in. Ce qui suit répertorie les valeurs valides pour cette commande.</br>-IN : Spécifie la classe Internet.</br>-CHAOS : Spécifie la classe Chaos.</br>-HESIOD : Spécifie la classe MIT Athena Hesiod.</br>-TOUTE : Spécifie un des caractères génériques précédemment répertoriées.|
|{aide | ?}|Affiche un résumé de **nslookup** sous-commandes.|

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)