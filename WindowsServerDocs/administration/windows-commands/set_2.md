---
title: set_2
description: Rubrique de référence pour set_2, qui définit le contexte, les options, le mode détaillé et le fichier de métadonnées pour la création de clichés instantanés.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: acf24663-1a50-4321-b48d-1717655e9476
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 655f379dd8c2d633aad0cbb470b17c6ccb90c4f7
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721871"
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
|contexte|Définit le contexte pour la création de clichés instantanés. Consultez [définir le contexte](set-context.md) pour la syntaxe et les paramètres.|
|Option|Définit des options pour la création de clichés instantanés. Pour connaître la syntaxe et les paramètres, consultez [set option](set-option.md) .|
|verbose|Active ou désactive le mode de sortie détaillé. Consultez [définir verbose](set-verbose.md) pour la syntaxe et les paramètres.|
|metadata|Définit le nom et l’emplacement du fichier de métadonnées de création de clichés instantanés. Consultez [définir les métadonnées](set-metadata.md) pour la syntaxe et les paramètres.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)