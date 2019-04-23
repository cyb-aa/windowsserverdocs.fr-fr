---
title: importer
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ddef3958bc431519e3cb89b658a58d1f4dba6938
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835260"
---
# <a name="import"></a>importer



importe une copie de clichés instantanés transportables à partir d’un fichier de métadonnées chargées dans le système.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
import
```

## <a name="remarks"></a>Notes

-   Clichés instantanés transportables ne sont pas stockées immédiatement sur le système. Les détails associés sont stockés dans un fichier XML de Document de composants de sauvegarde, DiskShadow automatiquement les demandes et les enregistre dans un fichier de métadonnées .cab dans le répertoire de travail. Vous pouvez modifier le chemin d’accès et le nom de ce fichier à l’aide de la **définir les métadonnées** commande.
-   Avant de pouvoir utiliser **importer**, vous devez charger un fichier de métadonnées de DiskShadow à l’aide du **charger les métadonnées** commande.

## <a name="BKMK_examples"></a>Exemples

Voici un exemple de script de DiskShadow qui illustre l’utilisation de la **importer** commande :
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

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)