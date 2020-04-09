---
title: nslookup set search
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 064ac660-8b04-4af9-8b2c-e4e0549771b8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9972919eae1be21d5dd30820d64dd1576b935666
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838302"
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