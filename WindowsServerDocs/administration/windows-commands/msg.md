---
title: msg
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9501cf3e-568e-4982-9987-8daecc6c17ff
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e44d754557f918c218e9e9d35149bffd983a0d54
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723863"
---
# <a name="msg"></a>msg

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Envoie un message à un utilisateur sur un serveur hôte de session Bureau à distance (hôte de session Bureau à distance).

> [!NOTE]
> Dans Windows Server 2008 R2, les services Terminal Server ont été renommés services Bureau à distance. Pour découvrir les nouveautés de la dernière version, consultez les nouveautés [de services Bureau à distance dans Windows server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.

## <a name="syntax"></a>Syntaxe
```
msg {<UserName> | <SessionName> | <SessionID>| @<FileName> | *} [/server:<ServerName>] [/time:<Seconds>] [/v] [/w] [<Message>]
```

### <a name="parameters"></a>Paramètres

|      Paramètre       |                                                                                                                               Description                                                                                                                               |
|----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      <UserName>      |                                                                                                  Spécifie le nom de l’utilisateur pour lequel vous souhaitez recevoir le message.                                                                                                   |
|    <SessionName>     |                                                                                                 Spécifie le nom de la session à laquelle vous souhaitez recevoir le message.                                                                                                 |
|     <SessionID>      |                                                                                            Spécifie l’ID numérique de la session dont l’utilisateur doit recevoir un message.                                                                                            |
|     @<FileName>      |                                                                         Identifie un fichier contenant une liste de noms d’utilisateurs, de noms de sessions et d’ID de session pour lesquels vous souhaitez recevoir le message.                                                                         |
|          \*          |                                                                                                           Envoie le message à tous les noms d’utilisateur sur le système.                                                                                                            |
| /server:<ServerName> |                                              Spécifie le serveur hôte de session Bureau à distance dont vous souhaitez recevoir le message. S’il n’est pas spécifié, **/Server** utilise le serveur sur lequel vous êtes actuellement connecté.                                              |
|   /Time<Seconds>    | Spécifie la durée pendant laquelle le message que vous avez envoyé s’affiche sur l’écran de l’utilisateur. Une fois que la limite de temps est atteinte, le message disparaît. Si aucune limite de temps n’est définie, le message reste sur l’écran de l’utilisateur jusqu’à ce que l’utilisateur puisse voir le message et clique sur **OK**. |
|          /v          |                                                                                                         Affiche des informations sur les actions en cours d’exécution.                                                                                                         |
|          /w          |         Attend un accusé de réception de la part de l’utilisateur que le message a été reçu. Utilisez ce paramètre avec **/Time :**<*seconds*> pour éviter un éventuel délai si l’utilisateur ne répond pas immédiatement. L’utilisation de ce paramètre avec **/v** est également utile.          |
|      <Message>       |                  Spécifie le texte du message que vous souhaitez envoyer. Si aucun message n’est spécifié, vous êtes invité à entrer un message. Pour envoyer un message contenu dans un fichier, tapez le symbole « inférieur à » (<) suivi du nom de fichier.                  |
|          /?          |                                                                                                                  Affiche l'aide à l'invite de commandes.                                                                                                                   |

## <a name="remarks"></a>Notes 
-   Si vous ne spécifiez pas d’utilisateur ou de session, **MSG** affiche un message d’erreur. Quand vous spécifiez une session, celle-ci doit être active.
-   L’utilisateur doit disposer de l’autorisation d’accès spécial message pour envoyer un message.

## <a name="examples"></a>Exemples
-   Pour envoyer le message avec le droit de l’utilisateur à 13H00 aujourd’hui à toutes les sessions pour User1, tapez :
    ```
    msg User1 Let's meet at 1PM today
    ```
-   Pour envoyer le même message à la session modeM02, tapez :
    ```
    msg modem02 Let's meet at 1PM today
    ```
-   Pour envoyer le message à la session 12, tapez :
    ```
    msg 12 Let's meet at 1PM today
    ```
-   Pour envoyer le message à toutes les sessions contenues dans le fichier USERlist, tapez :
    ```
    msg @userlist Let's meet at 1PM today
    ```
-   Pour envoyer le message à tous les utilisateurs qui ont ouvert une session, tapez :
    ```
    msg * Let's meet at 1PM today
    ```
-   Pour envoyer le message à tous les utilisateurs, avec un délai d’attente d’accusé de réception (par exemple, 10 secondes), tapez :
    ```
    msg * /time:10 Let's meet at 1PM today
    ```

## <a name="additional-references"></a>Références supplémentaires
-  - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-  [Référence des commandes des services Bureau à distance (services Terminal Server)](remote-desktop-services-terminal-services-command-reference.md)
