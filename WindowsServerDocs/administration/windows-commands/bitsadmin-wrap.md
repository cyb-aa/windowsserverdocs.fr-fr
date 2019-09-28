---
title: bitsadmin wrap
description: Rubrique relative aux commandes Windows pour **Bitsadmin Wrap** -encapsule toute ligne de texte de sortie qui s’étend au-delà du bord le plus à droite de la fenêtre de commande jusqu’à la ligne suivante.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 14e57522-539d-4621-ad15-09f7a44ccab7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5609fb6f38716795a545e0c7fe3939f893a8c8d5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380679"
---
# <a name="bitsadmin-wrap"></a>bitsadmin wrap

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Encapsule la sortie pour l’ajuster dans une fenêtre de commande.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /Wrap Job
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------|--------|
|Tâche|Nom complet ou GUID du travail|

## <a name="remarks"></a>Notes

Spécifiez avant les autres commutateurs. Par défaut, tous les commutateurs, à l’exception du commutateur [Bitsadmin Monitor](bitsadmin-monitor.md) , encapsulent la sortie.

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant récupère des informations pour le travail nommé *myDownloadJob* et encapsule la sortie.

```
C:\>bitsadmin /Wrap /Info myDownloadJob /verbose
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
