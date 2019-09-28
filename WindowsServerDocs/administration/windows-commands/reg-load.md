---
title: charge reg
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3b0b2b1b-f510-4108-9e9d-7057e924aa6e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: db661e311e3fe8c393750716de5dab375e7817f4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384701"
---
# <a name="reg-load"></a>charge reg



Écrit les sous-clés et les entrées enregistrées dans une autre sous-clé du Registre. Destiné à être utilisé avec les fichiers temporaires utilisés pour la résolution des problèmes ou la modification des entrées de registre.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
reg load KeyName FileName
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|@no__t 0KeyName >|Spécifie le chemin d’accès complet de la sous-clé à charger. Pour spécifier des ordinateurs distants, incluez le nom de l’ordinateur (au format \\ @ no__t-1ComputerName @ no__t-2 dans le cadre du *keyName*. Si vous omettez \\ @ no__t-1ComputerName \, l’opération est effectuée par défaut sur l’ordinateur local. Le *keyName* doit inclure une clé racine valide. Les clés racines valides pour l’ordinateur local sont : HKLM, HKCU, HKCR, HKU et HKCC. Si un ordinateur distant est spécifié, les clés racines valides sont les suivantes : HKLM et HKU.|
|\<Nom de fichier >|Spécifie le nom et le chemin d’accès du fichier à charger. Ce fichier doit être créé à l’avance à l’aide de l’opération **reg save** et de l’extension. HIV.|
|/?|Affiche l’aide pour **reg Load** à l’invite de commandes.|

## <a name="remarks"></a>Notes

Le tableau suivant répertorie les valeurs renvoyées pour l’opération de **chargement de Reg** .

|Value|Description|
|-----|-----------|
|0|Succès|
|1|Échec|

## <a name="BKMK_examples"></a>Illustre

Pour charger le fichier nommé TempHive. HIV dans la clé HKLM\TempHive, tapez :
```
REG LOAD HKLM\TempHive TempHive.hiv
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)