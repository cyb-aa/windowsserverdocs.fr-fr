---
title: nslookup set timeout
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 07afdaf4-ffec-496f-a188-4e91cf1a28f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 68e9630b9690c9b6c9d4c316f8b328897289362c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723540"
---
# <a name="nslookup-set-timeout"></a>nslookup set timeout

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

modifie le nombre initial de secondes d’attente d’une réponse à une demande de recherche.
## <a name="syntax"></a>Syntaxe
```
set timeout=<Number>
```
### <a name="parameters"></a>Paramètres

|    Paramètre    |                                           Description                                            |
|-----------------|--------------------------------------------------------------------------------------------------|
|    <Number>     | Spécifie le nombre de secondes d’attente d’une réponse. Le nombre de secondes d’attente par défaut est de 5. |
| {Help &#124; ?} |                      Affiche un bref résumé des sous-commandes **nslookup** .                       |

## <a name="remarks"></a>Notes 
- Quand une réponse à une demande n’est pas reçue au cours de la période spécifiée, le délai d’attente est doublé et la demande est renvoyée. Vous pouvez utiliser la commande **Set Retry** pour contrôler le nombre de nouvelles tentatives.
  ## <a name="examples"></a>Exemples
  Pour définir le délai d’expiration pour l’obtention d’une réponse à 2 secondes :
  ```
  set timeout=2
  ```
  ## <a name="additional-references"></a>Références supplémentaires
  - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
  [nslookup Set Retry](nslookup-set-retry.md)
