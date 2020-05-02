---
title: nslookup set retry
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 615fdfa2-fa29-47a8-8c9e-a6c5b45b3b71
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1baeeaefedc211434f46bd0cfad713f093a873bf
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723582"
---
# <a name="nslookup-set-retry"></a>nslookup set retry

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Définit le nombre de nouvelles tentatives.
## <a name="syntax"></a>Syntaxe
```
set retry=<Number>
```
### <a name="parameters"></a>Paramètres

|    Paramètre    |                                      Description                                       |
|-----------------|----------------------------------------------------------------------------------------|
|    <Number>     | Spécifie la nouvelle valeur pour le nombre de nouvelles tentatives. Le nombre de tentatives par défaut est de 4. |
| {Help &#124; ?} |                 Affiche un bref résumé des sous-commandes **nslookup** .                  |

## <a name="remarks"></a>Notes 
- Quand une réponse à une demande n’est pas reçue dans un laps de temps donné, le délai d’attente est doublé et la demande est renvoyée. La valeur de nouvelle tentative contrôle le nombre de fois qu’une demande est renvoyée avant d’abandonner. Vous pouvez modifier le délai d’attente à l’aide de la sous-commande **Set Timeout** .
  ## <a name="additional-references"></a>Références supplémentaires
  - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
  [nslookup Set Timeout](nslookup-set-timeout.md)
