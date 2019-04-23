---
title: ksetup:addkpasswd
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d3196995-1b38-48ff-ba08-911cfab77317
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2a85eb6dfe30c33126504744a7659fe2cc573087
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856820"
---
# <a name="ksetupaddkpasswd"></a>ksetup:addkpasswd



Ajoute une adresse de serveur de mot de passe (Kpasswd) Kerberos pour un domaine. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
ksetup /addkpasswd <RealmName> [<KpasswdName>]
```

### <a name="parameters"></a>Paramètres

Si le domaine Kerberos que la station de travail authentifiera pour prendre en charge du protocole Kerberos modifier le protocole de mot de passe, vous pouvez configurer un ordinateur client exécutant le système d’exploitation Windows pour utiliser un serveur de mot de passe Kerberos. Ce paramètre est configuré sur le côté du domaine.

|Paramètre|Description|
|---------|-----------|
|\<RealmName>|Le nom de domaine est indiqué comme un nom DNS en majuscules, telles que CORP. CONTOSO.COM et est répertorié en tant que la valeur par défaut domaine ou du domaine Kerberos = quand **ksetup** est exécuté.|
|\<KpasswdName>|Le nom de contrôleur de domaine Kerberos qui doit être utilisé en tant que le serveur de mot de passe Kerberos est établi comme un nom de domaine complet de la casse, comme mitkdc.microsoft.com. Si le nom de contrôleur de domaine Kerberos est omis, DNS peut être utilisé pour localiser le KDC.|

## <a name="remarks"></a>Notes

Si le domaine Kerberos que la station de travail authentifiera pour prendre en charge du protocole Kerberos modifier le protocole de mot de passe, vous pouvez configurer un ordinateur client exécutant le système d’exploitation Windows pour utiliser un serveur de mot de passe Kerberos.

Exécutez la commande **ksetup** pour vérifier le nom de contrôleur de domaine Kerberos. Si **kpasswd =** n’apparaît pas dans la sortie, le mappage n’a pas été configuré.

Vous pouvez ajouter des noms KDC supplémentaires à la fois.

## <a name="BKMK_Examples"></a>Exemples

Configurer le domaine, CORP. CONTOSO.COM, de sorte qu’il utilise le serveur KDC Windows non-, mitkdc.contoso.com, que le serveur de mot de passe :
```
ksetup /addkpasswd CORP.CONTOSO.COM mitkdc.contoso.com
```
Ceci aboutit à un serveur de mot de passe non Windows Kerberos qui contrôle tous les mots de passe pour l’authentification entre elle et le domaine.

#### <a name="additional-references"></a>Références supplémentaires

-   [Ksetup](ksetup.md)
-   [Ksetup:delkpasswd](ksetup-delkpasswd.md)
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)