---
title: utilisateur de la requête
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a670fb78-c055-464a-b61d-3a85632c52c5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6e54d1b9701cc2f3a4f229a3910d5b325fee87c6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813940"
---
# <a name="query-user"></a>utilisateur de la requête

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche des informations sur les sessions utilisateur sur un serveur hôte de Session Bureau à distance (hôte de Session Bureau à distance).
Pour obtenir des exemples montrant comment utiliser cette commande, consultez [exemples](#BKMK_examples).
> [!NOTE]
> Dans Windows Server 2008 R2, les services Terminal Server ont été renommés services Bureau à distance. Pour savoir quelles sont les nouveautés dans la version la plus récente, consultez [les nouveautés des Services Bureau à distance dans Windows Server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.
## <a name="syntax"></a>Syntaxe
```
query user [<UserName> | <SessionName> | <SessionID>] [/server:<ServerName>]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|<UserName>|Spécifie le nom d’ouverture de session de l’utilisateur que vous souhaitez interroger.|
|<SessionName>|Spécifie le nom de la session que vous souhaitez interroger.|
|<SessionID>|Spécifie l’ID de la session que vous souhaitez interroger.|
|/server:<ServerName>|Spécifie le serveur hôte de Session Bureau à distance que vous souhaitez interroger. Sinon, le serveur hôte de Session Bureau à distance en cours est utilisé.|
|/?|Affiche l'aide à l'invite de commandes.|
## <a name="remarks"></a>Notes
-   Vous pouvez utiliser cette commande pour savoir si un utilisateur spécifique est connecté à un serveur hôte de Session Bureau à distance par spécifique. **utilisateur de la requête** renvoie les informations suivantes :
    -   Le nom de l’utilisateur
    -   Le nom de la session sur le serveur hôte de Session Bureau à distance
    -   L’ID de session
    -   L’état de la session (active ou déconnectée)
    -   Temps d’inactivité (le nombre de minutes depuis la dernière séquence de touches ou mouvements de la souris lors de la session)
    -   Date et heure de l’utilisateur connecté
-   Pour utiliser **utilisateur de la requête**, vous devez avoir l’autorisation contrôle total ou informations autorisation spéciale d’accès de requête.
-   Si vous utilisez **utilisateur de la requête** sans spécifier <*nom d’utilisateur*>, <*SessionName*>, ou <*SessionID*>, une liste de tous les les utilisateurs connectés au serveur est retournée. Sinon, vous pouvez également utiliser **interroger session** pour afficher une liste de toutes les sessions sur un serveur.
-   Lorsque **utilisateur de la requête** renvoie des informations, un symbole supérieur à (>) s’affiche avant la session active.
-   Le **/server** paramètre est obligatoire uniquement si vous utilisez **utilisateur de la requête** à partir d’un serveur distant.
## <a name="BKMK_examples"></a>Exemples
-   Pour afficher des informations sur tous les utilisateurs connectés au système, tapez :
    ```
    query user
    ```
-   Pour afficher des informations sur l’utilisateur USER1 sur le serveur SERver1, tapez :
    ```
    query user USER1 /server:SERver1
    ```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[requête](query.md)
[Services Bureau à distance &#40;Services Terminal Server&#41; référence des commandes](remote-desktop-services-terminal-services-command-reference.md)
