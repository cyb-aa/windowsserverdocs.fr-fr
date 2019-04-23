---
title: reg import
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 7d1dd1b61848671b528c62fd22fe656e14fda7b1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861840"
---
# <a name="reg-import"></a>reg import



Copie le contenu d’un fichier qui contient exporté des sous-clés de Registre, des entrées et des valeurs dans le Registre de l’ordinateur local.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
Reg import FileName
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<FileName>|Spécifie le nom et le chemin d’accès du fichier qui a du contenu doit être copié dans le Registre de l’ordinateur local. Ce fichier doit être créé au préalable à l’aide de **reg export**.|
|/?|Affiche l’aide de **reg import** à l’invite de commandes.|

## <a name="remarks"></a>Notes

Le tableau suivant répertorie les valeurs de retour pour la **reg import** opération.

|Value|Description|
|-----|-----------|
|0|Opération réussie|
|1|Échec|

## <a name="BKMK_examples"></a>Exemples

Pour importer des entrées de Registre à partir du fichier nommé AppBkUp.reg, tapez :
```
reg import AppBkUp.reg
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)