---
title: reg export
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0ad9526f-1e29-4fa5-9d2d-feaa92f12d7c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d7aeddb4b069b1baf5b8f7aaea2730a2b25bdad7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889650"
---
# <a name="reg-export"></a>reg export



Copie les sous-clés spécifiés, les entrées et les valeurs de l’ordinateur local dans un fichier pour le transfert vers d’autres serveurs.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
Reg export KeyName FileName [/y]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<KeyName>|Spécifie le chemin d’accès complet de la sous-clé. L’opération d’exportation fonctionne uniquement avec l’ordinateur local. KeyName doit inclure une clé racine valide. Clés racines valides sont : HKLM, HKCU, HKCR, HKU et HKCC.|
|\<FileName>|Spécifie le nom et le chemin d’accès du fichier doit être créé pendant l’opération. Le fichier doit avoir une extension .reg.|
|/y|Remplace tout fichier existant portant le nom *FileName* sans demander confirmation.|
|/?|Affiche l’aide de **reg export** à l’invite de commandes.|

## <a name="remarks"></a>Notes

Le tableau suivant répertorie les valeurs de retour pour la **reg export** opération.

|Value|Description|
|-----|-----------|
|0|Opération réussie|
|1|Échec|

## <a name="BKMK_examples"></a>Exemples

Pour exporter le contenu de toutes les sous-clés et valeurs de la clé MyApp vers le fichier AppBkUp.reg, tapez :
```
reg export HKLM\Software\MyCo\MyApp AppBkUp.reg
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)