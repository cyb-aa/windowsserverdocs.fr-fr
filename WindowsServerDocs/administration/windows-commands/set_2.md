---
title: set_2
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: acf24663-1a50-4321-b48d-1717655e9476
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3f5523646fddbfec31cb3900fc09230efc1c7813
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862610"
---
# <a name="set2"></a>set_2



Définit le contexte, les options, le mode détaillé et fichier de métadonnées pour la création de clichés instantanés. Si utilisée sans paramètres, **définir** répertorie tous les paramètres actuels.

## <a name="syntax"></a>Syntaxe

```
set
set context {clientaccessible | persistent [nowriters] | volatile [nowriters]}
set option {[differential | plex] [transportable] [[rollbackrecover] [txfrecover] | [noautorecover]]}
set verbose {on|off}
set metadata <MetaData.cab>
```

## <a name="set-sub-commands"></a>Sous-commandes de jeu

|Sous-commande|Description|
|-----------|-----------|
|Contexte|Définit le contexte pour la création de clichés instantanés. Consultez [définir le contexte](set-context.md) pour la syntaxe et les paramètres.|
|option|Définit les options pour la création de clichés instantanés. Consultez [définir option](set-option.md) pour la syntaxe et les paramètres.|
|commentaires|Active ou désactive la le mode documenté. Consultez [valeur commentaires](set-verbose.md) pour la syntaxe et les paramètres.|
|métadonnées|Définit le nom et l’emplacement du fichier de métadonnées de la création de clichés instantanés. Consultez [définir les métadonnées](set-metadata.md) pour la syntaxe et les paramètres.|
|/?|Affiche l'aide à l'invite de commandes.|

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)