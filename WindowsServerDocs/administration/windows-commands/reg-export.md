---
title: Reg Export
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0ad9526f-1e29-4fa5-9d2d-feaa92f12d7c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5901014511fed0c17a641e1ed183ddbf40dd44c8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836492"
---
# <a name="reg-export"></a>Reg Export



Copie les sous-clés, les entrées et les valeurs spécifiées de l’ordinateur local dans un fichier à des fins de transfert vers d’autres serveurs.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
Reg export KeyName FileName [/y]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<KeyName >|Spécifie le chemin d’accès complet de la sous-clé. L’opération d’exportation fonctionne uniquement avec l’ordinateur local. Le KeyName doit inclure une clé racine valide. Les clés racines valides sont : HKLM, HKCU, HKCR, HKU et HKCC.|
|Nom de fichier \<>|Spécifie le nom et le chemin d’accès du fichier à créer au cours de l’opération. Le fichier doit avoir une extension. reg.|
|/y|Remplace tout fichier existant portant le nom *filename* sans demander confirmation.|
|/?|Affiche l’aide pour **reg Export** à l’invite de commandes.|

## <a name="remarks"></a>Notes

Le tableau suivant répertorie les valeurs renvoyées pour l’opération **reg Export** .

|Valeur|Description|
|-----|-----------|
|0|Opération réussie|
|1|Échec|

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour exporter le contenu de toutes les sous-clés et valeurs de la clé MyApp dans le fichier AppBkUp. reg, tapez :
```
reg export HKLM\Software\MyCo\MyApp AppBkUp.reg
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)