---
ms.assetid: e5f55f3e-8d2a-4526-8d67-36a539126c22
title: la hiérarchisation fsutil
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: dcb69e4e9c71a723bfd735eb7915472f1232a92b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859250"
---
# <a name="fsutil-tiering"></a>la hiérarchisation fsutil
>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10

Permet la gestion des fonctions de niveau de stockage, comme la définition et la désactivation des indicateurs et répertorier des niveaux.

## <a name="syntax"></a>Syntaxe

```
fsutil tiering [clearflags] <volume> <flags>
fsutil tiering [queryflags] <volume>
fsutil tiering [regionlist] <volume>
fsutil tiering [setflags] <volume> <flags>
fsutil tiering [tierlist] <volume>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------------|---------------|
|ClearFlags|Désactive les indicateurs de comportement de hiérarchisation d’un volume.|
|\<volume>|Spécifie le volume.|
|/TrNH|Pour les volumes avec le stockage hiérarchisé, provoque la chaleur collecte pour être désactivé.<br /><br>S’applique uniquement à NTFS et ReFS.|
|queryflags|Interroge les indicateurs de comportement de hiérarchisation d’un volume.|
|regionlist|Répertorie les régions à plusieurs niveaux d’un volume et de leurs niveaux de stockage respectives.|
|définis par|Active les indicateurs de comportement de hiérarchisation d’un volume.|
|tierlist|Répertorie les tieres de stockage associé à un volume.|


### <a name="examples"></a>Exemples

Pour interroger les indicateurs sur le volume C, tapez :

```
fsutil tiering clearflags C:
```

Pour définir les indicateurs sur le volume C, tapez :

```
fsutil tiering setflags C: /TrNH
```

Pour effacer les indicateurs sur le volume C, tapez :

```
fsutil tiering clearflags C: /TrNH
```

Pour répertorier les régions de volume C et leurs niveaux de stockage respectives, tapez :

```
fsutil tiering regionlist C:
```

Pour répertorier les niveaux de volume C, tapez :

```
fsutil tiering tierlist C:
```



### <a name="additional-references"></a>Références supplémentaires
[Clé de la syntaxe de ligne de commande](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

