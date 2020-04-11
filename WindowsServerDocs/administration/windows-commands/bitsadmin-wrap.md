---
title: bitsadmin wrap
description: La rubrique commandes Windows pour **Bitsadmin Wrap**, qui encapsule toute ligne de texte de sortie qui s’étend au-delà du bord le plus à droite de la fenêtre de commande jusqu’à la ligne suivante.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 14e57522-539d-4621-ad15-09f7a44ccab7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e754a765d94661baf24190431b455584d29991ec
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122563"
---
# <a name="bitsadmin-wrap"></a>bitsadmin wrap

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Encapsule toute ligne de texte de sortie qui s’étend au-delà du bord le plus à droite de la fenêtre de commande jusqu’à la ligne suivante. Vous devez spécifier ce commutateur avant tout autre commutateur.

Par défaut, tous les commutateurs, à l’exception du commutateur [Bitsadmin Monitor](bitsadmin-monitor.md) , encapsulent le texte de sortie.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /wrap <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ---------- |
| Tâche | Nom complet ou GUID du travail. |

## <a name="examples"></a>Exemples

L’exemple suivant récupère des informations pour le travail nommé *myDownloadJob* et encapsule la sortie.

```
C:\>bitsadmin /wrap /info myDownloadJob /verbose
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
