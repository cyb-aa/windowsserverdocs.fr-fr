---
title: Vssadmin delete ombres
description: Une description de la commande de shadows vssadmin delete.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 83074017e4ae412cf0aec654f6ab5901ad8039e2
ms.sourcegitcommit: 2977c707a299929c6ab0d1e0adab2e1c644b8306
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63706625"
---
# <a name="vssadmin-delete-shadows"></a>Vssadmin delete ombres

>S’applique à : Windows 10, Windows 8.1, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Supprime les clichés instantanés d’un volume spécifié.

## <a name="syntax"></a>Syntaxe

```PowerShell
vssadmin delete shadows /for=<ForVolumeSpec> [/oldest | /all | /shadow=<ShadowID>] [/quiet]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---|---|
|/for=\<ForVolumeSpec>|Spécifie le cliché instantané du quel volume est supprimé.|
|/oldest|Supprime uniquement le cliché le plus ancien.|
|/all|Supprime tous les clichés instantanés du volume spécifié.|
|/shadow=\<ShadowID>|Supprime le cliché instantané spécifié par ShadowID. Pour obtenir l’ID du cliché instantané, utilisez le **de liste vssadmin** commande. Lorsque vous entrez un ID du cliché instantané, utilisez le format suivant, où chaque *X* représente un caractère hexadécimal :<br><br>XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX|
|/quiet|Spécifie que la commande n’affichera pas les messages en cours d’exécution.|

## <a name="remarks"></a>Notes

Vous pouvez uniquement supprimer les clichés instantanés avec le type accessibles par les clients.

## <a name="examples"></a>Exemples

Pour supprimer l’ancienne copie de clichés instantanés de volume C, entrez la commande suivante :

```PowerShell
vssadmin delete shadows /for=c: /oldest
```

## <a name="additional-references"></a>Références supplémentaires

* [Clé de la syntaxe de ligne de commande](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771080(v%3dws.11))
* [Vssadmin](vssadmin.md)
* [Liste vssadmin](vssadmin-list-shadows.md)