---
title: À l’aide de la commande add-DriverGroupFilter
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a66c5e68-99ea-4e47-b68d-8109633ae336
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 16962bd87fabd1ee689b962547c2b1aa470283c1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817380"
---
# <a name="using-the-add-drivergroupfilter-command"></a>À l’aide de la commande add-DriverGroupFilter



Ajoute un filtre à un groupe de pilotes sur un serveur.

## <a name="syntax"></a>Syntaxe

```
WDSUTIL /Add-DriverGroupFilter /DriverGroup:<Group Name> [/Server:<Server name>] /FilterType:<Filter Type> /Policy:{Include | Exclude} /Value:<Value> [/Value:<Value> ...]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/ DriverGroup :\<nom du groupe >|Spécifie le nom du groupe pilote.|
|[/ Server :\<nom du serveur >]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom de domaine complet. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|/FilterType:\<FilterType>|Spécifie le type de filtre à ajouter au groupe. Vous pouvez spécifier plusieurs types de filtre dans une seule commande. Chaque type de filtre doit être suivie **/Policy** et inclure au moins un **/valeur**. \<FilterType > peut être **BiosVendor**, **BiosVersion**, **ChassisType**, **fabricant**, **Uuid**, **OsVersion**, **OsEdition**, ou **OsLanguage**. Pour plus d’informations sur l’obtention des valeurs pour tous les autres types de filtre, consultez [filtres de groupe pilote](https://go.microsoft.com/fwlink/?LinkID=155158) (https://go.microsoft.com/fwlink/?LinkID=155158).|
|[/ Stratégie : {inclure | Exclude}]|Si **/Policy** a la valeur **Include**, les ordinateurs clients qui correspondent au filtre sont autorisés à installer les pilotes dans ce groupe. Si **/Policy** a la valeur **exclure**, les ordinateurs clients qui correspondent au filtre n’êtes pas autorisés à installer les pilotes dans ce groupe.|
|[/ Valeur :\<valeur >]|Spécifie la valeur de client qui correspond à **/FilterType**. Vous pouvez spécifier plusieurs valeurs pour un type unique. Consultez la liste suivante pour les valeurs valides pour **ChassisType**. Pour plus d’informations sur l’obtention des valeurs pour tous les autres types de filtre, consultez [filtres de groupe pilote](https://go.microsoft.com/fwlink/?LinkID=155158) (https://go.microsoft.com/fwlink/?LinkID=155158).</br>**Autres**</br>**UnknownChassis**</br>**Bureau**</br>**LowProfileDesktop**</br>**PizzaBox**</br>**MiniTower**</br>**Tour**</br>**Portable**</br>**Laptop**</br>**Notebook**</br>**Handheld**</br>**DockingStation**</br>**AllInOne**</br>**SubNotebook**</br>**SpaceSaving**</br>**LunchBox**</br>**MainSystemChassis**</br>**ExpansionChassis**</br>**SubChassis**</br>**BusExpansionChassis**</br>**PeripheralChassis**</br>**StorageChassis**</br>**RackMountChassis**</br>**SealedCaseComputer**</br>**MultiSystemChassis**</br>**CompactPci**</br>**AdvancedTca**|

## <a name="BKMK_examples"></a>Exemples

Pour ajouter un filtre à un groupe de pilotes, tapez une des opérations suivantes :
```
WDSUTIL /Add-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /Value:Name1 /Value:Name2
```
```
WDSUTIL /Add-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /Value:Name1 /FilterType:ChassisType /Policy:Exclude /Value:Tower /Value:MiniTower
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)

