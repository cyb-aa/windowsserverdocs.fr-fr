---
title: ver
description: La rubrique commandes Windows pour ver, qui affiche le numéro de version du système d’exploitation.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5a9c6cd4-b67d-4b30-8c56-5f9798eafd2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d0d0676dcfa6546e4bbf74c4c58a24f51744d00f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830192"
---
# <a name="ver"></a>ver



Affiche le numéro de version du système d’exploitation.

Cette commande est prise en charge dans l’invite de commandes Windows (cmd. exe), mais pas dans PowerShell.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
ver
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

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
