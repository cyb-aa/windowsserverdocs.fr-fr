---
title: nslookup server
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 608267f8-f7b4-412a-8dcd-e08b5ffc2085
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8de2d8a801d57a9197d5e8ec7d3b6adb2d00dd04
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838682"
---
# <a name="nslookup-server"></a>nslookup server

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Remplace le serveur par défaut par le domaine DNS (Domain Name System) spécifié.
## <a name="syntax"></a>Syntaxe
```
server <DNSDomain>
```
### <a name="parameters"></a>Paramètres

|    Paramètre    |                          Description                           |
|-----------------|----------------------------------------------------------------|
|   <DNSDomain>   | Obligatoire. Spécifie le nouveau domaine DNS pour le serveur par défaut. |
| {Help &#124; ?} |     Affiche un bref résumé des sous-commandes **nslookup** .      |

## <a name="remarks"></a>Notes
- La commande **serveur** utilise le serveur par défaut actuel pour rechercher les informations relatives au domaine DNS spécifié. Cela diffère de la commande **lserver** , qui utilise le serveur initial.
  ## <a name="additional-references"></a>Références supplémentaires
  - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
  [nslookup lserver](nslookup-lserver.md)
