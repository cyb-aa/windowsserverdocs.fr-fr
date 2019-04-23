---
title: Liste vssadmin
description: Une description de la liste vssadmin occulte commande.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 07da36da1473563c3236a4fafc3ceae06259981a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861150"
---
# <a name="vssadmin-list-shadows"></a>Liste vssadmin

>S’applique à : Windows 10, Windows 8.1, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Répertorie tous les clichés instantanés d’un volume spécifié. Si vous utilisez cette commande sans paramètres, il affiche tous les clichés instantanés de volume sur l’ordinateur dans l’ordre de dictée par **le jeu de copies clichés instantanés**.

## <a name="syntax"></a>Syntaxe

```PowerShell
vssadmin list shadows [/for=<ForVolumeSpec>] [/shadow=<ShadowID>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---|---|
|/for=\<ForVolumeSpec>|Spécifie le volume apparaît pour les clichés instantanés.|
|/shadow=\<ShadowID>|Répertorie le cliché instantané spécifié par ShadowID. Pour obtenir l’ID du cliché instantané, utilisez le **de liste vssadmin** commande. Lorsque vous tapez un ID du cliché instantané, utilisez le format suivant, où chaque *X* représente un caractère hexadécimal :<br><br>XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX|

## <a name="additional-references"></a>Références supplémentaires

* [Clé de la syntaxe de ligne de commande](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771080(v%3dws.11))
* [Vssadmin](vssadmin.md)