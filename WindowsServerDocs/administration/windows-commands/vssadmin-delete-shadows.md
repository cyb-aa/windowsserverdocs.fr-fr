---
title: Vssadmin supprimer les ombres
description: Description de la commande vssadmin Delete Shadows.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: e18823c5c030aa1a7b8f032f820e415f36fd7827
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720268"
---
# <a name="vssadmin-delete-shadows"></a>Vssadmin supprimer les ombres

> S’applique à : Windows 10, Windows 8.1, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Supprime les clichés instantanés d’un volume spécifié.

## <a name="syntax"></a>Syntaxe

```PowerShell
vssadmin delete shadows /for=<ForVolumeSpec> [/oldest | /all | /shadow=<ShadowID>] [/quiet]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---|---|
|/for =\<VolumeFor>|Spécifie le cliché instantané du volume qui sera supprimé.|
|/oldest|Supprime uniquement le cliché instantané le plus ancien.|
|/all|Supprime tous les clichés instantanés du volume spécifié.|
|/Shadow =\<ShadowID>|Supprime le cliché instantané spécifié par ShadowID. Pour afficher l’ID du cliché instantané, utilisez la commande **vssadmin list Shadows** . Lorsque vous entrez un ID de cliché instantané, utilisez le format suivant, où chaque *X* représente un caractère hexadécimal :<br><br>XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX|
|/quiet|Spécifie que la commande n’affichera pas de messages pendant l’exécution.|

## <a name="remarks"></a>Notes 

Vous pouvez uniquement supprimer les clichés instantanés avec le type accessible par le client.

## <a name="examples"></a>Exemples

Pour supprimer le cliché instantané le plus ancien du volume C, entrez la commande suivante :

```PowerShell
vssadmin delete shadows /for=c: /oldest
```

## <a name="additional-references"></a>Références supplémentaires

* [Clé de syntaxe de ligne de commande](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771080(v%3dws.11))
* [Vssadmin](vssadmin.md)
* [Clichés instantanés de liste Vssadmin](vssadmin-list-shadows.md)