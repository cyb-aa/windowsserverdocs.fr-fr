---
title: finger
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 907ea637-5c6c-4752-84c2-46bbf2a68a33
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ec8040480a7cb75a5a42e051393e3db4a47f8e2f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725614"
---
# <a name="finger"></a>finger

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche des informations sur un utilisateur ou des utilisateurs sur un ordinateur distant spécifié (généralement un ordinateur exécutant UNIX) qui exécute le service ou démon Finger. L’ordinateur distant spécifie le format et la sortie de l’affichage des informations de l’utilisateur. Utilisé sans paramètres, **Finger** affiche l’aide. 
## <a name="syntax"></a>Syntaxe
```
finger [-l] [<User>] [@<Host>] [...]
```
#### <a name="parameters"></a>Paramètres

| Paramètre |                                                                            Description                                                                            |
|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    -l     |                                                          Affiche les informations utilisateur sous la forme d’une longue liste.                                                           |
|  <User>   | Spécifie l’utilisateur pour lequel vous souhaitez obtenir des informations. Si vous omettez le paramètre *User* , **Finger** affiche des informations sur tous les utilisateurs sur l’ordinateur spécifié. |
|  @<Host>  |        Spécifie l’ordinateur distant qui exécute le service finger dans lequel vous recherchez des informations sur l’utilisateur. Vous pouvez spécifier un nom d’ordinateur ou une adresse IP.        |
|    /?     |                                                               Affiche l'aide à l'invite de commandes.                                                                |

## <a name="remarks"></a>Notes 
Plusieurs User@Host paramètres peuvent être spécifiés.
Vous devez faire précéder les paramètres **Finger** d’un trait d’Union (-) plutôt que d’une barre oblique (/).
Cette commande est disponible uniquement si le protocole TCP/IP (Internet Protocol) est installé en tant que composant dans les propriétés d’une carte réseau dans connexions réseau.
Windows Server 2003 ne fournit pas de service finger.
## <a name="examples"></a>Exemples
Pour afficher des informations pour user1 sur l’ordinateur users.microsoft.com, tapez :
```
finger user1@users.microsoft.com
```
Pour afficher les informations de tous les utilisateurs sur l’ordinateur users.microsoft.com, tapez :
```
finger @users.microsoft.com
```
## <a name="additional-references"></a>Références supplémentaires
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
