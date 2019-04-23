---
title: ksetup:delenctypeattr
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4fc25ef3-e271-4229-a712-72c507df55aa
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9c2cc96e8156cafd3846422596abe62513e275b3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838130"
---
# <a name="ksetupdelenctypeattr"></a>ksetup:delenctypeattr



Supprime l’attribut de type de chiffrement pour le domaine. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
ksetup /delenctypeattr <DomainName> 
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<DomainName>|Nom du domaine auquel vous souhaitez établir une connexion. Utilisez le nom de domaine complet ou un formulaire simple du nom, tel que corp.contoso.com ou contoso.|

## <a name="remarks"></a>Notes

Pour afficher le type de chiffrement pour le Kerberos ticket-granting ticket (TGT ticket) et la clé de session, exécutez le **klist** commande et afficher la sortie.

Un message d’état s’affiche à l’achèvement réussi ou échoué.

Pour définir le domaine que vous souhaitez vous connecter à et utiliser, exécutez le **ksetup /domain \<DomainName >** commande.

## <a name="BKMK_Examples"></a>Exemples

Déterminer les types de chiffrement actuel qui sont définies sur cet ordinateur :
```
klist
```
Définissez le domaine à mit.contoso.com :
```
ksetup /domain mit.contoso.com
```
Vérifiez ce qui est l’attribut de type de chiffrement pour le domaine :
```
ksetup /getenctypeattr mit.contoso.com
```
Supprimez l’attribut de type de chiffrement de jeu pour le domaine mit.contoso.com :
```
ksetup /delenctypeattr mit.contoso.com
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Klist](klist.md)
-   [Ksetup:domain](ksetup-domain.md)
-   [Ksetup:addenctypeattr](ksetup-addenctypeattr.md)
-   [Ksetup:setenctypeattr](ksetup-setenctypeattr.md)
-   [Ksetup:delenctypeattr](ksetup-delenctypeattr.md)
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)