---
title: msinfo32
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a38f31d7-1766-4103-becc-9d0b87c2826d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9ec08171816476cf04bbfb70637ff8253192e21b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373380"
---
# <a name="msinfo32"></a>msinfo32

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Ouvre l’outil informations système pour afficher une vue complète du matériel, des composants système et de l’environnement logiciel sur l’ordinateur local. 
## <a name="syntax"></a>Syntaxe
```
msinfo32 [/pch] [/nfo <path>] [/report <path>] [/computer <computerName>] [/showcategories] [/category <CategoryID>] [/categories {+<CategoryID>(+<CategoryID>)|+all(-<CategoryID>)}]
```
### <a name="parameters"></a>Paramètres

|    Paramètre    |                                                                                                                                 Description                                                                                                                                  |
|-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     <path>      | Spécifie le fichier à ouvrir au format *C:\Folder1\File1.xxx*, où *C* est la lettre de lecteur, *dossier1* est le dossier, *fichier1* est le nom de fichier et *xxx* est l’extension de nom de fichier.<br /><br />Ce fichier peut être un fichier. **NFO**, **. xml**, **. txt**ou **. cab** . |
| <computerName>  |                                                                             Spécifie le nom de l’ordinateur cible ou local. Il peut s’agir d’un nom UNC, d’une adresse IP ou d’un nom d’ordinateur complet.                                                                              |
|  <CategoryID>   |                                                                                     Spécifie l’ID de l’élément de catégorie. Vous pouvez obtenir l’ID de catégorie à l’aide de **/showcategories**.                                                                                      |
|      /pch       |                                                                                                       Affiche l’historique du système dans l’outil informations système.                                                                                                       |
|      /nfo       |                                     Enregistre le fichier exporté sous la forme d’un fichier **. nfo** . Si le nom de fichier spécifié dans *chemin d’accès* ne se termine pas par une extension **. nfo** , l’extension **. nfo** est automatiquement ajoutée au nom de fichier.                                      |
|     /Report     |                                               Enregistre le fichier dans le *chemin d’accès* en tant que fichier texte. Le nom de fichier est enregistré exactement tel qu’il apparaît dans le *chemin d’accès*. L’extension. txt n’est pas ajoutée au fichier, sauf si elle est spécifiée dans le chemin d’accès.                                                |
|    /computer    |                                                                démarre l’outil informations système pour l’ordinateur distant spécifié. Vous devez disposer des autorisations appropriées pour accéder à l’ordinateur distant.                                                                |
| /showcategories |                         démarre l’outil informations système avec tous les ID de catégorie disponibles affichés, au lieu d’afficher les noms conviviaux ou localisés. Par exemple, la catégorie environnement logiciel est affichée en tant que catégorie **SWEnv** .                         |
|    /Category    |                                                                     démarre les informations système avec la catégorie spécifiée sélectionnée. Utilisez **/showcategories** pour afficher une liste des ID de catégorie disponibles.                                                                     |
|   /categories   |                          démarre les informations système uniquement lorsque la catégorie ou les catégories spécifiées sont affichées. Il limite également la sortie à la catégorie ou aux catégories sélectionnées. Utilisez **/showcategories** pour afficher une liste des ID de catégorie disponibles.                          |
|       /?        |                                                                                                                     Affiche l'aide à l'invite de commandes.                                                                                                                     |

## <a name="remarks"></a>Notes
Certaines catégories d’informations système contiennent de grandes quantités de données. Vous pouvez utiliser la commande **start/wait** pour optimiser les performances de création de rapports pour ces catégories. Pour plus d’informations, consultez [informations système](https://technet.microsoft.com/library/cc783305(v=ws.10).aspx).
## <a name="BKMK_Examples"></a>Illustre
Pour répertorier les ID de catégorie disponibles, tapez :
```
msinfo32 /showcategories
```
Pour démarrer l’outil informations système avec toutes les informations disponibles affichées, à l’exception des modules chargés, tapez :
```
msinfo32 /categories +all -loadedmodules
```
Pour afficher uniquement les informations de résumé du système et créer un fichier. nfo nommé syssum. nfo qui contient des informations dans la catégorie Résumé système, tapez :
```
msinfo32 /nfo syssum.nfo /categories +systemsummary
```
Pour afficher les informations de conflit de ressources et créer un fichier. nfo appelé Conflicts. nfo qui contient des informations sur les conflits de ressources, tapez :
```
msinfo32 /nfo conflicts.nfo /categories    +componentsproblemdevices+resourcesconflicts+resourcesforcedhardware
```
## <a name="additional-references"></a>Références supplémentaires
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

