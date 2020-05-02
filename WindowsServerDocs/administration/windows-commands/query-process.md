---
title: query process
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 36ce3ffc-0092-4eb1-a374-28e6616ca946
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 81132ebf6b75115086ed7cc2ab9f73d9d06e65e4
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722719"
---
# <a name="query-process"></a>query process

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche des informations sur les processus qui s’exécutent sur un serveur hôte de session Bureau à distance (hôte de session Bureau à distance).
Vous pouvez utiliser cette commande pour déterminer quels sont les programmes exécutés par un utilisateur spécifique, ainsi que les utilisateurs qui exécutent un programme spécifique.

> [!NOTE]
> Dans Windows Server 2008 R2, les services Terminal Server ont été renommés services Bureau à distance. Pour découvrir les nouveautés de la dernière version, consultez les nouveautés [de services Bureau à distance dans Windows server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.
> ## <a name="syntax"></a>Syntaxe
> ```
> query process [* | <ProcessID> | <UserName> | <SessionName> | /id:<nn> | <ProgramName>] [/server:<ServerName>]
> ```
> ### <a name="parameters"></a>Paramètres
> 
> |      Paramètre       |                                                                 Description                                                                  |
> |----------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
> |          \*          |                                                    répertorie les processus pour toutes les sessions.                                                     |
> |     <ProcessID>      |                                   Spécifie l’ID numérique identifiant le processus que vous souhaitez interroger.                                   |
> |      <UserName>      |                                       Spécifie le nom de l’utilisateur dont vous souhaitez répertorier les processus.                                       |
> |    <SessionName>     |                                     Spécifie le nom de la session dont vous souhaitez répertorier les processus.                                      |
> |       bootid<nn>       |                                      Spécifie l’ID de la session dont vous souhaitez répertorier les processus.                                       |
> |    <ProgramName>     |                     Spécifie le nom du programme dont vous souhaitez interroger les processus. L’extension. exe est requise.                     |
> | /server:<ServerName> | Spécifie le serveur hôte de session Bureau à distance dont vous souhaitez répertorier les processus. S’il n’est pas spécifié, le serveur sur lequel vous êtes actuellement connecté est utilisé. |
> |          /?          |                                                     Affiche l'aide à l'invite de commandes.                                                     |
> 
> ## <a name="remarks"></a>Notes 
> - Les administrateurs ont un accès complet à toutes les fonctions de **processus de requête** .
> - Si vous ne spécifiez pas le *nom d’utilisateur* <>, <paramètre *NomSession*>, **/ID :**<*nn*>, <**\\** *nom_programme*> ou *, le processus de **requête** affiche uniquement les processus qui appartiennent à l’utilisateur actuel.
> - Si une session est spécifiée, elle doit identifier une session active.
> - le **processus de requête** retourne les informations suivantes :
>   -   Utilisateur propriétaire du processus
>   -   Session propriétaire du processus
>   -   ID de la session
>   -   Nom du processus
>   -   ID du processus
> - Lorsque le **processus de requête** retourne des informations, un symbole supérieur à (>) est affiché avant chaque processus qui appartient à la session active.
>   ## <a name="examples"></a>Exemples
> - Pour afficher des informations sur les processus utilisés par toutes les sessions, tapez :
>   ```
>   query process *
>   ```
> - Pour afficher des informations sur les processus utilisés par l’ID de session 2, tapez :
>   ```
>   query process /ID:2
>   ```
>   ## <a name="additional-references"></a>Références supplémentaires
>   - [Command-Line Syntax Key](command-line-syntax-key.md)
>   Services Bureau à distance de[requête](query.md)
>   de clé de syntaxe de ligne de commande[(services Terminal Server) référence de commande](remote-desktop-services-terminal-services-command-reference.md)
