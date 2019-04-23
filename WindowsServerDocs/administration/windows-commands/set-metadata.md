---
title: Métadonnées du jeu
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 82d2cd9ba447a0ea261f91dc01c11e45dfc0aa9b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835990"
---
# <a name="set-metadata"></a>Métadonnées du jeu



Définit le nom et l’emplacement du fichier de métadonnées de la création du cliché instantané utilisé pour transférer des clichés instantanés d’un ordinateur à un autre. Si utilisée sans paramètres, **définir les métadonnées** affiche l’aide à l’invite de commandes.

## <a name="syntax"></a>Syntaxe

```
set metadata [<Drive>:][<Path>]<MetaData.cab>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[\<Drive>:][<Path>]|Spécifie l’emplacement pour créer le fichier de métadonnées.|
|\<MetaData.cab>|Spécifie le nom du fichier cab pour stocker les métadonnées de la création de clichés instantanés.|

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)