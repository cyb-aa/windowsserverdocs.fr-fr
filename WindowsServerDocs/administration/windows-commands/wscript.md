---
title: wscript
description: La rubrique commandes Windows pour WScript, qui fournit un environnement dans lequel les utilisateurs peuvent exécuter des scripts dans différents langages utilisant divers modèles objet pour effectuer des tâches.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2fbaf193-cdbd-414c-84c9-bb5720f84c29
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/21/2018
ms.openlocfilehash: 77a0087eee1287699d66c4e1e5ab2aef69551440
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80828932"
---
# <a name="wscript"></a>wscript



Windows Script Host fournit un environnement dans lequel les utilisateurs peuvent exécuter des scripts dans un large éventail de langages qui utilisent divers modèles d’objet pour effectuer des tâches.

## <a name="syntax"></a>Syntaxe

```
wscript [<scriptname>] [/b] [/d] [/e:<engine>] [{/h:cscript|/h:wscript}] [/i] [/job:<identifier>] [{/logo|/nologo}] [/s] [/t:<number>] [/x] [/?] [<ScriptArguments>]
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|scriptname|Spécifie le chemin d’accès et le nom de fichier du fichier de script.|
|/b|Spécifie le mode batch, qui n’affiche pas les alertes, les erreurs de script ou les invites d’entrée. Il s’agit de l’inverse de **/i**.|
|/d|Démarre le débogueur.|
|/e|Spécifie le moteur utilisé pour exécuter le script. Cela vous permet d’exécuter des scripts qui utilisent une extension de nom de fichier personnalisée. Sans le paramètre/e, vous ne pouvez exécuter que des scripts qui utilisent des extensions de nom de fichier inscrites. Par exemple, si vous essayez d’exécuter cette commande :<br>```cscript test.admin```<br>Vous recevrez le message d’erreur suivant : erreur d’entrée : il n’existe aucun moteur de script pour l’extension de fichier. admin.<br>L’un des avantages de l’utilisation d’extensions de nom de fichier non standard est qu’elle protège contre le double-clic accidentel d’un script et l’exécution d’un fichier que vous ne souhaitez pas exécuter vraiment. <br>Cela ne crée pas d’association permanente entre l’extension de nom de fichier. admin et VBScript. Chaque fois que vous exécutez un script qui utilise une extension de nom de fichier. admin, vous devez utiliser le paramètre/e.|
|/h : cscript|Inscrit **cscript. exe** comme hôte de script par défaut pour l’exécution des scripts.|
|/h : WScript|Inscrit **Wscript. exe** comme hôte de script par défaut pour l’exécution des scripts. Il s’agit de la valeur par défaut lorsque l’option **/h** est omise.|
|/i|Spécifie le mode interactif, qui affiche les alertes, les erreurs de script et les invites d’entrée.</br>Il s’agit de la valeur par défaut et de l’opposé de **/b**.|
|/travail : identificateur de\<>|Exécute la tâche identifiée par l' *identificateur* dans un fichier de script **. wsf** .|
|/logo|Spécifie que la bannière Windows Script Host est affichée dans la console avant l’exécution du script.</br>Il s’agit de la valeur par défaut et de l’opposé de **/nologo**.|
|/nologo|Spécifie que la bannière Windows Script Host ne s’affiche pas avant l’exécution du script. Il s’agit de l’opposé de **/logo**.|
|/s|Enregistre les options d’invite de commandes actuelles pour l’utilisateur actuel.|
|/t : nombre\<>|Spécifie la durée maximale d’exécution du script (en secondes). Vous pouvez spécifier jusqu’à 32 767 secondes.</br>La valeur par défaut est aucune limite de durée.|
|/x|Démarre le script dans le débogueur.|
|ScriptArguments|Spécifie les arguments passés au script. Chaque argument de script doit être précédé d’une barre oblique (/).|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Pour exécuter cette tâche, vous n'avez pas besoin d'avoir un profil d'administrateur. Par conséquent, pour des raisons de sécurité, envisagez d'effectuer cette tâche en tant qu'utilisateur ne disposant pas des informations d'identification d'administration.
-   Pour ouvrir une invite de commandes, dans l’écran d’**accueil**, tapez **cmd**, puis cliquez sur l’**invite de commandes**.
-   Chaque paramètre est facultatif ; Toutefois, vous ne pouvez pas spécifier d’arguments de script sans spécifier de script. Si vous ne spécifiez pas de script ou d’argument de script, **Wscript. exe** affiche la boîte de dialogue **Windows Script Host Settings** , que vous pouvez utiliser pour définir les propriétés de script globales de tous les scripts exécutés par **Wscript. exe** sur l’ordinateur local.
-   Le paramètre **/t** empêche l’exécution excessive de scripts en définissant un minuteur. Lorsque le délai dépasse la valeur spécifiée, **wscript** interrompt le moteur de script et met fin au processus.
-   Les fichiers de script Windows ont généralement l’une des extensions de nom de fichier suivantes : **. wsf**, **. vbs**, **. js**.
-   Si vous double-cliquez sur un fichier de script avec une extension qui n’a pas d’association, la boîte **de dialogue Ouvrir avec** s’affiche. Sélectionnez **wscript** ou **cscript**, puis sélectionnez **toujours utiliser ce programme pour ouvrir ce type de fichier**. Cela inscrit **Wscript. exe** ou **cscript. exe** comme hôte de script par défaut pour les fichiers de ce type de fichier.
-   Vous pouvez définir des propriétés pour des scripts individuels. Pour plus d’informations, consultez [vue d’ensemble de Windows Script Host](https://technet.microsoft.com/library/cc738350(v=ws.10).aspx) .
-   Windows Script Host peut utiliser des fichiers de script **. wsf** . Chaque fichier **. wsf** peut utiliser plusieurs moteurs de script et effectuer plusieurs tâches.

## <a name="additional-references"></a>Références supplémentaires

-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
