---
title: nfsstat
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: da7a9768-44bd-404b-97ee-c388d00dc395
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 94eb389fdd694c08dcd1d77325f145a81e59ea0f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838882"
---
# <a name="nfsstat"></a>nfsstat



Vous pouvez utiliser **nfsstat** pour afficher ou réinitialiser le nombre d’appels effectués vers le serveur pour NFS.

## <a name="syntax"></a>Syntaxe

```
nfsstat [-z]
```

## <a name="description"></a>Description

Lorsqu’il est utilisé sans l’option **-z** , l’utilitaire de ligne de commande **nfsstat** affiche le nombre d’appels NFS v2, NFS v3 et Mount v3 effectués au serveur, car les compteurs ont été définis sur 0, au moment du démarrage du service ou de la réinitialisation des compteurs à l’aide de **nfsstat-z**.