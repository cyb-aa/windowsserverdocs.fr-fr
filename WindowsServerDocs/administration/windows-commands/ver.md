---
title: ver
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5a9c6cd4-b67d-4b30-8c56-5f9798eafd2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e48b3b1061edf793c88693b3353753c6a4cedcfc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362725"
---
# <a name="ver"></a>ver



Affiche le numéro de version du système d’exploitation.

Cette commande est prise en charge dans l’invite de commandes Windows (cmd. exe), mais pas dans PowerShell.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
ver
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="BKMK_examples"></a>Illustre

Pour obtenir le numéro de version du système d’exploitation à partir de l’interface de commande (cmd. exe), tapez :

```
ver
```

La commande ver ne fonctionne pas dans PowerShell. Pour obtenir la version du système d’exploitation à partir de PowerShell, tapez :

```powershell
$PSVersionTable.BuildVersion
````


#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
