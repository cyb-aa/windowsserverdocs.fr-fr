---
title: processus de requête
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 36ce3ffc-0092-4eb1-a374-28e6616ca946
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5ee42286691444c3a667801be3174514a81441c6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836962"
---
# <a name="query-process"></a>processus de requête

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche des informations sur les processus qui s’exécutent sur un serveur hôte de session Bureau à distance (hôte de session Bureau à distance).
Vous pouvez utiliser cette commande pour déterminer quels sont les programmes exécutés par un utilisateur spécifique, ainsi que les utilisateurs qui exécutent un programme spécifique.
pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_examples).
> [!NOTE]
> Sous Windows Server 2008 R2, les services Terminal Server sont appelés Services Bureau à distance. Pour découvrir les nouveautés de la dernière version, consultez les nouveautés [de services Bureau à distance dans Windows server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.
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
> |       /ID :<nn>       |                                      Spécifie l’ID de la session dont vous souhaitez répertorier les processus.                                       |
> |    <ProgramName>     |                     Spécifie le nom du programme dont vous souhaitez interroger les processus. L’extension. exe est requise.                     |
> | /server:<ServerName> | Spécifie le serveur hôte de session Bureau à distance dont vous souhaitez répertorier les processus. S’il n’est pas spécifié, le serveur sur lequel vous êtes actuellement connecté est utilisé. |
> |          /?          |                                                     Affiche l'aide à l'invite de commandes.                                                     |
> 
> ## <a name="remarks"></a>Notes
> - Les administrateurs ont un accès complet à toutes les fonctions de **processus de requête** .
> - Si vous ne spécifiez pas le*nom d’utilisateur*< >, < nom de*session*>, **/ID :** <*nn*>, <*nom_programme*> ou **\\** *, le **processus de requête** affiche uniquement les processus qui appartiennent à l’utilisateur actuel.
> - Si une session est spécifiée, elle doit identifier une session active.
> - le **processus de requête** retourne les informations suivantes :
>   -   Utilisateur propriétaire du processus
>   -   Session propriétaire du processus
>   -   ID de la session
>   -   Nom du processus
>   -   ID du processus
> - Lorsque le **processus de requête** retourne des informations, un symbole supérieur à (>) est affiché avant chaque processus qui appartient à la session active.
>   ## <a name="examples"></a><a name=BKMK_examples></a>Illustre
> - Pour afficher des informations sur les processus utilisés par toutes les sessions, tapez :
>   ```
>   query process *
>   ```
> - Pour afficher des informations sur les processus utilisés par l’ID de session 2, tapez :
>   ```
>   query process /ID:2
>   ```
>   ## <a name="additional-references"></a>Références supplémentaires
>   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
>   
>   de [requête](query.md) [services Bureau à distance (services Terminal Server) de la commande](remote-desktop-services-terminal-services-command-reference.md)
