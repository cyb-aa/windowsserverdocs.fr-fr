---
title: nslookup root
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9c29edc3-ec49-43f2-bc49-86bf0612d816
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bdfbe40443cf8f2fec2f81608bb93603cd74937f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723692"
---
# <a name="nslookup-root"></a>nslookup root

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

remplace le serveur par défaut par le serveur pour la racine de l’espace de noms de domaine DNS (Domain Name System).
## <a name="syntax"></a>Syntaxe
```
root 
```
### <a name="parameters"></a>Paramètres

|    Paramètre    |                      Description                      |
|-----------------|-------------------------------------------------------|
| {Help &#124; ?} | Affiche un bref résumé des sous-commandes **nslookup** . |

## <a name="remarks"></a>Notes 
- Actuellement, le serveur de noms ns.nic.ddn.mil est utilisé. Cette commande est un synonyme de lserver ns.nic.ddn.mil. Vous pouvez modifier le nom du serveur racine à l’aide de la commande **Set root** .
  ## <a name="additional-references"></a>Références supplémentaires
  - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
  [nslookup définir la racine](nslookup-set-root.md)
