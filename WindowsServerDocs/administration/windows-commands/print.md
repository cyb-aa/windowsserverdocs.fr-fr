---
title: print
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: ada0657e2f17754e55e97e6488aac99fb0025afb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372152"
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
|/d : \<PrinterName >|Spécifie l’imprimante pour laquelle vous souhaitez imprimer le travail. Pour imprimer sur une imprimante connectée localement, spécifiez le port de votre ordinateur sur lequel l’imprimante est connectée.</br>-Les valeurs valides pour les ports parallèles sont LPT1, LPT2 et LPT3.</br>-Les valeurs valides pour les ports série sont COM1, COM2, COM3 et COM4.</br>Vous pouvez également spécifier une imprimante réseau en utilisant son nom de file d’attente (\\ @ no__t-1*ServerName*\*PrinterName *). Si vous ne spécifiez pas d’imprimante, le travail d’impression est envoyé à LPT1 par défaut.|
|> @no__t 0Drive :|Spécifie le lecteur logique ou physique dans lequel se trouve le fichier que vous souhaitez imprimer. Ce paramètre n’est pas obligatoire si le fichier que vous souhaitez imprimer se trouve sur le lecteur actif.|
|@no__t 0Path >|Spécifie l’emplacement du fichier que vous souhaitez imprimer. Ce paramètre n’est pas obligatoire si le fichier que vous souhaitez imprimer se trouve dans le répertoire actif.|
|\<FileName > [...]|Obligatoire. Spécifie le fichier que vous souhaitez imprimer. Vous pouvez inclure plusieurs fichiers dans une même commande.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Un fichier peut être imprimé en arrière-plan si vous l’envoyez à une imprimante connectée à un port série ou parallèle sur l’ordinateur local.
-   Vous pouvez effectuer de nombreuses tâches de configuration à partir de l’invite de commandes à l’aide de la commande **mode** .

    Pour plus d’informations sur les éléments suivants, consultez [mode](mode.md) :  
    -   Configuration d’une imprimante connectée à un port parallèle
    -   Configuration d’une imprimante connectée à un port série
    -   Affichage de l’état d’une imprimante
    -   Préparation d’une imprimante pour le changement de page de codes

## <a name="BKMK_examples"></a>Illustre

Pour envoyer le fichier Report. txt situé dans le répertoire actif à une imprimante connectée à LPT2 sur l’ordinateur local, tapez :
```
print /d:lpt2 report.txt
```
Pour envoyer le fichier Report. txt situé dans le répertoire c:\Accounting à la file d’attente à l’impression Printer1 sur le serveur \\ @ no__t-1CopyRoom, tapez :
```
print /d:\\copyroom\printer1 c:\accounting\report.txt 
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

[Référence des commandes d’impression](print-command-reference.md)

[Mode](mode.md)