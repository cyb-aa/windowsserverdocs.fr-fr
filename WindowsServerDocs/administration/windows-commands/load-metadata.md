---
title: Chargement des métadonnées
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2c535487-668b-44fc-babb-ff59cf7d190e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b52b5040fc8c834b04cad83ca4b0cfab103fdc43
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871330"
---
# <a name="load-metadata"></a>Chargement des métadonnées



Charge un fichier .cab de métadonnées avant d’importer une copie de clichés instantanés transportables ou charge les métadonnées de writer dans le cas d’une restauration. Si utilisée sans paramètres, **charger les métadonnées** affiche l’aide à l’invite de commandes.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
load metadata [<Drive>:][<Path>]<MetaData.cab>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[\<Drive>:][<Path>]|Spécifie l’emplacement du fichier de métadonnées.|
|MetaData.cab|Spécifie le fichier .cab de métadonnées à charger.|

## <a name="remarks"></a>Notes

-   Vous pouvez utiliser la **importer** commande pour importer une copie de clichés instantanés transportables basé sur les métadonnées spécifiées par **charger les métadonnées**.
-   Cette commande est nécessaire avant du **commencer restauration** commande pour charger les enregistreurs sélectionnées et les composants pour la restauration.

## <a name="BKMK_examples"></a>Exemples

Pour charger un fichier de métadonnées appelé metafile.cab à partir de l’emplacement par défaut, tapez :
```
load metadata metafile.cab
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)