---
title: subst
description: Découvrez comment associer un chemin d’accès à une lettre de lecteur.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3e69234c-2312-4343-868b-afc1017c622a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f3010d1e58fbd360b8311512e6664873b020c12b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383754"
---
# <a name="subst"></a>subst



Associe un chemin d’accès à une lettre de lecteur. En cas d’utilisation sans paramètre, **subst** affiche le nom des lecteurs virtuels en vigueur.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
subst [<Drive1>: [<Drive2>:]<Path>] 
subst <Drive1>: /d
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Lecteur1 >:|Spécifie le lecteur virtuel auquel vous souhaitez affecter un chemin d’accès.|
|[\<lecteur2 >:]\<chemin d’accès >|Spécifie le lecteur physique et le chemin d’accès que vous souhaitez affecter à un lecteur virtuel.|
|/d|Supprime un lecteur (virtuel) substitué.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Les commandes suivantes ne fonctionnent pas et ne doivent pas être utilisées sur les lecteurs spécifiés dans la commande **subst** :

    **chkdsk**

    **diskcomp**

    **diskcopy**

    **format**

    **label**

    **recover**
-   Le paramètre *lecteur1* doit être compris dans la plage spécifiée par la commande **LASTDRIVE** . Si ce n’est pas le cas, **subst** affiche le message d’erreur suivant :

    `Invalid parameter - drive1:`

## <a name="BKMK_examples"></a>Illustre

Pour créer un lecteur virtuel Z pour le chemin d’accès B:\User\Betty\Forms, tapez :
```
subst z: b:\user\betty\forms 
```
Au lieu de taper le chemin d’accès complet, vous pouvez accéder à ce répertoire en tapant la lettre du lecteur virtuel suivie d’un signe deux-points, comme suit :
```
z: 
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)