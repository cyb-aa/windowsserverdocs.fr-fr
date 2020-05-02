---
title: rd
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 42e672f6-5bc2-4c16-af25-18e7ed2dd555
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cb169a44f9613b237af71321f9619d9ea93a6912
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722643"
---
# <a name="rd"></a>rd



Supprime un répertoire. Cette commande est identique à la commande **RmDir** .



## <a name="syntax"></a>Syntaxe

```
rd [<Drive>:]<Path> [/s [/q]]
rmdir [<Drive>:]<Path> [/s [/q]]
```

### <a name="parameters"></a>Paramètres

|     Paramètre     |                                                                 Description                                                                  |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| [\<Lecteur> :]<Path> |                      Spécifie l’emplacement et le nom du répertoire que vous souhaitez supprimer. Le *chemin d’accès* est obligatoire.                       |
|        /s         |                     Supprime une arborescence de répertoires (répertoire spécifié et tous ses sous-répertoires, y compris tous les fichiers).                      |
|        /q         | Spécifie le mode silencieux. Ne demande pas de confirmation lors de la suppression d’une arborescence de répertoires. (Notez que **/q** fonctionne uniquement si **/s** est spécifié.) |
|        /?         |                                                     Affiche l'aide à l'invite de commandes.                                                     |

## <a name="remarks"></a>Notes 

-   Vous ne pouvez pas supprimer un répertoire qui contient des fichiers, y compris des fichiers système ou cachés. Si vous tentez de le faire, le message suivant s’affiche :

    `The directory is not empty`

    Utilisez la commande **dir/a** pour répertorier tous les fichiers (y compris les fichiers cachés et système). Utilisez ensuite la commande **Attrib** avec **-h** pour supprimer les attributs de fichier masqués, **-s** pour supprimer les attributs de fichier système ou **-h-s** pour supprimer les attributs de fichier système et masqués. Une fois les attributs masqués et de fichier supprimés, vous pouvez supprimer les fichiers.
-   Si vous insérez une barre oblique\) inverse (au début du *chemin d’accès*, le *chemin d’accès* démarre dans le répertoire racine (quel que soit le répertoire actif).
-   Vous ne pouvez pas utiliser **rd** pour supprimer le répertoire actif. Si vous tentez de supprimer le répertoire actif, le message d’erreur suivant s’affiche :

    `The process cannot access the file because it is being used by another process.`

    Si vous recevez ce message d’erreur, vous devez passer à un autre répertoire (et non à un sous-répertoire du répertoire actif), puis utiliser le **Bureau à distance** (spécifiez le *chemin d’accès* si nécessaire).
-   La commande **rd** , avec des paramètres différents, est disponible à partir de la console de récupération.

## <a name="examples"></a>Exemples

Vous ne pouvez pas supprimer le répertoire dans lequel vous travaillez actuellement. Vous devez passer à un répertoire qui ne se trouve pas dans le répertoire actif. Par exemple, pour passer au répertoire parent, tapez :
```
cd ..
```
Vous pouvez maintenant supprimer en toute sécurité le répertoire souhaité.

Utilisez l’option **/s** pour supprimer une arborescence de répertoires. Par exemple, pour supprimer un répertoire nommé test (et tous ses sous-répertoires et fichiers) du répertoire actif, tapez :
```
rd /s test
```
Pour exécuter l’exemple précédent en mode silencieux, tapez :
```
rd /s /q test
```

> [!CAUTION]
> Lorsque vous exécutez **rd/s** en mode silencieux, la totalité de l’arborescence de répertoires est supprimée sans confirmation. Assurez-vous que les fichiers importants sont déplacés ou sauvegardés avant d’utiliser l’option de ligne de commande **/q** .

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)