---
title: Commandes Git Bash courantes pour une utilisation avec GitHub
description: Une liste de certaines commandes fréquemment utilisées dans Git Bash lorsque vous travaillez avec GitHub.
author: eross-msft
ms.author: lizross
ms.date: 05/06/2019
ms.openlocfilehash: 210acaf2b7911892bcfd81b6bbe1975f141308a1
ms.sourcegitcommit: 7e54a1bcd31cd2c6b18fd1f21b03f5cfb6165bf3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65461689"
---
# <a name="common-git-bash-commands"></a>Commandes Git Bash courantes

Voici certaines des commandes plus utilisées dans Git Bash, selon un lorsque vous les utiliserez dans la création de votre contenue et la modification des processus.

## <a name="master-branch-related"></a>Maître liés à la branche

Vous devez toujours utiliser master comme base pour n’importe quelle nouvelle branche.

| Command | Description |
|---------|-------------|
| `git checkout master` | Basculer vers master à partir de n’importe quelle autre branche |
| `git pull upstream master` | Mettre à jour votre copie locale du serveur principal à partir du dépôt de production |

## <a name="branch-related"></a>Liés à la branche

| Command | Description |
|---------|-------------|
| `git branch` | Afficher vos branches existantes |
| `git checkout -B <name-of-branch>` | Créer une branche |
| `git checkout <name-of-branch>` | Remplacez par une autre branche |
| `git status` | Vérifiez ce qui se passe dans votre branche |
| `git branch -D <name-of-branch>` | Supprimer une branche existante (assurant que vous n’êtes pas qu’il contient) |

## <a name="check-in-related-done-as-a-group-in-order"></a>Vérification dans liés (effectuée en tant que groupe, dans l’ordre)

| Command | Description |
|---------|-------------|
| `git add --all` | Une fois que vous enregistrez votre travail, ajoutez-le à une branche |
| `git commit -m “public comment, including quotes”` | Validez vos modifications dans votre branche |
| `git pull upstream master` | Mettre à jour votre copie locale du serveur principal à partir du dépôt de production |
| `git push origin <name-of-branch>` | Push vers la version à distance de votre branche locale |