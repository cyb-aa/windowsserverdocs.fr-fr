---
title: set_2
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: e91bee5f0d351e461d16ccd22478d67f26887728
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370914"
---
# <a name="set_2"></a>set_2



Définit le contexte, les options, le mode détaillé et le fichier de métadonnées pour la création de clichés instantanés. En cas d’utilisation sans paramètre, **Set** répertorie tous les paramètres actuels.

## <a name="syntax"></a>Syntaxe

```
set
set context {clientaccessible | persistent [nowriters] | volatile [nowriters]}
set option {[differential | plex] [transportable] [[rollbackrecover] [txfrecover] | [noautorecover]]}
set verbose {on|off}
set metadata <MetaData.cab>
```

## <a name="set-sub-commands"></a>Définir des sous-commandes

|Sous-commande|Description|
|-----------|-----------|
|Contexte|Définit le contexte pour la création de clichés instantanés. Consultez [définir le contexte](set-context.md) pour la syntaxe et les paramètres.|
|Option|Définit des options pour la création de clichés instantanés. Pour connaître la syntaxe et les paramètres, consultez [set option](set-option.md) .|
|commentaires|Active ou désactive le mode de sortie détaillé. Consultez [définir verbose](set-verbose.md) pour la syntaxe et les paramètres.|
|métadonnées|Définit le nom et l’emplacement du fichier de métadonnées de création de clichés instantanés. Consultez [définir les métadonnées](set-metadata.md) pour la syntaxe et les paramètres.|
|/?|Affiche l'aide à l'invite de commandes.|

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)