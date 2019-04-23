---
title: charge de reg
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ebc75ad78b7334f4d48a085f6870a443b31fa2a9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852190"
---
# <a name="reg-load"></a>charge de reg



Écritures enregistrement des sous-clés et entrées dans une autre sous-clé dans le Registre. Destinée aux fichiers temporaires qui sont utilisés pour la résolution des problèmes ou la modification des entrées de Registre.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
reg load KeyName FileName
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<KeyName>|Spécifie le chemin d’accès complet de la sous-clé à charger. Pour spécifier les ordinateurs distants, incluez le nom d’ordinateur (au format \\ \\ComputerName\) dans le cadre de la *KeyName*. En omettant \\ \\ComputerName\, l’opération par défaut sur l’ordinateur local. Le *KeyName* doit inclure une clé racine valide. Les clés racine valide pour l’ordinateur local sont : HKLM, HKCU, HKCR, HKU et HKCC. Si un ordinateur distant est spécifié, les clés racines valides sont : HKLM et HKU.|
|\<FileName>|Spécifie le nom et le chemin d’accès du fichier à charger. Ce fichier doit être créé au préalable à l’aide de la **reg enregistrer** opération et une extension .hiv.|
|/?|Affiche l’aide de **reg charge** à l’invite de commandes.|

## <a name="remarks"></a>Notes

Le tableau suivant répertorie les valeurs de retour pour la **reg charge** opération.

|Value|Description|
|-----|-----------|
|0|Opération réussie|
|1|Échec|

## <a name="BKMK_examples"></a>Exemples

Pour charger le fichier nommé RuchTemp.hiv à la clé HKLM\RuchTemp, tapez :
```
REG LOAD HKLM\TempHive TempHive.hiv
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)