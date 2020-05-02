---
title: subst
description: Découvrez comment associer un chemin d’accès à une lettre de lecteur.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3e69234c-2312-4343-868b-afc1017c622a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 62ba0de33e69998e7d3e343b1e53c1de7e630e10
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721610"
---
# <a name="subst"></a>subst



Associe un chemin d’accès à une lettre de lecteur. En cas d’utilisation sans paramètre, **subst** affiche le nom des lecteurs virtuels en vigueur.



## <a name="syntax"></a>Syntaxe

```
subst [<Drive1>: [<Drive2>:]<Path>] 
subst <Drive1>: /d
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<> lecteur1 :|Spécifie le lecteur virtuel auquel vous souhaitez affecter un chemin d’accès.|
|[\<Lecteur2> :] \<Chemin d’accès>|Spécifie le lecteur physique et le chemin d’accès que vous souhaitez affecter à un lecteur virtuel.|
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

## <a name="examples"></a><a name="BKMK_examples"></a>Illustre

Pour créer un lecteur virtuel Z pour le chemin d’accès B:\User\Betty\Forms, tapez :
```
subst z: b:\user\betty\forms 
```
Au lieu de taper le chemin d’accès complet, vous pouvez accéder à ce répertoire en tapant la lettre du lecteur virtuel suivie d’un signe deux-points, comme suit :
```
z: 
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)