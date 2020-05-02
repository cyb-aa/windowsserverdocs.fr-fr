---
title: nslookup set search
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 064ac660-8b04-4af9-8b2c-e4e0549771b8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2e3f5bce42d3614b535b2dfb00c4c9ea9cac2346
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723563"
---
# <a name="nslookup-set-search"></a>nslookup set search



Ajoute les noms de domaine DNS (Domain Name System) dans la liste de recherche du domaine DNS à la demande jusqu’à ce qu’une réponse soit reçue. Cela s’applique lorsque le jeu et la demande de recherche contiennent au moins un point, mais ne se terminent pas par un point final.

## <a name="syntax"></a>Syntaxe

```
set [no]search
```

### <a name="parameters"></a>Paramètres

|  Paramètre   |                                                                          Description                                                                          |
|--------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **nosearch** |                            Arrête l’ajout des noms de domaine DNS (Domain Name System) dans la liste de recherche du domaine DNS à la demande.                            |
|  **recherche**  | Ajoute les noms de domaine DNS (Domain Name System) dans la liste de recherche du domaine DNS à la demande jusqu’à ce qu’une réponse soit reçue. La syntaxe par défaut est **Search**. |
|    {aide     |                                                                              ?}                                                                               |

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)