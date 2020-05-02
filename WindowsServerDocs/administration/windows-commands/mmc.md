---
title: mmc
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7bfa4030-ce42-40fb-922f-2f5145a80872
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c168a9f4d91422f3877fd0c210c76248d1172b00
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723964"
---
# <a name="mmc"></a>mmc

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

À l’aide des options de ligne de commande MMC, vous pouvez ouvrir une console **MMC** spécifique, ouvrir la console **MMC** en mode auteur, ou spécifier que la version 32 bits ou 64 bits de **MMC** est ouverte.
## <a name="syntax"></a>Syntaxe
```
mmc <path>\<filename>.msc [/a] [/64] [/32]
```
#### <a name="parameters"></a>Paramètres

|       Paramètre        |                                                                                                 Description                                                                                                 |
|------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <path>\\<filename>. msc |        démarre **MMC** et ouvre une console enregistrée. Vous devez spécifier le chemin d’accès complet et le nom de fichier pour le fichier de console enregistré. Si vous ne spécifiez pas de fichier de console, **MMC** ouvre une nouvelle console.         |
|           /a           |                                                               Ouvre une console enregistrée en mode auteur.  Utilisé pour apporter des modifications aux consoles enregistrées.                                                                |
|          /64           |                         Ouvre la version 64 bits de **MMC** (MMC64). Utilisez cette option uniquement si vous exécutez un système d’exploitation Microsoft 64 bits et souhaitez utiliser un composant logiciel enfichable 64 bits.                          |
|          /32           | Ouvre la version 32 bits de **MMC** (MMC32). Quand vous exécutez un système d’exploitation Microsoft 64 bits, vous pouvez exécuter des composants logiciels enfichables 32 bits en ouvrant MMC avec cette option de ligne de commande lorsque vous avez des composants logiciels enfichables 32 bits uniquement. |
|           /?           |                                                                                    Affiche l'aide à l'invite de commandes.                                                                                     |

## <a name="remarks"></a>Notes 
- À l' <path> **\\** <filename>aide de l’option de ligne de commande **. msc** , vous pouvez utiliser des variables d’environnement pour créer des lignes de commande ou des raccourcis qui ne dépendent pas de l’emplacement explicite des fichiers de console. Par exemple, si le chemin d’accès à un fichier de console se trouve dans le dossier système (par exemple, **MMC c:\winnt\system32\ console_name. msc**), vous pouvez utiliser la chaîne de données extensible **% systemroot%** pour spécifier l’emplacement (**MMC% systemroot% \ system32 \ console_name. msc**). Cela peut être utile si vous déléguez des tâches à des personnes de votre organisation qui travaillent sur des ordinateurs différents.
- À l’aide de l’option de ligne de commande **/A** lorsque les consoles sont ouvertes avec cette option, elles sont ouvertes en mode auteur, quel que soit leur mode par défaut. Cela ne modifie pas définitivement le paramètre de mode par défaut des fichiers. Lorsque vous omettez cette option, MMC ouvre les fichiers de console en fonction de leurs paramètres de mode par défaut.
- Après avoir ouvert **MMC** ou un fichier de console en mode auteur, vous pouvez ouvrir une console existante en cliquant sur **ouvrir** dans le menu de la **console** .
- Vous pouvez utiliser la ligne de commande pour créer des raccourcis pour ouvrir **MMC** et les consoles enregistrées. Une commande de ligne de commande fonctionne avec la commande **exécuter** du menu **Démarrer** , dans une fenêtre d’invite de commandes, dans des raccourcis ou dans un fichier de commandes ou un programme qui appelle la commande.
  ## <a name="additional-references"></a>Références supplémentaires
- - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

