---
title: serverweroptin
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f3c0b0af-cafb-4f09-8b36-5a357ddf392d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 18b4a56888b3f23bf3bac4a12b2dba7079b50923
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834632"
---
# <a name="serverweroptin"></a>serverweroptin

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
## <a name="examples"></a><a name=BKMK_Examples></a>Illustre
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

