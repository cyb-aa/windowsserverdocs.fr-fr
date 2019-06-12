---
title: start
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0173f9b3-5cd7-4edb-b01e-d02193b4fadc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ab7388633681120442544adf4ee0e337d8599854
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441227"
---
# <a name="start"></a>start



Démarre une fenêtre d’invite de commandes distincte pour exécuter une commande ou le programme spécifié.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
start ["<Title>"] [/d <Path>] [/i] [{/min | /max}] [{/separate | /shared}] [{/low | /normal | /high | /realtime | /abovenormal | belownormal}] [/affinity <HexAffinity>] [/wait] [/b {<Command> | <Program>} [<Parameters>]]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|«\<Title > »|Spécifie le titre à afficher dans la barre de titre de fenêtre invite de commandes.|
|/d \<Path>|Spécifie le répertoire de démarrage.|
|/i|Passe l’environnement de démarrage Cmd.exe à la nouvelle fenêtre d’invite de commandes. Si **/i** n’est pas spécifié, l’environnement actuel est utilisé.|
|/min \| /max|Indique qu’il faut réduire ( **/min**) ou optimiser ( **/max**) la nouvelle fenêtre d’invite de commandes.|
|/ séparer \| / shared|Démarre les programmes 16 bits dans un espace de mémoire distincts ( **/séparer**) ou partagée d’espace de mémoire ( **/ shared**). Ces options ne sont pas prises en charge sur les plateformes 64 bits.|
|/low \| /normal \| /high \| /realtime \| /abovenormal \| /belownormal|Démarre une application dans la classe de priorité spécifié. Les valeurs de classe de priorité valides sont **/faible**, **/normal**, **/élevée**, **/realtime**, **/abovenormal**, et **/belownormal**.|
|/affinity \<HexAffinity >|S’applique le masque d’affinité de processeur spécifié (exprimé en un nombre hexadécimal) vers la nouvelle application.|
|/wait|Démarre une application et attend que celle-ci à la fin.|
|/b|Démarre une application sans ouvrir une nouvelle fenêtre d’invite de commandes. CTRL + C est ignoré, sauf si l’application active le traitement de CTRL + C. Utilisez CTRL + Pause pour interrompre l’application.|
|/b \<commande > \| \<programme >|Spécifie la commande ou le programme à démarrer.|
|\<Paramètres >|Spécifie les paramètres à passer à la commande ou d’un programme.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

- Vous pouvez exécuter des fichiers non exécutables via leur association de fichiers en tapant le nom du fichier en tant que commande.
- Lorsque vous exécutez une commande qui contient la chaîne « CMD » en tant que premier jeton sans qualificateur d’extension ou de chemin d’accès, « CMD » est remplacé par la valeur de la variable COMSPEC. Cela empêche les utilisateurs de capter **cmd** depuis le répertoire actif.
- Lorsque vous exécutez une application (GUI) d’interface utilisateur graphique de 32 bits, **cmd** n’attend pas de l’application à se fermer avant de retourner à l’invite de commandes. Ce comportement ne se produit pas si vous exécutez l’application à partir d’un script de commande.
- Lorsque vous exécutez une commande qui utilise un premier jeton qui ne contient-elle pas une extension, Cmd.exe utilise la valeur de la variable d’environnement PATHEXT pour déterminer quelles extensions rechercher et dans quel ordre. La valeur par défaut pour la variable PATHEXT est :  
  ```
  .COM;.EXE;.BAT;.CMD 
  ```  
  Notez que la syntaxe est identique à la variable de chemin d’accès par des points-virgules pour séparer chaque extension.
- Lorsqu’il recherche un fichier exécutable, si aucune correspondance n’est sur n’importe quelle extension, **Démarrer** vérifie si le nom correspond à un nom de répertoire. Le cas échéant, **Démarrer** ouvre Explorer.exe sur ce chemin d’accès.

## <a name="BKMK_examples"></a>Exemples

Pour démarrer le programme MonApp à l’invite de commandes et continuer à utiliser la fenêtre d’invite de commandes actuelle, tapez :
```
start myapp 
```
Pour afficher le **Démarrer** rubrique d’aide en ligne de commande dans une fonction agrandie fenêtre d’invite de commandes, tapez :
```
start /max start /?
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
