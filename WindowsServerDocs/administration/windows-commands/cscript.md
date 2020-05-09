---
title: cscript
description: Rubrique de référence pour la commande Cscript, qui démarre un script pour qu’il s’exécute dans un environnement de ligne de commande.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fba3cbca-594e-4663-bb22-4ee0f63a1ac6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bfba4b6a1c75183d58664e74da22bb7f8b866739
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993159"
---
# <a name="cscript"></a>cscript

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Démarre un script à exécuter dans un environnement de ligne de commande.

>[!IMPORTANT]
> Pour effectuer cette tâche, vous n'avez pas besoin de disposer d'informations d'identification d'administration. Par conséquent, pour des raisons de sécurité, envisagez d'effectuer cette tâche en tant qu'utilisateur ne disposant pas des informations d'identification d'administration.

## <a name="syntax"></a>Syntaxe

```
cscript <scriptname.extension> [/b] [/d] [/e:<engine>] [{/h:cscript | /h:wscript}] [/i] [/job:<identifier>] [{/logo | /nologo}] [/s] [/t:<seconds>] [x] [/u] [/?] [<scriptarguments>]
```

#### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| scriptname. extension | Spécifie le chemin d’accès et le nom de fichier du fichier de script avec l’extension de nom de fichier facultative. |
| /b | Spécifie le mode batch, qui n’affiche pas les alertes, les erreurs de script ou les invites d’entrée. |
| /d | Démarre le débogueur. |
| /e:`<engine>` | Spécifie le moteur utilisé pour exécuter le script. |
| /h : cscript | Inscrit cscript. exe comme hôte de script par défaut pour l’exécution des scripts. |
| /h : WScript | Inscrit WScript. exe comme hôte de script par défaut pour l’exécution des scripts. Il s’agit de la valeur par défaut. |
| /i | Spécifie le mode interactif, qui affiche les alertes, les erreurs de script et les invites d’entrée. Il s’agit de la valeur par défaut `/b`et de l’inverse de. |
| /travail<identifier> | Exécute la tâche identifiée par l' *identificateur* dans un fichier de script. wsf. |
| /logo | Spécifie que la bannière Windows Script Host est affichée dans la console avant l’exécution du script. Il s’agit de la valeur par défaut `/nologo`et de l’inverse de. |
| /nologo | Spécifie que la bannière Windows Script Host ne s’affiche pas avant l’exécution du script. |
| /s | Enregistre les options d’invite de commandes actuelles pour l’utilisateur actuel. |
| /t:<seconds> | Spécifie la durée maximale d’exécution du script (en secondes). Vous pouvez spécifier jusqu’à 32 767 secondes. La valeur par défaut est aucune limite de durée. |
| /U | Spécifie Unicode pour l’entrée et la sortie redirigées à partir de la console. |
| /x | Démarre le script dans le débogueur. |
| /? | Affiche les paramètres de commande disponibles et fournit de l’aide pour les utiliser. Cela revient à taper **cscript. exe** sans paramètres ni script. |
| scriptarguments | Spécifie les arguments passés au script. Chaque argument de script doit être précédé d’une barre oblique**/**(). |

#### <a name="remarks"></a>Notes 

- Chaque paramètre est facultatif ; Toutefois, vous ne pouvez pas spécifier d’arguments de script sans spécifier de script. Si vous ne spécifiez pas de script ou d’argument de script, cscript. exe affiche la syntaxe cscript. exe et les options d’ordinateur hôte valides.

- Le paramètre **/t** empêche l’exécution excessive de scripts en définissant un minuteur. Lorsque la durée d’exécution dépasse la valeur spécifiée, cscript interrompt le moteur de script et met fin au processus.

- Les fichiers de script Windows ont généralement l’une des extensions de nom de fichier suivantes :. wsf,. vbs,. js. Windows Script Host peut utiliser des fichiers de script. wsf. Chaque fichier. wsf peut utiliser plusieurs moteurs de script et effectuer plusieurs tâches.

- Si vous double-cliquez sur un fichier de script avec une extension qui n’a pas d’association, la boîte **de dialogue Ouvrir avec** s’affiche. Sélectionnez WScript ou cscript, puis sélectionnez **toujours utiliser ce programme pour ouvrir ce type de fichier**. Cela enregistre WScript. exe ou cscript en tant qu’hôte de script par défaut pour les fichiers de ce type de fichier.

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
