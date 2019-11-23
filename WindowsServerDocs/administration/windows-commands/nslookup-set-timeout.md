---
title: nslookup set timeout
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 07afdaf4-ffec-496f-a188-4e91cf1a28f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 32fcfcaeccb6599e9aaca21f9c085bb00857479c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372759"
---
# <a name="nslookup-set-timeout"></a>nslookup set timeout

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

modifie le nombre initial de secondes d’attente d’une réponse à une demande de recherche.
## <a name="syntax"></a>Syntaxe
```
set timeout=<Number>
```
## <a name="parameters"></a>Paramètres

|    Paramètre    |                                           Description                                            |
|-----------------|--------------------------------------------------------------------------------------------------|
|    <Number>     | Spécifie le nombre de secondes d’attente d’une réponse. Le nombre de secondes d’attente par défaut est de 5. |
| {Help &#124; ?} |                      Affiche un bref résumé des sous-commandes **nslookup** .                       |

## <a name="remarks"></a>Notes
- Quand une réponse à une demande n’est pas reçue au cours de la période spécifiée, le délai d’attente est doublé et la demande est renvoyée. Vous pouvez utiliser la commande **Set Retry** pour contrôler le nombre de nouvelles tentatives.
  ## <a name="BKMK_examples"></a>Illustre
  L’exemple suivant définit le délai d’attente pour l’obtention d’une réponse à 2 secondes :
  ```
  set timeout=2
  ```
  ## <a name="additional-references"></a>Références supplémentaires
  [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
  [nslookup Set Retry](nslookup-set-retry.md)
