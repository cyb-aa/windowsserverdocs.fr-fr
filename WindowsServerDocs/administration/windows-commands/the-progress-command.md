---
title: La commande de progression
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8ce5e77b-e13f-4ac3-948d-31770a6c7e25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5fff31c91b4d267011f2d738b4fc3acb3f0f2377
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832700"
---
# <a name="the-progress-command"></a>La commande de progression



Affiche les progrès lorsqu’une commande est en cours d’exécution. Vous pouvez utiliser **/progression** avec toutes les autres commandes WDSUTIL que vous exécutez. Notez que vous devez spécifier **/verbose** et **/progression** directement après **WDSUTIL**.

## <a name="syntax"></a>Syntaxe

```
WDSUTIL /progress <commands>
```

## <a name="examples"></a>Exemples

Pour initialiser le serveur et afficher la progression, tapez :
```
WDSUTIL /Verbose /Progress /Initialize-Server /Server:MyWDSServer /RemInst:"C:\RemoteInstall"
```