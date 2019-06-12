---
title: mmc
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7bfa4030-ce42-40fb-922f-2f5145a80872
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4bdc093bd16ea08153b7dbc4a0e3380251f2ed7d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437330"
---
# <a name="mmc"></a>mmc

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

À l’aide des options de ligne de commande mmc, vous pouvez ouvrir un spécifique **mmc** console, ouvrez **mmc** en mode auteur, ou pour spécifier que la version 32 bits ou 64 bits de **mmc** est ouvert.
## <a name="syntax"></a>Syntaxe
```
mmc <path>\<filename>.msc [/a] [/64] [/32]
```
### <a name="parameters"></a>Paramètres

|       Paramètre        |                                                                                                 Description                                                                                                 |
|------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <path>\\<filename>.msc |        démarre **mmc** et ouvre une console enregistrée. Vous devez spécifier le chemin d’accès et le nom complet pour le fichier de console enregistrée. Si vous ne spécifiez pas un fichier de console, **mmc** ouvre une nouvelle console.         |
|           /a           |                                                               Ouvre une console enregistrée en mode auteur.  Permet d’apporter des modifications aux consoles enregistrées.                                                                |
|          /64           |                         Ouvre la version 64 bits de **mmc** (mmc64). Utilisez cette option uniquement si vous exécutez un système d’exploitation Microsoft 64 bits et que vous souhaitez utiliser un composant logiciel enfichable 64 bits.                          |
|          /32           | Ouvre la version 32 bits de **mmc** (mmc32). Lorsque vous exécutez un système d’exploitation Microsoft 64 bits, vous pouvez exécuter des composants logiciels enfichables 32 bits en ouvrant de mmc avec cette option de ligne de commande lorsque vous disposez uniquement de composants logiciels enfichables 32 bits. |
|           /?           |                                                                                    Affiche l'aide à l'invite de commandes.                                                                                     |

## <a name="remarks"></a>Notes
- À l’aide de la <path> **\\** <filename> **.msc** option de ligne de commande, vous pouvez utiliser des variables d’environnement pour créer des lignes de commande ou des raccourcis qui ne dépendent pas explicites emplacement des fichiers de console. Par exemple, si le chemin d’accès à un fichier de console est dans le dossier système (par exemple, **mmc c:\winnt\system32\console_name.msc**), vous pouvez utiliser la chaîne de données extensible **%SystemRoot%** pour spécifier l’emplacement (**mmc%systemroot%\system32\console_name.msc**). Cela peut être utile si vous déléguez des tâches aux personnes de votre organisation qui travaillent sur des ordinateurs différents.
- À l’aide de la **/a** option de ligne de commande lors de l’ouverture des consoles avec cette option, ils sont ouverts en mode auteur, quel que soit leur mode par défaut. Cela ne modifie pas définitivement le paramètre de mode par défaut pour les fichiers ; Lorsque vous omettez cette option, mmc ouvre les fichiers de console en fonction de leurs paramètres de mode par défaut.
- Après avoir ouvert **mmc** ou un fichier de console en mode auteur, vous pouvez ouvrir n’importe quelle console existante en cliquant sur **ouvrir** sur le **Console** menu.
- Vous pouvez utiliser la ligne de commande pour créer des raccourcis pour l’ouverture **mmc** et les consoles enregistrées. Une ligne de commande fonctionne avec les **exécuter** commande sur le **Démarrer** menu, dans n’importe quelle fenêtre d’invite de commandes, dans les raccourcis ou dans n’importe quel fichier de commandes ou d’un programme qui appelle la commande.
  ## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

