---
title: ksetup:setenctypeattr
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 88fb913e-6b57-48d9-8c16-a035ab2977ac
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a91539ec7a9e0ce4c75d5165da1b88ae36d3fe6c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879200"
---
# <a name="ksetupsetenctypeattr"></a>ksetup:setenctypeattr



Définit l’attribut de type de chiffrement pour le domaine. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
ksetup /setenctypeattr <Domain name> {DES-CBC-CRC | DES-CBC-MD5 | RC4-HMAC-MD5 | AES128-CTS-HMAC-SHA1-96 | AES256-CTS-HMAC-SHA1-96}
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<DomainName>|Nom du domaine auquel vous souhaitez établir une connexion. Utilisez le nom de domaine complet ou un formulaire simple du nom, tel que corp.contoso.com ou contoso.|
|Type de chiffrement|Doit être un des types de chiffrement pris en charge suivants :</br>-   DES-CBC-CRC</br>-   DES-CBC-MD5</br>-   RC4-HMAC-MD5</br>-   AES128-CTS-HMAC-SHA1-96</br>-   AES256-CTS-HMAC-SHA1-96|

## <a name="remarks"></a>Notes

Pour afficher le type de chiffrement pour le Kerberos ticket-granting ticket (TGT ticket) et la clé de session, exécutez le **klist** commande et afficher la sortie.

Vous pouvez définir ou ajouter plusieurs types de chiffrement en séparant les types de chiffrement dans la commande par un espace. Toutefois, vous pouvez uniquement le faire pour un domaine à la fois.

Si la commande réussit ou échoue, un message d’état s’affiche.

Pour définir le domaine que vous souhaitez vous connecter à et utiliser, exécutez le **ksetup /domain \<DomainName >** commande.

## <a name="BKMK_Examples"></a>Exemples

Déterminer les types de chiffrement actuel qui sont définies sur cet ordinateur :
```
klist
```
Définissez le domaine corp.contoso.com :
```
ksetup /domain corp.contoso.com
```
La valeur est l’attribut de type de chiffrement AES-256-CTS-HMAC-SHA1-96 pour le domaine corp.contoso.com :
```
ksetup /setenctypeattr corp.contoso.com AES-256-CTS-HMAC-SHA1-96
```
Vérifiez que l’attribut de type de chiffrement a été définie comme prévu pour le domaine :
```
ksetup /getenctypeattr corp.contoso.com
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Klist](klist.md)
-   [Ksetup:domain](ksetup-domain.md)
-   [Ksetup:addenctypeattr](ksetup-addenctypeattr.md)
-   [Ksetup:getenctypeattr](ksetup-getenctypeattr.md)
-   [Ksetup:delenctypeattr](ksetup-delenctypeattr.md)
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)