---
title: set_2
description: La rubrique commandes Windows pour set_2, qui définit le contexte, les options, le mode détaillé et le fichier de métadonnées pour la création de clichés instantanés.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: acf24663-1a50-4321-b48d-1717655e9476
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fa467625997824a11b2303572a063d591f59bdd6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834382"
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
|context|Définit le contexte pour la création de clichés instantanés. Consultez [définir le contexte](set-context.md) pour la syntaxe et les paramètres.|
|Option|Définit des options pour la création de clichés instantanés. Pour connaître la syntaxe et les paramètres, consultez [set option](set-option.md) .|
|commentaires|Active ou désactive le mode de sortie détaillé. Consultez [définir verbose](set-verbose.md) pour la syntaxe et les paramètres.|
|métadonnées|Définit le nom et l’emplacement du fichier de métadonnées de création de clichés instantanés. Consultez [définir les métadonnées](set-metadata.md) pour la syntaxe et les paramètres.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)