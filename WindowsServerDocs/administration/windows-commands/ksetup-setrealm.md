---
title: Ksetup:setrealm
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: aa6b2a21904ec4dae1e60def5bd36647291b1af6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877400"
---
# <a name="ksetupsetrealm"></a>Ksetup:setrealm



Définit le nom d’un domaine Kerberos. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
ksetup /setrealm <DNSDomainName>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<DNSDomainName>|Le nom de domaine DNS peut être sous la forme d’un nom de domaine complet ou le nom de domaine simple.|

## <a name="remarks"></a>Notes

Le paramètre de nom de domaine DNS doit être entré en lettres majuscules. Sinon, le **ksetup** commande vous demandera de vérification continuer.

Définissez le domaine Kerberos sur un contrôleur de domaine n’est pas pris en charge. Une tentative pour ce faire, entraîne un avertissement et Échec d’une commande.

## <a name="BKMK_Examples"></a>Exemples

Définir le domaine pour cet ordinateur à un nom de domaine spécifique à restreindre l’accès par un contrôleur de domaine non simplement au domaine Kerberos de CONTOSO :
```
ksetup /setrealm CONTOSO
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Ksetup](ksetup.md)
-   [Ksetup:removerealm](ksetup-removerealm.md)