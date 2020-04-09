---
title: Charger les métadonnées
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2c535487-668b-44fc-babb-ff59cf7d190e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b8db98611fd78c6e30070901effafddd6e678c16
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841022"
---
# <a name="load-metadata"></a>Charger les métadonnées



Charge un fichier de métadonnées. cab avant d’importer un cliché instantané transportable ou charge les métadonnées de l’enregistreur dans le cas d’une restauration. En cas d’utilisation sans paramètre, **charger les métadonnées** affiche l’aide à l’invite de commandes.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
load metadata [<Drive>:][<Path>]<MetaData.cab>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[\<> de lecteur :] [<Path>]|Spécifie l’emplacement du fichier de métadonnées.|
|MetaData. cab|Spécifie le fichier. cab de métadonnées à charger.|

## <a name="remarks"></a>Notes

-   Vous pouvez utiliser la commande **Importer** pour importer un cliché instantané transportable basé sur les métadonnées spécifiées par **charger les métadonnées**.
-   Cette commande est nécessaire avant la commande **Begin Restore** pour charger les enregistreurs et les composants sélectionnés pour la restauration.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour charger un fichier de métadonnées appelé Metafile. cab à partir de l’emplacement par défaut, tapez :
```
load metadata metafile.cab
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)