---
title: subst
description: Découvrez comment associer un chemin d’accès à une lettre de lecteur.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 858195de89ca8661cf47c25b6cf9b519cc4efbf8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858070"
---
# <a name="subst"></a>subst



Associe un chemin d’accès à une lettre de lecteur. Si utilisée sans paramètres, **subst** affiche les noms des lecteurs virtuels en vigueur.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
subst [<Drive1>: [<Drive2>:]<Path>] 
subst <Drive1>: /d
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Lecteur1 > :|Spécifie le lecteur virtuel auquel vous souhaitez affecter un chemin d’accès.|
|[\<Lecteur2 > :]\<chemin d’accès >|Spécifie le lecteur physique et le chemin d’accès que vous souhaitez affecter à un lecteur virtuel.|
|/d|Supprime un lecteur substitué (virtuel).|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Les commandes suivantes ne fonctionnent pas et ne doit pas être utilisés sur les lecteurs qui sont spécifiés dans le **subst** commande :

    **chkdsk**

    **diskcomp**

    **diskcopy**

    **format**

    **label**

    **recover**
-   Le *lecteur1* paramètre doit être dans la plage spécifiée par la **lastdrive** commande. Si ce n’est pas le cas, **subst** affiche le message d’erreur suivant :

    `Invalid parameter - drive1:`

## <a name="BKMK_examples"></a>Exemples

Pour créer un lecteur virtuel Z pour le chemin d’accès B:\User\Betty\Forms, tapez :
```
subst z: b:\user\betty\forms 
```
Au lieu de taper le chemin d’accès complet, vous pouvez atteindre ce répertoire en tapant la lettre du lecteur virtuel suivie d’un signe deux-points comme suit :
```
z: 
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)