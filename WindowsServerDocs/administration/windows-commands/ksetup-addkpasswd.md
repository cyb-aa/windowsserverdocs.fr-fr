---
title: 'Ksetup : addkpasswd'
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d3196995-1b38-48ff-ba08-911cfab77317
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c260c711ae87f88be8b9466e73afaf3fe1c83a1e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724737"
---
# <a name="ksetupaddkpasswd"></a>Ksetup : addkpasswd



Ajoute une adresse de serveur de mot de passe Kerberos (kpasswd) pour un domaine.

## <a name="syntax"></a>Syntaxe

```
ksetup /addkpasswd <RealmName> [<KpasswdName>]
```

#### <a name="parameters"></a>Paramètres

Si le domaine Kerberos sur lequel la station de travail doit s’authentifier prend en charge le protocole de modification Kerberos, vous pouvez configurer un ordinateur client exécutant le système d’exploitation Windows pour qu’il utilise un serveur de mot de passe Kerberos. Ce paramètre est configuré côté domaine.

|Paramètre|Description|
|---------|-----------|
|\<RealmName>|Le nom de domaine est indiqué en tant que nom DNS en majuscules, par exemple CORP. CONTOSO.COM et est indiqué comme domaine par défaut ou domaine = lorsque **Ksetup** est exécuté.|
|\<KpasswdName>|Le nom du KDC qui doit être utilisé en tant que serveur de mot de passe Kerberos est indiqué comme un nom de domaine complet qui ne respecte pas la casse, par exemple mitkdc.microsoft.com. Si le nom du KDC est omis, DNS peut être utilisé pour localiser les KDC.|

## <a name="remarks"></a>Notes 

Si le domaine Kerberos sur lequel la station de travail doit s’authentifier prend en charge le protocole de modification Kerberos, vous pouvez configurer un ordinateur client exécutant le système d’exploitation Windows pour qu’il utilise un serveur de mot de passe Kerberos.

Exécutez la commande **Ksetup** pour vérifier le nom du KDC. Si **kpasswd =** n’apparaît pas dans la sortie, le mappage n’a pas été configuré.

Vous pouvez ajouter des noms KDC supplémentaires un par un.

## <a name="examples"></a>Exemples

Configurez le domaine, CORP. CONTOSO.COM, afin qu’il utilise le serveur KDC non-Windows, mitkdc.contoso.com, comme serveur de mot de passe :
```
ksetup /addkpasswd CORP.CONTOSO.COM mitkdc.contoso.com
```
Cela aboutit à un serveur de mot de passe Kerberos non-Windows qui contrôle tous les mots de passe pour l’authentification entre l’informatique et le domaine.

## <a name="additional-references"></a>Références supplémentaires

-   [Ksetup](ksetup.md)
-   [Ksetup:delkpasswd](ksetup-delkpasswd.md)
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)