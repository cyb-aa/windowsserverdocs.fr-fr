---
title: Dfsdiag testsites
description: Rubrique de référence pour Dfsdiag testsites, qui vérifie la configuration des sites des services de domaine Active Directory (AD DS) en vérifiant que les serveurs qui jouent le rôle de serveurs d’espaces de noms ou de cibles de dossier (lien) ont les mêmes associations de sites sur tous les contrôleurs de domaine.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 39a0d415-7eb7-4a26-861b-7ff00c45dcda
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 54eb7c7ec44d7cd4872960ca29cd3146b710f472
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82992851"
---
# <a name="dfsdiag-testsites"></a>Dfsdiag testsites

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vérifie la configuration des sites des services de domaine Active Directory (AD DS) en vérifiant que les serveurs qui jouent le rôle de serveurs d’espaces de noms ou de cibles de dossier (lien) ont les mêmes associations de sites sur tous les contrôleurs de domaine.

## <a name="syntax"></a>Syntaxe

```
dfsdiag /testsites </machine:<server name>| /DFSpath:<namespace root or DFS folder> [/recurse]> [/full]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| `/machine:<server name>` | Nom du serveur sur lequel vérifier l’Association de sites. |
| `/DFSpath:<namespace root or DFS folder>` | La racine de l’espace de noms ou le dossier de système de fichiers DFS (DFS) (lien) avec des cibles pour lesquelles vérifier l’Association de site. |
| /recurse | Énumère et vérifie les associations de sites pour toutes les cibles de dossiers sous la racine d’espace de noms spécifiée. |
| /Full | Vérifie que AD DS et que le Registre du serveur contiennent les mêmes informations d’association de site. |

## <a name="examples"></a>Exemples

Pour vérifier les associations de sites sur *machine\MyServer*, tapez :

```
dfsdiag /testsites /machine:MyServer
```

Pour vérifier un dossier système de fichiers DFS (DFS) afin de vérifier l’Association de site, ainsi que de vérifier que les AD DS et le Registre du serveur contiennent les mêmes informations d’association de site, tapez :

```
dfsdiag /TestSites /DFSpath:\\contoso.com\namespace1\folder1 /full
```

Pour vérifier la racine d’un espace de noms afin de vérifier l’Association de site, ainsi que l’énumération et la vérification des associations de sites pour toutes les cibles de dossiers sous la racine d’espace de noms spécifiée, et en vérifiant que AD DS et le Registre du serveur contiennent les mêmes informations d’association de site, tapez :

```
dfsdiag /testsites /DFSpath:\\contoso.com\namespace2 /recurse /full
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Dfsdiag](dfsdiag.md)
