---
title: Reg Import
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0be103de-08fc-4f02-b590-361782680b3e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3e7e033091752f97086fd27fcb94e62469f0cced
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722553"
---
# <a name="reg-import"></a>Reg Import



Copie le contenu d’un fichier qui contient des sous-clés, des entrées et des valeurs de Registre exportées dans le registre de l’ordinateur local.



## <a name="syntax"></a>Syntaxe

```
Reg import FileName
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Nom de fichier>|Spécifie le nom et le chemin d’accès du fichier dont le contenu doit être copié dans le registre de l’ordinateur local. Ce fichier doit être créé à l’avance à l’aide de **reg Export**.|
|/?|Affiche l’aide pour **reg Import** à l’invite de commandes.|

## <a name="remarks"></a>Notes 

Le tableau suivant répertorie les valeurs renvoyées pour l’opération **reg Import** .

|Valeur|Description|
|-----|-----------|
|0|Succès|
|1|Échec|

## <a name="examples"></a>Exemples

Pour importer des entrées de Registre à partir du fichier nommé AppBkUp. reg, tapez :
```
reg import AppBkUp.reg
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)