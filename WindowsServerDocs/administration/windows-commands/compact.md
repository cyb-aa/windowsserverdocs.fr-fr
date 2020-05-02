---
title: compact
description: Rubrique de référence pour la commande compact, qui affiche ou modifie la compression des fichiers ou des répertoires sur les partitions NTFS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 429b3752-df0a-43a4-a210-df2f3ad03c3b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 52830530fa281025fcfd970b7675b98004e2a918
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82710946"
---
# <a name="compact"></a>compact

Affiche ou modifie la compression des fichiers ou des répertoires sur les partitions NTFS. En cas d’utilisation sans paramètre, **compact** affiche l’état de compression du répertoire actif et des fichiers qu’il contient.

## <a name="syntax"></a>Syntaxe

```
compact [/c | /u] [/s[:<dir>]] [/a] [/i] [/f] [/q] [<filename>[...]]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| /C | Compresse le répertoire ou le fichier spécifié. |
| /U | Décompresse le répertoire ou le fichier spécifié. |
| /s [ :`<dir>`] | Applique la commande **compact** à tous les sous-répertoires du répertoire spécifié (ou du répertoire actif si aucun répertoire n’est spécifié). |
| /a | Affiche les fichiers cachés ou système. |
| /i | Ignore les erreurs. |
| /f | Force la compression ou la décompression du répertoire ou du fichier spécifié. **/f** est utilisé dans le cas d’un fichier partiellement compressé lorsque l’opération a été interrompue par un incident système. Pour forcer la compression du fichier dans son intégralité, utilisez les paramètres **/c** et **/f** et spécifiez le fichier partiellement compressé. |
| /q | Signale uniquement les informations les plus importantes. |
| `<filename>` | Spécifie le fichier ou le répertoire. Vous pouvez utiliser plusieurs noms de fichiers, ainsi que les **&#42;** et **?** caractères génériques. |
| /? | Affiche l'aide à l'invite de commandes. |

#### <a name="remarks"></a>Notes 

- Cette commande est la version de ligne de commande de la fonctionnalité de compression du système de fichiers NTFS. L’état de compression d’un répertoire indique si les fichiers sont automatiquement compressés lorsqu’ils sont ajoutés à l’annuaire. La définition de l’état de compression d’un répertoire ne modifie pas nécessairement l’état de compression des fichiers qui se trouvent déjà dans le répertoire.

- Vous ne pouvez pas utiliser cette commande pour lire, écrire ou monter des volumes compressés à l’aide de DriveSpace ou de DoubleSpace. Vous ne pouvez pas non plus utiliser cette commande pour compresser des partitions FAT (File Allocation Table) ou FAT32.

## <a name="examples"></a>Exemples

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

Pour forcer la compression complète du fichier *rayures. bmp*, qui a été partiellement compressé pendant un incident système, tapez :

```
compact /c /f zebra.bmp
```

Pour supprimer l’attribut compressé du répertoire c:\tmp, sans modifier l’état de compression des fichiers de ce répertoire, tapez :

```
compact /u c:\tmp
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)