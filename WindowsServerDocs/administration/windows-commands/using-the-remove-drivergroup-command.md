---
title: Remove-DriverGroup
description: Rubrique relative aux commandes Windows pour Remove-DriverGroup, qui supprime un groupe de pilotes d’un serveur.
ms.prod: windows-server
ms.topic: article
ms.assetid: 1fefe9df-9782-433c-8abe-3f1a35e50da2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 56622c30b8b0af88a57c476eb4f03d598703d603
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830522"
---
# <a name="remove-drivergroup"></a>Remove-DriverGroup

Supprime un groupe de pilotes d’un serveur.

## <a name="syntax"></a>Syntaxe

```
WDSUTIL /Remove-DriverGroup /DriverGroup:<Group Name> [/Server:<Server name>]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/DriverGroup : nom du groupe de\<>|Spécifie le nom du groupe de pilotes à supprimer.|
|[/Server :\<nom du serveur >]|Spécifie le nom du serveur. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour supprimer un groupe de pilotes, tapez l’un des éléments suivants :
```
WDSUTIL /Remove-DriverGroup /DriverGroup:PrinterDrivers
```
```
WDSUTIL /Remove-DriverGroup /DriverGroup:PrinterDrivers /Server:MyWdsServer
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)