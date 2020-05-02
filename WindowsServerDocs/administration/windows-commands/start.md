---
title: start
description: Rubrique de référence pour Start, qui démarre une fenêtre d’invite de commandes distincte pour exécuter un programme ou une commande spécifique.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0173f9b3-5cd7-4edb-b01e-d02193b4fadc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 388a6b0871382c7704813298e6978a40cadeb709
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721761"
---
# <a name="start"></a>start

Démarre une fenêtre d’invite de commandes distincte pour exécuter un programme ou une commande spécifique.



## <a name="syntax"></a>Syntaxe

```
start [<Title>] [/d <Path>] [/i] [{/min | /max}] [{/separate | /shared}] [{/low | /normal | /high | /realtime | /abovenormal | belownormal}] [/affinity <HexAffinity>] [/wait] [/elevate] [/b] [<Command> [<Parameter>... ] | <Program> [<Parameter>... ]]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Titre>|Spécifie le titre à afficher dans la barre de titre de la fenêtre d’invite de commandes.|
|> \<du chemin d’accès/d|Spécifie le répertoire de démarrage.|
|/i|Passe l’environnement de démarrage cmd. exe à la nouvelle fenêtre d’invite de commandes. Si **/i** n’est pas spécifié, l’environnement actuel est utilisé.|
|/min \| /Max|Spécifie de réduire (**/min**) ou d’agrandir (**/Max**) la nouvelle fenêtre d’invite de commandes.|
|/Separate \| /Shared|Démarre les programmes 16 bits dans un espace mémoire séparé (**/Separate**) ou dans un espace mémoire partagé (**/Shared**). Ces options ne sont pas prises en charge sur les plateformes 64 bits.|
|/Low \| /normal \| /High \| /Realtime \| /AboveNormal \| /BelowNormal|Démarre une application dans la classe de priorité spécifiée. Les valeurs de classe de priorité valides sont **/Low**, **/normal**, **/High**, **/Realtime**, **/AboveNormal**et **/BelowNormal**.|
|> \<HexAffinity/Affinity|Applique le masque d’affinité de processeur spécifié (exprimé sous forme de nombre hexadécimal) à la nouvelle application.|
|/Wait|Démarre une application et attend qu’elle se termine.|
|/elevate|Exécute l’application en tant qu’administrateur.|
|/b|Démarre une application sans ouvrir une nouvelle fenêtre d’invite de commandes. La gestion CTRL + C est ignorée, sauf si l’application active le traitement CTRL + C. Utilisez CTRL + Pause pour interrompre l’application.|
|\<Commande> \| \<le programme>|Spécifie la commande ou le programme à démarrer.|
|\<> de paramètres...|Spécifie les paramètres à passer à la commande ou au programme.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes 

- Vous pouvez exécuter des fichiers non exécutables par le biais de leur association de fichiers en tapant le nom du fichier sous la forme d’une commande.
- Quand vous exécutez une commande qui contient la chaîne CMD comme premier jeton sans extension ou qualificateur de chemin d’accès, CMD est remplacé par la valeur de la variable COMSPEC. Cela empêche les utilisateurs de choisir **cmd** dans le répertoire actif.
- Lorsque vous exécutez une application GUI 32 bits, **cmd** n’attend pas la fermeture de l’application avant de revenir à l’invite de commandes. Ce comportement ne se produit pas si vous exécutez l’application à partir d’un script de commande.
- Quand vous exécutez une commande qui utilise un premier jeton qui ne contient pas d’extension, cmd. exe utilise la valeur de la variable d’environnement PATHEXT pour déterminer les extensions à rechercher et dans quel ordre. La valeur par défaut de la variable PATHEXT est la suivante :  
  ```
  .COM;.EXE;.BAT;.CMD;.VBS;.VBE;.JS;.JSE;.WSF;.WSH;.MSC 
  ```  
  Notez que la syntaxe est la même que la variable PATH, avec des points-virgules pour séparer chaque extension.
- Lors de la recherche d’un fichier exécutable, s’il n’existe aucune correspondance sur une extension, **Démarrez** vérifie si le nom correspond à un nom de répertoire. Si c’est le cas, **Démarrer** ouvre Explorer. exe sur ce chemin.

## <a name="examples"></a>Exemples

Pour démarrer le programme myapp à partir de l’invite de commandes et conserver l’utilisation de la fenêtre d’invite de commandes actuelle, tapez :
```
start myapp 
```
Pour afficher la rubrique d’aide de la ligne de commande **Start** dans une fenêtre d’invite de commandes agrandie distincte, tapez :
```
start /max start /?
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
