---
title: importer
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7bd78d76-0560-4d47-944c-fe960be2c10b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 50a095c323806dd523994c36c5b427d4ecedf8ef
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375503"
---
# <a name="import"></a>importer



Importe un cliché instantané transportable à partir d’un fichier de métadonnées chargé dans le système.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
import
```

## <a name="remarks"></a>Notes

-   Les clichés instantanés transportables ne sont pas stockés sur le système immédiatement. Leurs détails sont stockés dans un fichier XML de documents de composants de sauvegarde, que DiskShadow demande et enregistre automatiquement dans un fichier de métadonnées. cab dans le répertoire de travail. Vous pouvez modifier le chemin d’accès et le nom de ce fichier à l’aide de la commande **définir les métadonnées** .
-   Avant de pouvoir utiliser l' **importation**, vous devez charger un fichier de métadonnées DiskShadow à l’aide de la commande **charger les métadonnées** .

## <a name="BKMK_examples"></a>Illustre

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

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)