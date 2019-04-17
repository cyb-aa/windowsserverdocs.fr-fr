---
title: Vssadmin delete ombres
description: Description de la commande ombres vssadmin à supprimer.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 71119174c7fc5929eb4e1982183ba0aed7eb1735
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/22/2018
ms.locfileid: "2082064"
---
# <a name="vssadmin-delete-shadows"></a>Vssadmin delete ombres

>S’applique à: Windows 10, Windows 8.1, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Supprime les clichés instantanés d’un volume spécifié.

## <a name="syntax"></a>Syntaxe

```PowerShell
vssadmin delete shadows /for=<ForVolumeSpec> [/oldest | /all | /shadow=<ShadowID>] [/quiet]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---|---|
|/ for = \ < VolumeFor >|Spécifie quels de cliché instantané sera supprimé.|
|/ le plus ancien|Supprime uniquement le cliché instantané le plus ancien.|
|/All|Supprime tous les clichés instantanés du volume spécifié.|
|/ shadow = \ < ShadowID >|Supprime le cliché instantané spécifié par ShadowID. Pour obtenir l’ID du cliché instantané, utilisez la commande **vssadmin les ombres de liste** . Lorsque vous entrez un ID du cliché instantané, utilisez le format suivant, où chaque *X* représente un caractère hexadécimal:<br><br>XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX.|
|/ silencieux|Spécifie que la commande n’affichera pas les messages en cours d’exécution.|

## <a name="remarks"></a>Notes

Vous ne pouvez supprimer des clichés instantanés avec le type accessibles au client.

## <a name="examples"></a>Exemples

Pour supprimer le plus ancien cliché instantané du volume C, entrez cette commande:

```PowerShell
vssadmin delete shadows /for=c: /oldest
```

## <a name="additional-references"></a>Références supplémentaires

* [Clé de la syntaxe de ligne de commande](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771080(v%3dws.11))
* [Vssadmin](vssadmin.md)
* [Ombres de liste vssadmin](vssadmin-list-shadows.md)