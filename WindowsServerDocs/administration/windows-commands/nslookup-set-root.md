---
title: nslookup set root
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8ad5393c-d4fd-4594-8187-576b1dcde60a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d913669fd4fede06c9983756df1bbf626ca430ac
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723569"
---
# <a name="nslookup-set-root"></a>nslookup set root

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Modifie le nom du serveur racine utilisé pour les requêtes.
## <a name="syntax"></a>Syntaxe
```
set root=<RootServer>
```
### <a name="parameters"></a>Paramètres

|    Paramètre    |                                   Description                                    |
|-----------------|----------------------------------------------------------------------------------|
|  <RootServer>   | Spécifie le nouveau nom du serveur racine. La valeur par défaut est ns.nic.ddn.mil. |
| {Help &#124; ?} |              Affiche un bref résumé des sous-commandes **nslookup** .               |

## <a name="remarks"></a>Notes 
- La sous-commande **Set root** affecte la sous-commande **racine** .
  ## <a name="additional-references"></a>Références supplémentaires
  - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
  [nslookup racine](nslookup-root.md)
