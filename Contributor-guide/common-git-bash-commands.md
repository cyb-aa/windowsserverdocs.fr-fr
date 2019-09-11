---
title: Commandes courantes de l’interpréteur git pour une utilisation avec GitHub
description: Liste de quelques-unes des commandes les plus fréquemment utilisées dans Git bash lors de l’utilisation de GitHub.
author: eross-msft
ms.author: lizross
ms.date: 05/06/2019
ms.openlocfilehash: 4ce5d4d8ce382e9ba421c20595715ec473cca241
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70864996"
---
# <a name="common-git-bash-commands"></a>Commandes courantes de git bash

Voici quelques-unes des commandes les plus utilisées dans Git bash, en fonction du moment où vous les utiliserez dans votre processus de création et de modification de contenu.

## <a name="master-branch-related"></a>Principale liée à la branche

Vous devez toujours utiliser le maître comme base pour toute nouvelle branche.

| Command | Description |
|---------|-------------|
| `git checkout master` | Basculer vers Master à partir d’une autre branche |
| `git pull upstream master` | Mettre à jour votre copie locale de Master à partir du référentiel de production |

## <a name="branch-related"></a>Lié à la branche

| Command | Description |
|---------|-------------|
| `git branch` | Voir vos branches existantes |
| `git checkout -B <name-of-branch>` | Créer une branche |
| `git checkout <name-of-branch>` | Basculer vers une autre branche |
| `git status` | Vérifier ce qui se passe dans votre branche |
| `git branch -D <name-of-branch>` | Supprimer une branche existante (en vous assurant qu’elle ne s’y trouve pas) |

## <a name="check-in-related-done-as-a-group-in-order"></a>En relation avec l’archivage (effectué en tant que groupe, dans l’ordre)

| Command | Description |
|---------|-------------|
| `git add --all` | Après avoir enregistré votre travail, ajoutez-le à une branche |
| `git commit -m “public comment, including quotes”` | Valider vos modifications dans votre branche |
| `git pull upstream master` | Mettre à jour votre copie locale de Master à partir du référentiel de production |
| `git push origin <name-of-branch>` | Envoyer vers la version distante de votre branche locale |