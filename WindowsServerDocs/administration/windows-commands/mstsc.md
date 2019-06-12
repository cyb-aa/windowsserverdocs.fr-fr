---
title: mstsc
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 59801227-1e7e-4dbd-96e6-f54102a3ce92
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b6f89c1e3b0d36f14dbd55f9e6994c788305b30d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437185"
---
# <a name="mstsc"></a>mstsc

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

crée des connexions aux serveurs de l’hôte de Session Bureau à distance (hôte de Session Bureau à distance) ou d’autres ordinateurs à distance, les modifications d’un fichier de configuration de connexion Bureau à distance (.rdp) et migre les fichiers de connexion qui ont été créés avec le Gestionnaire de connexions Client pour les nouveaux fichiers de connexion .rdp.
Pour obtenir des exemples montrant comment utiliser cette commande, consultez [exemples](#BKMK_examples).
> [!NOTE]
> Dans Windows Server 2008 R2, les services Terminal Server ont été renommés services Bureau à distance. Pour savoir quelles sont les nouveautés dans la version la plus récente, consultez [les nouveautés des Services Bureau à distance dans Windows Server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.

## <a name="syntax"></a>Syntaxe
```
mstsc.exe [<Connection File>] [/v:<Server>[:<Port>]] [/admin] [/f] [/w:<Width> /h:<Height>] [/public] [/span]
mstsc.exe /edit <Connection File>
mstsc.exe /migrate
```

## <a name="parameters"></a>Paramètres

|        Paramètre        |                                                         Description                                                         |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------|
|    <Connection File>    |                                   Spécifie le nom d’un fichier .rdp pour la connexion.                                    |
|   / v: < server [ :<Port>]   |                Spécifie l’ordinateur distant et, éventuellement, le numéro de port auquel vous souhaitez vous connecter.                 |
|         /admin          |                                   Vous connecte à une session pour administrer le serveur.                                   |
|           /f            |                                    démarre la connexion Bureau à distance en mode plein écran.                                    |
|       /w:<Width>        |                                      Spécifie la largeur de la fenêtre du Bureau à distance.                                      |
|       /h:<Height>       |                                     Spécifie la hauteur de la fenêtre du Bureau à distance.                                      |
|         /public         |                  Exécute le Bureau à distance en mode public. En mode public, les bitmaps et les mots de passe ne sont pas mises en cache.                  |
|          /span          | Correspond à la largeur du Bureau à distance et la hauteur du bureau virtuel local, la répartition sur plusieurs moniteurs si nécessaire. |
| /Edit <Connection File> |                                         Ouvre le fichier .rdp spécifié pour la modification.                                          |
|        /migrate         |       Migre les fichiers de connexion qui ont été créés avec le Gestionnaire de connexions Client aux nouveaux fichiers de connexion .rdp.       |
|           /?            |                                            Affiche l'aide à l'invite de commandes.                                             |

## <a name="remarks"></a>Notes
-   Default.RDP est stocké pour chaque utilisateur dans un fichier masqué dans le dossier Documents de l’utilisateur. Utilisateur créé les fichiers .rdp sont enregistrées par défaut dans le dossier Documents de l’utilisateur, mais elles peuvent être enregistrées n’importe où.
-   Pour englober les analyses, les moniteurs doivent utiliser la même résolution et doivent être alignées horizontalement (autrement dit, côte à côte). Il n’existe actuellement aucune prise en charge pour s’étendant sur plusieurs moniteurs verticalement sur le système client.

## <a name="BKMK_examples"></a>Exemples
-   Pour vous connecter à une session en mode plein écran, tapez :
    ```
    mstsc /f
    ```
-   Pour ouvrir un fichier nommé nomfichier.RDP en vue de modification, tapez :
    ```
    mstsc /edit filename.rdp
    ```

#### <a name="additional-references"></a>Références supplémentaires
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Services Bureau à distance &#40;Services Terminal Server&#41; référence de la commande](remote-desktop-services-terminal-services-command-reference.md)
