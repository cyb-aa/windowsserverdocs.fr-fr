---
title: Ombres de liste vssadmin
description: Une description de la liste vssadmin masque la commande.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 07da36da1473563c3236a4fafc3ceae06259981a
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/22/2018
ms.locfileid: "2082155"
---
# <a name="vssadmin-list-shadows"></a>Ombres de liste vssadmin

>S’applique à: Windows 10, Windows 8.1, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Répertorie tous les clichés instantanés d’un volume spécifié. Si vous utilisez cette commande sans paramètres, elle affiche tous les clichés instantanés de volume sur l’ordinateur dans l’ordre dictée par **Définir de copie VSS**.

## <a name="syntax"></a>Syntaxe

```PowerShell
vssadmin list shadows [/for=<ForVolumeSpec>] [/shadow=<ShadowID>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---|---|
|/ for = \ < VolumeFor >|Spécifie le volume pour les clichés instantanés sont répertoriés.|
|/ shadow = \ < ShadowID >|Répertorie le cliché instantané spécifié par ShadowID. Pour obtenir l’ID du cliché instantané, utilisez la commande **vssadmin les ombres de liste** . Lorsque vous tapez un ID du cliché instantané, utilisez le format suivant, où chaque *X* représente un caractère hexadécimal:<br><br>XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX.|

## <a name="additional-references"></a>Références supplémentaires

* [Clé de la syntaxe de ligne de commande](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771080(v%3dws.11))
* [Vssadmin](vssadmin.md)