---
title: nslookup lserver
description: 'Rubrique de commandes de Windows pour ***- '
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
ms.openlocfilehash: 2f2f787915f2b941d6c098d44de1bb0e04dbd491
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436911"
---
# <a name="nslookup-lserver"></a>nslookup lserver

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

modifie le serveur par défaut pour le domaine du système DNS (Domain Name) spécifié.
## <a name="syntax"></a>Syntaxe
```
lserver <DNSDomain> 
```
## <a name="parameters"></a>Paramètres

|    Paramètre    |                      Description                      |
|-----------------|-------------------------------------------------------|
|   <DNSDomain>   | Spécifie le nouveau domaine DNS du serveur par défaut.  |
| {aide &#124; ?} | Affiche un résumé de **nslookup** sous-commandes. |

## <a name="remarks"></a>Notes
- Le **lserver** commande utilise le serveur initial pour rechercher les informations relatives au domaine DNS spécifié. Il s’agit Contrairement à la **server** commande, qui utilise le serveur par défaut actuel.
  ## <a name="additional-references"></a>Références supplémentaires
  [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
  [nslookup server](nslookup-server.md)
