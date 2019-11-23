---
title: 'Ksetup : addkdc'
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 66efe4e56007aff39b83c92dfea2afaadcfc0210
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375212"
---
# <a name="ksetupaddkdc"></a>Ksetup : addkdc



Ajoute une adresse de centre de distribution de clés (KDC) pour le domaine Kerberos donné. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
ksetup /addkdc <RealmName> [<KDCName>] 
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<RealmName >|Le nom de domaine est indiqué en tant que nom DNS en majuscules, par exemple CORP. CONTOSO.COM, et elle est indiquée comme domaine par défaut lorsque **Ksetup** est exécuté. C’est à ce domaine que vous essayez d’ajouter l’autre KDC.|
|\<KDCName >|Le nom KDC est indiqué comme un nom de domaine complet qui ne respecte pas la casse, par exemple mitkdc.microsoft.com. Si le nom du KDC est omis, DNS localise les KDC.|

## <a name="remarks"></a>Notes

Ces mappages sont stockés dans le Registre sous **HKEY_LOCAL_MACHINE \system\currentcontrolset\control\lsa\kerberos\domains**. Pour déployer des données de configuration de domaine Kerberos sur plusieurs ordinateurs, utilisez le composant logiciel enfichable modèle de configuration de sécurité et la distribution de stratégie au lieu d’utiliser **Ksetup** explicitement sur des ordinateurs individuels.

L’ordinateur doit être redémarré pour que le nouveau paramètre de domaine soit utilisé.

Pour vérifier le nom de domaine par défaut de l’ordinateur, ou pour vérifier que cette commande fonctionnait comme prévu, exécutez **Ksetup** à l’invite de commandes et vérifiez la sortie du KDC ajouté.

## <a name="BKMK_Examples"></a>Illustre

Configurez un serveur KDC non-Windows et le domaine que la station de travail doit utiliser :
```
ksetup /addkdc CORP.CONTOSO.COM mitkdc.contoso.com
```
Exécutez l’outil Ksetup sur la ligne de commande du même ordinateur que dans la commande précédente pour définir le mot de passe du compte d’ordinateur local sur «p@sswrd1% ». Ensuite, redémarrez l’ordinateur.
```
Ksetup /setcomputerpassword p@sswrd1%
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Ksetup](ksetup.md)
-   [Ksetup:setcomputerpassword](ksetup-setcomputerpassword.md)
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)