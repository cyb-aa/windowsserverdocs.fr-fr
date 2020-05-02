---
title: path
description: Découvrez comment définir la variable d’environnement PATH.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1bfa1349-e79a-472b-a9e6-d7a91149ae8f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 52d4a99d21574f9cae3120201dcd4db0cd9a2202
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723375"
---
# <a name="path"></a>path



Définit le chemin d’accès de la commande dans la variable d’environnement PATH (l’ensemble de répertoires utilisés pour rechercher des fichiers exécutables). S’il est utilisé sans paramètres, **path** affiche le chemin d’accès à la commande actuelle.



## <a name="syntax"></a>Syntaxe

```
path [[<Drive>:]<Path>[;...][;%PATH%]]
path ;
```

### <a name="parameters"></a>Paramètres

|     Paramètre     |                                                                                                     Description                                                                                                      |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [\<Lecteur> :]<Path> |                                                                            Spécifie le lecteur et le répertoire à définir dans le chemin d’accès de la commande.                                                                             |
|         ;         | Sépare les répertoires dans le chemin d’accès de la commande. S’il est utilisé sans autre **paramètre,** efface les chemins d’accès aux commandes existants de la variable d’environnement PATH et indique à cmd. exe de rechercher uniquement dans le répertoire actif. |
|      D       |                                                         Ajoute le chemin d’accès de commande à l’ensemble existant de répertoires figurant dans la variable d’environnement PATH.                                                         |
|        /?         |                                                                                         Affiche l'aide à l'invite de commandes.                                                                                         |

## <a name="remarks"></a>Notes 

-   Lorsque vous incluez **% path%** dans la syntaxe, cmd. exe le remplace par les valeurs de chemin d’accès de commande trouvées dans la variable d’environnement PATH, ce qui évite d’avoir à entrer manuellement ces valeurs à l’invite de commandes.
-   Le répertoire actif est toujours recherché avant les répertoires spécifiés dans le chemin d’accès de la commande.
-   Vous pouvez avoir des fichiers dans un répertoire qui partagent le même nom de fichier, mais qui ont des extensions différentes. Par exemple, vous pouvez avoir un fichier nommé Accnt.com qui démarre un programme de comptabilité et un autre fichier nommé ACCNT. bat, qui connecte votre serveur au réseau de comptabilité.

    Le système d’exploitation Windows recherche un fichier en utilisant les extensions de nom de fichier par défaut dans l’ordre de priorité suivant :. exe,. com,. bat et. cmd. Pour exécuter ACCNT. bat lorsque Accnt.com existe dans le même répertoire, vous devez inclure l’extension. bat à l’invite de commandes.
-   Si au moins deux fichiers du chemin d’accès de la commande ont le même nom de fichier et l’extension, **path** commence par Rechercher le nom de fichier spécifié dans le répertoire actif. Il recherche ensuite dans les répertoires dans le chemin d’accès de commande dans l’ordre dans lequel ils sont répertoriés dans la variable d’environnement PATH.
-   Si vous placez la commande **path** dans votre fichier Autoexec. NT, le système d’exploitation Windows ajoute automatiquement le chemin de recherche du sous-système MS-DOS spécifié chaque fois que vous ouvrez une session sur votre ordinateur. Cmd. exe n’utilise pas le fichier Autoexec. NT. Lorsqu’il est démarré à partir d’un raccourci, cmd. exe hérite des variables d’environnement définies dans Poste de travail/Properties/Advanced/Environment.

## <a name="examples"></a><a name="BKMK_examples"></a>Illustre

Pour rechercher les chemins C:\User\Taxes, B:\User\Invest et B:\Bin pour les commandes externes, tapez :

`path c:\user\taxes;b:\user\invest;b:\bin`

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)