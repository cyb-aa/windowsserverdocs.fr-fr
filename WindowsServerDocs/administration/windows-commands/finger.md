---
title: finger
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 907ea637-5c6c-4752-84c2-46bbf2a68a33
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 526363db3ecff4a9138c9cf13cbf330196e14ced
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439252"
---
# <a name="finger"></a>finger

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche des informations sur un ou plusieurs utilisateurs sur un ordinateur distant spécifié (généralement un ordinateur exécutant UNIX) qui exécute le service finger ou démon. L’ordinateur distant spécifie le format et la sortie de l’affichage des informations utilisateur. Utilisée sans paramètres, **doigt** affiche l’aide. 
## <a name="syntax"></a>Syntaxe
```
finger [-l] [<User>] [@<Host>] [...]
```
### <a name="parameters"></a>Paramètres

| Paramètre |                                                                            Description                                                                            |
|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    -l     |                                                          Affiche les informations utilisateur sous forme de liste longue.                                                           |
|  <User>   | Spécifie l’utilisateur sur lequel vous souhaitez des informations. Si vous omettez le *utilisateur* paramètre, **doigt** affiche des informations sur tous les utilisateurs sur l’ordinateur spécifié. |
|  @<Host>  |        Spécifie l’ordinateur distant exécutant le service finger où vous recherchez des informations de l’utilisateur. Vous pouvez spécifier une adresse IP ou le nom de l’ordinateur.        |
|    /?     |                                                               Affiche l'aide à l'invite de commandes.                                                                |

## <a name="remarks"></a>Notes
Plusieurs User@Host paramètres peuvent être spécifiés.
Vous devez faire précéder **doigt** paramètres avec un trait d’union (-) plutôt qu’une barre oblique (/).
Cette commande est disponible uniquement si le protocole TCP/IP (Internet Protocol) est installé en tant que composant dans les propriétés d’une carte réseau dans Connexions réseau.
Windows Server 2003 ne fournit pas un service de doigt.
## <a name="BKMK_Examples"></a>Exemples
Pour afficher des informations pour user1 sur l’ordinateur utilisateurs.Microsoft.com, tapez :
```
finger user1@users.microsoft.com
```
Pour afficher des informations pour tous les utilisateurs sur l’ordinateur utilisateurs.Microsoft.com, tapez :
```
finger @users.microsoft.com
```
## <a name="additional-references"></a>Références supplémentaires
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
