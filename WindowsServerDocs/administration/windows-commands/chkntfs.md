---
title: chkntfs
description: Rubrique de référence pour la commande chkntfs, qui affiche ou modifie la vérification automatique du disque au démarrage de l’ordinateur.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 93eca810-8699-4716-8e9d-aecd54f704be
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 69b8e21a7b43538b6296666d813f2b33daa8045f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82713656"
---
# <a name="chkntfs"></a>chkntfs

Affiche ou modifie la vérification automatique du disque au démarrage de l’ordinateur. Si elle est utilisée sans options, **chkntfs** affiche le système de fichiers du volume spécifié. Si la vérification automatique des fichiers est planifiée pour s’exécuter, **chkntfs** indique si le volume spécifié est modifié ou est planifié pour être vérifié lors du prochain démarrage de l’ordinateur.

> [!NOTE]
> Pour exécuter **chkntfs**, vous devez être membre du groupe administrateurs.

## <a name="syntax"></a>Syntaxe

```
chkntfs <volume> [...]
chkntfs [/d]
chkntfs [/t[:<time>]]
chkntfs [/x <volume> [...]]
chkntfs [/c <volume> [...]]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| `<volume>` [...] | Spécifie un ou plusieurs volumes à vérifier au démarrage de l’ordinateur. Les volumes valides incluent des lettres de lecteur (suivies de deux-points), de points de montage ou de noms de volumes. |
| /d | Restaure tous les paramètres par défaut de **chkntfs** , à l’exception de la durée du compte à rebours pour la vérification automatique des fichiers. Par défaut, tous les volumes sont vérifiés au démarrage de l’ordinateur et **chkdsk** s’exécute sur ceux qui sont modifiés. |
| /t [`:<time>`] | Remplace le délai d’exécution du compte à rebours d’amorçage de Autochk. exe par la durée spécifiée en secondes. Si vous n’entrez pas de temps, **/t** affiche la durée du compte à rebours en cours. |
| /x `<volume>` [...] | Spécifie un ou plusieurs volumes à exclure de la vérification au démarrage de l’ordinateur, même si le volume est marqué comme nécessitant **chkdsk**. |
| /c `<volume>` [...] | Planifie un ou plusieurs volumes à vérifier au démarrage de l’ordinateur et exécute **chkdsk** sur ceux qui ont été modifiés. |
| /? | Affiche l'aide à l'invite de commandes. |

## <a name="examples"></a>Exemples

Pour afficher le type de système de fichiers pour le lecteur C, tapez :

```
chkntfs c:
```

> [!NOTE]
> Si la vérification automatique des fichiers est planifiée pour s’exécuter, une sortie supplémentaire s’affiche, indiquant si le lecteur est impropre ou a été planifié manuellement pour être vérifié lors du prochain démarrage de l’ordinateur.

Pour afficher le compte à rebours d’initiation de Autochk. exe, tapez :

```
chkntfs /t
```

Pour modifier le délai d’exécution du compte à rebours d’amorçage de Autochk. exe à 30 secondes, tapez :

```
chkntfs /t:30
```

> [!NOTE]
> Bien que vous puissiez définir la valeur zéro du compte à rebours d’amorçage de Autochk. exe, cela vous empêchera d’annuler une vérification de fichier automatique qui peut s’avérer fastidieuse.

Pour exclure la vérification de plusieurs volumes, vous devez les répertorier dans une seule commande. Par exemple, pour exclure les volumes D et E, tapez :

```
chkntfs /x d: e:
```

> [!IMPORTANT]
> L’option de ligne de commande **/x** n’est pas cumulée. Si vous le tapez plusieurs fois, l’entrée la plus récente remplace l’entrée précédente.

Pour planifier la vérification automatique des fichiers sur le volume D, mais pas sur les volumes C ou E, tapez les commandes suivantes dans l’ordre :

```
chkntfs /d
chkntfs /x c: d: e:
chkntfs /c d:
```

> [!IMPORTANT]
> L’option de ligne de commande **/c** est accumulation. Si vous tapez **/c** plusieurs fois, chaque entrée reste. Pour vous assurer que seul un volume particulier est activé, réinitialisez les valeurs par défaut pour effacer toutes les commandes précédentes, exclure tous les volumes de la vérification, puis planifier la vérification automatique des fichiers sur le volume souhaité.

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
