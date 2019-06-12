---
title: ksetup:delkpasswd
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2db0bfcd-bc08-48e3-9f30-65b6411839c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a1c701707f736fe51a1f4af70a2571e63025f281
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438045"
---
# <a name="ksetupdelkpasswd"></a>ksetup:delkpasswd

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Supprime un serveur de mot de passe Kerberos (Kpasswd) pour un domaine. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).
## <a name="syntax"></a>Syntaxe
```
ksetup /delkpasswd <RealmName> <KpasswdName>
```
### <a name="parameters"></a>Paramètres

|   Paramètre   |                                                                                                   Description                                                                                                   |
|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  <RealmName>  |                                Le nom de domaine est indiqué comme un nom DNS en majuscules, telles que CORP. CONTOSO.COM et est répertorié en tant que la valeur par défaut domaine ou du domaine Kerberos = quand **ksetup** est exécuté.                                |
| <KpasswdName> | Le nom de contrôleur de domaine Kerberos à utiliser en tant que le serveur de mot de passe Kerberos est établi comme un nom de domaine complet, non-respect de la casse, comme mitkdc.contoso.com. Si le nom de contrôleur de domaine Kerberos est omis, DNS peut être utilisé pour localiser le KDC. |

## <a name="remarks"></a>Notes
Exécutez la commande **ksetup** pour vérifier le nom de contrôleur de domaine Kerberos. Si **kpasswd =** n’apparaît pas dans la sortie, puis le mappage n’a pas été configuré. Plusieurs mappages apparaît, si définie.
## <a name="BKMK_Examples"></a>Exemples
vérifier le domaine CORP. CONTOSO.COM utilise le mitkdc.contoso.com serveur KDC Windows non - que le serveur de mot de passe :
```
ksetup /delkpasswd CORP.CONTOSO.COM mitkdc.contoso.com
```
Pour vérifier que la commande a fonctionné comme prévu, exécutez **ksetup** sur l’ordinateur Windows pour vérifier le domaine CORP. CONTOSO.COM n’est pas mappé à un serveur de mot de passe Kerberos (le nom de contrôleur de domaine Kerberos).
## <a name="additional-references"></a>Références supplémentaires
-   [ksetup](ksetup.md)
-   [ksetup:delkpasswd](ksetup-delkpasswd.md)
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
