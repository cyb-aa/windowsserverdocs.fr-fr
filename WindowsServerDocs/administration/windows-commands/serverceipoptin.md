---
title: serverceipoptin
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3d7d7fa7-0689-4797-b802-36fe260d21a0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c0625694053a2d673cd86e12d6f6d54662a6c81d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834678"
---
# <a name="serverceipoptin"></a>serverceipoptin

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous permet de participer au Programme d’amélioration du produit (CEIP).
## <a name="syntax"></a>Syntaxe
```
serverceipoptin [/query] [/enable] [/disable]
```
#### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|Query|vérifie le paramètre actuel.|
|/Enable|Active la participation.|
|/Disable|Désactive la participation.|
|/?|Affiche l'aide à l'invite de commandes.|
## <a name="examples"></a><a name=BKMK_Examples></a>Illustre
Pour vérifier les paramètres actuels, tapez :
```
serverceipoptin /query
```
Pour activer la participation, tapez :
```
serverceipoptin /enable
```
Pour désactiver la participation, tapez :
```
serverceipoptin /disable
```
## <a name="additional-references"></a>Références supplémentaires
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

