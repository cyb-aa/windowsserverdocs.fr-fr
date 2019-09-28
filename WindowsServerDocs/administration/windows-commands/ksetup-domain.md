---
title: 'Ksetup : domaine'
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 0a4d9f09def32c7518046c25887f4154020c5d7e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375118"
---
# <a name="ksetupdomain"></a>Ksetup : domaine



Définit le nom de domaine pour toutes les opérations Kerberos. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
ksetup /domain <DomainName>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|@no__t 0DomainName >|Nom du domaine auquel vous souhaitez établir une connexion. Utilisez le nom de domaine complet ou une forme simple du nom, par exemple contoso.com ou contoso.|

## <a name="remarks"></a>Notes

Aucun.

## <a name="BKMK_Examples"></a>Illustre

Établissez une connexion à un domaine valide, tel que Microsoft, à l’aide de la sous-commande/mapuser :
```
ksetup /mapuser principal@realm domain-user /domain domain-name
```
Une fois la connexion établie, vous recevrez un nouveau TGT ou un ticket TGT existant sera actualisé.

#### <a name="additional-references"></a>Références supplémentaires

-   [Ksetup](ksetup.md)
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)