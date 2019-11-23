---
title: 'Ksetup : setenctypeattr'
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: bcc268ecb591e3008d7274f6c9f6d6017c86f99d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374933"
---
# <a name="ksetupsetenctypeattr"></a>Ksetup : setenctypeattr



Définit l’attribut de type de chiffrement pour le domaine. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
ksetup /setenctypeattr <Domain name> {DES-CBC-CRC | DES-CBC-MD5 | RC4-HMAC-MD5 | AES128-CTS-HMAC-SHA1-96 | AES256-CTS-HMAC-SHA1-96}
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<DomainName >|Nom du domaine auquel vous souhaitez établir une connexion. Utilisez le nom de domaine complet ou une forme simple du nom, par exemple corp.contoso.com ou contoso.|
|Type de chiffrement|Il doit s’agir de l’un des types de chiffrement pris en charge suivants :</br>-DES-CBC-CRC</br>-DES-CBC-MD5</br>-RC4-HMAC-MD5</br>-AES128-CTS-HMAC-SHA1-96</br>-AES256-CTS-HMAC-SHA1-96|

## <a name="remarks"></a>Notes

Pour afficher le type de chiffrement du ticket TGT (Ticket-Granting Ticket) Kerberos et de la clé de session, exécutez la commande **Klist** et affichez la sortie.

Vous pouvez définir ou ajouter plusieurs types de chiffrement en séparant les types de chiffrement dans la commande par un espace. Toutefois, vous ne pouvez le faire que pour un domaine à la fois.

Si la commande réussit ou échoue, un message d’État s’affiche.

Pour définir le domaine auquel vous souhaitez vous connecter et utiliser, exécutez la commande **Ksetup/domain \<DomainName >** .

## <a name="BKMK_Examples"></a>Illustre

Déterminez les types de chiffrement actuels qui sont définis sur cet ordinateur :
```
klist
```
Définissez le domaine sur corp.contoso.com :
```
ksetup /domain corp.contoso.com
```
Définissez l’attribut type de chiffrement sur AES-256-CTS-HMAC-SHA1-96 pour le domaine corp.contoso.com :
```
ksetup /setenctypeattr corp.contoso.com AES-256-CTS-HMAC-SHA1-96
```
Vérifiez que l’attribut type de chiffrement a été défini comme étant destiné au domaine :
```
ksetup /getenctypeattr corp.contoso.com
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Klist](klist.md)
-   [Ksetup:domain](ksetup-domain.md)
-   [Ksetup:addenctypeattr](ksetup-addenctypeattr.md)
-   [Ksetup:getenctypeattr](ksetup-getenctypeattr.md)
-   [Ksetup:delenctypeattr](ksetup-delenctypeattr.md)
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)