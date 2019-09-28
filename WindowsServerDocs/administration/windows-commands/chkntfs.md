---
title: chkntfs
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 93eca810-8699-4716-8e9d-aecd54f704be
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f940fe81f0e7e01495e071931059b2375b78bb22
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379345"
---
# <a name="chkntfs"></a>chkntfs



Affiche ou modifie la vérification automatique du disque au démarrage de l’ordinateur. Si elle est utilisée sans options, **chkntfs** affiche le système de fichiers du volume spécifié. Si la vérification automatique des fichiers est planifiée pour s’exécuter, **chkntfs** indique si le volume spécifié est modifié ou est planifié pour être vérifié lors du prochain démarrage de l’ordinateur.

> [!NOTE]
> Pour exécuter **chkntfs**, vous devez être membre du groupe administrateurs.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
chkntfs <Volume> [...]
chkntfs [/d]
chkntfs [/t[:<Time>]]
chkntfs [/x <Volume> [...]]
chkntfs [/c <Volume> [...]]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Volume > [...]|Spécifie un ou plusieurs volumes à vérifier au démarrage de l’ordinateur. Les volumes valides incluent des lettres de lecteur (suivies de deux-points), de points de montage ou de noms de volumes.|
|/d|Restaure tous les paramètres par défaut de **chkntfs** , à l’exception de la durée du compte à rebours pour la vérification automatique des fichiers. Par défaut, tous les volumes sont vérifiés au démarrage de l’ordinateur et **chkdsk** s’exécute sur ceux qui sont modifiés.|
|/t [ : \<Time >]|Remplace le délai d’exécution du compte à rebours d’amorçage de Autochk. exe par la durée spécifiée en secondes. Si vous n’entrez pas de temps, **/t** affiche la durée du compte à rebours en cours.|
|/x \<Volume > [...]|Spécifie un ou plusieurs volumes à exclure de la vérification au démarrage de l’ordinateur, même si le volume est marqué comme nécessitant **chkdsk**.|
|/c \<Volume > [...]|Planifie un ou plusieurs volumes à vérifier au démarrage de l’ordinateur et exécute **chkdsk** sur ceux qui ont été modifiés.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="BKMK_examples"></a>Illustre

Pour afficher le type de système de fichiers pour le lecteur C, tapez :
```
chkntfs c:
```
La sortie suivante indique un système de fichiers NTFS :
```
The type of the file system is NTFS.
```

> [!NOTE]
> Si la vérification automatique des fichiers est planifiée pour s’exécuter, une sortie supplémentaire s’affiche, indiquant si le lecteur est impropre ou a été planifié manuellement pour être vérifié lors du prochain démarrage de l’ordinateur.

Pour afficher le compte à rebours d’initiation de Autochk. exe, tapez :
```
chkntfs /t
```
Par exemple, si la durée du compte à rebours est définie sur 10 secondes, la sortie suivante s’affiche :
```
The AUTOCHK initiation countdown time is set to 10 second(s).
```
Pour modifier le délai d’exécution du compte à rebours d’amorçage de Autochk. exe à 30 secondes, tapez :
```
chkntfs /t:30
```

> [!NOTE]
> Bien que vous puissiez définir la valeur zéro du compte à rebours d’amorçage de Autochk. exe, cela vous empêchera d’annuler une vérification de fichier automatique qui peut s’avérer fastidieuse.

L’option de ligne de commande **/x** n’est pas cumulée. Si vous le tapez plusieurs fois, l’entrée la plus récente remplace l’entrée précédente. Pour exclure la vérification de plusieurs volumes, vous devez les répertorier dans une seule commande. Par exemple, pour exclure les volumes D et E, tapez :
```
chkntfs /x d: e:
```
L’option de ligne de commande **/c** est accumulation. Si vous tapez **/c** plusieurs fois, chaque entrée reste. Pour vous assurer que seul un volume particulier est activé, réinitialisez les valeurs par défaut pour effacer toutes les commandes précédentes, exclure tous les volumes de la vérification, puis planifier la vérification automatique des fichiers sur le volume souhaité.

Par exemple, pour planifier la vérification automatique des fichiers sur le volume D, mais pas sur les volumes C ou E, tapez les commandes suivantes dans l’ordre :
```
chkntfs /d
chkntfs /x c: d: e:
chkntfs /c d:
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)