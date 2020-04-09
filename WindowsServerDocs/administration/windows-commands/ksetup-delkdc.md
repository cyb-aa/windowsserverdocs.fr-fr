---
title: 'Ksetup : delkdc'
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7d6ec389-094c-4a7b-a78b-605497ddc289
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 63b264da227d51b6f47f982c66828536bd677920
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841692"
---
# <a name="ksetupdelkdc"></a>Ksetup : delkdc



Supprime les instances de noms de centre de distribution de clés (KDC) pour le domaine Kerberos. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
ksetup /delkdc <RealmName> <KDCName>
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<RealmName >|Le nom de domaine est indiqué en tant que nom DNS en majuscules, par exemple CORP. CONTOSO.COM, et elle est indiquée comme domaine par défaut lorsque **Ksetup** est exécuté. Il s’agit de ce domaine à partir duquel vous tentez de supprimer l’autre KDC.|
|\<KDCName >|Le nom KDC est indiqué comme un nom de domaine complet qui ne respecte pas la casse, par exemple mitkdc.contoso.com.|

## <a name="remarks"></a>Notes

Ces mappages sont stockés dans le registre dans **HKEY_LOCAL_MACHINE \system\currentcontrolset\control\lsa\kerberos\domains**. Pour supprimer les données de configuration de domaine de plusieurs ordinateurs, utilisez le composant logiciel enfichable modèle de configuration de sécurité et la distribution de stratégie au lieu d’utiliser le modèle d’informations de **sécurité explicitement sur** des ordinateurs individuels.

Sur les ordinateurs exécutant Windows 2000 Server avec Service Pack 1 (SP1) et versions antérieures, l’ordinateur doit être redémarré avant que la configuration du paramètre de domaine modifié ne soit utilisée.

Pour vérifier le nom de domaine par défaut de l’ordinateur, ou pour vérifier que cette commande fonctionnait comme prévu, exécutez **Ksetup** à l’invite de commandes et vérifiez que le KDC qui a été supprimé n’existe pas dans la liste.

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre

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