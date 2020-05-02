---
title: 'Ksetup : removerealm'
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 39f0c6f0-4c50-4781-941e-0893495405e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bb7bf4663594a6c164d6495a9ba4cd81942afb79
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724606"
---
# <a name="ksetupremoverealm"></a>Ksetup : removerealm



Supprime du registre toutes les informations pour le domaine spécifié.

## <a name="syntax"></a>Syntaxe

```
ksetup /removerealm <RealmName>
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<RealmName>|Le nom de domaine est indiqué en tant que nom DNS en majuscules, par exemple CORP. CONTOSO.COM, et elle est indiquée comme domaine par défaut lorsque **Ksetup** est exécuté.|

## <a name="remarks"></a>Notes 

Le nom de domaine est stocké à deux emplacements dans le registre : **HKEY_LOCAL_MACHINE \system\controlset001** et **\CurrentControlSet\Control\Lsa\Kerberos**.

Vous ne pouvez pas supprimer le nom de domaine par défaut du contrôleur de domaine, car cela réinitialisera ses informations DNS et risquerait de rendre le contrôleur de domaine inutilisable.

## <a name="examples"></a>Exemples

Par erreur, définissez le nom de domaine en mal orthographié. COM sur l’ordinateur local en CORP. CONTOSO. &
```
ksetup /setrealm CORP.CONTOSO.CON
```
Supprimez ce nom de domaine erroné de l’ordinateur local :
```
ksetup /removerealm CORP.CONTOSO.CON
```
Vérifiez la suppression en exécutant **Ksetup** et passez en revue la sortie.

## <a name="additional-references"></a>Références supplémentaires

-   [Ksetup](ksetup.md)
-   [Ksetup:setrealm](ksetup-setrealm.md)
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)