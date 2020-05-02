---
title: Charger les métadonnées
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2c535487-668b-44fc-babb-ff59cf7d190e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3d3132bca86533ec3f2d0a27247bd3c116cf55b6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724467"
---
# <a name="load-metadata"></a>Charger les métadonnées



Charge un fichier de métadonnées. cab avant d’importer un cliché instantané transportable ou charge les métadonnées de l’enregistreur dans le cas d’une restauration. En cas d’utilisation sans paramètre, **charger les métadonnées** affiche l’aide à l’invite de commandes.



## <a name="syntax"></a>Syntaxe

```
load metadata [<Drive>:][<Path>]<MetaData.cab>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[\<Lecteur> :] [<Path>]|Spécifie l’emplacement du fichier de métadonnées.|
|MetaData. cab|Spécifie le fichier. cab de métadonnées à charger.|

## <a name="remarks"></a>Notes 

-   Vous pouvez utiliser la commande **Importer** pour importer un cliché instantané transportable basé sur les métadonnées spécifiées par **charger les métadonnées**.
-   Cette commande est nécessaire avant la commande **Begin Restore** pour charger les enregistreurs et les composants sélectionnés pour la restauration.

## <a name="examples"></a>Exemples

Pour charger un fichier de métadonnées appelé Metafile. cab à partir de l’emplacement par défaut, tapez :
```
load metadata metafile.cab
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)