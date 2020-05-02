---
title: 'Ksetup : addenctypeattr'
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 32cc87d7-b9e1-4d14-9eb7-3b439c55aa3a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 26441f739979cde31715e5fb06b5f3ab59845d97
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724748"
---
# <a name="ksetupaddenctypeattr"></a>Ksetup : addenctypeattr



Ajoute l’attribut type de chiffrement à la liste des types possibles pour le domaine.

## <a name="syntax"></a>Syntaxe

```
ksetup /addenctypeattr <DomainName> {DES-CBC-CRC | DES-CBC-MD5 | RC4-HMAC-MD5 | AES128-CTS-HMAC-SHA1-96 | AES256-CTS-HMAC-SHA1-96}
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Nom_domaine>|Nom du domaine auquel vous souhaitez établir une connexion. Utilisez le nom de domaine complet ou une forme simple du nom, par exemple corp.contoso.com ou contoso.|
|Type de chiffrement|Il doit s’agir de l’un des types de chiffrement pris en charge suivants :</br>-DES-CBC-CRC</br>-DES-CBC-MD5</br>-RC4-HMAC-MD5</br>-AES128-CTS-HMAC-SHA1-96</br>-AES256-CTS-HMAC-SHA1-96|

## <a name="remarks"></a>Notes 

Pour afficher le type de chiffrement du ticket TGT (Ticket-Granting Ticket) Kerberos et de la clé de session, exécutez la commande **Klist** et affichez la sortie.

Vous pouvez définir ou ajouter plusieurs types de chiffrement en séparant les types de chiffrement dans la commande par un espace. Toutefois, vous ne pouvez le faire que pour un domaine à la fois.

Si la commande réussit ou échoue, un message d’État s’affiche.

Pour définir le domaine auquel vous souhaitez vous connecter et utiliser, exécutez la commande **Ksetup/Domain \<DomainName>** .

## <a name="examples"></a>Exemples

Déterminez les types de chiffrement actuels qui sont définis sur cet ordinateur :
```
klist
```
Définissez le domaine sur corp.contoso.com :
```
ksetup /domain corp.contoso.com
```
Ajoutez le type de chiffrement AES-256-CTS-HMAC-SHA1-96 à la liste des types possibles pour le domaine corp.contoso.com :
```
ksetup /addenctypeattr corp.contoso.com AES-256-CTS-HMAC-SHA1-96
```
Définissez l’attribut type de chiffrement sur AES-256-CTS-HMAC-SHA1-96 pour le domaine corp.contoso.com :
```
ksetup /setenctypeattr corp.contoso.com AES-256-CTS-HMAC-SHA1-96
```
Vérifiez que l’attribut type de chiffrement a été défini comme étant destiné au domaine :
```
ksetup /getenctypeattr corp.contoso.com
```

## <a name="additional-references"></a>Références supplémentaires

-   [Klist](klist.md)
-   [Ksetup:domain](ksetup-domain.md)
-   [Ksetup:setenctypeattr](ksetup-setenctypeattr.md)
-   [Ksetup:getenctypeattr](ksetup-getenctypeattr.md)
-   [Ksetup:delenctypeattr](ksetup-delenctypeattr.md)
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)