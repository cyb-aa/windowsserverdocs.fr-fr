---
title: ksetup:removerealm
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 39f0c6f0-4c50-4781-941e-0893495405e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 579b0772e4642389b90aa370dad80a3eebea9d34
ms.sourcegitcommit: 08eba714d3ceb5f2dfb5486d6b990da1aa4dcbdd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65564721"
---
# <a name="ksetupremoverealm"></a>ksetup:removerealm



Supprime toutes les informations pour le domaine Kerberos spécifié à partir du Registre. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
ksetup /removerealm <RealmName>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<RealmName>|Le nom de domaine est indiqué comme un nom DNS en majuscules, telles que CORP. CONTOSO.COM et il est répertorié en tant que le domaine par défaut lorsque **ksetup** est exécuté.|

## <a name="remarks"></a>Notes

Le nom de domaine est stocké dans deux emplacements dans le Registre : **HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001** et **\CurrentControlSet\Control\Lsa\Kerberos**.

Vous ne pouvez pas supprimer le nom de domaine par défaut à partir du contrôleur de domaine, car cette opération réinitialise ses informations de DNS, et il suppression rendrait le contrôleur de domaine.

## <a name="BKMK_Examples"></a>Exemples

Par erreur la valeur est le nom de domaine par la faute d’orthographe dans «.COM » sur l’ordinateur local CORP. CONTOSO. CON
```
ksetup /setrealm CORP.CONTOSO.CON
```
Supprimer ce nom de domaine erronées à partir de l’ordinateur local :
```
ksetup /removerealm CORP.CONTOSO.CON
```
Vérifier la suppression en exécutant **ksetup** et examinez la sortie.

#### <a name="additional-references"></a>Références supplémentaires

-   [Ksetup](ksetup.md)
-   [Ksetup:setrealm](ksetup-setrealm.md)
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)