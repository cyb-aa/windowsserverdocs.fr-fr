---
title: cscript
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 32cfd70266abdcdc247bf79b34ae220a73729837
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378802"
---
# <a name="cscript"></a>cscript

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

démarre un script pour qu’il s’exécute dans un environnement de ligne de commande.
## <a name="syntax"></a>Syntaxe
```
cscript <Scriptname.extension> [/B] [/D] [/E:<Engine>] [{/H:cscript|/H:wscript}] [/I] [/Job:<Identifier>] [{/Logo|/NoLogo}] [/S] [/T:<Seconds>] [/X] [/U] [/?] [<ScriptArguments>]
```
### <a name="parameters"></a>Paramètres

|      Paramètre       |                                                                      Description                                                                       |
|----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|
| Scriptname. extension |                                 Spécifie le chemin d’accès et le nom de fichier du fichier de script avec l’extension de nom de fichier facultative.                                 |
|          /B          |                                Spécifie le mode batch, qui n’affiche pas les alertes, les erreurs de script ou les invites d’entrée.                                |
|          /D          |                                                                  Démarre le débogueur.                                                                  |
|     /E : <Engine>      |                                                  Spécifie le moteur utilisé pour exécuter le script.                                                  |
|      /H : cscript      |                                         Inscrit cscript. exe comme hôte de script par défaut pour l’exécution des scripts.                                          |
|      /H : WScript      |                               Inscrit WScript. exe comme hôte de script par défaut pour l’exécution des scripts. Il s’agit de l’option par défaut.                               |
|          /I          |        Spécifie le mode interactif, qui affiche les alertes, les erreurs de script et les invites d’entrée. Il s’agit de la valeur par défaut et de l’opposé de **/b**.         |
|  /Travail : <Identifier>   |                                             Exécute la tâche identifiée par l' *identificateur* dans un fichier de script. wsf.                                             |
|        /Logo         | Spécifie que la bannière Windows Script Host est affichée dans la console avant l’exécution du script. Il s’agit de la valeur par défaut et de l’opposé de **/nologo**. |
|       /Nologo        |                                 Spécifie que la bannière Windows Script Host ne s’affiche pas avant l’exécution du script.                                 |
|          COMMUTATEUR          |                                             Enregistre les options d’invite de commandes actuelles pour l’utilisateur actuel.                                             |
|     /T : <Seconds>     |            Spécifie la durée maximale d’exécution du script (en secondes). Vous pouvez spécifier jusqu’à 32 767 secondes. La valeur par défaut est aucune limite de durée.             |
|          /U.          |                                      Spécifie Unicode pour l’entrée et la sortie redirigées à partir de la console.                                       |
|          /PATH          |                                                           Démarre le script dans le débogueur.                                                           |
|          /?          |  Affiche les paramètres de commande disponibles et fournit de l’aide pour les utiliser. Cela revient à taper **cscript. exe** sans paramètres ni script.  |
|   ScriptArguments    |                        Spécifie les arguments passés au script. Chaque argument de script doit être précédé d’une barre oblique ( **/** ).                         |

### <a name="remarks"></a>Notes
-   Pour exécuter cette tâche, vous n’avez pas besoin d’avoir d’informations d’identification administratives. Par conséquent, et par mesure de sécurité, il est conseillé d’exécuter cette tâche en tant qu’utilisateur sans informations d’identification d’administrateur.
-   Pour ouvrir une invite de commandes, dans l’écran d' **Accueil** , tapez **cmd**, puis cliquez sur **invite de commandes**.
-   Chaque paramètre est facultatif ; Toutefois, vous ne pouvez pas spécifier d’arguments de script sans spécifier de script. Si vous ne spécifiez pas de script ou d’argument de script, cscript. exe affiche la syntaxe cscript. exe et les options d’ordinateur hôte valides.
-   Le paramètre **/t** empêche l’exécution excessive de scripts en définissant un minuteur. Lorsque la durée d’exécution dépasse la valeur spécifiée, cscript interrompt le moteur de script et met fin au processus.
-   Les fichiers de script Windows ont généralement l’une des extensions de nom de fichier suivantes :. wsf,. vbs,. js.
-   Vous pouvez définir des propriétés pour des scripts individuels. Pour plus d’informations, consultez les rubriques connexes.
-   Windows Script Host peut utiliser des fichiers de script. wsf. Chaque fichier. wsf peut utiliser plusieurs moteurs de script et effectuer plusieurs tâches.
-   Si vous double-cliquez sur un fichier de script avec une extension qui n’a pas d’association, la boîte **de dialogue Ouvrir avec** s’affiche. Sélectionnez WScript ou cscript, puis sélectionnez **toujours utiliser ce programme pour ouvrir ce type de fichier**. Cela enregistre WScript. exe ou cscript en tant qu’hôte de script par défaut pour les fichiers de ce type de fichier.
-   Vous pouvez définir des propriétés pour des scripts individuels. Pour plus d’informations, consultez [Références supplémentaires](#BKMK_references) .
-   Windows Script Host peut utiliser des fichiers de script. wsf. Chaque fichier. wsf peut utiliser plusieurs moteurs de script et effectuer plusieurs tâches.

#### <a name="BKMK_references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
