---
title: serverceipoptin
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3d7d7fa7-0689-4797-b802-36fe260d21a0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f400a8f66f15e5a138cf355ad54d276cfa7f3ce3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371009"
---
# <a name="serverceipoptin"></a>serverceipoptin

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Vous permet de participer au Programme d’amélioration du produit (CEIP).
## <a name="syntax"></a>Syntaxe
```
serverceipoptin [/query] [/enable] [/disable]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|Query|vérifie le paramètre actuel.|
|/Enable|Active la participation.|
|/Disable|Désactive la participation.|
|/?|Affiche l'aide à l'invite de commandes.|
## <a name="BKMK_Examples"></a>Illustre
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
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

