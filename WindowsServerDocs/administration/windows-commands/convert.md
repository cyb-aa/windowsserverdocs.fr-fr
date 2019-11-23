---
title: convert
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 96e437c0-1aa3-46ab-9078-a7b8cdaf3792
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c1e22f67768bbe2f37f3627ca69b162cae96f2d4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379078"
---
# <a name="convert"></a>convert



Convertit les volumes FAT (File Allocation Table) et FAT32 dans le système de fichiers NTFS, en laissant intacts les fichiers et les répertoires existants. Les volumes convertis dans le système de fichiers NTFS ne peuvent pas être reconvertis en FAT ou FAT32.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
convert [<Volume>] /fs:ntfs [/v] [/cvtarea:<FileName>] [/nosecurity] [/x]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|> du volume \<|Spécifie la lettre de lecteur (suivie d’un signe deux-points), d’un point de montage ou d’un nom de volume à convertir au format NTFS.|
|/FS : NTFS|Obligatoire. Convertit le volume au format NTFS.|
|/v|Exécute **Convert** en mode détaillé, qui affiche tous les messages pendant le processus de conversion.|
|/CvtArea :\<nom de fichier >|Spécifie que la table de fichiers maîtres (MFT) et d’autres fichiers de métadonnées NTFS sont écrits dans un fichier d’espace réservé existant et contigu. Ce fichier doit se trouver dans le répertoire racine du système de fichiers à convertir. L’utilisation du paramètre **/Cvtarea** peut entraîner un système de fichiers moins fragmenté après la conversion. Pour de meilleurs résultats, la taille de ce fichier doit être de 1 Ko multiplié par le nombre de fichiers et de répertoires dans le système de fichiers, même si l’utilitaire **Convert** accepte des fichiers de n’importe quelle taille.</br>Important : vous devez créer le fichier d’espace réservé à l’aide de la commande **fsutil file CreateNew** avant d’exécuter **Convert**. **Convert** ne crée pas ce fichier pour vous. **Convert** remplace ce fichier par des métadonnées NTFS. Après la conversion, tout espace inutilisé dans ce fichier est libéré.|
|/nosecurity|Spécifie que les paramètres de sécurité des fichiers et répertoires convertis autorisent l’accès de tous les utilisateurs.|
|/x|Démonte le volume, si nécessaire, avant qu’il ne soit converti. Tous les descripteurs ouverts sur le volume ne sont alors plus valides.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Si **Convert** ne peut pas verrouiller le lecteur (par exemple, le lecteur est le volume système ou le lecteur actif), vous avez la possibilité de convertir le lecteur la prochaine fois que vous redémarrez l’ordinateur. Si vous ne pouvez pas redémarrer l’ordinateur immédiatement pour terminer la conversion, planifiez une heure de redémarrage de l’ordinateur et attendez que le processus de conversion se termine.
-   Pour les volumes convertis à partir de FAT ou FAT32 vers NTFS :

    En raison de l’utilisation des disques existants, la table MFT est créée à un autre emplacement que sur un volume à l’origine formaté avec NTFS. les performances du volume peuvent donc ne pas être aussi bonnes que sur les volumes au format NTFS. Pour des performances optimales, envisagez de recréer ces volumes et de les mettre en forme avec le système de fichiers NTFS.

    La conversion de volume FAT ou FAT32 en NTFS laisse les fichiers intacts, mais le volume peut manquer d’avantages en matière de performances par rapport aux volumes initialement formatés avec NTFS. Par exemple, la table MFT peut devenir fragmentée sur des volumes convertis. En outre, sur les volumes de démarrage convertis, **Convert** applique la même sécurité par défaut que celle appliquée pendant installation de Windows.

## <a name="BKMK_examples"></a>Illustre

Pour convertir le volume sur le lecteur E en NTFS et afficher tous les messages pendant le processus de conversion, tapez :
```
convert e: /fs:ntfs /v
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)