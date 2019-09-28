---
title: Commande de progression
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 841d9103354e3162489492ba7dd97e726b37d37d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370179"
---
# <a name="the-progress-command"></a>Commande de progression



Affiche la progression pendant l’exécution d’une commande. Vous pouvez utiliser **/Progress** avec les autres commandes WDSUTIL que vous exécutez. Notez que vous devez spécifier **/Verbose** et **/Progress** directement après **WDSUTIL**.

## <a name="syntax"></a>Syntaxe

```
WDSUTIL /progress <commands>
```

## <a name="examples"></a>Exemples

Pour initialiser le serveur et afficher la progression, tapez :
```
WDSUTIL /Verbose /Progress /Initialize-Server /Server:MyWDSServer /RemInst:"C:\RemoteInstall"
```