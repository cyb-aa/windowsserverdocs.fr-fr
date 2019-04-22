---
title: nslookup set search
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: bf952a0337e23c0426265c6c0a4a8387a6ab45e1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816990"
---
# <a name="nslookup-set-search"></a>nslookup set search



Ajoute les noms de domaine système DNS (Domain Name) dans la liste de recherche du domaine DNS à la demande jusqu'à ce qu’une réponse est reçue. Cela s’applique lorsque le jeu et la recherche demandent contenir au moins un point, mais ne se terminent pas par un point final.

## <a name="syntax"></a>Syntaxe

```
set [no]search
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|**nosearch**|Arrête l’ajout des noms de domaine le système DNS (Domain Name) dans la liste de recherche du domaine DNS à la demande.|
|**search**|Ajoute les noms de domaine système DNS (Domain Name) dans la liste de recherche du domaine DNS à la demande jusqu'à ce qu’une réponse est reçue. La syntaxe par défaut est **recherche**.|
|{aide | ?}|Affiche un résumé de **nslookup** sous-commandes.|

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)