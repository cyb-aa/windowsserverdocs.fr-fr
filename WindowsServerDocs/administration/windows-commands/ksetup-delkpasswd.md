---
title: 'Ksetup : delkpasswd'
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: dce7d9666040ff0c234139932ea60e3589dfecb2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375135"
---
# <a name="ksetupdelkpasswd"></a>Ksetup : delkpasswd

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

supprime un serveur de mot de passe Kerberos (kpasswd) pour un domaine. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).
## <a name="syntax"></a>Syntaxe
```
ksetup /delkpasswd <RealmName> <KpasswdName>
```
### <a name="parameters"></a>Paramètres

|   Paramètre   |                                                                                                   Description                                                                                                   |
|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  <RealmName>  |                                Le nom de domaine est indiqué en tant que nom DNS en majuscules, par exemple CORP. CONTOSO.COM et est indiqué comme domaine par défaut ou domaine = lorsque **Ksetup** est exécuté.                                |
| <KpasswdName> | Le nom KDC à utiliser comme serveur de mot de passe Kerberos est indiqué comme un nom de domaine complet qui ne respecte pas la casse, par exemple mitkdc.contoso.com. Si le nom du KDC est omis, DNS peut être utilisé pour localiser les KDC. |

## <a name="remarks"></a>Notes
Exécutez la commande **Ksetup** pour vérifier le nom du KDC. Si **kpasswd =** n’apparaît pas dans la sortie, cela signifie que le mappage n’a pas été configuré. Les mappages multiples sont listés, s’ils sont définis.
## <a name="BKMK_Examples"></a>Illustre
Vérifiez le domaine CORP. CONTOSO.COM utilise le serveur KDC non-Windows mitkdc.contoso.com comme serveur de mot de passe :
```
ksetup /delkpasswd CORP.CONTOSO.COM mitkdc.contoso.com
```
Pour vérifier que la commande fonctionnait comme prévu, exécutez **Ksetup** sur l’ordinateur Windows pour vérifier le domaine Corp. CONTOSO.COM n’est pas mappé à un serveur de mot de passe Kerberos (nom KDC).
## <a name="additional-references"></a>Références supplémentaires
-   [ksetup](ksetup.md)
-   [Ksetup : delkpasswd](ksetup-delkpasswd.md)
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
