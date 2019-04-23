---
title: reg delete
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 369ef3bda37ab8e143a14f0f9707b9bbf14bd5f8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877080"
---
# <a name="reg-delete"></a>reg delete



Supprime une sous-clé ou des entrées de Registre.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
Reg delete <KeyName> [{/v ValueName | /ve | /va}] [/f]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<KeyName>|Spécifie le chemin d’accès complet de la sous-clé ou l’entrée à supprimer. Pour spécifier un ordinateur distant, incluez le nom d’ordinateur (au format \\ \\ComputerName\) dans le cadre de la *KeyName*. En omettant \\ \\ComputerName\, l’opération par défaut sur l’ordinateur local. Le *KeyName* doit inclure une clé racine valide. Les clés racine valide pour l’ordinateur local sont : HKLM, HKCU, HKCR, HKU et HKCC. Si un ordinateur distant est spécifié, les clés racines valides sont : HKLM et HKU.|
|/v \<ValueName>|Supprime une entrée spécifique sous la sous-clé. Si aucune entrée n’est spécifiée, toutes les entrées et les sous-clés sous la sous-clé seront supprimés.|
|/ve|Spécifie que seules les entrées qui n’ont aucune valeur seront supprimées.|
|/va|Supprime toutes les entrées sous la sous-clé spécifiée. Sous-clés sous la sous-clé spécifiée ne sont pas supprimés.|
|/f|Supprime la sous-clé de Registre existant ou l’entrée sans demander confirmation.|
|/?|Affiche l’aide de **reg delete** à l’invite de commandes.|

## <a name="remarks"></a>Notes

Le tableau suivant répertorie les valeurs de retour pour la **reg delete** opération.

|Value|Description|
|-----|-----------|
|0|Opération réussie|
|1|Échec|

## <a name="BKMK_examples"></a>Exemples

Pour supprimer la clé de Registre délai d’attente et son toutes les sous-clés et valeurs, tapez :
```
REG DELETE HKLM\Software\MyCo\MyApp\Timeout
```
Pour supprimer la valeur de Registre MTU sous HKLM\Software\MaClé sur l’ordinateur nommé ZODIAQUE, tapez :
```
REG DELETE \\ZODIAC\HKLM\Software\MyCo /v MTU
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)