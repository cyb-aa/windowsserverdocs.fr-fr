---
title: 'Ksetup : delkdc'
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7d6ec389-094c-4a7b-a78b-605497ddc289
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 19ebe322d414d1ae9007275772ccd747f6f0ff8d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724655"
---
# <a name="ksetupdelkdc"></a>Ksetup : delkdc



Supprime les instances de noms de centre de distribution de clés (KDC) pour le domaine Kerberos.

## <a name="syntax"></a>Syntaxe

```
ksetup /delkdc <RealmName> <KDCName>
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<RealmName>|Le nom de domaine est indiqué en tant que nom DNS en majuscules, par exemple CORP. CONTOSO.COM, et elle est indiquée comme domaine par défaut lorsque **Ksetup** est exécuté. Il s’agit de ce domaine à partir duquel vous tentez de supprimer l’autre KDC.|
|\<KDCName>|Le nom KDC est indiqué comme un nom de domaine complet qui ne respecte pas la casse, par exemple mitkdc.contoso.com.|

## <a name="remarks"></a>Notes 

Ces mappages sont stockés dans le registre dans **HKEY_LOCAL_MACHINE \system\currentcontrolset\control\lsa\kerberos\domains**. Pour supprimer les données de configuration de domaine de plusieurs ordinateurs, utilisez le composant logiciel enfichable modèle de configuration de sécurité et la distribution de stratégie au lieu d’utiliser le modèle d’informations de **sécurité explicitement sur** des ordinateurs individuels.

Sur les ordinateurs exécutant Windows 2000 Server avec Service Pack 1 (SP1) et versions antérieures, l’ordinateur doit être redémarré avant que la configuration du paramètre de domaine modifié ne soit utilisée.

Pour vérifier le nom de domaine par défaut de l’ordinateur, ou pour vérifier que cette commande fonctionnait comme prévu, exécutez **Ksetup** à l’invite de commandes et vérifiez que le KDC qui a été supprimé n’existe pas dans la liste.

## <a name="examples"></a>Exemples

Les exigences de sécurité de cet ordinateur ont été modifiées, de sorte que le lien entre le domaine Windows et le domaine non-Windows doit être supprimé. Tout d’abord, déterminez l’Association à supprimer et générez la sortie des associations existantes :
```
ksetup
```
Supprimez l’Association à l’aide de la commande suivante :
```
Ksetup /delkdc CORP.CONTOSO.COM mitkdc.contoso.com
```

## <a name="additional-references"></a>Références supplémentaires

-   [Ksetup](ksetup.md)
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)