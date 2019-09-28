---
title: Ombres de la liste vssadmin
description: Description de la commande vssadmin list Shadows.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 49bee3deac463b68fda94097bb183bcbf1c89810
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362592"
---
# <a name="vssadmin-list-shadows"></a>Ombres de la liste vssadmin

>S’applique à : Windows 10, Windows 8.1, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Répertorie tous les clichés instantanés existants d’un volume spécifié. Si vous utilisez cette commande sans paramètres, elle affiche tous les clichés instantanés de volume sur l’ordinateur dans l’ordre dicté par le jeu de clichés **instantanés**.

## <a name="syntax"></a>Syntaxe

```PowerShell
vssadmin list shadows [/for=<ForVolumeSpec>] [/shadow=<ShadowID>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---|---|
|/for = @no__t 0ForVolumeSpec >|Spécifie le volume pour lequel les clichés instantanés seront listés.|
|/Shadow = \<ShadowID >|Répertorie les clichés instantanés spécifiés par ShadowID. Pour afficher l’ID du cliché instantané, utilisez la commande **vssadmin list Shadows** . Lorsque vous tapez un ID de cliché instantané, utilisez le format suivant, où chaque *X* représente un caractère hexadécimal :<br><br>XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX|

## <a name="additional-references"></a>Références supplémentaires

* [Clé de syntaxe de ligne de commande](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771080(v%3dws.11))
* [Vssadmin](vssadmin.md)