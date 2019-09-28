---
title: nslookup set search
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 064ac660-8b04-4af9-8b2c-e4e0549771b8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d9da08a296d61789dbafeccde5d46c8a220d874c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372779"
---
# <a name="nslookup-set-search"></a>nslookup set search



Ajoute les noms de domaine DNS (Domain Name System) dans la liste de recherche du domaine DNS à la demande jusqu’à ce qu’une réponse soit reçue. Cela s’applique lorsque le jeu et la demande de recherche contiennent au moins un point, mais ne se terminent pas par un point final.

## <a name="syntax"></a>Syntaxe

```
set [no]search
```

## <a name="parameters"></a>Paramètres

|  Paramètre   |                                                                          Description                                                                          |
|--------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **nosearch** |                            Arrête l’ajout des noms de domaine DNS (Domain Name System) dans la liste de recherche du domaine DNS à la demande.                            |
|  **recherche**  | Ajoute les noms de domaine DNS (Domain Name System) dans la liste de recherche du domaine DNS à la demande jusqu’à ce qu’une réponse soit reçue. La syntaxe par défaut est **Search**. |
|    {aide     |                                                                              ?}                                                                               |

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)