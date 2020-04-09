---
title: reg supprimer
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cee05071-1607-4ab1-b8ab-65caebeb85c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 726a3c700a9278dbc7abb1873aae7ea3c957bbb5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836502"
---
# <a name="reg-delete"></a>reg supprimer



Supprime une sous-clé ou des entrées du Registre.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
Reg delete <KeyName> [{/v ValueName | /ve | /va}] [/f]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<KeyName >|Spécifie le chemin d’accès complet de la sous-clé ou de l’entrée à supprimer. Pour spécifier un ordinateur distant, incluez le nom de l’ordinateur (au format \\\\ComputerName\) dans le *nom*de l’ordinateur. Si vous omettez \\\\ComputerName \, l’opération est effectuée par défaut sur l’ordinateur local. Le *keyName* doit inclure une clé racine valide. Les clés racines valides pour l’ordinateur local sont : HKLM, HKCU, HKCR, HKU et HKCC. Si un ordinateur distant est spécifié, les clés racines valides sont les suivantes : HKLM et HKU.|
|/v \<ValueName >|Supprime une entrée spécifique sous la sous-clé. Si aucune entrée n’est spécifiée, toutes les entrées et sous-clés sous la sous-clé seront supprimées.|
|/ve|Spécifie que seules les entrées qui n’ont pas de valeur seront supprimées.|
|/va|Supprime toutes les entrées sous la sous-clé spécifiée. Les sous-clés sous la sous-clé spécifiée ne sont pas supprimées.|
|/f|Supprime la sous-clé ou l’entrée de Registre existante sans demander de confirmation.|
|/?|Affiche l’aide de la commande **reg delete** à l’invite de commandes.|

## <a name="remarks"></a>Notes

Le tableau suivant répertorie les valeurs renvoyées pour l’opération **reg delete** .

|Valeur|Description|
|-----|-----------|
|0|Opération réussie|
|1|Échec|

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

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