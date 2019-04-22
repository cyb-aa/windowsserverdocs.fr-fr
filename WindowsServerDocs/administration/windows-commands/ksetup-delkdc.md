---
title: ksetup:delkdc
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7d6ec389-094c-4a7b-a78b-605497ddc289
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6e2fa065ca60338b04cc8718e199805cc494c908
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814560"
---
# <a name="ksetupdelkdc"></a>ksetup:delkdc



Supprime des instances de noms de centre de Distribution de clés (KDC) pour le domaine Kerberos. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
ksetup /delkdc <RealmName> <KDCName>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<RealmName>|Le nom de domaine est indiqué comme un nom DNS en majuscules, telles que CORP. CONTOSO.COM et il est répertorié en tant que le domaine par défaut lorsque **ksetup** est exécuté. Il est de ce domaine à partir de laquelle vous essayez de supprimer l’autre contrôleur de domaine Kerberos.|
|\<KDCName>|Le nom de contrôleur de domaine Kerberos est établi comme un nom de domaine complet, non-respect de la casse, comme mitkdc.contoso.com.|

## <a name="remarks"></a>Notes

Ces mappages sont stockés dans le Registre **HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\LSA\Kerberos\Domains**. Pour supprimer les données de configuration de domaine à partir de plusieurs ordinateurs, utilisez la distribution de logiciel enfichable et la stratégie de modèle de Configuration de sécurité au lieu d’utiliser **ksetup** explicitement sur des ordinateurs individuels.

Sur les ordinateurs exécutant Windows 2000 Server avec Service Pack 1 (SP1) et versions antérieures, l’ordinateur doit être redémarré avant que la configuration du paramètre domaine modifié sera utilisée.

Pour vérifier le nom de domaine par défaut pour l’ordinateur, ou pour vérifier que cette commande a fonctionné comme prévu, exécutez **ksetup** à l’invite de commandes et vérifiez que le contrôleur de domaine Kerberos qui a été supprimé n’existe pas dans la liste.

## <a name="BKMK_Examples"></a>Exemples

Les exigences de sécurité pour cet ordinateur ont été modifiés, le lien entre le domaine Windows et le domaine non Windows doit être supprimé. Tout d’abord, déterminez laquelle l’association à supprimer et de produire le résultat des associations existantes :
```
ksetup
```
Supprimer l’association à l’aide de la commande suivante :
```
Ksetup /delkdc CORP.CONTOSO.COM mitkdc.contoso.com
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Ksetup](ksetup.md)
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)