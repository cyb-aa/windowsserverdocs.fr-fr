---
title: Ajout de reg
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d9ad143e-dc10-4e2e-a229-408393c40079
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d46fc2df23391a1dbb782014addc68d9522d603a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441916"
---
# <a name="reg-add"></a>Ajout de reg


Ajoute une nouvelle sous-clé ou une entrée au Registre.

## <a name="syntax"></a>Syntaxe

```
reg add <KeyName> [{/v ValueName | /ve}] [/t DataType] [/s Separator] [/d Data] [/f]
```
Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="parameters"></a>Paramètres

|      Paramètre      |                                                                                                                                                                                                                                                                   Description                                                                                                                                                                                                                                                                   |
|---------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| \<KeyName<em>></em> | Spécifie le chemin d’accès complet de la sous-clé ou l’entrée à ajouter. Pour spécifier un ordinateur distant, incluez le nom d’ordinateur (au format \\ \\ \<nom_ordinateur >\) dans le cadre de la *KeyName*. En omettant \\ \\ComputerName\, l’opération par défaut sur l’ordinateur local. Le *KeyName* doit inclure une clé racine valide. Les clés racine valide pour l’ordinateur local sont : HKLM, HKCU, HKCR, HKU et HKCC. Si un ordinateur distant est spécifié, les clés racines valides sont : HKLM et HKU. Si le nom de clé de Registre contient un espace, placez le nom de clé entre guillemets. |
|   /v \<ValueName>   |                                                                                                                                                                                                                                Spécifie le nom de l’entrée de Registre à ajouter sous la sous-clé spécifiée.                                                                                                                                                                                                                                 |
|         /ve         |                                                                                                                                                                                                                                Spécifie que l’entrée de Registre qui est ajoutée au Registre a une valeur null.                                                                                                                                                                                                                                |
|     /t \<Type>      |                                                                                                                                          Spécifie le type pour l’entrée de Registre. *Type* doit être une des opérations suivantes :</br>REG_SZ</br>REG_MULTI_SZ</br>REG_DWORD_BIG_ENDIAN</br>REG_DWORD</br>REG_BINARY</br>REG_DWORD_LITTLE_ENDIAN</br>REG_LINK</br>REG_FULL_RESOURCE_DESCRIPTOR</br>REG_EXPAND_SZ                                                                                                                                          |
|   /s \<séparateur >   |                                                                                                                                                              Spécifie le caractère à utiliser pour séparer plusieurs instances de données lorsque le type de données REG_MULTI_SZ est spécifié, et plus d’une entrée doit être répertorié. Si non spécifié, le séparateur par défaut est **\0**.                                                                                                                                                              |
|     /d \<Data>      |                                                                                                                                                                                                                                                 Spécifie les données de la nouvelle entrée de Registre.                                                                                                                                                                                                                                                  |
|         /f          |                                                                                                                                                                                                                                           Ajoute l’entrée de Registre sans demander confirmation.                                                                                                                                                                                                                                           |
|         /?          |                                                                                                                                                                                                                                              Affiche l’aide de **reg ajouter** à l’invite de commandes.                                                                                                                                                                                                                                               |

## <a name="remarks"></a>Notes

-   Sous-arborescences ne peut pas être ajoutés à cette opération. Cette version de **reg** ne pas demander confirmation lors de l’ajout d’une sous-clé.
-   Le tableau suivant répertorie les valeurs de retour pour la **reg ajouter** opération.

| Value | Description |
|-------|-------------|
|   0   |   Opération réussie   |
|   1   |   Échec   |

-   Pour le type de clé REG_EXPAND_SZ, utilisez le symbole de signe insertion ( **^** ) avec **%** « dans le paramètre /d

## <a name="BKMK_examples"></a>Exemples

Pour ajouter la clé HKLM\Software\MaClé sur l’ordinateur distant ABC, tapez :
```
REG ADD \\ABC\HKLM\Software\MyCo
```
Pour ajouter une entrée de Registre à HKLM\Software\MyCo avec une valeur nommée **données** de type REG_BINARY et les données de **fe340ead**, type :
```
REG ADD HKLM\Software\MyCo /v Data /t REG_BINARY /d fe340ead
```
Pour ajouter une entrée de Registre à valeurs multiples à HKLM\Software\MyCo avec un nom de la valeur de **MRU** de type REG_MULTI_SZ et les données de **fax\0mail\0\0**, type :
```
REG ADD HKLM\Software\MyCo /v MRU /t REG_MULTI_SZ /d fax\0mail\0\0
```
Pour ajouter une entrée de Registre étendue à HKLM\Software\MyCo avec un nom de la valeur de **chemin d’accès** de type REG_EXPAND_SZ et les données de **%SystemRoot%** , type :
```
REG ADD HKLM\Software\MyCo /v Path /t REG_EXPAND_SZ /d ^%systemroot^%
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
