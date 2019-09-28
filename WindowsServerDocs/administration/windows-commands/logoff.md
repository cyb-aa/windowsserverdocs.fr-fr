---
title: fermeture de session
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 939f09cc-de8c-436c-a05d-aca5f2a06371
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d09b58823f12d0b26bf21c00638b58046119bdab
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374229"
---
# <a name="logoff"></a>fermeture de session

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Déconnecte un utilisateur d’une session sur un serveur hôte de session Bureau à distance (hôte de session Bureau à distance) et supprime la session du serveur.
Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_examples).

> [!NOTE]
> Dans Windows Server 2008 R2, les services Terminal Server ont été renommés services Bureau à distance. Pour découvrir les nouveautés de la dernière version, consultez les nouveautés [de services Bureau à distance dans Windows server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.

## <a name="syntax"></a>Syntaxe
```
logoff [<SessionName> | <SessionID>] [/server:<ServerName>] [/v]
```
## <a name="parameters"></a>Paramètres

|      Paramètre       |                                                                             Description                                                                              |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <SessionName>     |                                                                  Spécifie le nom de la session.                                                                  |
|     <SessionID>      |                                                 Spécifie l’ID numérique qui identifie la session sur le serveur.                                                 |
| /server:<ServerName> | Spécifie le serveur hôte de session Bureau à distance qui contient la session dont vous souhaitez fermer la session. S’il n’est pas spécifié, le serveur sur lequel vous êtes actuellement actif est utilisé. |
|          /v          |                                                       Affiche des informations sur les actions en cours d’exécution.                                                        |
|          /?          |                                                                 Affiche l'aide à l'invite de commandes.                                                                 |

## <a name="remarks"></a>Notes
- Vous pouvez toujours vous déconnecter de la session à laquelle vous êtes actuellement connecté. Toutefois, vous devez disposer de l’autorisation contrôle total pour déconnecter les utilisateurs d’autres sessions.
- La déconnexion d’un utilisateur à partir d’une session sans avertissement peut entraîner la perte de données au niveau de la session de l’utilisateur. Vous devez envoyer un message à l’utilisateur à l’aide de la commande **MSG** pour avertir l’utilisateur avant d’effectuer cette action.
- Si <*SessionID*> ou <*session*> n’est pas spécifié, **Logoff** déconnecte l’utilisateur de la session active. Si vous spécifiez <*nom_session*>, il doit s’agir d’un service actif.
- Lorsque vous fermez la session d’un utilisateur, tous les processus se terminent et la session est supprimée du serveur.
- Vous ne pouvez pas fermer la session d’un utilisateur à partir de la session de la console.
  ## <a name="BKMK_examples"></a>Illustre
- Pour fermer la session active d’un utilisateur, tapez :
  ```
  logoff
  ```
- Pour fermer la session d’un utilisateur à l’aide de l’ID de la session, par exemple session 12, tapez :
  ```
  logoff 12
  ```
- Pour fermer la session d’un utilisateur à partir d’une session à l’aide du nom de la session et du serveur, par exemple session TERM04 sur Server1, tapez :
  ```
  logoff TERM04 /server:Server1
  ```

#### <a name="additional-references"></a>Références supplémentaires
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Services Bureau à distance &#40;la référence&#41; des commandes des services Terminal Server](remote-desktop-services-terminal-services-command-reference.md)
