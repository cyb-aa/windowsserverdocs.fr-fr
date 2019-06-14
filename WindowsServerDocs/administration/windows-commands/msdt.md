---
title: msdt
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ead1b672-a120-4e16-94aa-a8e13602c1d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ba411cf73026afe9990e5c32824e3dc277507891
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437229"
---
# <a name="msdt"></a>msdt



Appelle un pack de dépannage en ligne de commande ou dans le cadre d’un script automatisé et active des options supplémentaires sans entrée utilisateur.

## <a name="syntax"></a>Syntaxe

```
msdt </id <name> | /path <name> | /cab < name>> <</parameter> [options] … <parameter> [options]>>
```

## <a name="parameters"></a>Paramètres

Le tableau suivant contient les paramètres et les options prises en charge par msdt.exe.


|      Paramètre      |                                                                                            Description                                                                                             |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /id \<package name> |        Spécifie le package de diagnostic à exécuter. Pour obtenir la liste des packages disponibles, consultez l’ID de Pack de résolution des problèmes dans « disponible le dépannage des packs ? section plus loin dans cette rubrique.         |
|  / Path \<directory  |                                                                                           fichier de .diagpkg                                                                                            |
|   /dci \<passkey>   |                                        Pré-remplit le champ de clé d’accès dans msdt. Ce paramètre est utilisé uniquement lorsqu’un fournisseur de support a fourni une clé de sécurité.                                         |
|  /dt \<directory>   | Affiche l’historique de résolution des problèmes dans le répertoire spécifié. Résultats de diagnostics sont stockés dans l’utilisateur **%LOCALAPPDATA%\Diagnostics** ou **%LOCALAPPDATA%\ElevatedDiagnostics** répertoires. |
| /AF \<fichier de réponses >  |                                               Spécifie un fichier de réponses au format XML qui contient les réponses à un ou plusieurs des interactions Diagnostics.                                               |

