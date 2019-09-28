---
title: nslookup server
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 0ce609f62f2d87024e1d75b43869b3b867e946e2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373058"
---
# <a name="nslookup-server"></a>nslookup server

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Remplace le serveur par défaut par le domaine DNS (Domain Name System) spécifié.
## <a name="syntax"></a>Syntaxe
```
server <DNSDomain>
```
## <a name="parameters"></a>Paramètres

|    Paramètre    |                          Description                           |
|-----------------|----------------------------------------------------------------|
|   <DNSDomain>   | Obligatoire. Spécifie le nouveau domaine DNS pour le serveur par défaut. |
| {Help &#124; ?} |     Affiche un bref résumé des sous-commandes **nslookup** .      |

## <a name="remarks"></a>Notes
- La commande **serveur** utilise le serveur par défaut actuel pour rechercher les informations relatives au domaine DNS spécifié. Cela diffère de la commande **lserver** , qui utilise le serveur initial.
  ## <a name="additional-references"></a>Références supplémentaires
  [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)[nslookup lserver](nslookup-lserver.md) 
  
