---
title: msg
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9501cf3e-568e-4982-9987-8daecc6c17ff
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b6c6193fc439140fa559643427067066e819c502
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879550"
---
# <a name="msg"></a>msg

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Envoie un message à un utilisateur sur un serveur hôte de Session Bureau à distance (hôte de Session Bureau à distance).
Pour obtenir des exemples montrant comment utiliser cette commande, consultez [exemples](#BKMK_examples).
> [!NOTE]
> Dans Windows Server 2008 R2, les services Terminal Server ont été renommés services Bureau à distance. Pour savoir quelles sont les nouveautés dans la version la plus récente, consultez [les nouveautés des Services Bureau à distance dans Windows Server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.

## <a name="syntax"></a>Syntaxe
```
msg {<UserName> | <SessionName> | <SessionID>| @<FileName> | *} [/server:<ServerName>] [/time:<Seconds>] [/v] [/w] [<Message>]
```

## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|<UserName>|Spécifie le nom de l’utilisateur que vous souhaitez recevoir le message.|
|<SessionName>|Spécifie le nom de la session que vous souhaitez recevoir le message.|
|<SessionID>|Spécifie l’ID numérique de la session dont vous souhaitez recevoir un message de l’utilisateur.|
|@<FileName>|Identifie un fichier contenant une liste de noms d’utilisateur, les noms de session et les ID de session que vous souhaitez recevoir le message.|
|*|Envoie le message à tous les noms d’utilisateur sur le système.|
|/server:<ServerName>|Spécifie le serveur hôte de Session Bureau à distance dont la session ou utilisateur que vous souhaitez recevoir le message. Si non spécifié, **/server** utilise le serveur auquel vous êtes actuellement connecté.|
|/Time :<Seconds>|Spécifie la durée pendant laquelle le message envoyé est affiché sur l’écran de l’utilisateur. Une fois la limite de temps est atteinte, le message disparaît. Si aucune limite de temps n’est définie, le message reste sur l’écran de l’utilisateur jusqu'à ce que l’utilisateur voit le message et clique sur **OK**.|
|/v|Affiche des informations sur les actions en cours d’exécution.|
|/w|Attend un accusé de réception à partir de l’utilisateur que le message a été reçu. Utilisez ce paramètre avec **/heure :**<*secondes*> afin d’éviter un long délai possible si l’utilisateur ne répond pas immédiatement. À l’aide de ce paramètre avec **/v** s’avère également utile.|
|<Message>|Spécifie le texte du message que vous souhaitez envoyer. Si aucun message n’est spécifié, vous devrez entrer un message. Pour envoyer un message qui est contenu dans un fichier, tapez le symbole supérieur à (<) suivie du nom de fichier.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes
-   Si vous ne spécifiez pas un utilisateur ou une session, **msg** affiche un message d’erreur. Lorsque vous spécifiez une session, il doit être active.
-   L’utilisateur doit avoir l’autorisation d’accès spéciale de Message pour envoyer un message.

## <a name="BKMK_examples"></a>Exemples
-   Pour envoyer que le message intitulé « à 13 h 00 aujourd'hui » à toutes les sessions pour User1, tapez :
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
-   Pour envoyer le message à tous les utilisateurs qui sont connectés, tapez :
    ```
    msg * Let's meet at 1PM today
    ```
-   Pour envoyer le message à tous les utilisateurs, avec un délai d’attente d’accusé de réception (par exemple, 10 secondes), tapez :
    ```
    msg * /time:10 Let's meet at 1PM today
    ```
    
#### <a name="additional-references"></a>Références supplémentaires
-  [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
-  [Services Bureau à distance &#40;Services Terminal Server&#41; référence de la commande](remote-desktop-services-terminal-services-command-reference.md)
