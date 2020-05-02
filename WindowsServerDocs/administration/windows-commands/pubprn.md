---
title: pubprn
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0bc7f7e3-84e1-4359-b477-7b1a1a0bd639
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 17ca9e98ef9e3423521b03c5c21be4b3f1538b62
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722777"
---
# <a name="pubprn"></a>pubprn

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Publie une imprimante sur les services de domaine Active Directory.

## <a name="syntax"></a>Syntaxe
```
cscript pubprn {<ServerName> | <UNCprinterpath>} 
LDAP://CN=<Container>,DC=<Container>
```

### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|\<Nom du serveur>|Spécifie le nom du serveur Windows Server qui héberge l’imprimante que vous souhaitez publier. Si vous ne spécifiez pas d’ordinateur, l’ordinateur local est utilisé.|
|\<UNCprinterpath>|Chemin d’accès UNC (Universal Naming Convention) à l’imprimante partagée que vous souhaitez publier.|
|LDAP://CN =<Container>, DC =<Container>|Spécifie le chemin d’accès au conteneur dans les services de domaine Active Directory où vous souhaitez publier l’imprimante.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes 
-   La commande **Pubprn** est un script Visual Basic situé dans le répertoire%windir%\system32\\\ <language> printing_Admin_Scripts. Pour utiliser cette commande, à l’invite de commandes, tapez **cscript** suivi du chemin d’accès complet au fichier Pubprn ou accédez au dossier approprié. Par exemple :
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\pubprn
    ```
-   Si les informations que vous fournissez contiennent des espaces, utilisez des guillemets autour du texte ( `computer Name`par exemple,).

## <a name="examples"></a>Exemples
Pour publier toutes les imprimantes sur \\l’ordinateur \Server1 dans le conteneur mycontainer du domaine mydomain.Company.com, tapez :
```
cscript Ppubprn Server1 LDAP://CN=MyContainer,DC=MyDomain,DC=company,DC=Com
```
Pour publier l’imprimante Laserprinter1 sur le \\serveur \Server1 sur le conteneur mycontainer dans le domaine mydomain.Company.com, tapez :
```
cscript Ppubprn \\Server1\Laserprinter1 LDAP://CN=MyContainer,DC=MyDomain,DC=company,DC=Com
```

## <a name="additional-references"></a>Références supplémentaires
- [Référence des](command-line-syntax-key.md)
[commandes d’impression](print-command-reference.md) de la clé de syntaxe de ligne de commande
