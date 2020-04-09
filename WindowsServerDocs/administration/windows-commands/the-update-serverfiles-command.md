---
title: Mettre à jour-ServerFiles
description: La rubrique commandes Windows pour Update-ServerFiles, qui met à jour les fichiers dans le dossier partagé REMINST à l’aide des fichiers les plus récents stockés dans le dossier%Windir%\System32\RemInst du serveur.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 23aa79df-38c6-401e-91bd-cd23811b30b4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 37cbb880246cf5e5ff6a9e007dbe720de8dd1cbe
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832952"
---
# <a name="update-serverfiles"></a>Mettre à jour-ServerFiles

Met à jour les fichiers dans le dossier partagé REMINST à l’aide des fichiers les plus récents stockés dans le dossier%Windir%\System32\RemInst du serveur. Pour garantir la validité de votre installation des services de déploiement Windows, vous devez exécuter cette commande une seule fois après chaque mise à niveau du serveur, Service Pack installation ou mise à jour des fichiers des services de déploiement Windows.

## <a name="syntax"></a>Syntaxe

```
WDSUTIL [Options] /Update-ServerFiles [/Server:<Server name>]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[/Server :\<nom du serveur >]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour mettre à jour les fichiers, tapez l’un des éléments suivants :
```
WDSUTIL /Update-ServerFiles
WDSUTIL /Verbose /Progress /Update-ServerFiles /Server:MyWDSServer
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)