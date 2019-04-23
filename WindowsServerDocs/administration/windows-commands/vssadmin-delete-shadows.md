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
ms.openlocfilehash: 71119174c7fc5929eb4e1982183ba0aed7eb1735
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847090"
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