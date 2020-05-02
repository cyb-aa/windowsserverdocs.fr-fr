---
title: Reg ajouter
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d9ad143e-dc10-4e2e-a229-408393c40079
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 85880a1a0fd92dca1f203d3b29df5300fab4eb00
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722600"
---
# <a name="reg-add"></a>Reg ajouter


Ajoute une nouvelle sous-clé ou une entrée au registre.

## <a name="syntax"></a>Syntaxe

```
reg add <KeyName> [{/v ValueName | /ve}] [/t DataType] [/s Separator] [/d Data] [/f]
```

### <a name="parameters"></a>Paramètres

|      Paramètre      |                                                                                                                                                                                                                                                                   Description                                                                                                                                                                                                                                                                   |
|---------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| \<KeyName<em>></em> | Spécifie le chemin d’accès complet de la sous-clé ou de l’entrée à ajouter. Pour spécifier un ordinateur distant, incluez le nom de l’ordinateur ( \\ \\ \<au format \) ComputerName>dans le cadre du *keyName*. \\ \\Si vous omettez ComputerName \, l’opération est effectuée par défaut sur l’ordinateur local. Le *keyName* doit inclure une clé racine valide. Les clés racines valides pour l’ordinateur local sont : HKLM, HKCU, HKCR, HKU et HKCC. Si un ordinateur distant est spécifié, les clés racines valides sont les suivantes : HKLM et HKU. Si le nom de la clé de registre contient un espace, mettez-le entre guillemets. |
|   /v \<valueName>   |                                                                                                                                                                                                                                Spécifie le nom de l’entrée de Registre à ajouter dans la sous-clé spécifiée.                                                                                                                                                                                                                                 |
|         /ve         |                                                                                                                                                                                                                                Spécifie que l’entrée de Registre ajoutée au registre a une valeur null.                                                                                                                                                                                                                                |
|     /t \<> de type      |                                                                                                                                          Spécifie le type de l’entrée de registre. Le *type* doit être l’un des suivants :</br>REG_SZ</br>REG_MULTI_SZ</br>REG_DWORD_BIG_ENDIAN</br>REG_DWORD</br>REG_BINARY</br>REG_DWORD_LITTLE_ENDIAN</br>REG_LINK</br>REG_FULL_RESOURCE_DESCRIPTOR</br>REG_EXPAND_SZ                                                                                                                                          |
|   > \<séparateur/s   |                                                                                                                                                              Spécifie le caractère à utiliser pour séparer plusieurs instances de données lorsque le type de données REG_MULTI_SZ est spécifié et que plusieurs entrées doivent être listées. S’il n’est pas spécifié, le séparateur par défaut est **\ 0**.                                                                                                                                                              |
|     /d \<> de données      |                                                                                                                                                                                                                                                 Spécifie les données de la nouvelle entrée de registre.                                                                                                                                                                                                                                                  |
|         /f          |                                                                                                                                                                                                                                           Ajoute l’entrée de registre sans demander confirmation.                                                                                                                                                                                                                                           |
|         /?          |                                                                                                                                                                                                                                              Affiche l’aide pour **reg Add** à l’invite de commandes.                                                                                                                                                                                                                                               |

## <a name="remarks"></a>Notes 

-   Impossible d’ajouter des sous-arborescences avec cette opération. Cette version de **reg** ne demande pas de confirmation lors de l’ajout d’une sous-clé.
-   Le tableau suivant répertorie les valeurs renvoyées pour l’opération **reg Add** .

| Valeur | Description |
|-------|-------------|
|   0   |   Succès   |
|   1   |   Échec   |

-   Pour le type de clé REG_EXPAND_SZ, utilisez le symbole de signe **^** insertion ( **%** ) avec l’intérieur du paramètre/d

## <a name="examples"></a>Exemples

Pour ajouter la clé HKLM\Software\MyCo sur l’ordinateur distant ABC, tapez :
```
REG ADD \\ABC\HKLM\Software\MyCo
```
Pour ajouter une entrée de Registre à HKLM\Software\MyCo avec une valeur nommée **Data** de type REG_BINARY et des données de **fe340ead**, tapez :
```
REG ADD HKLM\Software\MyCo /v Data /t REG_BINARY /d fe340ead
```
Pour ajouter une entrée de Registre à plusieurs valeurs à HKLM\Software\MyCo avec un nom de valeur **MRU** de type REG_MULTI_SZ et des données de **fax\0mail\0\0**, tapez :
```
REG ADD HKLM\Software\MyCo /v MRU /t REG_MULTI_SZ /d fax\0mail\0\0
```
Pour ajouter une entrée de Registre développée à HKLM\Software\MyCo avec un nom de valeur **chemin d’accès** de type REG_EXPAND_SZ et les données de **% systemroot%**, tapez :
```
REG ADD HKLM\Software\MyCo /v Path /t REG_EXPAND_SZ /d ^%systemroot^%
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
