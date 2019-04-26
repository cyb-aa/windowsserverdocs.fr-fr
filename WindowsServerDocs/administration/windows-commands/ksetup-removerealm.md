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
ms.openlocfilehash: 3f62208d6576890529be80b1c6cb3cc073a2b4e6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853360"
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

Définir par inadvertance le nom de domaine par les fautes d’orthographe ». COM ? sur l’ordinateur local pour CORP. CONTOSO. CON
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
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)