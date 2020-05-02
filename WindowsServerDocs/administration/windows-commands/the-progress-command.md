---
title: progress
description: Rubrique de référence pour la progression, qui affiche la progression pendant l’exécution d’une commande.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8ce5e77b-e13f-4ac3-948d-31770a6c7e25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3fcb06304df22208cef013d73a4ebf0af1991f88
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721432"
---
# <a name="progress"></a>progress

Affiche la progression pendant l’exécution d’une commande. Vous pouvez utiliser **/Progress** avec les autres commandes WDSUTIL que vous exécutez. Notez que vous devez spécifier **/Verbose** et **/Progress** directement après **WDSUTIL**.

## <a name="syntax"></a>Syntaxe

```
WDSUTIL /progress <commands>
```

## <a name="examples"></a>Exemples

Pour initialiser le serveur et afficher la progression, tapez :
```
WDSUTIL /Verbose /Progress /Initialize-Server /Server:MyWDSServer /RemInst:C:\RemoteInstall
```