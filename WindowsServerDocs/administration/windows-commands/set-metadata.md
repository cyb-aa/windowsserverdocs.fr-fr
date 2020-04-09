---
title: Définir les métadonnées
description: La rubrique commandes Windows pour définir les métadonnées, qui définit le nom et l’emplacement du fichier de métadonnées de création de clichés instantanés utilisé pour transférer des clichés instantanés d’un ordinateur à un autre.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 67e6f60a-b42a-451a-95cf-b22ace7d50c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b5bd728650cf163f98a82ff1e6f88755c4cc1aea
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834522"
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
|[\<> de lecteur :] [<Path>]|Spécifie l’emplacement où créer le fichier de métadonnées.|
|\<MetaData. cab >|Spécifie le nom du fichier CAB pour stocker les métadonnées de création de clichés instantanés.|

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)