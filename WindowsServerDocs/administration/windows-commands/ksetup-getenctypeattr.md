---
title: 'Ksetup : getenctypeattr'
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6c7ec002-355e-474d-bc27-27215049f1a8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f8363113d4fbb310d98b40d852b36a00f20320e6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724633"
---
# <a name="ksetupgetenctypeattr"></a>Ksetup : getenctypeattr



Récupère l’attribut de type de chiffrement pour le domaine.

## <a name="syntax"></a>Syntaxe

```
ksetup /getenctypeattr <DomainName> 
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Nom_domaine>|Nom du domaine auquel vous souhaitez établir une connexion. Utilisez le nom de domaine complet ou une forme simple du nom, par exemple corp.contoso.com ou contoso.|

## <a name="remarks"></a>Notes 

Pour afficher le type de chiffrement du ticket TGT (Ticket-Granting Ticket) Kerberos et de la clé de session, exécutez la commande **Klist** et affichez la sortie.

Si la commande réussit ou échoue, un message d’État s’affiche lorsque l’exécution a réussi ou a échoué.

Pour définir le domaine auquel vous souhaitez vous connecter et utiliser, exécutez la commande **Ksetup/Domain \<DomainName>** .

## <a name="examples"></a>Exemples

Vérifiez l’attribut type de chiffrement pour le domaine :
```
ksetup /getenctypeattr mit.contoso.com
```

## <a name="additional-references"></a>Références supplémentaires

-   [Klist](klist.md)
-   [Ksetup:domain](ksetup-domain.md)
-   [Ksetup:addenctypeattr](ksetup-addenctypeattr.md)
-   [Ksetup:setenctypeattr](ksetup-setenctypeattr.md)
-   [Ksetup:delenctypeattr](ksetup-delenctypeattr.md)
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)