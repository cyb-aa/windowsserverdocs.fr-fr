---
title: nslookup lserver
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aee5ea0b-bb17-4c14-bde7-2f7a91f2f22b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 30c5ba8b7fef9b09d854aca998948f7891d99a02
ms.sourcegitcommit: ee8e0b217be6f6b2532ee7265fb4be00c106e124
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70878126"
---
# <a name="nslookup-lserver"></a>nslookup lserver

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Remplace le serveur par défaut par le domaine DNS (Domain Name System) spécifié.
## <a name="syntax"></a>Syntaxe
```
lserver <DNSDomain> 
```
## <a name="parameters"></a>Paramètres

|    Paramètre    |                      Description                      |
|-----------------|-------------------------------------------------------|
|   <DNSDomain>   | Spécifie le nouveau domaine DNS pour le serveur par défaut.  |
| {Help &#124; ?} | Affiche un bref résumé des sous-commandes **nslookup** . |

## <a name="remarks"></a>Notes
- La commande **lserver** utilise le serveur initial pour rechercher les informations relatives au domaine DNS spécifié. Cela diffère de la commande de **serveur** , qui utilise le serveur par défaut actuel.
  ## <a name="additional-references"></a>Références supplémentaires
  [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)[nslookup serveur](nslookup-server.md) 
  
