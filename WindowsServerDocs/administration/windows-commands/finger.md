---
title: finger
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 8e16120eb19ff2f194fe2c8bdeb3af80ca459ebe
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377161"
---
# <a name="finger"></a>finger

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Affiche des informations sur un utilisateur ou des utilisateurs sur un ordinateur distant spécifié (généralement un ordinateur exécutant UNIX) qui exécute le service ou démon Finger. L’ordinateur distant spécifie le format et la sortie de l’affichage des informations de l’utilisateur. Utilisé sans paramètres, **Finger** affiche l’aide. 
## <a name="syntax"></a>Syntaxe
```
finger [-l] [<User>] [@<Host>] [...]
```
### <a name="parameters"></a>Paramètres

| Paramètre |                                                                            Description                                                                            |
|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    -l     |                                                          Affiche les informations utilisateur sous la forme d’une longue liste.                                                           |
|  <User>   | Spécifie l’utilisateur pour lequel vous souhaitez obtenir des informations. Si vous omettez le paramètre *User* , **Finger** affiche des informations sur tous les utilisateurs sur l’ordinateur spécifié. |
|  @<Host>  |        Spécifie l’ordinateur distant qui exécute le service finger dans lequel vous recherchez des informations sur l’utilisateur. Vous pouvez spécifier un nom d’ordinateur ou une adresse IP.        |
|    /?     |                                                               Affiche l'aide à l'invite de commandes.                                                                |

## <a name="remarks"></a>Notes
Plusieurs paramètres User@Host peuvent être spécifiés.
Vous devez faire précéder les paramètres **Finger** d’un trait d’Union (-) plutôt que d’une barre oblique (/).
Cette commande est disponible uniquement si le protocole TCP/IP (Internet Protocol) est installé en tant que composant dans les propriétés d’une carte réseau dans connexions réseau.
Windows Server 2003 ne fournit pas de service finger.
## <a name="BKMK_Examples"></a>Illustre
Pour afficher des informations pour user1 sur l’ordinateur users.microsoft.com, tapez :
```
finger user1@users.microsoft.com
```
Pour afficher les informations de tous les utilisateurs sur l’ordinateur users.microsoft.com, tapez :
```
finger @users.microsoft.com
```
## <a name="additional-references"></a>Références supplémentaires
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
