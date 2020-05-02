---
title: importer
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7bd78d76-0560-4d47-944c-fe960be2c10b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 72cbd6195de64a6a0a7f2c258e19b2d5eb1378b1
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724856"
---
# <a name="import"></a>importer



Importe un cliché instantané transportable à partir d’un fichier de métadonnées chargé dans le système.



## <a name="syntax"></a>Syntaxe

```
import
```

## <a name="remarks"></a>Notes

-   Les clichés instantanés transportables ne sont pas stockés sur le système immédiatement. Leurs détails sont stockés dans un fichier XML de documents de composants de sauvegarde, que DiskShadow demande et enregistre automatiquement dans un fichier de métadonnées. cab dans le répertoire de travail. Vous pouvez modifier le chemin d’accès et le nom de ce fichier à l’aide de la commande **définir les métadonnées** .
-   Avant de pouvoir utiliser l' **importation**, vous devez charger un fichier de métadonnées DiskShadow à l’aide de la commande **charger les métadonnées** .

## <a name="examples"></a>Exemples

Voici un exemple de script DiskShadow qui illustre l’utilisation de la commande d' **importation** :
```
#Sample DiskShadow script demonstrating IMPORT
SET CONTEXT PERSISTENT
SET CONTEXT TRANSPORTABLE
SET METADATA transHWshadow_p.cab
#P: is the volume supported by the Hardware Shadow Copy provider
ADD VOLUME P:
CREATE
END BACKUP
#The (transportable) shadow copy is not in the system yet.
#You can reset or exit now if you wish.

LOAD METADATA transHWshadow_p.cab
IMPORT
#The shadow copy will now be loaded into the system.
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)