---
title: Reg Import
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0be103de-08fc-4f02-b590-361782680b3e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2e1c7920a64469717c30cfcddda7b8002db5ba10
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384735"
---
# <a name="reg-import"></a>Reg Import



Copie le contenu d’un fichier qui contient des sous-clés, des entrées et des valeurs de Registre exportées dans le registre de l’ordinateur local.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
Reg import FileName
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Nom de fichier >|Spécifie le nom et le chemin d’accès du fichier dont le contenu doit être copié dans le registre de l’ordinateur local. Ce fichier doit être créé à l’avance à l’aide de **reg Export**.|
|/?|Affiche l’aide pour **reg Import** à l’invite de commandes.|

## <a name="remarks"></a>Notes

Le tableau suivant répertorie les valeurs renvoyées pour l’opération **reg Import** .

|Value|Description|
|-----|-----------|
|0|Succès|
|1|Échec|

## <a name="BKMK_examples"></a>Illustre

Pour importer des entrées de Registre à partir du fichier nommé AppBkUp. reg, tapez :
```
reg import AppBkUp.reg
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)