---
title: processus de requête
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 36ce3ffc-0092-4eb1-a374-28e6616ca946
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e15a41fc931d2cf04d60759a63e3b80392265175
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840720"
---
# <a name="query-process"></a>processus de requête

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche des informations sur les processus qui s’exécutent sur un serveur hôte de Session Bureau à distance (hôte de Session Bureau à distance).
Vous pouvez utiliser cette commande pour connaître les programmes d’un utilisateur spécifique est en cours d’exécution, et également les utilisateurs qui exécutent un programme spécifique.
Pour obtenir des exemples montrant comment utiliser cette commande, consultez [exemples](#BKMK_examples).
> [!NOTE]
> Dans Windows Server 2008 R2, les services Terminal Server ont été renommés services Bureau à distance. Pour savoir quelles sont les nouveautés dans la version la plus récente, consultez [les nouveautés des Services Bureau à distance dans Windows Server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.
## <a name="syntax"></a>Syntaxe
```
query process [* | <ProcessID> | <UserName> | <SessionName> | /id:<nn> | <ProgramName>] [/server:<ServerName>]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|*|Répertorie les processus pour toutes les sessions.|
|<ProcessID>|Spécifie l’ID numérique qui identifie le processus que vous souhaitez interroger.|
|<UserName>|Spécifie le nom de l’utilisateur dont vous souhaitez répertorier les processus.|
|<SessionName>|Spécifie le nom de la session dont vous souhaitez répertorier les processus.|
|/ID :<nn>|Spécifie l’ID de la session dont vous souhaitez répertorier les processus.|
|<ProgramName>|Spécifie le nom du programme dont vous souhaitez interroger les processus. L’extension .exe est requise.|
|/server:<ServerName>|Spécifie le serveur hôte de Session de bureau à distance dont vous souhaitez répertorier les processus. Si non spécifié, le serveur où vous êtes actuellement connecté est utilisé.|
|/?|Affiche l'aide à l'invite de commandes.|
## <a name="remarks"></a>Notes
-   Les administrateurs ont un accès complet à tous les **interroger les processus** fonctions.
-   Si vous ne spécifiez pas la <*nom d’utilisateur*>, <*SessionName*>, **/id :**<*nn*>, <*Nom_programme*>, ou **\*** paramètres, **processus de requête** affiche uniquement les processus qui appartiennent à l’utilisateur actuel.
-   Si une session est spécifiée, elle doit identifier une session active.
-   **processus de requête** renvoie les informations suivantes :
    -   L’utilisateur propriétaire du processus
    -   La session propriétaire du processus
    -   L’ID de la session
    -   Le nom du processus
    -   L’ID du processus
-   Lorsque **interroger les processus** retourne des informations, un symbole supérieur à (>) s’affiche devant chaque processus appartenant à la session active.
## <a name="BKMK_examples"></a>Exemples
-   Pour afficher des informations sur les processus utilisés par toutes les sessions, tapez :
    ```
    query process *
    ```
-   Pour afficher des informations sur les processus utilisé par l’ID session 2, tapez :
    ```
    query process /ID:2
    ```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[requête](query.md)
[Services Bureau à distance &#40;Services Terminal Server&#41; référence des commandes](remote-desktop-services-terminal-services-command-reference.md)
