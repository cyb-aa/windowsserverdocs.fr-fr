---
title: 'Ksetup : setrealm'
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ab268c40-276b-46ef-ab16-d5ce7667fbed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1bbe5c000b7e84066c19511639fe3d92d7e4b558
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374900"
---
# <a name="ksetupsetrealm"></a>Ksetup : setrealm



Définit le nom d’un domaine Kerberos. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
ksetup /setrealm <DNSDomainName>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|@no__t 0DNSDomainName >|Le nom de domaine DNS peut se présenter sous la forme d’un nom de domaine complet ou d’un nom de domaine simple.|

## <a name="remarks"></a>Notes

Le paramètre de nom de domaine DNS doit être entré en majuscules. Dans le cas contraire, la commande **Ksetup** vous demandera de poursuivre la vérification.

La définition du domaine Kerberos sur un contrôleur de domaine n’est pas prise en charge. Si vous tentez de le faire, vous obtiendrez un avertissement et un échec de la commande.

## <a name="BKMK_Examples"></a>Illustre

Définissez le domaine de cet ordinateur sur un nom de domaine spécifique pour limiter l’accès par un contrôleur non-domaine uniquement au domaine Kerberos CONTOSO :
```
ksetup /setrealm CONTOSO
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Ksetup](ksetup.md)
-   [Ksetup:removerealm](ksetup-removerealm.md)