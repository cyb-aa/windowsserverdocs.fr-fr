---
title: ksetup:addkdc
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 98bfc23a-14c4-401c-bcb3-9903c5cdde64
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d3e0d38bdec11618561ee4acaa32ffdd06695fab
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868530"
---
# <a name="ksetupaddkdc"></a>ksetup:addkdc



Ajoute une adresse de centre de Distribution de clés (KDC) pour le domaine Kerberos donné. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
ksetup /addkdc <RealmName> [<KDCName>] 
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<RealmName>|Le nom de domaine est indiqué comme un nom DNS en majuscules, telles que CORP. CONTOSO.COM et il est répertorié en tant que le domaine par défaut lorsque **ksetup** est exécuté. Il est de ce domaine que vous essayez d’ajouter l’autre contrôleur de domaine Kerberos.|
|\<KDCName>|Le nom de contrôleur de domaine Kerberos est établi comme un nom de domaine complet de la casse, comme mitkdc.microsoft.com. Si le nom de contrôleur de domaine Kerberos est omis, DNS localise KDC.|

## <a name="remarks"></a>Notes

Ces mappages sont stockés dans le Registre sous **HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\LSA\Kerberos\Domains**. Pour déployer des données de configuration de domaine Kerberos sur plusieurs ordinateurs, utilisez la distribution de logiciel enfichable et la stratégie de modèle de Configuration de sécurité au lieu d’utiliser **ksetup** explicitement sur des ordinateurs individuels.

L’ordinateur doit être redémarré avant que le nouveau paramètre de domaine sera être utilisé.

Pour vérifier le nom de domaine par défaut pour l’ordinateur, ou pour vérifier que cette commande a fonctionné comme prévu, exécutez **ksetup** à l’invite de commandes et vérifiez la sortie pour le KDC ajouté.

## <a name="BKMK_Examples"></a>Exemples

Configurer un serveur KDC Windows non - et le domaine que la station de travail doit utiliser :
```
ksetup /addkdc CORP.CONTOSO.COM mitkdc.contoso.com
```
Exécuter l’outil de Ksetup à la ligne de commande du même ordinateur, comme dans la commande précédente pour définir le mot de passe du compte ordinateur local «p@sswrd1% ?. Puis redémarrez l’ordinateur.
```
Ksetup /setcomputerpassword p@sswrd1%
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Ksetup](ksetup.md)
-   [Ksetup:setcomputerpassword](ksetup-setcomputerpassword.md)
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)