---
title: bitsadmin wrap
description: Rubrique de référence pour la commande Bitsadmin Wrap, qui encapsule toute ligne de texte de sortie qui s’étend au-delà du bord le plus à droite de la fenêtre de commande jusqu’à la ligne suivante.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 14e57522-539d-4621-ad15-09f7a44ccab7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8c1c2c78fd3cc78674ef497526ba236ad058fe83
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82707574"
---
# <a name="bitsadmin-wrap"></a>bitsadmin wrap

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Encapsule toute ligne de texte de sortie qui s’étend au-delà du bord le plus à droite de la fenêtre de commande jusqu’à la ligne suivante. Vous devez spécifier ce commutateur avant tout autre commutateur.

Par défaut, tous les commutateurs, à l’exception du commutateur [Bitsadmin Monitor](bitsadmin-monitor.md) , encapsulent le texte de sortie.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /wrap <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ---------- |
| travail | Nom complet ou GUID du travail. |

## <a name="examples"></a>Exemples

Pour récupérer des informations pour le travail nommé *myDownloadJob* et encapsuler le texte de sortie :

```
bitsadmin /wrap /info myDownloadJob /verbose
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
