---
title: chkntfs
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 876c0e0c254216ac217aea7d165d5f4e3a7da9b4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861990"
---
# <a name="chkntfs"></a>chkntfs



Affiche ou modifie le démarrage de l’ordinateur de la vérification du disque automatique. Si utilisée sans options, **chkntfs** affiche le système de fichiers du volume spécifié. Si la vérification automatique des fichiers est planifiée pour s’exécuter, **chkntfs** affiche si le volume spécifié a été modifié ou est planifié pour être vérifié au prochain l’ordinateur est démarré.

> [!NOTE]
> Pour exécuter **chkntfs**, vous devez être membre du groupe Administrateurs.

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
|\<Volume > [...]|Spécifie un ou plusieurs volumes pour vérifier le démarrage de l’ordinateur. Les volumes valides incluent les lettres de lecteur (suivies d’un signe deux-points), points ou des noms de volume de montage.|
|/d|Restaure toutes les **chkntfs** paramètres, sauf le compte à rebours pour la vérification automatique des fichiers par défaut. Par défaut, tous les volumes sont vérifiés que lorsque l’ordinateur est démarré, et **chkdsk** s’exécute sur celles qui sont incorrectes.|
|/t [ :\<heure >]|Modifie le compte à rebours Autochk.exe initiation à la quantité de temps exprimé en secondes. Si vous n’entrez pas une heure, **/t** affiche l’heure actuelle de compte à rebours.|
|/x \<volume > [...]|Spécifie un ou plusieurs volumes à exclure de la vérification du démarrage de l’ordinateur, même si le volume est marqué comme nécessitant **chkdsk**.|
|/c \<volume > [...]|Planifie un ou plusieurs volumes à vérifier lors de l’ordinateur est démarré et s’exécute **chkdsk** sur celles qui sont incorrectes.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="BKMK_examples"></a>Exemples

Pour afficher le type de système de fichiers pour le lecteur C, tapez :
```
chkntfs c:
```
La sortie suivante indique un système de fichiers NTFS :
```
The type of the file system is NTFS.
```

> [!NOTE]
> Si la vérification automatique des fichiers est planifiée pour s’exécuter, sortie supplémentaires s’affiche, indiquant si le lecteur a été modifié ou a été manuellement planifiée pour être coché la prochaine fois que l’ordinateur est démarré.

Pour afficher le compte à rebours Autochk.exe initiation, tapez :
```
chkntfs /t
```
Par exemple, si le compte à rebours est définie sur 10 secondes, la sortie suivante s’affiche :
```
The AUTOCHK initiation countdown time is set to 10 second(s).
```
Pour modifier le compte à rebours Autochk.exe initiation à 30 secondes, tapez :
```
chkntfs /t:30
```

> [!NOTE]
> Bien que vous pouvez définir le compte à rebours Autochk.exe initiation à zéro, cela donc vous empêchera d’annulation d’une vérification automatique des fichiers potentiellement longue.

Le **/x** option de ligne de commande n’est pas être cumulée. Si vous tapez plusieurs fois, l’entrée la plus récente remplace l’entrée précédente. Pour exclure plusieurs volumes en cours de vérification, vous devez répertorier chacun d’eux dans une seule commande. Par exemple, pour exclure les volumes D et E, tapez :
```
chkntfs /x d: e:
```
Le **/c** option de ligne de commande ne peut être cumulée. Si vous tapez **/c** plusieurs fois, chaque entrée reste. Pour vous assurer que seul un volume particulier est coché, réinitialiser les valeurs par défaut pour effacer toutes les commandes précédentes, excluez tous les volumes de la vérification et ensuite planifier la vérification sur le volume de votre choix automatique des fichiers.

Par exemple, pour planifier la vérification sur le volume D, mais pas les volumes de C ou E automatique des fichiers, tapez les commandes suivantes dans l’ordre :
```
chkntfs /d
chkntfs /x c: d: e:
chkntfs /c d:
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)