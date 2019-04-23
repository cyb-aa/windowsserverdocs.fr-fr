---
title: bitsadmin wrap
description: Rubrique de commandes de Windows pour **bitsadmin wrap** -encapsule toutes les lignes de sortie texte extension au-delà du périmètre plus à droite de la fenêtre de commande à la ligne suivante.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 4834a8a17c72394b6ee8f051ec76919af9880124
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881670"
---
# <a name="bitsadmin-wrap"></a>bitsadmin wrap

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Inclut dans un wrapper pour tenir dans une fenêtre de commande de sortie.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /Wrap Job
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------|--------|
|Tâche|Nom d’affichage ou le GUID du travail|

## <a name="remarks"></a>Notes

Spécifiez avant d’autres commutateurs. Par défaut, tous les commutateurs, sauf le [bitsadmin moniteur](bitsadmin-monitor.md) commutateur, encapsulez la sortie.

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant récupère des informations pour le travail nommé *myDownloadJob* et encapsule la sortie.

```
C:\>bitsadmin /Wrap /Info myDownloadJob /verbose
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
