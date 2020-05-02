---
title: reg supprimer
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cee05071-1607-4ab1-b8ab-65caebeb85c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a4ff643970bac021a6b7dcb731e64c412deb8df3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722573"
---
# <a name="reg-delete"></a>reg supprimer



Supprime une sous-clé ou des entrées du Registre.



## <a name="syntax"></a>Syntaxe

```
Reg delete <KeyName> [{/v ValueName | /ve | /va}] [/f]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<KeyName>|Spécifie le chemin d’accès complet de la sous-clé ou de l’entrée à supprimer. Pour spécifier un ordinateur distant, incluez le nom de l’ordinateur ( \\ \\au\) format ComputerName dans le cadre du *keyName*. \\ \\Si vous omettez ComputerName \, l’opération est effectuée par défaut sur l’ordinateur local. Le *keyName* doit inclure une clé racine valide. Les clés racines valides pour l’ordinateur local sont : HKLM, HKCU, HKCR, HKU et HKCC. Si un ordinateur distant est spécifié, les clés racines valides sont les suivantes : HKLM et HKU.|
|/v \<valueName>|Supprime une entrée spécifique sous la sous-clé. Si aucune entrée n’est spécifiée, toutes les entrées et sous-clés sous la sous-clé seront supprimées.|
|/ve|Spécifie que seules les entrées qui n’ont pas de valeur seront supprimées.|
|/va|Supprime toutes les entrées sous la sous-clé spécifiée. Les sous-clés sous la sous-clé spécifiée ne sont pas supprimées.|
|/f|Supprime la sous-clé ou l’entrée de Registre existante sans demander de confirmation.|
|/?|Affiche l’aide de la commande **reg delete** à l’invite de commandes.|

## <a name="remarks"></a>Notes 

Le tableau suivant répertorie les valeurs renvoyées pour l’opération **reg delete** .

|Valeur|Description|
|-----|-----------|
|0|Succès|
|1|Échec|

## <a name="examples"></a>Exemples

Pour supprimer le délai d’expiration de la clé de Registre et ses sous-clés et valeurs, tapez :
```
REG DELETE HKLM\Software\MyCo\MyApp\Timeout
```
Pour supprimer la valeur de Registre MTU sous HKLM\Software\MyCo sur l’ordinateur nommé ZODIAC, tapez :
```
REG DELETE \\ZODIAC\HKLM\Software\MyCo /v MTU
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)