---
title: ksetup:mapuser
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 84113e6e-89ff-488a-9cd0-f14bbf23b543
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2828f92b20cafcb571c81c8ceae28c741fbe025a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872860"
---
# <a name="ksetupmapuser"></a>ksetup:mapuser



Mappe le nom d’une entité Kerberos vers un compte. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
ksetup /mapuser <Principal> <Account>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Principal>|Le nom de domaine complet de n’importe quel principal ; par exemple, mike@corp.CONTOSO.COM.|
|\<Compte >|N’importe quel nom de groupe compte ou de sécurité qui existe sur cet ordinateur, telles que les invités, les utilisateurs de domaine ou administrateur.|

## <a name="remarks"></a>Notes

Un compte peut être spécifiquement identifié, telles que les invités du domaine. Ou vous pouvez utiliser le caractère générique (*) pour inclure tous les comptes.

Si un nom de compte est omis, le mappage est supprimé pour le principal spécifié.

L’ordinateur authentifiera uniquement les principaux du domaine donné si ils présentent des tickets Kerberos valides.

Utilisez **ksetup** sans paramètres ou arguments à voir actuel mis en correspondance les paramètres et le domaine par défaut.

Chaque fois que les modifications sont apportées pour le centre de Distribution de clés (KDC) externe et la configuration de domaine, un redémarrage de l’ordinateur où le paramètre a été modifié est nécessaire.

## <a name="BKMK_Examples"></a>Exemples

Mapper le compte Danseglio dans le domaine Kerberos CONTOSO pour le compte invité sur cet ordinateur, lui accorder tous les privilèges d’un membre du compte Invité prédéfini sans devoir s’authentifier à cet ordinateur :
```
ksetup /mapuser mike@corp.CONTOSO.COM guest
```
Supprimer le mappage du compte Danseglio au compte d’invité sur cet ordinateur pour lui empêcher de s’authentifier sur cet ordinateur avec ses informations d’identification à partir de CONTOSO :
```
ksetup /mapuser mike@corp.CONTOSO.COM 
```
Mapper le compte Danseglio dans le domaine CONTOSO Kerberos à n’importe quel compte existant sur cet ordinateur. (uniquement si l’utilisateur standard et les comptes invités sont actifs sur cet ordinateur, les privilèges de Mike seront définis à ceux) :
```
ksetup /mapuser mike@corp.CONTOSO.COM *
```
Mapper tous les comptes dans le domaine CONTOSO Kerberos à n’importe quel compte existant du même nom sur cet ordinateur :
```
ksetup /mapuser * *
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Ksetup](ksetup.md)