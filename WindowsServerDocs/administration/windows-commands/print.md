---
title: print
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aa2325d5-a993-4ed3-b996-255165452db8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d85fc5b2cd5f5ba09ebdf4756a5adb60c1759f2a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831550"
---
# <a name="print"></a>print



Envoie un fichier texte à une imprimante.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
Print [/d:<PrinterName>] [<Drive>:][<Path>]<FileName>[ ...]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/ d:\<nom_imprimante >|Spécifie l’imprimante que vous souhaitez le travail d’impression. Pour imprimer vers une imprimante connectée en local, spécifiez le port sur votre ordinateur où l’imprimante est connectée.</br>-Les valeurs valides pour les ports parallèles sont LPT1, LPT2 et LPT3.</br>-Les valeurs valides pour les ports série sont COM1, COM2, COM3 et COM4.</br>Vous pouvez également spécifier une imprimante réseau à l’aide de son nom de file d’attente (\\\\*nom_serveur*\*nom_imprimante *). Si vous ne spécifiez pas une imprimante, le travail d’impression est envoyé à LPT1 par défaut.|
|\<Lecteur > :|Spécifie le lecteur logique ou physique où se trouve le fichier que vous souhaitez imprimer. Ce paramètre n’est pas obligatoire si le fichier que vous souhaitez imprimer se trouve sur le lecteur actif.|
|\<Path>|Spécifie l’emplacement du fichier que vous souhaitez imprimer. Ce paramètre n’est pas obligatoire si le fichier que vous souhaitez imprimer se trouve dans le répertoire actif.|
|\<FileName>[ ...]|Obligatoire. Spécifie le fichier que vous souhaitez imprimer. Vous pouvez inclure plusieurs fichiers en une seule commande.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Un fichier peut imprimer en arrière-plan si vous l’envoyez à une imprimante connectée à un port série ou parallèle sur l’ordinateur local.
-   Vous pouvez effectuer de nombreuses tâches de configuration à partir de l’invite de commandes à l’aide de la **Mode** commande.

    Consultez [Mode](mode.md) pour plus d’informations sur :  
    -   Configuration d’une imprimante connectée à un port parallèle
    -   Configuration d’une imprimante connectée à un port série
    -   Affichage de l’état d’une imprimante
    -   Préparation d’une imprimante pour la commutation de page de code

## <a name="BKMK_examples"></a>Exemples

Pour envoyer le fichier rapport.txt situé dans le répertoire actif vers une imprimante connectée au port LPT2 sur l’ordinateur local, tapez :
```
print /d:lpt2 report.txt
```
Pour envoyer le fichier rapport.txt situé dans le répertoire c:\Accounting vers la file d’impression Imprimante1 le \\ \\CopyRoom serveur, tapez :
```
print /d:\\copyroom\printer1 c:\accounting\report.txt 
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)

[Référence des commandes d’impression](print-command-reference.md)

[Mode](mode.md)