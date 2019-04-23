---
title: convert
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 9fcb7b2190c3359b78145a2ef79a28e30639a89c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873160"
---
# <a name="convert"></a>convert



Convertit de fichiers FAT (allocation table) et les volumes FAT32 au système de fichiers NTFS, laissant intact des répertoires et fichiers existants. Les volumes convertis au système de fichiers NTFS ne peut pas être reconverties en FAT ou FAT32.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
convert [<Volume>] /fs:ntfs [/v] [/cvtarea:<FileName>] [/nosecurity] [/x]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Volume>|Spécifie la lettre de lecteur (suivie d’un signe deux-points), point de montage ou nom de volume pour convertir au format NTFS.|
|/fs:ntfs|Obligatoire. Convertit le volume au format NTFS.|
|/v|Exécutions **convertir** en mode documenté, qui affiche tous les messages pendant le processus de conversion.|
|/CvtArea :\<FileName >|Spécifie que la Table de fichiers principale (MFT) et d’autres fichiers de métadonnées NTFS sont écrits dans un fichier d’espace réservé existant contigu. Ce fichier doit être dans le répertoire racine du système de fichiers à convertir. Utilisation de la **/cvtarea** paramètre peut entraîner un système de fichiers moins fragmenté après la conversion. Pour de meilleurs résultats, la taille de ce fichier doit être de 1 Ko multiplié par le nombre de fichiers et répertoires du système de fichiers, bien que le **convertir** utilitaire accepte les fichiers de n’importe quelle taille.</br>Important : Vous devez créer le fichier de l’espace réservé à l’aide de la **fsutil fichier createnew** commande avant d’exécuter **convertir**. **Convertir** ne crée pas de ce fichier pour vous. **Convertir** substitue les métadonnées NTFS. Après la conversion, tout espace inutilisé dans ce fichier est libérée.|
|/NoSecurity|Spécifie que les paramètres de sécurité sur les fichiers et répertoires convertis permettent l’accès à tous les utilisateurs.|
|/x|Démonte le volume, si nécessaire, avant leur conversion. Tous les descripteurs ouverts sur le volume ne sont alors plus valides.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Si **convertir** ne peut pas verrouiller le lecteur (par exemple, que le lecteur est le volume système ou le lecteur en cours), vous disposez de l’option permettant de convertir le lecteur de la prochaine fois que vous redémarrez l’ordinateur. Si vous ne pouvez pas redémarrer immédiatement l’ordinateur pour terminer la conversion, planifiez une heure pour redémarrer l’ordinateur et autoriser le temps supplémentaire requis pour le processus de conversion se termine.
-   Pour les volumes dont il a été converti à partir de FAT ou FAT32 au format NTFS :

    En raison de l’utilisation du disque existant, le MFT est créé dans un autre emplacement que sur un volume formaté à l’origine avec NTFS, par conséquent, les performances de volume peut-être pas aussi bonne qualité que sur les volumes à l’origine formatés avec NTFS. Pour des performances optimales, envisagez de recréer ces volumes et les mettre en forme avec le système de fichiers NTFS.

    Conversion de volume FAT ou FAT32 en NTFS laisse intacts les fichiers, mais le volume peut ne disposent pas des avantages de performances par rapport aux volumes initialement formatés avec NTFS. Par exemple, le MFT peut devenir fragmentée sur les volumes convertis. En outre, sur le volume de démarrage **convertir** s’applique la même sécurité par défaut qui est appliquée pendant l’installation de Windows.

## <a name="BKMK_examples"></a>Exemples

Pour convertir le volume du lecteur E au format NTFS et afficher tous les messages pendant le processus de conversion, tapez :
```
convert e: /fs:ntfs /v
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)