---
title: session de requête
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: abc0ace8-0b74-4b6e-a937-a78bb4b61a1f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 173b6e53bbd5cd42f3172582a46277dccff7dcbd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836942"
---
# <a name="query-session"></a>session de requête

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche des informations sur les sessions sur un serveur hôte de session Bureau à distance (hôte de session Bureau à distance).
La liste contient des informations non seulement sur les sessions actives, mais également sur les autres sessions exécutées par le serveur.
pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_examples).
> [!NOTE]
> Sous Windows Server 2008 R2, les services Terminal Server sont appelés Services Bureau à distance. Pour découvrir les nouveautés de la dernière version, consultez les nouveautés [de services Bureau à distance dans Windows server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.
> ## <a name="syntax"></a>Syntaxe
> ```
> query session [<SessionName> | <UserName> | <SessionID>] [/server:<ServerName>] [/mode] [/flow] [/connect] [/counter]
> ```
> ### <a name="parameters"></a>Paramètres
> 
> |      Paramètre       |                                                      Description                                                      |
> |----------------------|-----------------------------------------------------------------------------------------------------------------------|
> |    <SessionName>     |                               Spécifie le nom de la session que vous souhaitez interroger.                               |
> |      <UserName>      |                           Spécifie le nom de l’utilisateur dont vous souhaitez interroger les sessions.                            |
> |     <SessionID>      |                                Spécifie l’ID de la session que vous souhaitez interroger.                                |
> | /server:<ServerName> |                  Identifie le serveur hôte de session Bureau à distance à interroger. La valeur par défaut est le serveur actuel.                   |
> |        /mode         |                                            Affiche les paramètres de la ligne active.                                            |
> |        /flow         |                                        Affiche les paramètres actuels de contrôle de Workflow.                                        |
> |       /Connect       |                                          Affiche les paramètres de connexion actuels.                                           |
> |       /Counter       | Affiche des informations sur les compteurs actuels, notamment le nombre total de sessions créées, déconnectées et reconnectées. |
> |          /?          |                                         Affiche l'aide à l'invite de commandes.                                          |
> 
> ## <a name="remarks"></a>Notes
> - Un utilisateur peut toujours interroger la session sur laquelle l’utilisateur est actuellement connecté. Pour interroger d’autres sessions, l’utilisateur doit disposer de l’autorisation d’accès spécial informations sur les requêtes.
> - Si vous ne spécifiez pas de session à l’aide de <*nomsession*>, <*nom d’utilisateur*> ou <*SessionID*>, la **session de requête** affiche des informations sur toutes les sessions actives dans le système.
> - Lorsque la **session de requête** retourne des informations, un symbole supérieur à (>) s’affiche avant la session active. Voici un exemple de sortie pour la **session de requête**:
>   ```
>   C:\>query session
>    SESSIONNAME    USERNAME       ID STATE  TYPE   DEVICE
>   console        Administrator1  0 active wdcon
>    rdp-tcp#1      User1           1 active wdtshare
>    rdp-tcp                        2 listen wdtshare
>                                   4 idle
>                                   5 idle
>   ```
>   Le symbole supérieur à (>) indique la session active. NOM_SESSION spécifie le nom affecté à la session. USERNAME indique le nom d’utilisateur de l’utilisateur connecté à la session. L’État fournit des informations sur l’état actuel de la session. TYPE indique le type de session. L’appareil, qui n’est pas présent pour la console ou les sessions connectées au réseau, est le nom de l’appareil affecté à la session. Le commentaire qui suit les informations de session provient du profil de session. Toutes les sessions dans lesquelles l’état initial est désactivé ne s’affichent pas dans la liste de sessions de **requête** tant qu’elles ne sont pas activées.
>   ## <a name="examples"></a><a name=BKMK_examples></a>Illustre
> - Pour afficher des informations sur toutes les sessions actives sur le serveur SERver2, tapez :
>   ```
>   query session /server:SERver2
>   ```
> - Pour afficher des informations sur la session active modeM02, tapez :
>   ```
>   query session modeM02
>   ```
>   ## <a name="additional-references"></a>Références supplémentaires
>   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
>   
>   de [requête](query.md) [services Bureau à distance (services Terminal Server) de la commande](remote-desktop-services-terminal-services-command-reference.md)
