---
title: 'Ksetup : getenctypeattr'
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6c7ec002-355e-474d-bc27-27215049f1a8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 60de138ac73140c69e9a863083e01a51c0e13ca3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841532"
---
# <a name="ksetupgetenctypeattr"></a>Ksetup : getenctypeattr



Récupère l’attribut de type de chiffrement pour le domaine. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
ksetup /getenctypeattr <DomainName> 
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<DomainName >|Nom du domaine auquel vous souhaitez établir une connexion. Utilisez le nom de domaine complet ou une forme simple du nom, par exemple corp.contoso.com ou contoso.|

## <a name="remarks"></a>Notes

Pour afficher le type de chiffrement du ticket TGT (Ticket-Granting Ticket) Kerberos et de la clé de session, exécutez la commande **Klist** et affichez la sortie.

Si la commande réussit ou échoue, un message d’État s’affiche lorsque l’exécution a réussi ou a échoué.

Pour définir le domaine auquel vous souhaitez vous connecter et utiliser, exécutez la commande **Ksetup/domain \<DomainName >** .

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre

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