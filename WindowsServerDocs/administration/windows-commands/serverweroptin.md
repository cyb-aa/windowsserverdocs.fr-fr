---
title: serverweroptin
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f3c0b0af-cafb-4f09-8b36-5a357ddf392d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a7d5791e059d31e416f848f6e8df648c8f9bd27d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371018"
---
# <a name="serverweroptin"></a>serverweroptin

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Vous permet d’activer le rapport d’erreurs.
## <a name="syntax"></a>Syntaxe
```
serverweroptin [/query] [/detailed] [/summary]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|Query|vérifie le paramètre actuel.|
|/detailed|Envoie automatiquement des rapports détaillés.|
|/Summary|Envoie automatiquement des rapports de synthèse.|
|/?|Affiche l'aide à l'invite de commandes.|
## <a name="BKMK_Examples"></a>Illustre
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
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

