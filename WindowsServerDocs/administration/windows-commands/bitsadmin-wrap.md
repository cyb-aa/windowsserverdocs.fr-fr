---
title: bitsadmin wrap
description: La rubrique commandes Windows pour Bitsadmin Wrap, qui encapsule toute ligne de texte de sortie qui s’étend au-delà du bord le plus à droite de la fenêtre de commande jusqu’à la ligne suivante.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 14e57522-539d-4621-ad15-09f7a44ccab7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 009a0452f44c4944ae110ca6b9e0570793c32a72
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848752"
---
# <a name="bitsadmin-wrap"></a>bitsadmin wrap

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Encapsule toute ligne de texte de sortie qui s’étend au-delà du bord le plus à droite de la fenêtre de commande jusqu’à la ligne suivante.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /Wrap Job
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------|--------|
|Tâche|Nom complet ou GUID du travail|

## <a name="remarks"></a>Notes

Spécifiez avant les autres commutateurs. Par défaut, tous les commutateurs, à l’exception du commutateur [Bitsadmin Monitor](bitsadmin-monitor.md) , encapsulent la sortie.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant récupère des informations pour le travail nommé *myDownloadJob* et encapsule la sortie.

```
C:\>bitsadmin /Wrap /Info myDownloadJob /verbose
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
