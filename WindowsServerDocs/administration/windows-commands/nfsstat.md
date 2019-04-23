---
title: nfsstat
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 9db8b903d4c3681b2b3bae3424f8af83696ae2c7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853290"
---
# <a name="nfsstat"></a>nfsstat



Vous pouvez utiliser **nfsstat appartient** pour afficher ou réinitialiser le nombre d’appels passés au serveur pour NFS.

## <a name="syntax"></a>Syntaxe

```
nfsstat [-z]
```

## <a name="description"></a>Description

Lorsqu’il est utilisé sans le **- z** option, le **nfsstat appartient** utilitaire de ligne de commande affiche le nombre de NFS V2, NFS V3 et les appels de montage V3 apportées au serveur dans la mesure où les compteurs ont été définies sur 0, soit lorsque le service démarrage ou lors de la réinitialisation des compteurs à l’aide de **nfsstat appartient - z**.