---
title: mstsc
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 59801227-1e7e-4dbd-96e6-f54102a3ce92
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e3355aa310bb9919b5218052878d94416f577381
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723830"
---
# <a name="mstsc"></a>mstsc

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

crée des connexions aux serveurs Bureau à distance hôte de session (hôte de session Bureau à distance) ou à d’autres ordinateurs distants, modifie un fichier de configuration Connexion Bureau à distance (. RDP) existant et migre les fichiers de connexion hérités qui ont été créés avec le gestionnaire de connexions client vers de nouveaux fichiers de connexion. RDP.

> [!NOTE]
> Dans Windows Server 2008 R2, les services Terminal Server ont été renommés services Bureau à distance. Pour découvrir les nouveautés de la dernière version, consultez les nouveautés [de services Bureau à distance dans Windows server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.

## <a name="syntax"></a>Syntaxe
```
mstsc.exe [<Connection File>] [/v:<Server>[:<Port>]] [/admin] [/f] [/w:<Width> /h:<Height>] [/public] [/span]
mstsc.exe /edit <Connection File>
mstsc.exe /migrate
```

### <a name="parameters"></a>Paramètres

|        Paramètre        |                                                         Description                                                         |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------|
|    <Connection File>    |                                   Spécifie le nom d’un fichier. RDP pour la connexion.                                    |
|  /v : <Server\>[ : <port\>] |                Spécifie l’ordinateur distant et, éventuellement, le numéro de port auquel vous souhaitez vous connecter.                 |
|         /admin          |                                   Permet de vous connecter à une session d’administration du serveur.                                   |
|           /f            |                                    démarre Connexion Bureau à distance en mode plein écran.                                    |
|       /w<Width>        |                                      Spécifie la largeur de la fenêtre de Bureau à distance.                                      |
|       /h<Height>       |                                     Spécifie la hauteur de la fenêtre de Bureau à distance.                                      |
|         /public         |                  Exécute Bureau à distance en mode public. En mode public, les mots de passe et les bitmaps ne sont pas mis en cache.                  |
|          /span          | Met en correspondance la largeur et la hauteur du Bureau à distance avec le bureau virtuel local, en répartissant sur plusieurs analyses si nécessaire. |
| /Edit<Connection File> |                                         Ouvre le fichier. rdp spécifié pour la modification.                                          |
|        /migrate         |       Migre les fichiers de connexion hérités qui ont été créés avec le gestionnaire de connexions client vers de nouveaux fichiers de connexion. RDP.       |
|           /?            |                                            Affiche l'aide à l'invite de commandes.                                             |

## <a name="remarks"></a>Notes 
-   Default. RDP est stocké pour chaque utilisateur sous la forme d’un fichier caché dans le dossier Documents de l’utilisateur. Les fichiers. RDP créés par l’utilisateur sont enregistrés par défaut dans le dossier Documents de l’utilisateur, mais peuvent être enregistrés n’importe où.
-   Pour s’étendre sur plusieurs moniteurs, les moniteurs doivent utiliser la même résolution et doivent être alignés horizontalement (autrement dit, côte à côte). Il n’existe actuellement aucune prise en charge pour la répartition verticale de plusieurs moniteurs sur le système client.

## <a name="examples"></a>Exemples
-   Pour vous connecter à une session en mode plein écran, tapez :
    ```
    mstsc /f
    ```
-   Pour ouvrir un fichier appelé nom_fichier. RDP pour le modifier, tapez :
    ```
    mstsc /edit filename.rdp
    ```

## <a name="additional-references"></a>Références supplémentaires
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Référence des commandes des services Bureau à distance (services Terminal Server)](remote-desktop-services-terminal-services-command-reference.md)
