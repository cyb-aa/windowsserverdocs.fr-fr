---
title: rd
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 42e672f6-5bc2-4c16-af25-18e7ed2dd555
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 94231e3ec032280beb91a14db7949a1296c2d811
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442030"
---
# <a name="rd"></a>rd



Supprime un répertoire. Cette commande est identique à la **rmdir** commande.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
rd [<Drive>:]<Path> [/s [/q]]
rmdir [<Drive>:]<Path> [/s [/q]]
```

## <a name="parameters"></a>Paramètres

|     Paramètre     |                                                                 Description                                                                  |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| [\<Drive>:]<Path> |                      Spécifie l’emplacement et le nom du répertoire que vous souhaitez supprimer. *Chemin d’accès* est requis.                       |
|        /s         |                     Supprime une arborescence de répertoires (le répertoire spécifié et tous ses sous-répertoires, y compris tous les fichiers).                      |
|        /q         | Spécifie le mode silencieux. Ne demande pas de confirmation lors de la suppression d’une arborescence de répertoires. (Notez que **/q** fonctionne uniquement si **/s** est spécifié.) |
|        /?         |                                                     Affiche l'aide à l'invite de commandes.                                                     |

## <a name="remarks"></a>Notes

-   Vous ne pouvez pas supprimer un répertoire qui contient les fichiers, y compris masqué ou les fichiers système. Si vous tentez de le faire, le message suivant apparaît :

    `The directory is not empty`

    Utilisez le **dir /a** commande pour répertorier tous les fichiers (y compris masqués et les fichiers système). Puis utiliser le **attrib** avec **-h** pour supprimer les attributs de fichier masqué, **-s** pour supprimer les attributs de fichier système ou **-h -s** à supprimer les deux masqués et les attributs de fichier système. Après le texte masqué et les attributs de fichier ont été supprimées, vous pouvez supprimer les fichiers.
-   Si vous insérez une barre oblique inverse (\) au début de *chemin d’accès*, *chemin d’accès* démarre dans le répertoire racine (quel que soit le répertoire actif).
-   Vous ne pouvez pas utiliser **Bureau à distance** pour supprimer le répertoire actif. Si vous tentez de supprimer le répertoire actif, le message d’erreur suivant s’affiche :

    `The process cannot access the file because it is being used by another process.`

    Si vous recevez ce message d’erreur, vous devez modifier dans un autre répertoire (pas un sous-répertoire du répertoire actif) et ensuite utiliser **Bureau à distance** (spécifiez *chemin d’accès* si nécessaire).
-   Le **Bureau à distance** commande, avec des paramètres différents, est disponible à partir de la Console de récupération.

## <a name="BKMK_examples"></a>Exemples

Vous ne pouvez pas supprimer le répertoire dans lequel vous travaillez actuellement dans. Vous devez modifier dans un répertoire qui n’est pas dans le répertoire actif. Par exemple, pour changer le répertoire parent, tapez :
```
cd ..
```
Vous pouvez supprimer maintenant en toute sécurité le répertoire souhaité.

Utilisez le **/s** option pour supprimer une arborescence de répertoires. Par exemple, pour supprimer un répertoire nommé Test (et tous ses sous-répertoires et fichiers) depuis le répertoire actif, type :
```
rd /s test
```
Pour exécuter l’exemple précédent en mode silencieux, tapez :
```
rd /s /q test
```

> [!CAUTION]
> Lorsque vous exécutez **/s du Bureau à distance** en mode silencieux, l’arborescence entière est supprimée sans confirmation. Assurez-vous que les fichiers importants sont déplacés ou sauvegardées avant d’utiliser le **/q** option de ligne de commande.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)