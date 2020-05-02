---
title: Clichés instantanés de liste Vssadmin
description: Description de la commande vssadmin list Shadows.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: bc715b3df9e4f4dd6d2de82be9346edc7d88805e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720260"
---
# <a name="vssadmin-list-shadows"></a>Clichés instantanés de liste Vssadmin

> S’applique à : Windows 10, Windows 8.1, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Répertorie tous les clichés instantanés existants d’un volume spécifié. Si vous utilisez cette commande sans paramètres, elle affiche tous les clichés instantanés de volume sur l’ordinateur dans l’ordre dicté par le jeu de clichés **instantanés**.

## <a name="syntax"></a>Syntaxe

```PowerShell
vssadmin list shadows [/for=<ForVolumeSpec>] [/shadow=<ShadowID>]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---|---|
|/for =\<VolumeFor>|Spécifie le volume pour lequel les clichés instantanés seront listés.|
|/Shadow =\<ShadowID>|Répertorie les clichés instantanés spécifiés par ShadowID. Pour afficher l’ID du cliché instantané, utilisez la commande **vssadmin list Shadows** . Lorsque vous tapez un ID de cliché instantané, utilisez le format suivant, où chaque *X* représente un caractère hexadécimal :<br><br>XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX|

## <a name="additional-references"></a>Références supplémentaires

* [Clé de syntaxe de ligne de commande](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771080(v%3dws.11))
* [Vssadmin](vssadmin.md)