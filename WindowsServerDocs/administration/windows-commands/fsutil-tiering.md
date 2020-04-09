---
ms.assetid: e5f55f3e-8d2a-4526-8d67-36a539126c22
title: Hiérarchisation de fsutil
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 8227fafc6b29471e2f09db171645012967553429
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844052"
---
# <a name="fsutil-tiering"></a>Hiérarchisation de fsutil
>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10

Active la gestion des fonctions de niveau de stockage, telles que la définition et la désactivation des indicateurs et la liste des niveaux.

## <a name="syntax"></a>Syntaxe

```
fsutil tiering [clearflags] <volume> <flags>
fsutil tiering [queryflags] <volume>
fsutil tiering [regionlist] <volume>
fsutil tiering [setflags] <volume> <flags>
fsutil tiering [tierlist] <volume>
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------------|---------------|
|clearflags|Désactive les indicateurs de comportement de hiérarchisation d’un volume.|
|> du volume \<|Spécifie le volume.|
|/TrNH|Pour les volumes avec stockage hiérarchisé, la collecte de chaleur est désactivée.<br /><br>S’applique uniquement aux fichiers NTFS et ReFS.|
|queryflags|Interroge les indicateurs de comportement de hiérarchisation d’un volume.|
|regionlist|Répertorie les régions à plusieurs niveaux d’un volume et leurs niveaux de stockage respectifs.|
|setflags|Active les indicateurs de comportement de hiérarchisation d’un volume.|
|tierlist|Répertorie les niveaux de stockage associés à un volume.|


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

Pour répertorier les régions du volume C et leurs niveaux de stockage respectifs, tapez :

```
fsutil tiering regionlist C:
```

Pour répertorier les niveaux du volume C, tapez :

```
fsutil tiering tierlist C:
```



### <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

[Fsutil](Fsutil.md)

