---
title: bitsadmin getmodificationtime
description: 'Rubrique relative aux commandes Windows pour **Bitsadmin getmodificationtime** : récupère l’heure de la dernière modification du travail ou le transfert des données.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e543945e-92c4-491e-8c2d-344f8a3e342d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 48b4d252ce6161b288726190f41f08c64efdbcf2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381527"
---
# <a name="bitsadmin-getmodificationtime"></a>bitsadmin getmodificationtime



Récupère la dernière fois que le travail a été modifié ou que les données ont été transférées avec succès.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetModificationTime <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant récupère l’heure de la dernière modification de la tâche nommée *myDownloadJob*.
```
C:\>bitsadmin /GetModificationTime myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)