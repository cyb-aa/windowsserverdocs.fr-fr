---
title: Définir les métadonnées
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 67e6f60a-b42a-451a-95cf-b22ace7d50c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ac73a4131d3f4065cd1aeae873734b079ad664e2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370953"
---
# <a name="set-metadata"></a>Définir les métadonnées



Définit le nom et l’emplacement du fichier de métadonnées de création de clichés instantanés utilisé pour transférer des clichés instantanés d’un ordinateur à un autre. En cas d’utilisation sans paramètre, l' **option définir les métadonnées** affiche l’aide à l’invite de commandes.

## <a name="syntax"></a>Syntaxe

```
set metadata [<Drive>:][<Path>]<MetaData.cab>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[@no__t 0Drive >:] [<Path>]|Spécifie l’emplacement où créer le fichier de métadonnées.|
|@no__t -0MetaData. cab >|Spécifie le nom du fichier CAB pour stocker les métadonnées de création de clichés instantanés.|

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)