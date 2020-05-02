---
title: Sous-commande Set-DriverGroupFilter
description: Rubrique de référence pour la sous-commande Set-DriverGroupFilter, qui ajoute ou supprime un filtre de groupe de pilotes existant d’un groupe de pilotes.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 829ab1f0-4514-421e-9cc0-767b238da69c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f8a8d3567785b54a722b1787c519de7eb0b7a229
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721731"
---
# <a name="subcommand-set-drivergroupfilter"></a>Sous-commande : Set-DriverGroupFilter

Ajoute ou supprime un filtre de groupe de pilotes existant d’un groupe de pilotes.

## <a name="syntax"></a>Syntaxe

```
WDSUTIL /Set-DriverGroupFilter /DriverGroup:<Group Name> [/Server:<Server name>] /FilterType:<Filter Type> [/Policy:{Include | Exclude}] [/AddValue:<Value> [/AddValue:<Value> ...]] [/RemoveValue:<Value> [/RemoveValue:<Value> ...]]
```

### <a name="parameters"></a>Paramètres

|         Paramètre          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /DriverGroup :\<nom de groupe> |                                                                                                                                                                                                                                                                                                                                                                                                                                                                 Spécifie le nom du groupe de pilotes.                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|  [/Server :\<nom du serveur>]  |                                                                                                                                                                                                                                                                                                                                                                                                                Spécifie le nom du serveur. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.                                                                                                                                                                                                                                                                                                                                                                                                                 |
| /FilterType :\<FilterType>  |                                                                                                                                                                                                                                                                       Spécifie le type de filtre de groupe de pilotes à ajouter ou supprimer. Vous pouvez spécifier plusieurs filtres dans une même commande. Pour chaque **/FilterType**, vous pouvez ajouter ou supprimer plusieurs valeurs à l’aide de **/RemoveValue** et **/AddValue**. \<Le> FilterType peut être l’un des suivants :</br>**BiosVendor**</br>**BiosVersion**</br>**ChassisType**</br>**Fabricant**</br>**Universel**</br>**OsVersion**</br>**OsEdition**</br>**OsLanguage**                                                                                                                                                                                                                                                                        |
|     [/Policy : {include      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                Exclure}]                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
|    [/AddValue :\<valeur>]    | Spécifie la nouvelle valeur client à ajouter au filtre. Vous pouvez spécifier plusieurs valeurs pour un type de filtre unique. Consultez la liste suivante pour connaître les valeurs d’attribut valides pour **ChassisType**. Pour plus d’informations sur l’obtention des valeurs de tous les autres types de filtres,<https://go.microsoft.com/fwlink/?LinkID=155158>voir filtres de groupes de [pilotes](https://go.microsoft.com/fwlink/?LinkID=155158) ().</br>**Autres**</br>**UnknownChassis**</br>**Bureau**</br>**LowProfileDesktop**</br>**PizzaBox**</br>**Tour**</br>**410**</br>**Portatif**</br>**Ordinateur portable**</br>**Notebook**</br>**Poche**</br>**DockingStation**</br>**AllInOne**</br>**Sous-bloc-notes**</br>**SpaceSaving**</br>**LunchBox**</br>**MainSystemChassis**</br>**ExpansionChassis**</br>**Sous-châssis**</br>**BusExpansionChassis**</br>**PeripheralChassis**</br>**StorageChassis**</br>**RackMountChassis**</br>**SealedCaseComputer**</br>**MultiSystemChassis**</br>**CompactPci**</br>**AdvancedTca** |
|  [/RemoveValue :\<valeur>]   |                                                                                                                                                                                                                                                                                                                                                                                                                                     Spécifie la valeur client existante à supprimer du filtre, comme spécifié avec **/AddValue**.                                                                                                                                                                                                                                                                                                                                                                                                                                      |

## <a name="examples"></a>Exemples

Pour supprimer un filtre, tapez l’un des éléments suivants :
```
WDSUTIL /Set-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /AddValue:Name1 /RemoveValue:Name2
```
```
WDSUTIL /Set-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /RemoveValue:Name1 /FilterType:ChassisType /Policy:Exclude /AddValue:Tower /AddValue:MiniTower
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)