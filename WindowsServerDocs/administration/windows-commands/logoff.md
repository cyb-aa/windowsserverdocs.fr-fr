---
title: fermeture de session
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 88b3cbbbd52a965fd0a0de3c164e8dbc1f90d053
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437523"
---
# <a name="logoff"></a>fermeture de session

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se déconnecte un utilisateur à partir d’une session sur un serveur hôte de Session Bureau à distance (hôte de Session Bureau à distance) et supprime la session à partir du serveur.
Pour obtenir des exemples montrant comment utiliser cette commande, consultez [exemples](#BKMK_examples).

> [!NOTE]
> Dans Windows Server 2008 R2, les services Terminal Server ont été renommés services Bureau à distance. Pour savoir quelles sont les nouveautés dans la version la plus récente, consultez [les nouveautés des Services Bureau à distance dans Windows Server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.

## <a name="syntax"></a>Syntaxe
```
logoff [<SessionName> | <SessionID>] [/server:<ServerName>] [/v]
```
## <a name="parameters"></a>Paramètres

|      Paramètre       |                                                                             Description                                                                              |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <SessionName>     |                                                                  Spécifie le nom de la session.                                                                  |
|     <SessionID>      |                                                 Spécifie l’ID numérique qui identifie la session sur le serveur.                                                 |
| /server:<ServerName> | Spécifie le serveur hôte de Session Bureau à distance qui contient la session dont vous souhaitez déconnecter l’utilisateur. Si non spécifié, le serveur sur lequel vous êtes actuellement connecté est utilisé. |
|          /v          |                                                       Affiche des informations sur les actions en cours d’exécution.                                                        |
|          /?          |                                                                 Affiche l'aide à l'invite de commandes.                                                                 |

## <a name="remarks"></a>Notes
- Vous pouvez toujours vous déconnecter de la session à laquelle vous êtes actuellement connecté. Toutefois, vous devez déconnecter les utilisateurs d’autres sessions de l’autorisation contrôle total.
- Déconnexion d’un utilisateur à partir d’une session sans avertissement peut entraîner une perte de données lors de la session. Vous devez envoyer un message à l’utilisateur à l’aide de la **msg** commande pour avertir l’utilisateur avant d’effectuer cette action.
- Si <*SessionID*> ou <*SessionName*> n’est pas spécifié, **fermeture de session** se déconnecte l’utilisateur de la session active. Si vous spécifiez <*SessionName*>, il doit être active.
- Lorsque vous vous déconnectez un utilisateur, tous les processus s’arrêtent et la session est supprimée à partir du serveur.
- Vous ne pouvez pas déconnecter un utilisateur à partir de la session de console.
  ## <a name="BKMK_examples"></a>Exemples
- Pour déconnecter un utilisateur à partir de la session active, tapez :
  ```
  logoff
  ```
- Pour déconnecter un utilisateur à partir d’une session en utilisant l’ID de session, par exemple la session 12, tapez :
  ```
  logoff 12
  ```
- Pour déconnecter un utilisateur à partir d’une session en utilisant le nom de la session et le serveur, par exemple session TERM04 sur Server1, tapez :
  ```
  logoff TERM04 /server:Server1
  ```

#### <a name="additional-references"></a>Références supplémentaires
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Services Bureau à distance &#40;Services Terminal Server&#41; référence de la commande](remote-desktop-services-terminal-services-command-reference.md)
