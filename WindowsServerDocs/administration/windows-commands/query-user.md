---
title: interroger l’utilisateur
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a670fb78-c055-464a-b61d-3a85632c52c5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e8c095226a5445e976e47e461044ec002dc007fe
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722692"
---
# <a name="query-user"></a>interroger l’utilisateur

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche des informations sur les sessions utilisateur sur un serveur hôte de session Bureau à distance (hôte de session Bureau à distance).

> [!NOTE]
> Dans Windows Server 2008 R2, les services Terminal Server ont été renommés services Bureau à distance. Pour découvrir les nouveautés de la dernière version, consultez les nouveautés [de services Bureau à distance dans Windows server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.
> ## <a name="syntax"></a>Syntaxe
> ```
> query user [<UserName> | <SessionName> | <SessionID>] [/server:<ServerName>]
> ```
> ### <a name="parameters"></a>Paramètres
> 
> |      Paramètre       |                                                     Description                                                     |
> |----------------------|---------------------------------------------------------------------------------------------------------------------|
> |      <UserName>      |                            Spécifie le nom d’ouverture de session de l’utilisateur que vous souhaitez interroger.                             |
> |    <SessionName>     |                              Spécifie le nom de la session que vous souhaitez interroger.                              |
> |     <SessionID>      |                               Spécifie l’ID de la session que vous souhaitez interroger.                               |
> | /server:<ServerName> | Spécifie le serveur hôte de session Bureau à distance que vous souhaitez interroger. Dans le cas contraire, le serveur hôte de session Bureau à distance actuel est utilisé. |
> |          /?          |                                        Affiche l'aide à l'invite de commandes.                                         |
> 
> ## <a name="remarks"></a>Notes 
> - Vous pouvez utiliser cette commande pour déterminer si un utilisateur spécifique est connecté à un serveur hôte de session Bureau à distance spécifique. l’utilisateur de la **requête** renvoie les informations suivantes :
>   -   Nom de l'utilisateur.
>   -   Nom de la session sur le serveur hôte de session Bureau à distance
>   -   ID de session
>   -   État de la session (active ou déconnectée)
>   -   Durée d’inactivité (nombre de minutes depuis la dernière frappe ou déplacement de la souris au cours de la session)
>   -   Date et heure auxquelles l’utilisateur a ouvert une session
> - Pour utiliser l’utilisateur de la **requête**, vous devez disposer de l’autorisation contrôle total ou informations spéciales sur l’accès aux requêtes.
> - Si vous utilisez **query User** sans spécifier <*UserName*>, <*nomsession*> ou <*SessionID*>, une liste de tous les utilisateurs qui sont connectés au serveur est retournée. Vous pouvez également utiliser la session de **requête** pour afficher une liste de toutes les sessions sur un serveur.
> - Quand l’utilisateur de la **requête** retourne des informations, un symbole supérieur à (>) s’affiche avant la session active.
> - Le paramètre **/Server** est requis uniquement si vous utilisez **interroger l’utilisateur** à partir d’un serveur distant.
>   ## <a name="examples"></a>Exemples
> - Pour afficher des informations sur tous les utilisateurs connectés au système, tapez :
>   ```
>   query user
>   ```
> - Pour afficher des informations sur l’utilisateur USER1 sur le serveur SERver1, tapez :
>   ```
>   query user USER1 /server:SERver1
>   ```
>   ## <a name="additional-references"></a>Références supplémentaires
>   - [Command-Line Syntax Key](command-line-syntax-key.md)
>   Services Bureau à distance de[requête](query.md)
>   de clé de syntaxe de ligne de commande[(services Terminal Server) référence de commande](remote-desktop-services-terminal-services-command-reference.md)
