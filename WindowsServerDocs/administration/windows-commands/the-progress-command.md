---
title: avancement
description: Rubrique relative aux commandes Windows pour la progression, qui affiche la progression pendant l’exécution d’une commande.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8ce5e77b-e13f-4ac3-948d-31770a6c7e25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9957174203df8f2f5c02bf3ab8a4f3406701a8da
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833122"
---
# <a name="progress"></a>avancement

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