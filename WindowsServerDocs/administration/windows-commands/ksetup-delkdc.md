---
title: 'Ksetup : delkdc'
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: b918f8c2fa0ec09c2aae77517a0ee2c9e77ce2dc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375139"
---
# <a name="ksetupdelkdc"></a>Ksetup : delkdc



Supprime les instances de noms de centre de distribution de clés (KDC) pour le domaine Kerberos. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
ksetup /delkdc <RealmName> <KDCName>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|@no__t 0RealmName >|Le nom de domaine est indiqué en tant que nom DNS en majuscules, par exemple CORP. CONTOSO.COM, et elle est indiquée comme domaine par défaut lorsque **Ksetup** est exécuté. Il s’agit de ce domaine à partir duquel vous tentez de supprimer l’autre KDC.|
|@no__t 0KDCName >|Le nom KDC est indiqué comme un nom de domaine complet qui ne respecte pas la casse, par exemple mitkdc.contoso.com.|

## <a name="remarks"></a>Notes

Ces mappages sont stockés dans le registre dans **HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\LSA\Kerberos\Domains**. Pour supprimer les données de configuration de domaine de plusieurs ordinateurs, utilisez le composant logiciel enfichable modèle de configuration de sécurité et la distribution de stratégie au lieu d’utiliser le modèle d’informations de **sécurité explicitement sur** des ordinateurs individuels.

Sur les ordinateurs exécutant Windows 2000 Server avec Service Pack 1 (SP1) et versions antérieures, l’ordinateur doit être redémarré avant que la configuration du paramètre de domaine modifié ne soit utilisée.

Pour vérifier le nom de domaine par défaut de l’ordinateur, ou pour vérifier que cette commande fonctionnait comme prévu, exécutez **Ksetup** à l’invite de commandes et vérifiez que le KDC qui a été supprimé n’existe pas dans la liste.

## <a name="BKMK_Examples"></a>Illustre

Les exigences de sécurité de cet ordinateur ont été modifiées, de sorte que le lien entre le domaine Windows et le domaine non-Windows doit être supprimé. Tout d’abord, déterminez l’Association à supprimer et générez la sortie des associations existantes :
```
ksetup
```
Supprimez l’Association à l’aide de la commande suivante :
```
Ksetup /delkdc CORP.CONTOSO.COM mitkdc.contoso.com
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Ksetup](ksetup.md)
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)