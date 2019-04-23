---
title: Sous-commande DriverGroupFilter de jeu
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 829ab1f0-4514-421e-9cc0-767b238da69c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 339beda0d6e92c7632cb16566c8db7be5f1079af
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835060"
---
# <a name="subcommand-set-drivergroupfilter"></a>Subcommand: set-DriverGroupFilter



Ajoute ou supprime un filtre de groupe de pilotes existant à partir d’un groupe de pilotes.

## <a name="syntax"></a>Syntaxe

```
WDSUTIL /Set-DriverGroupFilter /DriverGroup:<Group Name> [/Server:<Server name>] /FilterType:<Filter Type> [/Policy:{Include | Exclude}] [/AddValue:<Value> [/AddValue:<Value> ...]] [/RemoveValue:<Value> [/RemoveValue:<Value> ...]]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/ DriverGroup :\<nom du groupe >|Spécifie le nom du groupe pilote.|
|[/ Server :\<nom du serveur >]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom de domaine complet. Si un nom de serveur n’est pas spécifié, le serveur local est utilisé.|
|/FilterType:\<FilterType>|Spécifie le type de filtre de groupe pilote pour ajouter ou supprimer. Vous pouvez spécifier plusieurs filtres dans une seule commande. Pour chaque **/FilterType**, vous pouvez ajouter ou supprimer plusieurs valeurs à l’aide de **/RemoveValue** et **/AddValue**. \<FilterType > peut prendre l’une des opérations suivantes :</br>**BiosVendor**</br>**BiosVersion**</br>**ChassisType**</br>**Fabricant**</br>**Uuid**</br>**OsVersion**</br>**OsEdition**</br>**OsLanguage**|
|[/ Stratégie : {inclure | Exclude}]|Spécifie la nouvelle stratégie à définir sur le filtre. Si **/Policy** a la valeur **Include**, les ordinateurs clients qui correspondent au filtre sont autorisés à installer les pilotes dans ce groupe. Si **/Policy** a la valeur **exclure**, puis les ordinateurs clients qui correspondent au filtre n’êtes pas autorisés à installer les pilotes dans ce groupe.|
|[/ AddValue :\<valeur >]|Spécifie la nouvelle valeur de client à ajouter au filtre. Vous pouvez spécifier plusieurs valeurs pour un type de filtre unique. Consultez la liste suivante des valeurs d’attribut valide pour **ChassisType**. Pour plus d’informations sur l’obtention des valeurs pour tous les autres types de filtre, consultez [filtres de groupe pilote](https://go.microsoft.com/fwlink/?LinkID=155158) (https://go.microsoft.com/fwlink/?LinkID=155158).</br>**Autres**</br>**UnknownChassis**</br>**Bureau**</br>**LowProfileDesktop**</br>**PizzaBox**</br>**MiniTower**</br>**Tour**</br>**Portable**</br>**Laptop**</br>**Notebook**</br>**Handheld**</br>**DockingStation**</br>**AllInOne**</br>**SubNotebook**</br>**SpaceSaving**</br>**LunchBox**</br>**MainSystemChassis**</br>**ExpansionChassis**</br>**SubChassis**</br>**BusExpansionChassis**</br>**PeripheralChassis**</br>**StorageChassis**</br>**RackMountChassis**</br>**SealedCaseComputer**</br>**MultiSystemChassis**</br>**CompactPci**</br>**AdvancedTca**|
|[/ RemoveValue :\<valeur >]|Spécifie la valeur de client existant à supprimer le filtre spécifié avec **/AddValue**.|

## <a name="BKMK_examples"></a>Exemples

Pour supprimer un filtre, tapez une des opérations suivantes :
```
WDSUTIL /Set-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /AddValue:Name1 /RemoveValue:Name2
```
```
WDSUTIL /Set-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /RemoveValue:Name1 /FilterType:ChassisType /Policy:Exclude /AddValue:Tower /AddValue:MiniTower
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)