---
title: À l’aide de la commande add-DriverGroup
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2a92fe8f-03f9-462a-b99e-f23275259807
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 322a9a671f90bf56f6357289f7727c142a0145cc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825090"
---
# <a name="using-the-add-drivergroup-command"></a>À l’aide de la commande add-DriverGroup

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ajoute un groupe de pilotes au serveur.
Pour obtenir des exemples de la façon dont vous pouvez utiliser cette commande, consultez [exemples](#BKMK_examples).
## <a name="syntax"></a>Syntaxe
```
wdsutil /add-DriverGroup /DriverGroup:<Group Name>\n\
[/Server:<Server name>] [/Enabled:{Yes | No}] [/Applicability:{Matched | All}] [/Filtertype:<Filter type> /Policy:{Include | Exclude} /Value:<Value> [/Value:<Value> ...]]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/ DriverGroup :<Group Name>|Spécifie le nom du nouveau groupe pilote.|
|/ Server :<Server name>|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom de domaine complet. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|/ Enabled : {Oui &#124; non}|Active ou désactive le package.|
|/ Mise en application : {mis en correspondance &#124; tous les}|Spécifie lesquelles packages à installer si les critères de filtre sont remplies. **Mise en correspondance** signifie installer uniquement les packages de pilotes qui correspondent à un matériel client s. **Tous les** signifie installer tous les packages aux clients, quel que soit leur matériel.|
|/ Filtertype :<Filtertype>|Spécifie le type du filtre à ajouter au groupe. Vous pouvez spécifier plusieurs types de filtre dans une seule commande. Chaque type de filtre doit être suivie **/Policy** et au moins un **/valeur**. <Filtertype> Peut prendre l’une des opérations suivantes :<br /><br />**BiosVendor**<br /><br />**Biosversion**<br /><br />**Chassistype**<br /><br />**Fabricant**<br /><br />**Uuid**<br /><br />**Osversion**<br /><br />**Osedition**<br /><br />**OsLanguage**<br /><br />Pour plus d’informations sur l’obtention des valeurs pour tous les autres types de filtre, consultez [filtres de groupe pilote](https://go.microsoft.com/fwlink/?LinkID=155158) (https://go.microsoft.com/fwlink/?LinkID=155158).|
|[/ Stratégie : {incluent &#124; exclure}]|Spécifie la stratégie à définir sur le filtre. Si **/Policy** a la valeur **Include**, les ordinateurs clients qui correspondent au filtre sont autorisés à installer les pilotes dans ce groupe. Si **/Policy** a la valeur **exclure**, puis les ordinateurs clients qui correspondent au filtre n’êtes pas autorisés à installer les pilotes dans ce groupe.|
|[/ Valeur :<Value>]|Spécifie la valeur de client qui correspond à **/Filtertype**. Vous pouvez spécifier plusieurs valeurs pour un type unique. Consultez la liste suivante pour les valeurs valides pour certains types de filtre. Les attributs suivants concernent **Chassistype**. Pour plus d’informations sur l’obtention des valeurs pour tous les autres types de filtre, consultez [filtres de groupe pilote](https://go.microsoft.com/fwlink/?LinkID=155158) (https://go.microsoft.com/fwlink/?LinkID=155158).<br /><br />**Autres**<br /><br />**UnknownChassis**<br /><br />**Bureau**<br /><br />**LowProfileDesktop**<br /><br />**PizzaBox**<br /><br />**MiniTower**<br /><br />**Tour**<br /><br />**Portable**<br /><br />**Laptop**<br /><br />**Notebook**<br /><br />**Handheld**<br /><br />**DockingStation**<br /><br />**AllInOne**<br /><br />**SubNotebook**<br /><br />**SpaceSaving**<br /><br />**LunchBox**<br /><br />**MainSystemChassis**<br /><br />**ExpansionChassis**<br /><br />**SubChassis**<br /><br />**BusExpansionChassis**<br /><br />**PeripheralChassis**<br /><br />**StoraeChassis**<br /><br />**RackmountChassis**<br /><br />**SealedCasecomputer**<br /><br />**MultiSystemChassis**<br /><br />**compactPci**<br /><br />**AdvancedTca**|
## <a name="BKMK_examples"></a>Exemples
Pour ajouter un groupe de pilotes, tapez une des opérations suivantes :
```
wdsutil /add-DriverGroup /DriverGroup:printerdrivers /Enabled:Yes
```
```
wdsutil /add-DriverGroup /DriverGroup:printerdrivers /Applicability:All /Filtertype:Manufacturer /Policy:Include /Value:Name1 /Filtertype:Chassistype /Policy:Exclude /Value:Tower /Value:MiniTower
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande add-DriverGroupPackage](using-the-add-drivergrouppackage-command.md)
[à l’aide de la commande add-DriverGroupPackages](using-the-add-drivergrouppackages-command.md) 
 [ À l’aide de la commande add-DriverGroupFilter](using-the-add-drivergroupfilter-command.md)
