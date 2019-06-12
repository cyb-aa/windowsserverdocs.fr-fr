---
title: change user
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6202f024-8cf5-411e-89b1-ee37ff46499d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bedef9f996554a3b5745b47f646204646fdfa9a6
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434410"
---
# <a name="change-user"></a>change user

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

modifie le mode d’installation pour le serveur hôte de Session Bureau à distance (hôte de Session Bureau à distance).
Pour obtenir des exemples montrant comment utiliser cette commande, consultez [exemples](#BKMK_examples).
> [!NOTE]
> Dans Windows Server 2008 R2, les services Terminal Server ont été renommés services Bureau à distance. Pour savoir quelles sont les nouveautés dans la version la plus récente, consultez [les nouveautés des Services Bureau à distance dans Windows Server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.
> ## <a name="syntax"></a>Syntaxe
> ```
> change user {/execute | /install | /query}
> ```
> ## <a name="parameters"></a>Paramètres
> 
> | Paramètre |                                                                                                 Description                                                                                                  |
> |-----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> | / exécuter  |                                                                Active le mappage de fichier .ini au répertoire de base. Il s’agit du paramètre par défaut.                                                                 |
> | /install  | Désactive le mappage de fichier .ini au répertoire de base. Tous les fichiers .ini sont lus et écrits sur le répertoire système. Vous devez désactiver le mappage du fichier .ini lors de l’installation des applications sur un serveur hôte de Session de bureau à distance. |
> |  /query   |                                                                             Affiche le paramètre actuel pour le mappage de fichier .ini.                                                                              |
> |    /?     |                                                                                     Affiche l'aide à l'invite de commandes.                                                                                     |
> 
> ## <a name="remarks"></a>Notes
> - Utilisez **modification user /install** avant d’installer une application pour créer des fichiers .ini de l’application dans le répertoire système. Ces fichiers sont utilisés comme source lors de la création des fichiers .ini spécifiques à l’utilisateur. Après avoir installé l’application, utilisez **changer d’utilisateur / exécuter** pour rétablir le mappage de fichier .ini standard.
> - La première fois que vous exécutez l’application, il recherche dans le répertoire de base pour ses fichiers .ini. Si les fichiers .ini sont introuvables dans le répertoire de base, mais sont trouvent dans le répertoire système, Services Bureau à distance copie les fichiers .ini dans le répertoire de base, vérifier que chaque utilisateur dispose d’une copie unique des fichiers .ini de l’application. Les nouveaux fichiers .ini sont créés dans le répertoire de base.
> - Chaque utilisateur doit posséder une copie unique des fichiers .ini pour une application. Cela empêche les instances où les différents utilisateurs peuvent avoir des configurations d’application incompatibles (par exemple, des répertoires par défaut ou des résolutions d’écran).
> - Lorsque le système est en mode d’installation (autrement dit, **modification user /install**), plusieurs choses se produisent. Toutes les entrées de Registre sont créées sont masquées sous **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Microsoft\Windows NT\Currentversion\Terminal Server\Install**, dans un le **\SOFTWARE** sous-clé ou le **\MACHINE** sous-clé. Sous-clés ajouté à **HKEY_CURrenT_USER** sont copiées sous la **\SOFTWARE** sous-clé et sous-clés ajouté à **HKEY_LOCAL_MACHINE** sont copiées sous la **\ MACHINE** sous-clé. Si l’application interroge le répertoire Windows à l’aide d’appels système, telles que GetWindowsdirectory, le serveur hôte de Session Bureau à distance retourne le répertoire systemroot. Si des entrées de fichier .ini sont ajoutées à l’aide d’appels système, telles que WritePrivateProfileString, ils sont ajoutés aux fichiers .ini sous le répertoire systemroot.
> - Lorsque le système revient au mode d’exécution (autrement dit, **changer d’utilisateur / exécuter**), et l’application tente de lire une entrée de Registre sous **HKEY_CURrenT_USER** qui n’existe pas, les Services Bureau à distance vérifie si une copie de la clé existe sous le **\Terminal Server\Install** sous-clé. Le cas échéant, les sous-clés sont copiées vers l’emplacement approprié sous **HKEY_CURrenT_USER**. Si l’application tente de lire à partir d’un fichier .ini qui n’existe pas, les Services Bureau à distance recherche ce fichier .ini sous la racine du système. Si le fichier .ini est à la racine du système, il est copié dans le sous-répertoire \Windows du répertoire de base de l’utilisateur. Si l’application interroge le répertoire Windows, le serveur hôte de Session Bureau à distance retourne le sous-répertoire \Windows du répertoire de base de l’utilisateur.
> - Lorsque vous ouvrez une session, les Services Bureau à distance vérifie si les fichiers système .ini sont plus récents que les fichiers .ini sur votre ordinateur. Si la version du système est plus récente, votre fichier .ini est remplacé ou fusionnée avec la version plus récente. Cela dépend si le bit INISYNC, 0 x 40, est définie pour ce fichier .ini. Votre version précédente du fichier .ini est renommée en Inifile.ctx. Si les valeurs de Registre du système sous la **\Terminal Server\Install** sous-clé sont plus récents que votre version sous **HKEY_CURrenT_USER**, votre version des sous-clés est supprimée et remplacée par les nouvelles sous-clés à partir de **\Terminal Server\Install**.
>   ## <a name="BKMK_examples"></a>Exemples
> - Pour désactiver le mappage de fichier .ini dans le répertoire de base, tapez :
>   ```
>   change user /install
>   ```
> - Pour activer le mappage de fichier .ini dans le répertoire de base, tapez :
>   ```
>   change user /execute
>   ```
> - Pour afficher le paramètre actuel pour le mappage de fichier .ini, tapez :
>   ```
>   change user /query
>   ```
>   #### <a name="additional-references"></a>Références supplémentaires
>   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
>   [modifier](change.md)
>   [Services Bureau à distance &#40;Services Terminal Server&#41; référence des commandes](remote-desktop-services-terminal-services-command-reference.md)
