---
title: wscript
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2fbaf193-cdbd-414c-84c9-bb5720f84c29
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/21/2018
ms.openlocfilehash: 771c1231ee5379ec797f535505839de8671e32a8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821300"
---
# <a name="wscript"></a>wscript



Windows Script Host fournit un environnement dans lequel les utilisateurs peuvent exécuter des scripts dans une variété de langues qui utilisent une variété de modèles d’objet pour effectuer des tâches.

## <a name="syntax"></a>Syntaxe

```
wscript [<scriptname>] [/b] [/d] [/e:<engine>] [{/h:cscript|/h:wscript}] [/i] [/job:<identifier>] [{/logo|/nologo}] [/s] [/t:<number>] [/x] [/?] [<ScriptArguments>]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|scriptname|Spécifie le chemin d’accès et le nom du fichier de script.|
|/b|Spécifie le mode de traitement par lots, qui n’affiche pas les alertes, les erreurs de script ou les invites d’entrée. Ceci est l’opposé de **/i**.|
|/d|Démarre le débogueur.|
|/e|Spécifie le moteur est utilisé pour exécuter le script. Cela vous permet d’exécuter des scripts qui utilisent une extension de nom de fichier personnalisé. Sans le paramètre /e, vous pouvez uniquement exécuter des scripts qui utilisent des extensions de nom de fichier enregistré. Par exemple, si vous essayez d’exécuter cette commande :<br>```cscript test.admin```<br>Vous recevrez ce message d’erreur : Erreur d’entrée : Il n’existe aucun moteur de script pour l’extension de fichier «. Admin. »<br>L’un des avantages de l’utilisation des extensions de nom de fichier non standard sont qu’il contre accidentellement en double-cliquant sur un script et en cours d’exécution quelque chose vous vraiment voulions pas s’exécuter. <br>Cela ne crée pas une association entre l’extension de nom de fichier .admin et VBScript permanente. Chaque fois que vous exécutez un script qui utilise une extension de nom de fichier .admin, vous devez utiliser le paramètre /e.|
|/h:cscript|Inscrit **cscript.exe** en tant que l’hôte de script par défaut pour l’exécution de scripts.|
|/h:wscript|Inscrit **wscript.exe** en tant que l’hôte de script par défaut pour l’exécution de scripts. Ceci est la valeur par défaut lorsque le **/h** option est omise.|
|/i|Spécifie le mode interactif, qui affiche les alertes, les erreurs de script et les invites d’entrée.</br>C’est la valeur par défaut et l’opposé de **/b**.|
|/job:\<identifier>|Exécute le travail identifié par *identificateur* dans un **.wsf** fichier de script.|
|/logo|Spécifie que la bannière de Windows Script Host est affichée dans la console avant l’exécution du script.</br>C’est la valeur par défaut et l’opposé de **/nologo**.|
|/nologo|Spécifie que la bannière de Windows Script Host n’est pas affichée avant l’exécution du script. Ceci est l’opposé de **/logo**.|
|/s|Enregistre les options d’invite de commandes en cours pour l’utilisateur actuel.|
|/t:\<number>|Spécifie le temps maximal de qu'exécution du script (en secondes). Vous pouvez spécifier jusqu'à 32 767 secondes.</br>La valeur par défaut n’existe aucune limite de temps.|
|/x|lance le script dans le débogueur.|
|ScriptArguments|Spécifie les arguments passés au script. Chaque argument de script doit être précédé d’une barre oblique (/).|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Pour exécuter cette tâche, vous n’avez pas besoin d’avoir d’informations d’identification administratives. Par conséquent, et par mesure de sécurité, il est conseillé d’exécuter cette tâche en tant qu’utilisateur sans informations d’identification d’administrateur.
-   Pour ouvrir une invite de commandes, dans l’écran d’**accueil**, tapez **cmd**, puis cliquez sur l’**invite de commandes**.
-   Chaque paramètre est facultatif ; Toutefois, vous ne pouvez pas spécifier des arguments de script sans spécifier un script. Si vous ne spécifiez pas un script ou arguments de script, **wscript.exe** affiche le **paramètres Windows Script Host** boîte de dialogue, vous pouvez utiliser pour définir les propriétés globales pour tous les scripts que **wscript.exe** s’exécute sur l’ordinateur local.
-   Le **/t** paramètre empêche toute exécution excessive de scripts en définissant un minuteur. Lorsque la durée dépasse la valeur spécifiée, **wscript** interrompt le moteur de script et met fin au processus.
-   Fichiers de script Windows ont généralement une des extensions de nom de fichier suivantes : **.wsf**, **.vbs**, **.js**.
-   Si vous double-cliquez sur un fichier de script avec une extension qui ne possède aucune association, le **ouvrir avec** boîte de dialogue s’affiche. Sélectionnez **wscript** ou **cscript**, puis sélectionnez **toujours utiliser ce programme pour ouvrir ce type de fichier**. Cette opération inscrit **wscript.exe** ou **cscript.exe** en tant que l’hôte de script par défaut pour les fichiers de ce type de fichier.
-   Vous pouvez définir les propriétés des scripts individuels. Consultez [vue d’ensemble de Windows Script Host](https://technet.microsoft.com/library/cc738350(v=ws.10).aspx) pour plus d’informations.
-   Hôte de Script Windows peut utiliser **.wsf** fichiers de script. Chaque **.wsf** fichier peut utiliser plusieurs moteurs de script et effectuer plusieurs tâches.

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
