---
title: msdt
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ead1b672-a120-4e16-94aa-a8e13602c1d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 87c1e4e8cd6d9de036b47de590867a6531d0335a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839252"
---
# <a name="msdt"></a>msdt



Appelle un pack de résolution des problèmes sur la ligne de commande ou dans le cadre d’un script automatisé, et active des options supplémentaires sans entrée d’utilisateur.

## <a name="syntax"></a>Syntaxe

```
msdt </id <name> | /path <name> | /cab < name>> <</parameter> [options] … <parameter> [options]>>
```

### <a name="parameters"></a>Paramètres

Le tableau suivant répertorie les paramètres et les options pris en charge par MSDT. exe.


|      Paramètre      |                                                                                            Description                                                                                             |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /ID \<nom du package > |        Spécifie le package de diagnostic à exécuter. Pour obtenir la liste des packages disponibles, consultez l’ID du Pack de résolution des problèmes dans la section packs de résolution des problèmes disponibles, plus loin dans cette rubrique.         |
|  Répertoire de \</Path  |                                                                                           fichier. diagpkg                                                                                            |
|   clé de/DCI \<>   |                                        Préremplit le champ de clé d’une clé de clé dans MSDT. Ce paramètre est utilisé uniquement lorsqu’un fournisseur de support a fourni une clé de clé.                                         |
|  Répertoire/DT \<>   | Affiche l’historique de résolution des problèmes dans le répertoire spécifié. Les résultats des diagnostics sont stockés dans les répertoires **%LocalAppData%\Diagnostics** ou **%LocalAppData%\ElevatedDiagnostics** de l’utilisateur. |
| /AF \<fichier de réponses >  |                                               Spécifie un fichier de réponses au format XML qui contient les réponses à une ou plusieurs interactions de diagnostic.                                               |

