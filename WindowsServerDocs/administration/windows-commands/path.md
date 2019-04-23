---
title: path
description: Découvrez comment définir la variable d’environnement PATH.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1bfa1349-e79a-472b-a9e6-d7a91149ae8f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 65ccaf23b0e19319383952f3a1ca436aaf4d06fd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856460"
---
# <a name="path"></a>path



Définit le chemin d’accès de la commande dans la variable d’environnement PATH (l’ensemble de répertoires utilisés pour rechercher des fichiers exécutables). Si utilisée sans paramètres, **chemin d’accès** affiche le chemin actuel de la commande.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
path [[<Drive>:]<Path>[;...][;%PATH%]]
path ;
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[\<Drive>:]<Path>|Spécifie le lecteur et le répertoire à définir dans le chemin d’accès de la commande.|
|;|Sépare les répertoires dans le chemin d’accès de la commande. Si utilisée sans les autres paramètres, **;** efface les chemins d’accès de commande existantes à partir de la variable d’environnement PATH et dirige Cmd.exe de rechercher uniquement dans le répertoire actif.|
|CHEMIN D’ACCÈS %|Ajoute le chemin d’accès de la commande à l’ensemble existant de répertoires répertoriés dans la variable d’environnement PATH.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Lorsque vous incluez **% path%** dans la syntaxe, Cmd.exe remplace par les valeurs de chemin d’accès de commande trouvées dans la variable d’environnement PATH, éliminant la nécessité d’entrer manuellement ces valeurs à l’invite de commandes.
-   Le répertoire actif est recherché toujours avant les répertoires spécifiés dans le chemin d’accès de la commande.
-   Vous pouvez avoir des fichiers dans un répertoire qui partagent le même nom de fichier mais avez différentes extensions. Par exemple, vous pouvez avoir un fichier nommé compta.com qui démarre un programme de comptabilité et un autre appelé compta.bat qui connecte votre serveur au réseau du système de comptabilité.

    Le système d’exploitation de Windows recherche un fichier à l’aide des extensions de nom de fichier par défaut dans l’ordre de priorité suivant : .exe, .com, .bat, et. cmd. Pour exécuter compta.bat alors compta.com existe dans le même répertoire, vous devez inclure l’extension .bat à l’invite de commandes.
-   Si deux ou plusieurs fichiers dans le chemin d’accès de commande ont le même nom de fichier et l’extension, **chemin d’accès** nom de la première recherche le fichier spécifié dans le répertoire actif. Il recherche ensuite dans les répertoires dans le chemin d’accès de commande dans l’ordre d’apparition dans la variable d’environnement PATH.
-   Si vous placez le **chemin d’accès** commande dans votre fichier Autoexec.nt, le système d’exploitation Windows ajoute automatiquement le chemin de recherche du sous-système MS-DOS spécifié chaque fois que vous ouvrez une session votre ordinateur. Cmd.exe n’utilise pas le fichier Autoexec.nt. Démarrage à partir d’un raccourci, Cmd.exe hérite les variables d’environnement définies dans mon ordinateur/propriétés/Avancé/environnement.

## <a name="BKMK_examples"></a>Exemples

Pour rechercher les chemins d’accès C:\User\Taxes, B:\User\Invest et B:\Bin pour des commandes externes, tapez :

`path c:\user\taxes;b:\user\invest;b:\bin`

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)