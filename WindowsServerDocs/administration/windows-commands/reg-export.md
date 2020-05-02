---
title: Reg Export
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0ad9526f-1e29-4fa5-9d2d-feaa92f12d7c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b2c697595d5d19c953ef85f7a2e334c6fe05329d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722560"
---
# <a name="reg-export"></a>Reg Export



Copie les sous-clés, les entrées et les valeurs spécifiées de l’ordinateur local dans un fichier à des fins de transfert vers d’autres serveurs.



## <a name="syntax"></a>Syntaxe

```
Reg export KeyName FileName [/y]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<KeyName>|Spécifie le chemin d’accès complet de la sous-clé. L’opération d’exportation fonctionne uniquement avec l’ordinateur local. Le KeyName doit inclure une clé racine valide. Les clés racines valides sont : HKLM, HKCU, HKCR, HKU et HKCC.|
|\<Nom de fichier>|Spécifie le nom et le chemin d’accès du fichier à créer au cours de l’opération. Le fichier doit avoir une extension. reg.|
|/y|Remplace tout fichier existant portant le nom *filename* sans demander confirmation.|
|/?|Affiche l’aide pour **reg Export** à l’invite de commandes.|

## <a name="remarks"></a>Notes 

Le tableau suivant répertorie les valeurs renvoyées pour l’opération **reg Export** .

|Valeur|Description|
|-----|-----------|
|0|Succès|
|1|Échec|

## <a name="examples"></a>Exemples

Pour exporter le contenu de toutes les sous-clés et valeurs de la clé MyApp dans le fichier AppBkUp. reg, tapez :
```
reg export HKLM\Software\MyCo\MyApp AppBkUp.reg
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)