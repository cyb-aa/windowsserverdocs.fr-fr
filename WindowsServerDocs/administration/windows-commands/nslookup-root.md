---
title: nslookup root
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9c29edc3-ec49-43f2-bc49-86bf0612d816
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d11179ff3cd22acd9df67261e7ab752aa159201a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838642"
---
# <a name="nslookup-root"></a>nslookup root

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

remplace le serveur par défaut par le serveur pour la racine de l’espace de noms de domaine DNS (Domain Name System).
## <a name="syntax"></a>Syntaxe
```
root 
```
### <a name="parameters"></a>Paramètres

|    Paramètre    |                      Description                      |
|-----------------|-------------------------------------------------------|
| {Help &#124; ?} | Affiche un bref résumé des sous-commandes **nslookup** . |

## <a name="remarks"></a>Notes
- Actuellement, le serveur de noms ns.nic.ddn.mil est utilisé. Cette commande est un synonyme de lserver ns.nic.ddn.mil. Vous pouvez modifier le nom du serveur racine à l’aide de la commande **Set root** .
  ## <a name="additional-references"></a>Références supplémentaires
  - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
  [nslookup définir la racine](nslookup-set-root.md)
