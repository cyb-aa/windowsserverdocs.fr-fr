---
title: nslookup set timeout
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 07afdaf4-ffec-496f-a188-4e91cf1a28f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8506511fc203f94d395851471f6a981ef0765928
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838272"
---
# <a name="nslookup-set-timeout"></a>nslookup set timeout

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

modifie le nombre initial de secondes d’attente d’une réponse à une demande de recherche.
## <a name="syntax"></a>Syntaxe
```
set timeout=<Number>
```
### <a name="parameters"></a>Paramètres

|    Paramètre    |                                           Description                                            |
|-----------------|--------------------------------------------------------------------------------------------------|
|    <Number>     | Spécifie le nombre de secondes d’attente d’une réponse. Le nombre de secondes d’attente par défaut est de 5. |
| {Help &#124; ?} |                      Affiche un bref résumé des sous-commandes **nslookup** .                       |

## <a name="remarks"></a>Notes
- Quand une réponse à une demande n’est pas reçue au cours de la période spécifiée, le délai d’attente est doublé et la demande est renvoyée. Vous pouvez utiliser la commande **Set Retry** pour contrôler le nombre de nouvelles tentatives.
  ## <a name="examples"></a><a name=BKMK_examples></a>Illustre
  L’exemple suivant définit le délai d’attente pour l’obtention d’une réponse à 2 secondes :
  ```
  set timeout=2
  ```
  ## <a name="additional-references"></a>Références supplémentaires
  - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
  [nslookup Set Retry](nslookup-set-retry.md)
