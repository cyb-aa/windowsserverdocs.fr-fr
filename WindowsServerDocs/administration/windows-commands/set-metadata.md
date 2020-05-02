---
title: Définir les métadonnées
description: Rubrique de référence sur la définition des métadonnées, qui définit le nom et l’emplacement du fichier de métadonnées de création de clichés instantanés utilisé pour transférer des clichés instantanés d’un ordinateur à un autre.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 67e6f60a-b42a-451a-95cf-b22ace7d50c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 683e54a7efc072d8709d6257771ba6bc5bde206e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721914"
---
# <a name="set-metadata"></a>Définir les métadonnées

Définit le nom et l’emplacement du fichier de métadonnées de création de clichés instantanés utilisé pour transférer des clichés instantanés d’un ordinateur à un autre. En cas d’utilisation sans paramètre, l' **option définir les métadonnées** affiche l’aide à l’invite de commandes.

## <a name="syntax"></a>Syntaxe

```
set metadata [<Drive>:][<Path>]<MetaData.cab>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[\<Lecteur> :] [<Path>]|Spécifie l’emplacement où créer le fichier de métadonnées.|
|\<MetaData. cab>|Spécifie le nom du fichier CAB pour stocker les métadonnées de création de clichés instantanés.|

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)