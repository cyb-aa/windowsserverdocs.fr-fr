---
title: nfsstat
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: da7a9768-44bd-404b-97ee-c388d00dc395
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4f9db119596b5602f18acfa10af6aa1b7cbbc9b2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373180"
---
# <a name="nfsstat"></a>nfsstat



Vous pouvez utiliser **nfsstat** pour afficher ou réinitialiser le nombre d’appels effectués vers le serveur pour NFS.

## <a name="syntax"></a>Syntaxe

```
nfsstat [-z]
```

## <a name="description"></a>Description

Lorsqu’il est utilisé sans l’option **-z** , l’utilitaire de ligne de commande **nfsstat** affiche le nombre d’appels NFS v2, NFS v3 et Mount v3 effectués au serveur depuis que les compteurs ont été définis sur 0, au moment du démarrage du service ou de la réinitialisation des compteurs à l’aide **de nfsstat-z**.