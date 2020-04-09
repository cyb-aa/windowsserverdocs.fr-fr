---
title: Remove-DriverGroupPackage
description: Rubrique relative aux commandes Windows pour Remove-DriverGroupPackage, qui supprime un package de pilotes d’un groupe de pilotes sur un serveur.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2e48616d-d6a4-45f0-a5c6-efe62bf6a0ed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9dd30f430bd91bf2680cd1138270526bce965f9d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830462"
---
# <a name="remove-drivergrouppackage"></a>Remove-DriverGroupPackage



supprime un package de pilotes d’un groupe de pilotes sur un serveur.

## <a name="syntax"></a>Syntaxe

```
WDSUTIL /Remove-DriverGroupPackage /DriverGroup:<Group Name> [/Server:<Server Name>] {/DriverPackage:<Name> | /PackageId:<ID>}
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[/Server :\<nom du serveur >]|Spécifie le nom du serveur. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|[/DriverPackage : nom de\<>]|Spécifie le nom du package de pilotes à supprimer.|
|[/PackageId : ID de\<>]|Spécifie l’ID des services de déploiement Windows du package de pilotes à supprimer. Vous devez spécifier cette option si le package de pilotes ne peut pas être identifié de manière unique par son nom.|

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

```
WDSUTIL /Remove-DriverGroupPackage /DriverGroup:PrinterDrivers /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318}
```
```
WDSUTIL /Remove-DriverGroupPackage /DriverGroup:PrinterDrivers /DriverPackage:XYZ
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)