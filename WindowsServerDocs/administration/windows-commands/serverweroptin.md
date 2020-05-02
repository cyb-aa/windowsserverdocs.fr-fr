---
title: serverweroptin
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f3c0b0af-cafb-4f09-8b36-5a357ddf392d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a3acba57aa012c57c5c6109ed948ce6bb5b28078
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721951"
---
# <a name="serverweroptin"></a>serverweroptin

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous permet d’activer le rapport d’erreurs.
## <a name="syntax"></a>Syntaxe
```
serverweroptin [/query] [/detailed] [/summary]
```
#### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|Query|vérifie le paramètre actuel.|
|/detailed|Envoie automatiquement des rapports détaillés.|
|/Summary|Envoie automatiquement des rapports de synthèse.|
|/?|Affiche l'aide à l'invite de commandes.|
## <a name="examples"></a>Exemples
Pour vérifier le paramètre actuel, tapez :
```
serverweroptin /query
```
Pour envoyer automatiquement des rapports détaillés, tapez :
```
serverweroptin /detailed
```
Pour envoyer automatiquement des rapports de synthèse, tapez
```
serverweroptin /summary
```
## <a name="additional-references"></a>Références supplémentaires
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

