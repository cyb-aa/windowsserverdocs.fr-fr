---
title: replace
description: Découvrez comment utiliser la commande replace pour remplacer les fichiers.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6143661e-d90f-4812-b265-6669b567dd1f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: bce4622983271bde06614ebc04418871490cff91
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818890"
---
# <a name="replace"></a>replace



Remplace les fichiers. Si utilisé avec le **/a** option, **remplacer** ajoute de nouveaux fichiers dans un répertoire au lieu de remplacer les fichiers existants.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
replace [<Drive1>:][<Path1>]<FileName> [<Drive2>:][<Path2>] [/a] [/p] [/r] [/w] 
replace [<Drive1>:][<Path1>]<FileName> [<Drive2>:][<Path2>] [/p] [/r] [/s] [/w] [/u] 
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[\<Drive1>:][\<Path1>]\<FileName>|Spécifie l’emplacement et le nom du fichier source ou un ensemble de fichiers. *Nom de fichier* est obligatoire et peut inclure des caractères génériques (**&#42;** et **?**).|
|[\<Drive2>:][\<Path2>]|Spécifie l’emplacement du fichier de destination. Vous ne pouvez pas spécifier un nom de fichier pour les fichiers que vous remplacez. Si vous ne spécifiez pas un lecteur ou un chemin d’accès, **remplacer** utilise le lecteur actuel et le répertoire comme destination.|
|/a|Ajoute de nouveaux fichiers dans le répertoire de destination au lieu de remplacer les fichiers existants. Vous ne pouvez pas utiliser cette option de ligne de commande avec le **/s** ou **/u** option de ligne de commande.|
|/p|Vous demande confirmation avant de remplacer un fichier de destination ou l’ajout d’un fichier source.|
|/r|Remplace les fichiers en lecture seule et non protégées. Si vous essayez de remplacer un fichier en lecture seule, mais vous ne spécifiez pas **/r**, une erreur résulte et arrête l’opération de remplacement.|
|/w|Attend que vous insérez un disque avant le début de la recherche pour les fichiers sources. Si vous ne spécifiez pas **/w**, **remplacer** commence à remplacer ou à ajouter les fichiers immédiatement une fois que vous appuyez sur ENTRÉE.|
|/s|Recherche tous les sous-répertoires du répertoire de destination et remplace les fichiers correspondants. Vous ne pouvez pas utiliser **/s** avec la **/a** option de ligne de commande. Le **remplacer** commande ne recherche pas de sous-répertoires qui sont spécifiés dans *chemin1*.|
|/u|Remplace uniquement les fichiers sur le répertoire de destination qui sont plus anciens que ceux dans le répertoire source. Vous ne pouvez pas utiliser **/u** avec la **/a** option de ligne de commande.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   En tant que **remplacer** ajoute ou remplace des fichiers, le fichier de noms sont affichés dans l’écran. Après avoir **remplacer** est terminé, une ligne de résumé s’affiche dans un des formats suivants :  
    ```
    nnn files added
    nnn files replaced
    no file added
    no file replaced
    ```  
-   Si vous utilisez des disquettes et vous devez basculer les disques pendant la **remplacer** opération, vous pouvez spécifier le **/w** option de ligne de commande afin que **remplacer** attendra pour vous Basculer les disques.
-   Vous ne pouvez pas utiliser **remplacer** pour mettre à jour des fichiers cachés ou les fichiers système.
-   Le tableau suivant présente chaque code de sortie et une brève description de sa signification :  
    |Code de sortie|Description|
    |---------|-----------|
    |0|Le **remplacer** commande a été remplacé ou ajouté les fichiers.|
    |1|Le **remplacer** commande a rencontré une version incorrecte de MS-DOS.|
    |2|Le **remplacer** commande n’a pas pu trouver les fichiers sources.|
    |3|Le **remplacer** commande n’a pas pu trouver le chemin d’accès source ou de destination.|
    |5|L’utilisateur n’a pas accès aux fichiers que vous souhaitez remplacer.|
    |8|Il existe de mémoire système insuffisante pour exécuter la commande.|
    |11|L’utilisateur a utilisé une syntaxe incorrecte sur la ligne de commande.|

> [!NOTE]
> Vous pouvez utiliser le paramètre ERRORLEVEL sur le **si** ligne de commande dans un programme de traitement par lots pour traiter des codes de sortie qui sont retournés par **remplacer**.

## <a name="BKMK_examples"></a>Exemples

Pour mettre à jour toutes les versions d’un fichier nommé Tél.CLI (qui apparaissent dans plusieurs répertoires sur le lecteur C), avec la dernière version du fichier Tél.CLI à partir d’une disquette dans le lecteur A, tapez :

`replace a:\phones.cli c:\ /s`

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)