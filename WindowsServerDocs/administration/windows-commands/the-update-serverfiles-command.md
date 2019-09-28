---
title: Commande Update-ServerFiles
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 23aa79df-38c6-401e-91bd-cd23811b30b4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 93eeb0deaa527921db35f4ab955d2ccc46b57d7a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385850"
---
# <a name="the-update-serverfiles-command"></a>Commande Update-ServerFiles



Met à jour les fichiers dans le dossier partagé REMINST à l’aide des fichiers les plus récents stockés dans le dossier%Windir%\System32\RemInst du serveur. Pour garantir la validité de votre installation des services de déploiement Windows, vous devez exécuter cette commande une seule fois après chaque mise à niveau du serveur, Service Pack installation ou mise à jour des fichiers des services de déploiement Windows.

## <a name="syntax"></a>Syntaxe

```
WDSUTIL [Options] /Update-ServerFiles [/Server:<Server name>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[/Server : @no__t-nom 0Server >]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|

## <a name="BKMK_examples"></a>Illustre

Pour mettre à jour les fichiers, tapez l’un des éléments suivants :
```
WDSUTIL /Update-ServerFiles
WDSUTIL /Verbose /Progress /Update-ServerFiles /Server:MyWDSServer
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)