---
title: serverweroptin
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 29545be99b14042d16a6f3a4118e0746f18b14ab
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869640"
---
# <a name="serverweroptin"></a>serverweroptin

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous permet d’activer le rapport d’erreurs.
## <a name="syntax"></a>Syntaxe
```
serverweroptin [/query] [/detailed] [/summary]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/query|vérifie le paramètre actuel.|
|/detailed|Envoie automatiquement les rapports détaillés.|
|/ summary|Envoie automatiquement des rapports de synthèse.|
|/?|Affiche l'aide à l'invite de commandes.|
## <a name="BKMK_Examples"></a>Exemples
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
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)

