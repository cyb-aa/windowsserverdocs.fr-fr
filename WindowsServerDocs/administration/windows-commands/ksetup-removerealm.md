---
title: 'Ksetup : removerealm'
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 11858d8a24d4f125c83b3e4092ac48f336a9ef0b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374946"
---
# <a name="ksetupremoverealm"></a>Ksetup : removerealm



Supprime du registre toutes les informations pour le domaine spécifié. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
ksetup /removerealm <RealmName>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|@no__t 0RealmName >|Le nom de domaine est indiqué en tant que nom DNS en majuscules, par exemple CORP. CONTOSO.COM, et elle est indiquée comme domaine par défaut lorsque **Ksetup** est exécuté.|

## <a name="remarks"></a>Notes

Le nom de domaine est stocké à deux emplacements dans le registre : **HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001** et **\CurrentControlSet\Control\Lsa\Kerberos**.

Vous ne pouvez pas supprimer le nom de domaine par défaut du contrôleur de domaine, car cela réinitialisera ses informations DNS et risquerait de rendre le contrôleur de domaine inutilisable.

## <a name="BKMK_Examples"></a>Illustre

Par erreur, définissez le nom de domaine en orthographiant mal l’orthographe de « . COM » sur l’ordinateur local sur CORP. CONTOSO. &AMP;
```
ksetup /setrealm CORP.CONTOSO.CON
```
Supprimez ce nom de domaine erroné de l’ordinateur local :
```
ksetup /removerealm CORP.CONTOSO.CON
```
Vérifiez la suppression en exécutant **Ksetup** et passez en revue la sortie.

#### <a name="additional-references"></a>Références supplémentaires

-   [Ksetup](ksetup.md)
-   [Ksetup:setrealm](ksetup-setrealm.md)
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)