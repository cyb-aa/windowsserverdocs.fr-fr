---
title: Mettre à jour-ServerFiles
description: Rubrique de référence pour Update-ServerFiles, qui met à jour les fichiers dans le dossier partagé REMINST à l’aide des fichiers les plus récents stockés dans le dossier%Windir%\System32\RemInst du serveur.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 23aa79df-38c6-401e-91bd-cd23811b30b4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0005d8e198300c4aad9fdfc772957b460d6fee74
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721381"
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
|[/Server :\<nom du serveur>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|

## <a name="examples"></a>Exemples

Pour mettre à jour les fichiers, tapez l’un des éléments suivants :
```
WDSUTIL /Update-ServerFiles
WDSUTIL /Verbose /Progress /Update-ServerFiles /Server:MyWDSServer
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)