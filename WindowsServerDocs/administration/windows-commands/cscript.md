---
title: cscript
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fba3cbca-594e-4663-bb22-4ee0f63a1ac6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ef98a98088e345f267aa852318cee6e237604aa4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433988"
---
# <a name="cscript"></a>cscript

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

démarre un script afin qu’elle s’exécute dans un environnement de ligne de commande.
## <a name="syntax"></a>Syntaxe
```
cscript <Scriptname.extension> [/B] [/D] [/E:<Engine>] [{/H:cscript|/H:wscript}] [/I] [/Job:<Identifier>] [{/Logo|/NoLogo}] [/S] [/T:<Seconds>] [/X] [/U] [/?] [<ScriptArguments>]
```
### <a name="parameters"></a>Paramètres

|      Paramètre       |                                                                      Description                                                                       |
|----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|
| Scriptname.extension |                                 Spécifie le chemin d’accès et le nom du fichier de script avec l’extension de nom de fichier facultatif.                                 |
|          /B          |                                Spécifie le mode de traitement par lots, qui n’affiche pas les alertes, les erreurs de script ou les invites d’entrée.                                |
|          /D          |                                                                  Démarre le débogueur.                                                                  |
|     / E:<Engine>      |                                                  Spécifie le moteur est utilisé pour exécuter le script.                                                  |
|      / H :      |                                         Enregistre cscript.exe comme hôte de script par défaut pour l’exécution de scripts.                                          |
|      /H:wscript      |                               Inscrit wscript.exe comme l’hôte de script par défaut pour l’exécution de scripts. Il s’agit de l’option par défaut.                               |
|          /I          |        Spécifie le mode interactif, qui affiche les alertes, les erreurs de script et les invites d’entrée. C’est la valeur par défaut et l’opposé de **/B**.         |
|  / Travail :<Identifier>   |                                             Exécute le travail identifié par *identificateur* dans un fichier de script .wsf.                                             |
|        /Logo         | Spécifie que la bannière de Windows Script Host est affichée dans la console avant l’exécution du script. C’est la valeur par défaut et l’opposé de **/Nologo**. |
|       /Nologo        |                                 Spécifie que la bannière de Windows Script Host n’est pas affichée avant l’exécution du script.                                 |
|          /S          |                                             Enregistre les options d’invite de commandes en cours pour l’utilisateur actuel.                                             |
|     / T :<Seconds>     |            Spécifie le temps maximal de qu'exécution du script (en secondes). Vous pouvez spécifier jusqu'à 32 767 secondes. La valeur par défaut n’existe aucune limite de temps.             |
|          /U          |                                      Spécifie Unicode pour l’entrée et de sortie est redirigée à partir de la console.                                       |
|          /X          |                                                           lance le script dans le débogueur.                                                           |
|          /?          |  Affiche les paramètres de commande disponibles et fournit une aide pour leur utilisation. Cela équivaut à taper **cscript.exe** sans paramètres et aucun script.  |
|   ScriptArguments    |                        Spécifie les arguments passés au script. Chaque argument de script doit être précédé d’une barre oblique ( **/** ).                         |

### <a name="remarks"></a>Notes
-   Pour exécuter cette tâche, vous n’avez pas besoin d’avoir d’informations d’identification administratives. Par conséquent, et par mesure de sécurité, il est conseillé d’exécuter cette tâche en tant qu’utilisateur sans informations d’identification d’administrateur.
-   Pour ouvrir une invite de commandes, sur le **Démarrer** , tapez **cmd**, puis cliquez sur **invite de commandes**.
-   Chaque paramètre est facultatif ; Toutefois, vous ne pouvez pas spécifier des arguments de script sans spécifier un script. Si vous ne spécifiez pas un script ou arguments de script, cscript.exe affiche la syntaxe cscript.exe et les options d’hôte valide.
-   Le **/T** paramètre empêche toute exécution excessive de scripts en définissant un minuteur. Lorsque la durée d’exécution dépasse la valeur spécifiée, cscript interrompt le moteur de script et met fin au processus.
-   Fichiers de script Windows ont généralement une des extensions de nom de fichier suivantes : .wsf, .vbs et .js.
-   Vous pouvez définir les propriétés des scripts individuels. Pour plus d’informations, consultez les rubriques connexes.
-   Windows Script Host peut utiliser des fichiers de script .wsf. Chaque fichier .wsf peut utiliser plusieurs moteurs de script et effectuer plusieurs tâches.
-   Si vous double-cliquez sur un fichier de script avec une extension qui ne possède aucune association, le **ouvrir avec** boîte de dialogue s’affiche. Sélectionnez wscript ou cscript, puis **toujours utiliser ce programme pour ouvrir ce type de fichier**. Cela inscrit wscript.exe ou cscript comme hôte de script par défaut pour les fichiers de ce type de fichier.
-   Vous pouvez définir les propriétés des scripts individuels. Consultez [références supplémentaires](#BKMK_references) pour plus d’informations.
-   Windows Script Host peut utiliser des fichiers de script .wsf. Chaque fichier .wsf peut utiliser plusieurs moteurs de script et effectuer plusieurs tâches.

#### <a name="BKMK_references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
