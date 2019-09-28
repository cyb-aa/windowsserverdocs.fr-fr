---
title: compact
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 5e73369d69912437875a0151b1d9cfcfc85a30da
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379178"
---
# <a name="compact"></a>compact



Affiche ou modifie la compression des fichiers ou des répertoires sur les partitions NTFS. En cas d’utilisation sans paramètre, **compact** affiche l’état de compression du répertoire actif et des fichiers qu’il contient.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
compact [/c | /u] [/s[:<Dir>]] [/a] [/i] [/f] [/q] [<FileName>[...]]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/c|Compresse le répertoire ou le fichier spécifié.|
|/u.|Décompresse le répertoire ou le fichier spécifié.|
|/s [ : \<Dir >]|Applique la commande **compact** à tous les sous-répertoires du répertoire spécifié (ou du répertoire actif si aucun répertoire n’est spécifié).|
|/a|Affiche les fichiers cachés ou système.|
|/i|Ignore les erreurs.|
|/f|Force la compression ou la décompression du répertoire ou du fichier spécifié. **/f** est utilisé dans le cas d’un fichier partiellement compressé lorsque l’opération a été interrompue par un incident système. Pour forcer la compression du fichier dans son intégralité, utilisez les paramètres **/c** et **/f** et spécifiez le fichier partiellement compressé.|
|/q|Signale uniquement les informations les plus importantes.|
|\<Nom de fichier >|Spécifie le fichier ou le répertoire. Vous pouvez utiliser plusieurs noms de fichiers, ainsi **&#42;** que et **?** caractères génériques.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   La commande **compact** est la version de ligne de commande de la fonctionnalité de compression du système de fichiers NTFS. L’état de compression d’un répertoire indique si les fichiers sont automatiquement compressés lorsqu’ils sont ajoutés à l’annuaire. La définition de l’état de compression d’un répertoire ne modifie pas nécessairement l’état de compression des fichiers qui se trouvent déjà dans le répertoire.
-   Vous ne pouvez pas utiliser **compact** pour lire, écrire ou monter des volumes qui ont été compressés à l’aide de DriveSpace ou de DoubleSpace.
-   Vous ne pouvez pas utiliser **compact** pour compresser des partitions FAT (File Allocation Table) ou FAT32.

## <a name="BKMK_examples"></a>Illustre

Pour définir l’état de compression du répertoire actif, de ses sous-répertoires et des fichiers existants, tapez :
```
compact /c /s 
```
Pour définir l’état de compression des fichiers et des sous-répertoires dans le répertoire actif, sans modifier l’état de compression du répertoire actif, tapez :
```
compact /c /s *.*
```
Pour compresser un volume, à partir du répertoire racine du volume, tapez :
```
compact /c /i /s:\
```

> [!NOTE]
> Cet exemple définit l’état de compression de tous les répertoires (y compris le répertoire racine sur le volume) et compresse chaque fichier sur le volume. Le paramètre **/i** empêche les messages d’erreur d’interrompre le processus de compression.

Pour compresser tous les fichiers avec l’extension de nom de fichier. bmp dans le répertoire \tmp et dans tous les sous-répertoires de \tmp, sans modifier l’attribut compressé des répertoires, tapez :
```
compact /c /s:\tmp *.bmp
```
Pour forcer la compression complète du fichier rayures. bmp, qui a été partiellement compressé pendant un incident système, tapez :
```
compact /c /f zebra.bmp
```
Pour supprimer l’attribut compressé du répertoire C:\Tmp, sans modifier l’état de compression des fichiers de ce répertoire, tapez :
```
compact /u c:\tmp
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)