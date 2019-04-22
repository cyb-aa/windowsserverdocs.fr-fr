---
title: compact
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 429b3752-df0a-43a4-a210-df2f3ad03c3b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 068111b293a3eb3987b14744a1bfcf2fde26bced
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826030"
---
# <a name="compact"></a>compact



Affiche ou modifie la compression des fichiers ou répertoires sur des partitions NTFS. Si utilisée sans paramètres, **compact** affiche l’état de compression des fichiers qu’il contient et le répertoire actif.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
compact [/c | /u] [/s[:<Dir>]] [/a] [/i] [/f] [/q] [<FileName>[...]]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/c|Compresse le fichier ou répertoire spécifié.|
|/u|Décompresse le fichier ou répertoire spécifié.|
|/s [ :\<Dir >]|S’applique le **compact** commande à tous les sous-répertoires du répertoire spécifié (ou du répertoire actif si aucun n’est spécifié).|
|/a|Affiche masqués ou les fichiers système.|
|/i|Ignore les erreurs.|
|/f|Force la compression ou la décompression du répertoire spécifié ou du fichier. **/f** est utilisé dans le cas d’un fichier qui a été partiellement compressé lors de l’opération a été interrompue par une panne du système. Pour forcer le fichier doit être compressé dans son intégralité, utilisez le **/c** et **/f** paramètres et spécifiez le fichier compressé partiellement.|
|/q|Signale uniquement les informations essentielles.|
|\<FileName>|Spécifie le fichier ou répertoire. Vous pouvez utiliser plusieurs noms de fichiers et le **&#42;** et **?** caractères génériques.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Le **compact** commande est la version de ligne de commande de la fonctionnalité de compression du système de fichiers NTFS. L’état de compression d’un répertoire indique si les fichiers sont compressés automatiquement lorsqu’ils sont ajoutés au répertoire. Définition de l’état de compression d’un répertoire ne change pas nécessairement l’état de compression des fichiers qui se trouvent déjà dans le répertoire.
-   Vous ne pouvez pas utiliser **compact** en lecture, écriture ou montage de volumes qui ont été compressés à l’aide de DriveSpace ou DoubleSpace.
-   Vous ne pouvez pas utiliser **compact** pour compresser la table d’allocation de fichiers (FAT) ou des partitions FAT32.

## <a name="BKMK_examples"></a>Exemples

Pour définir l’état de compression du répertoire actif, ses sous-répertoires et fichiers existants, tapez :
```
compact /c /s 
```
Pour définir l’état de compression de fichiers et sous-répertoires dans le répertoire actif, sans en altérer l’état de compression du répertoire actif lui-même, tapez :
```
compact /c /s *.*
```
Pour compresser un volume, à partir du répertoire racine du volume, tapez :
```
compact /c /i /s:\
```

> [!NOTE]
> Cet exemple définit l’état de compression de tous les répertoires (y compris le répertoire racine sur le volume) et compresse chaque fichier sur le volume. Le **/i** paramètre empêche les messages d’erreur d’interrompre le processus de compression.

Pour compresser tous les fichiers portant l’extension de nom de fichier .bmp dans le répertoire \Tmp et tous les sous-répertoires de \Tmp, sans modifier l’attribut de compression des répertoires, tapez :
```
compact /c /s:\tmp *.bmp
```
Pour forcer la compression du fichier Zèbre.bmp qui avait été lors d’un incident système, tapez :
```
compact /c /f zebra.bmp
```
Pour supprimer l’attribut compressé à partir du répertoire C:\Tmp, sans modifier l’état de compression de tous les fichiers dans ce répertoire, tapez :
```
compact /u c:\tmp
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)