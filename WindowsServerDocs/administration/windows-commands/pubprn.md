---
title: pubprn
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0bc7f7e3-84e1-4359-b477-7b1a1a0bd639
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 844a13c1a650ebedcc0d5b4fbf65b9de671b2180
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372010"
---
# <a name="pubprn"></a>pubprn

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Publie une imprimante sur les services de domaine Active Directory.

## <a name="syntax"></a>Syntaxe
```
cscript pubprn {<ServerName> | <UNCprinterpath>} 
"LDAP://CN=<Container>,DC=<Container>"
```

## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|\<ServerName >|Spécifie le nom du serveur Windows Server qui héberge l’imprimante que vous souhaitez publier. Si vous ne spécifiez pas d’ordinateur, l’ordinateur local est utilisé.|
|\<UNCprinterpath >|Chemin d’accès UNC (Universal Naming Convention) à l’imprimante partagée que vous souhaitez publier.|
|« LDAP://CN =<Container>, DC =<Container>»|Spécifie le chemin d’accès au conteneur dans les services de domaine Active Directory où vous souhaitez publier l’imprimante.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes
-   La commande **Pubprn** est un script Visual Basic situé dans le répertoire%windir%\system32\ printing_Admin_Scripts\\<language>. Pour utiliser cette commande, à l’invite de commandes, tapez **cscript** suivi du chemin d’accès complet au fichier Pubprn ou accédez au dossier approprié. Par exemple :
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\pubprn
    ```
-   Si les informations que vous fournissez contiennent des espaces, utilisez des guillemets autour du texte (par exemple, `"computer Name"`).

## <a name="BKMK_examples"></a>Illustre
Pour publier toutes les imprimantes sur l’ordinateur \\\Server1 dans le conteneur MyContainer du domaine MyDomain.company.Com, tapez :
```
cscript Ppubprn Server1 "LDAP://CN=MyContainer,DC=MyDomain,DC=company,DC=Com"
```
Pour publier l’imprimante Laserprinter1 sur le serveur \\\Server1 sur le conteneur MyContainer dans le domaine MyDomain.company.Com, tapez :
```
cscript Ppubprn \\Server1\Laserprinter1 "LDAP://CN=MyContainer,DC=MyDomain,DC=company,DC=Com"
```

#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[référence de commande d’impression](print-command-reference.md)
