---
title: Remove-DriverGroupFilter
description: Rubrique de référence pour Remove-DriverGroupFilter, qui supprime une règle de filtre d’un groupe de pilotes sur un serveur.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 837bd5d4-c79d-4714-942d-9875bd8e61dc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dd6fcbc8f87539ac687927b9e58ed15edb524ef6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720414"
---
# <a name="remove-drivergroupfilter"></a>Remove-DriverGroupFilter



Supprime une règle de filtre d’un groupe de pilotes sur un serveur.

## <a name="syntax"></a>Syntaxe

```
WDSUTIL /Remove-DriverGroupFilter /DriverGroup:<Group Name> [/Server:<Server name>] /FilterType:<Filter Type>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/DriverGroup :\<nom de groupe>|Spécifie le nom du groupe de pilotes.|
|[/Server :\<nom du serveur>]|Spécifie le nom du serveur. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|[/FilterType :\<FilterType>]|Spécifie le type de filtre à supprimer du groupe. \<Le> FilterType peut être l’un des suivants :</br>**BiosVendor**</br>**BiosVersion**</br>**ChassisType**</br>**Fabricant**</br>**Universel**</br>**OsVersion**</br>**OsEdition**</br>**OsLanguage**|

## <a name="examples"></a>Exemples

Pour supprimer un filtre, tapez l’un des éléments suivants :
```
WDSUTIL /Remove-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer
```
```
WDSUTIL /Remove-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /FilterType:OSLanguage
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)