---
title: 'Ksetup : domaine'
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ef766e3-6071-44f2-946b-22ea5b61a508
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f127eaf33e9ef6d597851c31a4167ceaa3516abb
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724685"
---
# <a name="ksetupdomain"></a>Ksetup : domaine



Définit le nom de domaine pour toutes les opérations Kerberos.

## <a name="syntax"></a>Syntaxe

```
ksetup /domain <DomainName>
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Nom_domaine>|Nom du domaine auquel vous souhaitez établir une connexion. Utilisez le nom de domaine complet ou une forme simple du nom, par exemple contoso.com ou contoso.|

## <a name="remarks"></a>Notes 

Aucun.

## <a name="examples"></a>Exemples

Établissez une connexion à un domaine valide, tel que Microsoft, à l’aide de la sous-commande/mapuser :
```
ksetup /mapuser principal@realm domain-user /domain domain-name
```
Une fois la connexion établie, vous recevrez un nouveau TGT ou un ticket TGT existant sera actualisé.

## <a name="additional-references"></a>Références supplémentaires

-   [Ksetup](ksetup.md)
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)