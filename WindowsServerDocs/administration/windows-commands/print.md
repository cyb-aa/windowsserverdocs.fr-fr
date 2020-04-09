---
title: print
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: aa2325d5-a993-4ed3-b996-255165452db8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 36966d8d3beb032ee0dcee50d9bd5bc0111bf4f5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837372"
---
# <a name="print"></a>print



Envoie un fichier texte à une imprimante.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
Print [/d:<PrinterName>] [<Drive>:][<Path>]<FileName>[ ...]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/d :\<nom_imprimante >|Spécifie l’imprimante pour laquelle vous souhaitez imprimer le travail. Pour imprimer sur une imprimante connectée localement, spécifiez le port de votre ordinateur sur lequel l’imprimante est connectée.</br>-Les valeurs valides pour les ports parallèles sont LPT1, LPT2 et LPT3.</br>-Les valeurs valides pour les ports série sont COM1, COM2, COM3 et COM4.</br>Vous pouvez également spécifier une imprimante réseau en utilisant son nom de file d’attente (\\\\*ServerName*\*nom_imprimante *). Si vous ne spécifiez pas d’imprimante, le travail d’impression est envoyé à LPT1 par défaut.|
|> du lecteur de \<:|Spécifie le lecteur logique ou physique dans lequel se trouve le fichier que vous souhaitez imprimer. Ce paramètre n’est pas obligatoire si le fichier que vous souhaitez imprimer se trouve sur le lecteur actif.|
|Chemin de \<>|Spécifie l’emplacement du fichier que vous souhaitez imprimer. Ce paramètre n’est pas obligatoire si le fichier que vous souhaitez imprimer se trouve dans le répertoire actif.|
|\<nom de fichier > [...]|Obligatoire. Spécifie le fichier que vous souhaitez imprimer. Vous pouvez inclure plusieurs fichiers dans une même commande.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Un fichier peut être imprimé en arrière-plan si vous l’envoyez à une imprimante connectée à un port série ou parallèle sur l’ordinateur local.
-   Vous pouvez effectuer de nombreuses tâches de configuration à partir de l’invite de commandes à l’aide de la commande **mode** .

    Pour plus d’informations sur les éléments suivants, consultez [mode](mode.md) :  
    -   Configuration d’une imprimante connectée à un port parallèle
    -   Configuration d’une imprimante connectée à un port série
    -   Affichage de l’état d’une imprimante
    -   Préparation d’une imprimante pour le changement de page de codes

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour envoyer le fichier Report. txt situé dans le répertoire actif à une imprimante connectée à LPT2 sur l’ordinateur local, tapez :
```
print /d:lpt2 report.txt
```
Pour envoyer le fichier Report. txt situé dans le répertoire c:\Accounting à la file d’attente à l’impression Printer1 sur le serveur de \\\\CopyRoom, tapez :
```
print /d:\\copyroom\printer1 c:\accounting\report.txt 
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

[Référence des commandes d’impression](print-command-reference.md)

[Mode](mode.md)