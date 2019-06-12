---
title: session de requête
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: abc0ace8-0b74-4b6e-a937-a78bb4b61a1f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 25e2457d792b463ca861f0cba29f1c290684e7b0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442044"
---
# <a name="query-session"></a>session de requête

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche des informations sur les sessions sur un serveur hôte de Session Bureau à distance (hôte de Session Bureau à distance).
La liste inclut des informations non seulement sur les sessions actives, mais aussi sur d’autres sessions exécutée par le serveur.
Pour obtenir des exemples montrant comment utiliser cette commande, consultez [exemples](#BKMK_examples).
> [!NOTE]
> Dans Windows Server 2008 R2, les services Terminal Server ont été renommés services Bureau à distance. Pour savoir quelles sont les nouveautés dans la version la plus récente, consultez [les nouveautés des Services Bureau à distance dans Windows Server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.
> ## <a name="syntax"></a>Syntaxe
> ```
> query session [<SessionName> | <UserName> | <SessionID>] [/server:<ServerName>] [/mode] [/flow] [/connect] [/counter]
> ```
> ## <a name="parameters"></a>Paramètres
> 
> |      Paramètre       |                                                      Description                                                      |
> |----------------------|-----------------------------------------------------------------------------------------------------------------------|
> |    <SessionName>     |                               Spécifie le nom de la session que vous souhaitez interroger.                               |
> |      <UserName>      |                           Spécifie le nom de l’utilisateur dont vous souhaitez interroger les sessions.                            |
> |     <SessionID>      |                                Spécifie l’ID de la session que vous souhaitez interroger.                                |
> | /server:<ServerName> |                  Identifie le serveur hôte de Session Bureau à distance à la requête. La valeur par défaut est le serveur actuel.                   |
> |        / mode         |                                            Affiche les paramètres de ligne actuelle.                                            |
> |        /flow         |                                        Affiche les paramètres de contrôle de flux en cours.                                        |
> |       /connect       |                                          Paramètres de connexion affiche actuel.                                           |
> |       / counter       | Affiche les compteurs plus d’informations, notamment le nombre total de sessions créées, déconnectées et reconnectées. |
> |          /?          |                                         Affiche l'aide à l'invite de commandes.                                          |
> 
> ## <a name="remarks"></a>Notes
> - Un utilisateur peut toujours interroger la session à laquelle l’utilisateur est actuellement connecté. Pour interroger les autres sessions, l’utilisateur doit disposer d’autorisation d’accès spéciale de requête d’informations.
> - Si vous ne spécifiez pas d’une session à l’aide de <*SessionName*>, <*nom d’utilisateur*>, ou <*SessionID*>, **interroger session** Affiche des informations sur toutes les sessions actives dans le système.
> - Lorsque **interroger session** retourne des informations, un symbole supérieur à (>) s’affiche avant la session active. Voici le résultat de l’exemple pour **interroger session**:
>   ```
>   C:\>query session
>    SESSIONNAME    USERNAME       ID STATE  TYPE   DEVICE
>   console        Administrator1  0 active wdcon
>    rdp-tcp#1      User1           1 active wdtshare
>    rdp-tcp                        2 listen wdtshare
>                                   4 idle
>                                   5 idle
>   ```
>   Le symbole supérieur à (>) indique la session active. Nom de session indique le nom affecté à la session. Nom d’utilisateur indique le nom d’utilisateur de l’utilisateur connecté à la session. ÉTAT fournit des informations sur l’état actuel de la session. TYPE indique le type de session. APPAREIL, ce qui n’est pas présent pour la console ou les sessions connectées au réseau, est le nom du périphérique affecté à la session. Le commentaire qui suit les informations de session est à partir du profil de la session. Toutes les sessions dans laquelle l’état initial est configuré comme étant désactivé ne s’affichent pas dans le **interroger session** jusqu'à ce qu’elles sont activées.
>   ## <a name="BKMK_examples"></a>Exemples
> - Pour afficher des informations sur toutes les sessions actives sur le serveur Serveur2, tapez :
>   ```
>   query session /server:SERver2
>   ```
> - Pour afficher des informations sur la session active modeM02, tapez :
>   ```
>   query session modeM02
>   ```
>   #### <a name="additional-references"></a>Références supplémentaires
>   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
>   [requête](query.md)
>   [Services Bureau à distance &#40;Services Terminal Server&#41; référence des commandes](remote-desktop-services-terminal-services-command-reference.md)
