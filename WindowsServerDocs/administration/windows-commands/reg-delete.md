---
title: reg supprimer
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cee05071-1607-4ab1-b8ab-65caebeb85c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7156bf58b27da1602931f0dc1903de71d86764e7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384760"
---
# <a name="reg-delete"></a>reg supprimer



Supprime une sous-clé ou des entrées du Registre.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
Reg delete <KeyName> [{/v ValueName | /ve | /va}] [/f]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|@no__t 0KeyName >|Spécifie le chemin d’accès complet de la sous-clé ou de l’entrée à supprimer. Pour spécifier un ordinateur distant, incluez le nom de l’ordinateur (au format \\ @ no__t-1ComputerName @ no__t-2 dans le cadre du *keyName*. Si vous omettez \\ @ no__t-1ComputerName \, l’opération est effectuée par défaut sur l’ordinateur local. Le *keyName* doit inclure une clé racine valide. Les clés racines valides pour l’ordinateur local sont : HKLM, HKCU, HKCR, HKU et HKCC. Si un ordinateur distant est spécifié, les clés racines valides sont les suivantes : HKLM et HKU.|
|/v \<ValueName >|Supprime une entrée spécifique sous la sous-clé. Si aucune entrée n’est spécifiée, toutes les entrées et sous-clés sous la sous-clé seront supprimées.|
|/ve|Spécifie que seules les entrées qui n’ont pas de valeur seront supprimées.|
|/va|Supprime toutes les entrées sous la sous-clé spécifiée. Les sous-clés sous la sous-clé spécifiée ne sont pas supprimées.|
|/f|Supprime la sous-clé ou l’entrée de Registre existante sans demander de confirmation.|
|/?|Affiche l’aide de la commande **reg delete** à l’invite de commandes.|

## <a name="remarks"></a>Notes

Le tableau suivant répertorie les valeurs renvoyées pour l’opération **reg delete** .

|Value|Description|
|-----|-----------|
|0|Succès|
|1|Échec|

## <a name="BKMK_examples"></a>Illustre

Pour supprimer le délai d’expiration de la clé de Registre et ses sous-clés et valeurs, tapez :
```
REG DELETE HKLM\Software\MyCo\MyApp\Timeout
```
Pour supprimer la valeur de Registre MTU sous HKLM\Software\MyCo sur l’ordinateur nommé ZODIAC, tapez :
```
REG DELETE \\ZODIAC\HKLM\Software\MyCo /v MTU
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)