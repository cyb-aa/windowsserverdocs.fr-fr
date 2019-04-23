---
title: msinfo32
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: a3de5088b64105e970fc38f55ecaf54382670549
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843460"
---
# <a name="msinfo32"></a>msinfo32

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ouvre l’outil Informations système pour afficher une vue complète du matériel, des composants système et des environnement logiciel sur l’ordinateur local. 
## <a name="syntax"></a>Syntaxe
```
msinfo32 [/pch] [/nfo <path>] [/report <path>] [/computer <computerName>] [/showcategories] [/category <CategoryID>] [/categories {+<CategoryID>(+<CategoryID>)|+all(-<CategoryID>)}]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|<path>|Spécifie le fichier à ouvrir au format *C:\Folder1\File1.XXX*, où *C* est la lettre de lecteur, *Dossier1* est le dossier *File1*est le nom du fichier et *XXX* est l’extension de nom de fichier.<br /><br />Ce fichier peut être un **.nfo**, **.xml**, **.txt**, ou **.cab** fichier.|
|<computerName>|Spécifie le nom de l’ordinateur local ou la cible. Cela peut être un nom UNC, une adresse IP ou un nom complet de l’ordinateur.|
|<CategoryID>|Spécifie l’ID de l’élément de catégorie. Vous pouvez obtenir l’ID de catégorie à l’aide de **/showcategories**.|
|/pch|Affiche la vue de l’historique du système dans l’outil Informations système.|
|/nfo|Enregistre le fichier exporté sous un **.nfo** fichier. Si le nom du fichier qui est spécifié dans *chemin d’accès* ne se termine pas dans un **.nfo** extension, le **.nfo** extension est automatiquement ajoutée au nom de fichier.|
|/report|Enregistre le fichier dans *chemin d’accès* comme un fichier texte. Le nom de fichier est enregistré exactement tel qu’il apparaît dans *chemin d’accès*. L’extension .txt n’est pas ajoutée au fichier, sauf si elle est spécifiée dans le chemin d’accès.|
|/ Computer|démarre l’outil Informations système pour l’ordinateur distant spécifié. Vous devez disposer des autorisations appropriées pour accéder à l’ordinateur distant.|
|/showcategories|démarre l’outil Informations système avec tous disponible catégorie Qu'id affichés, plutôt que d’affichant le nom convivial ou localisé. Par exemple, la catégorie de l’environnement logiciel s’affiche en tant que le **SWEnv** catégorie.|
|/category|démarre les informations système avec la catégorie spécifiée. Utilisez **/showcategories** pour afficher une liste des ID de catégorie disponible.|
|/categories|démarre les informations système avec uniquement l’ou les catégories spécifiées. Il limite également la sortie vers l’ou les catégories sélectionnées. Utilisez **/showcategories** pour afficher une liste des ID de catégorie disponible.|
|/?|Affiche l'aide à l'invite de commandes.|
## <a name="remarks"></a>Notes
Certaines catégories d’informations système contiennent de grandes quantités de données. Vous pouvez utiliser la **start /wait** commande pour optimiser les performances de création de rapports pour ces catégories. Pour plus d’informations, consultez [informations système](https://technet.microsoft.com/library/cc783305(v=ws.10).aspx).
## <a name="BKMK_Examples"></a>Exemples
Pour répertorier les ID de catégorie, tapez :
```
msinfo32 /showcategories
```
Pour démarrer l’outil Informations système avec toutes les informations disponibles affichée, à l’exception des Modules chargés, tapez :
```
msinfo32 /categories +all -loadedmodules
```
Pour afficher uniquement les informations de résumé système et créer un fichier nommé syssum.nfo contenant les informations contenues dans la catégorie Résumé système ces informations, tapez :
```
msinfo32 /nfo syssum.nfo /categories +systemsummary
```
Pour afficher des informations sur les conflits de ressources et créer un fichier nommé Conflicts.nfo contenant des informations sur les conflits de ressources ces informations, tapez :
```
msinfo32 /nfo conflicts.nfo /categories    +componentsproblemdevices+resourcesconflicts+resourcesforcedhardware
```
## <a name="additional-references"></a>Références supplémentaires
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)

