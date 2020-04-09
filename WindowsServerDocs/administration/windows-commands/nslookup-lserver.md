---
title: nslookup lserver
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: aee5ea0b-bb17-4c14-bde7-2f7a91f2f22b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9d0d8619101d2e7b1f7fb6d6ed99d801c7c264f1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838632"
---
# <a name="nslookup-lserver"></a>nslookup lserver

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Remplace le serveur par défaut par le domaine DNS (Domain Name System) spécifié.
## <a name="syntax"></a>Syntaxe
```
lserver <DNSDomain> 
```
### <a name="parameters"></a>Paramètres

|    Paramètre    |                      Description                      |
|-----------------|-------------------------------------------------------|
|   <DNSDomain>   | Spécifie le nouveau domaine DNS pour le serveur par défaut.  |
| {Help &#124; ?} | Affiche un bref résumé des sous-commandes **nslookup** . |

## <a name="remarks"></a>Notes
- La commande **lserver** utilise le serveur initial pour rechercher les informations relatives au domaine DNS spécifié. Cela diffère de la commande de **serveur** , qui utilise le serveur par défaut actuel.
  ## <a name="additional-references"></a>Références supplémentaires
  - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
  [serveur nslookup](nslookup-server.md)
