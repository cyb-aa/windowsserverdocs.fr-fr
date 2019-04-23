---
title: ver
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 384a5e8adb6c8304033f7dc645184ff2b674ae39
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887170"
---
# <a name="ver"></a>ver



Affiche le numéro de version du système d’exploitation.

Cette commande est prise en charge dans l’invite de commandes de Windows (Cmd.exe), mais pas dans PowerShell.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
ver
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="BKMK_examples"></a>Exemples

Pour obtenir le numéro de version du système d’exploitation à partir de l’interface de commande (cmd.exe), tapez :

```
ver
```

La commande ver ne fonctionne pas dans PowerShell. Pour obtenir la version du système d’exploitation à partir de PowerShell, tapez :

```powershell
$PSVersionTable.BuildVersion
````


#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
