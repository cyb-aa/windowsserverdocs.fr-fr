---
title: 'Ksetup : addkdc'
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 98bfc23a-14c4-401c-bcb3-9903c5cdde64
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 76d592e4f1c32305d6f939a66a6ad42cd582b032
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724767"
---
# <a name="ksetupaddkdc"></a>Ksetup : addkdc



Ajoute une adresse de centre de distribution de clés (KDC) pour le domaine Kerberos donné.

## <a name="syntax"></a>Syntaxe

```
ksetup /addkdc <RealmName> [<KDCName>] 
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<RealmName>|Le nom de domaine est indiqué en tant que nom DNS en majuscules, par exemple CORP. CONTOSO.COM, et elle est indiquée comme domaine par défaut lorsque **Ksetup** est exécuté. C’est à ce domaine que vous essayez d’ajouter l’autre KDC.|
|\<KDCName>|Le nom KDC est indiqué comme un nom de domaine complet qui ne respecte pas la casse, par exemple mitkdc.microsoft.com. Si le nom du KDC est omis, DNS localise les KDC.|

## <a name="remarks"></a>Notes 

Ces mappages sont stockés dans le Registre sous **HKEY_LOCAL_MACHINE \system\currentcontrolset\control\lsa\kerberos\domains**. Pour déployer des données de configuration de domaine Kerberos sur plusieurs ordinateurs, utilisez le composant logiciel enfichable modèle de configuration de sécurité et la distribution de stratégie au lieu d’utiliser **Ksetup** explicitement sur des ordinateurs individuels.

L’ordinateur doit être redémarré pour que le nouveau paramètre de domaine soit utilisé.

Pour vérifier le nom de domaine par défaut de l’ordinateur, ou pour vérifier que cette commande fonctionnait comme prévu, exécutez **Ksetup** à l’invite de commandes et vérifiez la sortie du KDC ajouté.

## <a name="examples"></a>Exemples

Configurez un serveur KDC non-Windows et le domaine que la station de travail doit utiliser :
```
ksetup /addkdc CORP.CONTOSO.COM mitkdc.contoso.com
```
Exécutez l’outil Ksetup sur la ligne de commande du même ordinateur que dans la commande précédente pour définir le mot de passe du compte p@sswrd1d’ordinateur local sur%. Ensuite, redémarrez l'ordinateur.
```
Ksetup /setcomputerpassword p@sswrd1%
```

## <a name="additional-references"></a>Références supplémentaires

-   [Ksetup](ksetup.md)
-   [Ksetup:setcomputerpassword](ksetup-setcomputerpassword.md)
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)