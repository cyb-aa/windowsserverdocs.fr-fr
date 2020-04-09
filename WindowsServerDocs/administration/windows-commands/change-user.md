---
title: change user
description: La rubrique commandes Windows pour modifier l’utilisateur, qui modifie le mode d’installation du serveur hôte de session Bureau à distance.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6202f024-8cf5-411e-89b1-ee37ff46499d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b13fe66991d6e8bbb91938b550236f3aa9f51ee9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848062"
---
# <a name="change-user"></a>change user

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Modifie le mode d’installation du serveur hôte de session Bureau à distance.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

> [!NOTE]
> Sous Windows Server 2008 R2, les services Terminal Server sont appelés Services Bureau à distance. Pour découvrir les nouveautés de la dernière version, consultez les nouveautés [de services Bureau à distance dans Windows server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.

## <a name="syntax"></a>Syntaxe
```
change user {/execute | /install | /query}
```
### <a name="parameters"></a>Paramètres

| Paramètre |                                                                                                 Description                                                                                                  |
|-----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /Execute place  |                                                                Active le mappage de fichiers. ini dans le répertoire de départ. Il s’agit du paramètre par défaut.                                                                 |
| /install  | Désactive le mappage du fichier. ini vers le répertoire de départ. Tous les fichiers. ini sont lus et écrits dans le répertoire système. Vous devez désactiver le mappage de fichiers. ini lors de l’installation d’applications sur un serveur hôte de session Bureau à distance. |
|  Query   |                                                                             Affiche le paramètre actuel du mappage de fichiers. ini.                                                                              |
|    /?     |                                                                                     Affiche l'aide à l'invite de commandes.                                                                                     |

## <a name="remarks"></a>Notes
- Utilisez **modifier l’utilisateur/install** avant d’installer une application pour créer des fichiers. ini pour l’application dans le répertoire système. Ces fichiers sont utilisés comme source lors de la création de fichiers. ini spécifiques à l’utilisateur. Après l’installation de l’application, utilisez l’activité **change user/execute** pour revenir au mappage de fichier. ini standard.
- La première fois que vous exécutez l’application, elle recherche ses fichiers. ini dans le répertoire de démarrage. Si les fichiers. ini sont introuvables dans le répertoire de départ, mais se trouvent dans le répertoire système, Services Bureau à distance copie les fichiers. ini dans le répertoire de départ, en veillant à ce que chaque utilisateur dispose d’une copie unique des fichiers. ini de l’application. Tous les nouveaux fichiers. ini sont créés dans le répertoire de départ.
- Chaque utilisateur doit disposer d’une copie unique des fichiers. ini pour une application. Cela empêche les instances où des utilisateurs différents peuvent avoir des configurations d’application incompatibles (par exemple, des répertoires par défaut ou des résolutions d’écran différents).
- Lorsque le système est en mode d’installation (c’est-à-dire, **change user/install**), plusieurs événements se produisent. Toutes les entrées de Registre créées sont occultées sous **HKEY_LOCAL_MACHINE \Software\microsoft\windows NT\Currentversion\Terminal Server\Install**, soit dans la sous-clé **\Software** , soit dans la sous-clé **\MACHINE** . Les sous-clés ajoutées à **HKEY_CURRENT_USER** sont copiées sous la sous-clé **\Software** , et les sous-clés ajoutées à **HKEY_LOCAL_MACHINE** sont copiées sous la sous-clé **\MACHINE** . Si l’application interroge le répertoire Windows à l’aide d’appels système, comme GetWindowsdirectory, le serveur hôte de session Bureau à distance retourne le répertoire systemroot. Si des entrées de fichier. ini sont ajoutées à l’aide d’appels système, tels que WritePrivateProfileString, ils sont ajoutés aux fichiers. ini sous le répertoire systemroot.
- Lorsque le système revient en mode d’exécution (autrement dit, **change user/execute**) et que l’application tente de lire une entrée de registre sous **HKEY_CURRENT_USER** qui n’existe pas, services Bureau à distance vérifie si une copie de la clé existe dans la sous-clé **HKLM\SYSTEM\CurrentControlset\Control\Terminal Server\Install** . Si c’est le cas, les sous-clés sont copiées à l’emplacement approprié sous **HKEY_CURRRENT_USER**. Si l’application tente de lire à partir d’un fichier. ini qui n’existe pas, Services Bureau à distance recherche ce fichier. ini sous la racine système. Si le fichier. ini se trouve dans la racine système, il est copié dans le sous-répertoire \Windows du répertoire de destination de l’utilisateur. Si l’application interroge le répertoire Windows, le serveur hôte de session Bureau à distance renvoie le sous-répertoire \Windows du répertoire de démarrage de l’utilisateur.
- Lorsque vous ouvrez une session, Services Bureau à distance vérifie si ses fichiers System. ini sont plus récents que les fichiers. ini sur votre ordinateur. Si la version du système est plus récente, votre fichier. ini est soit remplacé, soit fusionné avec la version plus récente. Cela varie selon que le bit INISYNC, 0x40, est défini ou non pour ce fichier. ini. Votre version précédente du fichier. ini est renommée en inifile. ctx. Si les valeurs du Registre système sous la sous-clé **Hklm\system\currentcontrolset\control\terminal Server\Install** sont plus récentes que votre version sous **HKEY_CURRENT_USER**, votre version des sous-clés est supprimée et remplacée par les nouvelles sous-clés de **HKLM\SYSTEM\CurrentControlset\Control\Terminal Server\Install**.
  ## <a name="examples"></a><a name=BKMK_examples></a>Illustre
- Pour désactiver le mappage de fichiers. ini dans le répertoire de départ, tapez :
  ```
  change user /install
  ```
- Pour activer le mappage de fichiers. ini dans le répertoire de départ, tapez :
  ```
  change user /execute
  ```
- Pour afficher le paramètre actuel du mappage de fichier. ini, tapez :
  ```
  change user /query
  ```
  ## <a name="additional-references"></a>Références supplémentaires
  - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
  [modifier](change.md)
  [services Bureau à distance la référence de commande (services Terminal Server)](remote-desktop-services-terminal-services-command-reference.md)
