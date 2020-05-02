---
title: 'Ksetup : setrealm'
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ab268c40-276b-46ef-ab16-d5ce7667fbed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 453977ac39dd3a52b4f5a3104995f944e4a48392
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724558"
---
# <a name="ksetupsetrealm"></a>Ksetup : setrealm



Définit le nom d’un domaine Kerberos.

## <a name="syntax"></a>Syntaxe

```
ksetup /setrealm <DNSDomainName>
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<> nom_domaine_dns|Le nom de domaine DNS peut se présenter sous la forme d’un nom de domaine complet ou d’un nom de domaine simple.|

## <a name="remarks"></a>Notes 

Le paramètre de nom de domaine DNS doit être entré en majuscules. Dans le cas contraire, la commande **Ksetup** vous demandera de poursuivre la vérification.

La définition du domaine Kerberos sur un contrôleur de domaine n’est pas prise en charge. Si vous tentez de le faire, vous obtiendrez un avertissement et un échec de la commande.

## <a name="examples"></a>Exemples

Définissez le domaine de cet ordinateur sur un nom de domaine spécifique pour limiter l’accès par un contrôleur non-domaine uniquement au domaine Kerberos CONTOSO :
```
ksetup /setrealm CONTOSO
```

## <a name="additional-references"></a>Références supplémentaires

-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Ksetup](ksetup.md)
-   [Ksetup:removerealm](ksetup-removerealm.md)