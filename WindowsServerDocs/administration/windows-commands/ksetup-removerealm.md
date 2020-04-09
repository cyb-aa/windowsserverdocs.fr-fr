---
title: 'Ksetup : removerealm'
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 39f0c6f0-4c50-4781-941e-0893495405e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1465ce08c0cf45de828683324b29fb2df8d0e893
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841452"
---
# <a name="ksetupremoverealm"></a>Ksetup : removerealm



Supprime du registre toutes les informations pour le domaine spécifié. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
ksetup /removerealm <RealmName>
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<RealmName >|Le nom de domaine est indiqué en tant que nom DNS en majuscules, par exemple CORP. CONTOSO.COM, et elle est indiquée comme domaine par défaut lorsque **Ksetup** est exécuté.|

## <a name="remarks"></a>Notes

Le nom de domaine est stocké à deux emplacements dans le registre : **HKEY_LOCAL_MACHINE \system\controlset001** et **\CurrentControlSet\Control\Lsa\Kerberos**.

Vous ne pouvez pas supprimer le nom de domaine par défaut du contrôleur de domaine, car cela réinitialisera ses informations DNS et risquerait de rendre le contrôleur de domaine inutilisable.

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre

Par erreur, définissez le nom de domaine en mal orthographié. COM sur l’ordinateur local en CORP. CONTOSO. &AMP;
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