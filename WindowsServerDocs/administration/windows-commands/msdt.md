---
title: msdt
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 4c4342cf0fe588f5b41cd98a38a459ac41ca212a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373453"
---
# <a name="msdt"></a>msdt



Appelle un pack de résolution des problèmes sur la ligne de commande ou dans le cadre d’un script automatisé, et active des options supplémentaires sans entrée d’utilisateur.

## <a name="syntax"></a>Syntaxe

```
msdt </id <name> | /path <name> | /cab < name>> <</parameter> [options] … <parameter> [options]>>
```

## <a name="parameters"></a>Paramètres

Le tableau suivant répertorie les paramètres et les options pris en charge par MSDT. exe.


|      Paramètre      |                                                                                            Description                                                                                             |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| nom \<du package/ID > |        Spécifie le package de diagnostic à exécuter. Pour obtenir la liste des packages disponibles, consultez l’ID du Pack de résolution des problèmes dans la section « packs de résolution des problèmes disponibles » plus loin dans cette rubrique.         |
|  répertoire \</Path  |                                                                                           fichier. diagpkg                                                                                            |
|   > \<de clé de clé/DCI   |                                        Préremplit le champ de clé d’une clé de clé dans MSDT. Ce paramètre est utilisé uniquement lorsqu’un fournisseur de support a fourni une clé de clé.                                         |
|  > \<du répertoire/DT   | Affiche l’historique de résolution des problèmes dans le répertoire spécifié. Les résultats des diagnostics sont stockés dans les répertoires **%LocalAppData%\Diagnostics** ou **%LocalAppData%\ElevatedDiagnostics** de l’utilisateur. |
| > \<du fichier de réponses/AF  |                                               Spécifie un fichier de réponses au format XML qui contient les réponses à une ou plusieurs interactions de diagnostic.                                               |

