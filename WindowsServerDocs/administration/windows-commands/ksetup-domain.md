---
title: Ksetup:Domain
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2ef766e3-6071-44f2-946b-22ea5b61a508
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1f53e807891b434709b1a8faed7aae8e8d444f6e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857450"
---
# <a name="ksetupdomain"></a>Ksetup:Domain



Définit le nom de domaine pour toutes les opérations Kerberos. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
ksetup /domain <DomainName>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<DomainName>|Nom du domaine auquel vous souhaitez établir une connexion. Utilisez le nom de domaine complet ou un formulaire simple du nom, tel que contoso.com ou contoso.|

## <a name="remarks"></a>Notes

Aucun.

## <a name="BKMK_Examples"></a>Exemples

Établir une connexion à un domaine valide, tels que Microsoft à l’aide de la sous-commande /mapuser :
```
ksetup /mapuser principal@realm domain-user /domain domain-name
```
Lorsque la connexion est établie, vous recevez un nouveau ticket TGT ou un ticket TGT existante est actualisée.

#### <a name="additional-references"></a>Références supplémentaires

-   [Ksetup](ksetup.md)
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)