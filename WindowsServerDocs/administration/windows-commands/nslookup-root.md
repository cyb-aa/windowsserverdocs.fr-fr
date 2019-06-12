---
title: nslookup root
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9c29edc3-ec49-43f2-bc49-86bf0612d816
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 47a26be99a5eee510970d3eee6b486331a98b159
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436900"
---
# <a name="nslookup-root"></a>nslookup root

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

changements du serveur par défaut sur le serveur pour la racine de l’espace de noms de domaine système DNS (Domain Name).
## <a name="syntax"></a>Syntaxe
```
root 
```
## <a name="parameters"></a>Paramètres

|    Paramètre    |                      Description                      |
|-----------------|-------------------------------------------------------|
| {aide &#124; ?} | Affiche un résumé de **nslookup** sous-commandes. |

## <a name="remarks"></a>Notes
- Actuellement, le serveur de noms ns.nic.ddn.mil est utilisé. Cette commande est un synonyme de lserver ns.nic.ddn.mil. Vous pouvez modifier le nom du serveur racine avec le **racine du jeu** commande.
  ## <a name="additional-references"></a>Références supplémentaires
  [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
  [nslookup définissez racine](nslookup-set-root.md)
