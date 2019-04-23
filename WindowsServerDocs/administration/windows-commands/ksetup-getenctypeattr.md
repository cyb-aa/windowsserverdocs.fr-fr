---
title: ksetup:getenctypeattr
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6c7ec002-355e-474d-bc27-27215049f1a8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 302f94616f98eb350332b08ad37a58305a0a0be1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841990"
---
# <a name="ksetupgetenctypeattr"></a>ksetup:getenctypeattr



Récupère l’attribut de type de chiffrement pour le domaine. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
ksetup /getenctypeattr <DomainName> 
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<DomainName>|Nom du domaine auquel vous souhaitez établir une connexion. Utilisez le nom de domaine complet ou un formulaire simple du nom, tel que corp.contoso.com ou contoso.|

## <a name="remarks"></a>Notes

Pour afficher le type de chiffrement pour le Kerberos ticket-granting ticket (TGT ticket) et la clé de session, exécutez le **klist** commande et afficher la sortie.

Si la commande réussit ou échoue, un message d’état s’affiche à l’achèvement réussi ou échoué.

Pour définir le domaine que vous souhaitez vous connecter à et utiliser, exécutez le **ksetup /domain \<DomainName >** commande.

## <a name="BKMK_Examples"></a>Exemples

Vérifiez que l’attribut de type de chiffrement pour le domaine :
```
ksetup /getenctypeattr mit.contoso.com
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Klist](klist.md)
-   [Ksetup:domain](ksetup-domain.md)
-   [Ksetup:addenctypeattr](ksetup-addenctypeattr.md)
-   [Ksetup:setenctypeattr](ksetup-setenctypeattr.md)
-   [Ksetup:delenctypeattr](ksetup-delenctypeattr.md)
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)