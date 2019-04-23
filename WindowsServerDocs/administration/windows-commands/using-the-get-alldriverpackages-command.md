---
title: À l’aide de la commande get-AllDriverPackages
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9eb8fcb7-ef46-4c8d-9623-8969a3c8b8a4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7e531d9e175ba35aa092c1ef95ec5fe7d1eddff6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843650"
---
# <a name="using-the-get-alldriverpackages-command"></a>À l’aide de la commande get-AllDriverPackages

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche des informations sur les packages de pilotes sur un serveur qui correspondent aux critères de recherche spécifiés.
## <a name="syntax"></a>Syntaxe
```
wdsutil /Get-AllDriverPackages [/Server:<Server name>] [/Show:{Drivers | Files | All}] [/Filtertype:<Filter type> /Operator:{Equal | NotEqual | GreaterOrEqual | LessOrEqual | Contains} /Value:<Value> [/Value:<Value> ...]]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Le nom du serveur. Cela peut être le nom NetBIOS ou le nom de domaine complet. Si un nom de serveur n’est pas spécifié, le serveur local est utilisé.|
|[/ Afficher : {pilotes &#124; fichiers &#124; tous les}]|Indique les informations de package à afficher. Si **/afficher** n’est pas spécifié, la valeur par défaut est de retourner uniquement le pilote les métadonnées du package. **Pilotes** affiche la liste des pilotes dans le package. **Fichiers** affiche la liste des fichiers dans le package. **Tous les** affiche les pilotes et les fichiers.|
|/ Filtertype :<Filter type>|Spécifie l’attribut du package de pilotes à rechercher. Vous pouvez spécifier plusieurs attributs dans une seule commande. Vous devez également spécifier **/Operator** et **/valeur** avec cette option.<br /><br /><Filter type> Peut prendre l’une des opérations suivantes :<br /><br />**PackageId**<br /><br />**PackageName**<br /><br />**PackageEnabled**<br /><br />**Packagedateadded**<br /><br />**PackageInfFilename**<br /><br />**PackageClass**<br /><br />**PackageProvider**<br /><br />**PackageArchitecture**<br /><br />**PackageLocale**<br /><br />**PackageSigned**<br /><br />**PackagedatePublished**<br /><br />**Packageversion**<br /><br />**Driverdescription**<br /><br />**DriverManufacturer**<br /><br />**DriverHardwareId**<br /><br />**DrivercompatibleId**<br /><br />**DriverExcludeId**<br /><br />**DriverGroupId**<br /><br />**DriverGroupName**|
|/Operator:{Equal &#124; NotEqual &#124; GreaterOrEqual &#124; LessOrEqual &#124; Contains}|Spécifie la relation entre l’attribut et les valeurs. Vous pouvez spécifier **Contains** uniquement des attributs de chaîne. Vous pouvez spécifier **GreaterOrEqual** et **LessOrEqual** uniquement avec les attributs de date et de version.|
|/ Valeur :<Value>|Spécifie la valeur à rechercher sur spécifié <attribute>.  Vous pouvez spécifier plusieurs valeurs pour un seul **/Filtertype**. La liste ci-dessous décrit les attributs que vous pouvez spécifier pour chaque filtre. Pour plus d’informations sur ces attributs, consultez [les attributs de pilote et le Package](https://go.microsoft.com/fwlink/?LinkId=166895) (https://go.microsoft.com/fwlink/?LinkId=166895).<br /><br />-PackageId - spécifiez un GUID valide. Par exemple : {4d36e972-e325-11ce-bfc1-08002be10318}.<br />-PackageName spécifier toute valeur de chaîne.<br />-Spécifiez PackageEnabled - **Oui** ou **non**.<br />-Packagedateadded - spécifier la date au format suivant : AAAA/MM/JJ<br />-PackageInfFilename spécifier toute valeur de chaîne.<br />-PackageClass - spécifiez un nom de classe valide ou le GUID de la classe. Exemple : **Lecteur de disque**, **Net**, ou {4d36e972-e325-11ce-bfc1-08002be10318}.<br />-PackageProvider spécifier toute valeur de chaîne.<br />-Spécifiez PackageArchitecture - **x86**, **x64**, ou **ia64**.<br />-PackagLocale - spécifier un identificateur de langue valide. Par exemple : **en-US** ou **es-ES**.<br />-Spécifiez PackageSigned - **Oui** ou **non**.<br />-PackagedatePublished - spécifier la date au format suivant : AAAA/MM/JJ<br />-Packageversion spécifier la version au format suivant : a.b.x.y. Exemple : 6.1.0.0<br />-Driverdescription spécifier toute valeur de chaîne.<br />-DriverManufacturer spécifier toute valeur de chaîne.<br />-DriverHardwareId - spécifier n’importe quelle valeur de chaîne.<br />-DrivercompatibleId - spécifier n’importe quelle valeur de chaîne.<br />-DriverExcludeId spécifier toute valeur de chaîne.<br />-DriverGroupId spécifiez un GUID valide. Par exemple : {4d36e972-e325-11ce-bfc1-08002be10318}.<br />-DriverGroupName spécifier toute valeur de chaîne.|
## <a name="BKMK_examples"></a>Exemples
Pour afficher plus d’informations, tapez une des opérations suivantes :
```
wdsutil /Get-AllDriverPackages /Server:MyWdsServer /Show:All /Filtertype:DriverGroupName /Operator:Contains /Value:printer /Filtertype:PackageArchitecture /Operator:Equal /Value:x64 /Value:x86
```
```
wdsutil /Get-AllDriverPackages /Show:Drivers /Filtertype:Packagedateadded /Operator:GreaterOrEqual /Value:2008/01/01
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande get-DriverPackage](using-the-get-driverpackage-command.md)
[à l’aide de la commande get-DriverPackageFile](using-the-get-driverpackagefile-command.md)