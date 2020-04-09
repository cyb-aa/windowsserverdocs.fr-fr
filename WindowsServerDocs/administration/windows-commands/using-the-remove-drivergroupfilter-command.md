---
title: Remove-DriverGroupFilter
description: Rubrique relative aux commandes Windows pour Remove-DriverGroupFilter, qui supprime une règle de filtre d’un groupe de pilotes sur un serveur.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 837bd5d4-c79d-4714-942d-9875bd8e61dc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 08914b66c37d327ddef2a50d2f98adcfdbb88ffe
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830492"
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
|/DriverGroup : nom du groupe de\<>|Spécifie le nom du groupe de pilotes.|
|[/Server :\<nom du serveur >]|Spécifie le nom du serveur. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|[/FilterType :\<FilterType >]|Spécifie le type de filtre à supprimer du groupe. \<> FilterType peut être l’une des suivantes :</br>**BiosVendor**</br>**BiosVersion**</br>**ChassisType**</br>**Fécule**</br>**Universel**</br>**OsVersion**</br>**OsEdition**</br>**OsLanguage**|

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour supprimer un filtre, tapez l’un des éléments suivants :
```
WDSUTIL /Remove-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer
```
```
WDSUTIL /Remove-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /FilterType:OSLanguage
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)