---
title: Add-DriverGroupFilter
description: Rubrique de référence pour Add-DriverGroupFilter, qui ajoute un filtre à un groupe de pilotes sur un serveur.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a66c5e68-99ea-4e47-b68d-8109633ae336
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 45ed82940e89f4ab760fda4eed640cd62056daf8
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721119"
---
# <a name="add-drivergroupfilter"></a>Add-DriverGroupFilter

Ajoute un filtre à un groupe de pilotes sur un serveur.

## <a name="syntax"></a>Syntaxe

```
WDSUTIL /Add-DriverGroupFilter /DriverGroup:<Group Name> [/Server:<Server name>] /FilterType:<Filter Type> /Policy:{Include | Exclude} /Value:<Value> [/Value:<Value> ...]
```

### <a name="parameters"></a>Paramètres

|         Paramètre          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                            Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /DriverGroup :\<nom de groupe> |                                                                                                                                                                                                                                                                                                                                                                                                                                                              Spécifie le nom du groupe de pilotes.                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|  [/Server :\<nom du serveur>]  |                                                                                                                                                                                                                                                                                                                                                                                                               Spécifie le nom du serveur. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.                                                                                                                                                                                                                                                                                                                                                                                                               |
| /FilterType :\<FilterType>  |                                                                                                                                                                                                   Spécifie le type de filtre à ajouter au groupe. Vous pouvez spécifier plusieurs types de filtres dans une même commande. Chaque type de filtre doit être suivi de **/Policy** et inclure au moins un **/value**. \<Les> FilterType peuvent être **BiosVendor**, **BiosVersion**, **ChassisType**, **Manufacturer**, **UUID**, **OsVersion**, **OsEdition**ou **OsLanguage**. Pour plus d’informations sur l’obtention des valeurs de tous les autres types de filtres,<https://go.microsoft.com/fwlink/?LinkID=155158>voir filtres de groupes de [pilotes](https://go.microsoft.com/fwlink/?LinkID=155158) ().                                                                                                                                                                                                    |
|     [/Policy : {include      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                             Exclure}]                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|     [/Value :\<valeur>]      | Spécifie la valeur client qui correspond à **/FilterType**. Vous pouvez spécifier plusieurs valeurs pour un seul type. Consultez la liste suivante pour connaître les valeurs valides pour **ChassisType**. Pour plus d’informations sur l’obtention des valeurs de tous les autres types de filtres,<https://go.microsoft.com/fwlink/?LinkID=155158>voir filtres de groupes de [pilotes](https://go.microsoft.com/fwlink/?LinkID=155158) ().</br>**Autres**</br>**UnknownChassis**</br>**Bureau**</br>**LowProfileDesktop**</br>**PizzaBox**</br>**Tour**</br>**410**</br>**Portatif**</br>**Ordinateur portable**</br>**Notebook**</br>**Poche**</br>**DockingStation**</br>**AllInOne**</br>**Sous-bloc-notes**</br>**SpaceSaving**</br>**LunchBox**</br>**MainSystemChassis**</br>**ExpansionChassis**</br>**Sous-châssis**</br>**BusExpansionChassis**</br>**PeripheralChassis**</br>**StorageChassis**</br>**RackMountChassis**</br>**SealedCaseComputer**</br>**MultiSystemChassis**</br>**CompactPci**</br>**AdvancedTca** |

## <a name="examples"></a>Exemples

Pour ajouter un filtre à un groupe de pilotes, tapez l’un des éléments suivants :
```
WDSUTIL /Add-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /Value:Name1 /Value:Name2
```
```
WDSUTIL /Add-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /Value:Name1 /FilterType:ChassisType /Policy:Exclude /Value:Tower /Value:MiniTower
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

