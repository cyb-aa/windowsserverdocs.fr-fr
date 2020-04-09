---
title: 'Ksetup : mapuser'
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 84113e6e-89ff-488a-9cd0-f14bbf23b543
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f61c67fa21eccb77601b78aed51791259d609c5e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841482"
---
# <a name="ksetupmapuser"></a>Ksetup : mapuser



Mappe le nom d’un principal Kerberos à un compte. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
ksetup /mapuser <Principal> <Account>
```

#### <a name="parameters"></a>Paramètres

|  Paramètre   |                                                   Description                                                   |
|--------------|-----------------------------------------------------------------------------------------------------------------|
| \<> principal |              Nom de domaine complet d’un principal ; par exemple, mike@corp.CONTOSO.COM.              |
|  > de compte \<  | Tout nom de compte ou de groupe de sécurité existant sur cet ordinateur, par exemple un invité, un utilisateur de domaine ou un administrateur. |

## <a name="remarks"></a>Notes

Un compte peut être identifié spécifiquement, tel que les invités du domaine. Vous pouvez également utiliser le caractère générique (*) pour inclure tous les comptes.

Si un nom de compte est omis, le mappage est supprimé pour le principal spécifié.

L’ordinateur n’authentifiera que les principaux du domaine donné s’ils présentent des tickets Kerberos valides.

Utilisez **Ksetup** sans paramètres ou arguments pour voir les paramètres mappés actuels et le domaine par défaut.

Chaque fois que des modifications sont apportées à l’centre de distribution de clés externe (KDC) et à la configuration du domaine, un redémarrage de l’ordinateur sur lequel le paramètre a été modifié est nécessaire.

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre

Mappez le compte de Mike Danseglio dans le domaine Kerberos CONTOSO au compte invité sur cet ordinateur, en lui accordant tous les privilèges d’un membre du compte invité intégré sans avoir à s’authentifier sur cet ordinateur :
```
ksetup /mapuser mike@corp.CONTOSO.COM guest
```
Supprimez le mappage du compte de Mike Danseglio au compte invité sur cet ordinateur pour l’empêcher de s’authentifier auprès de cet ordinateur avec ses informations d’identification de CONTOSO :
```
ksetup /mapuser mike@corp.CONTOSO.COM 
```
Mappez le compte de Mike Danseglio dans le domaine Kerberos CONTOSO à un compte existant sur cet ordinateur. (si seuls les comptes utilisateur et invité standard sont actifs sur cet ordinateur, les privilèges de Mike sont définis sur ceux-ci) :
```
ksetup /mapuser mike@corp.CONTOSO.COM *
```
Mappez tous les comptes du domaine Kerberos CONTOSO à un compte existant portant le même nom sur cet ordinateur :
```
ksetup /mapuser * *
```

## <a name="additional-references"></a>Références supplémentaires

-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Ksetup](ksetup.md)