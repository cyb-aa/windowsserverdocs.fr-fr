---
title: pubprn
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 499ff2ade7ffc6c608791ba3da0ede0c3282c13d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831700"
---
# <a name="pubprn"></a>pubprn

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Publie une imprimante sur les Services de domaine active directory.

## <a name="syntax"></a>Syntaxe
```
cscript pubprn {<ServerName> | <UNCprinterpath>} 
"LDAP://CN=<Container>,DC=<Container>"
```

## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|\<ServerName>|Spécifie le nom du serveur Windows qui héberge l’imprimante que vous souhaitez publier. Si vous ne spécifiez pas un ordinateur, l’ordinateur local est utilisé.|
|\<UNCprinterpath >|Le chemin d’accès UNC Universal Naming Convention () à l’imprimante partagée que vous souhaitez publier.|
|"LDAP://CN=<Container>,DC=<Container>"|Spécifie le chemin d’accès au conteneur dans les Services de domaine active directory où vous souhaitez publier l’imprimante.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes
-   Le **pubprn** commande est un script Visual Basic situé dans le %WINdir%\System32\printing_Admin_Scripts\\ <language> directory. Pour utiliser cette commande, à une invite de commandes, tapez **cscript** suivie du chemin d’accès complet au fichier de pubprn, ou modifiez les répertoires vers le dossier approprié. Exemple :
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\pubprn
    ```
-   Si les informations que vous fournissez contient des espaces, utilisez des guillemets autour du texte (par exemple, `"computer Name"`).

## <a name="BKMK_examples"></a>Exemples
Pour publier toutes les imprimantes sur le \\ordinateur \Server1 au conteneur MyContainer dans le domaine MyDomain.company.Com, tapez :
```
cscript Ppubprn Server1 "LDAP://CN=MyContainer,DC=MyDomain,DC=company,DC=Com"
```
Pour publier l’imprimante Laserprinter1 sur le \\server \Server1 au conteneur MyContainer dans le domaine MyDomain.company.Com, tapez :
```
cscript Ppubprn \\Server1\Laserprinter1 "LDAP://CN=MyContainer,DC=MyDomain,DC=company,DC=Com"
```

#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[référence des commandes d’impression](print-command-reference.md)
