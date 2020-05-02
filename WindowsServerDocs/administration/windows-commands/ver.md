---
title: ver
description: Rubrique de référence pour ver, qui affiche le numéro de version du système d’exploitation.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5a9c6cd4-b67d-4b30-8c56-5f9798eafd2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7050dddda6cc27c50980f2e44f40e1f682c1d375
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720311"
---
# <a name="ver"></a>ver



Affiche le numéro de version du système d’exploitation.

Cette commande est prise en charge dans l’invite de commandes Windows (cmd. exe), mais pas dans PowerShell.



## <a name="syntax"></a>Syntaxe

```
ver
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="examples"></a>Exemples

Pour obtenir le numéro de version du système d’exploitation à partir de l’interface de commande (cmd. exe), tapez :

```
ver
```

La commande ver ne fonctionne pas dans PowerShell. Pour obtenir la version du système d’exploitation à partir de PowerShell, tapez :

```powershell
$PSVersionTable.BuildVersion
````


## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
