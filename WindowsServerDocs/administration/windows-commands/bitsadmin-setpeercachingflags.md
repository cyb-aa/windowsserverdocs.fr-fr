---
title: Bitsadmin setpeercachingflags
description: La rubrique commandes Windows pour **Bitsadmin setpeercachingflags** -définit des indicateurs qui déterminent si les fichiers du travail peuvent être mis en cache et desservis aux homologues et si le travail peut télécharger du contenu à partir de pairs.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3f54a127-fb68-49a5-b843-664ec833df67
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 147f28268f1b4dd6dfb40cff85f073feabbc35a0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380465"
---
# <a name="bitsadmin-setpeercachingflags"></a>Bitsadmin setpeercachingflags



Définit des indicateurs qui déterminent si les fichiers du travail peuvent être mis en cache et desservis aux homologues et si le travail peut télécharger du contenu à partir de pairs.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetPeerCachingFlags <Job> <value> 
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|
|Value|La valeur est un entier non signé avec l’interprétation suivante pour les bits dans la représentation binaire.</br>1-la tâche peut télécharger du contenu à partir de pairs.</br>2-les fichiers du travail peuvent être mis en cache et pris en charge par les homologues.|

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant définit des indicateurs pour le travail nommé *myJob* , ce qui lui permet de télécharger du contenu à partir de pairs.
```
C:\>bitsadmin / SetPeerCachingFlags myJob 1 
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)