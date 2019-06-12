---
title: nslookup server
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 608267f8-f7b4-412a-8dcd-e08b5ffc2085
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ba58e223d0aa35b4157b813b10bf1d274313a1c1
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436969"
---
# <a name="nslookup-server"></a>nslookup server

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

modifie le serveur par défaut pour le domaine du système DNS (Domain Name) spécifié.
## <a name="syntax"></a>Syntaxe
```
server <DNSDomain>
```
## <a name="parameters"></a>Paramètres

|    Paramètre    |                          Description                           |
|-----------------|----------------------------------------------------------------|
|   <DNSDomain>   | Obligatoire. Spécifie le nouveau domaine DNS du serveur par défaut. |
| {aide &#124; ?} |     Affiche un résumé de **nslookup** sous-commandes.      |

## <a name="remarks"></a>Notes
- Le **server** commande utilise le serveur par défaut actuel pour rechercher les informations relatives au domaine DNS spécifié. Il s’agit Contrairement à la **lserver** commande, qui utilise le serveur initial.
  ## <a name="additional-references"></a>Références supplémentaires
  [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
  [nslookup lserver](nslookup-lserver.md)
