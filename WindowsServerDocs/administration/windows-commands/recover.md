---
title: recover
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cf9be2e3-90c8-4773-a201-dc503b91948e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b316ed26f008a62f88aaeb4a7a7f3030d08f1588
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722625"
---
# <a name="recover"></a>recover



Récupère des informations lisibles à partir d’un disque défectueux ou défectueux.



## <a name="syntax"></a>Syntaxe

```
recover [<Drive>:][<Path>]<FileName>
```

### <a name="parameters"></a>Paramètres

|           Paramètre           |                                          Description                                          |
|-------------------------------|-----------------------------------------------------------------------------------------------|
| [\<Lecteur> :] [<Path>]<FileName> | Spécifie l’emplacement et le nom du fichier que vous souhaitez récupérer. Le *nom de fichier* est obligatoire. |
|              /?               |                             Affiche l'aide à l'invite de commandes.                              |

## <a name="remarks"></a>Notes 

-   La commande de **récupération** lit un fichier, secteur par secteur, et récupère les données à partir des bons secteurs. Les données des secteurs incorrects sont perdues.
-   Les secteurs incorrects signalés par **chkdsk** ont été marqués comme étant incorrects lorsque votre disque a été préparé pour fonctionner. Ils ne présentent aucun danger, et la **récupération** ne les affecte pas.
-   Étant donné que toutes les données des secteurs incorrects sont perdues lorsque vous récupérez un fichier, vous ne devez récupérer qu’un seul fichier à la fois.
-   Vous ne pouvez pas utiliser de caractères génériques (**&#42;** et **?**) avec la commande **recover** . Vous devez spécifier un fichier (et l’emplacement du fichier s’il ne se trouve pas dans le répertoire actif).

## <a name="examples"></a>Exemples

Pour récupérer le fichier Story. txt dans le répertoire \Fiction sur le lecteur D, tapez :
```
recover d:\fiction\story.txt 
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)