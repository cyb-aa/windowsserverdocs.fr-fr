---
title: Dfsdiag TestSites
description: Rubrique de référence pour Dfsdiag TestSites, qui vérifie la configuration des sites des services de domaine Active Directory (AD DS) en vérifiant que les serveurs qui jouent le rôle de serveurs d’espaces de noms ou de cibles de dossier (lien) ont les mêmes associations de sites sur tous les contrôleurs de domaine.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 39a0d415-7eb7-4a26-861b-7ff00c45dcda
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 68048699a812beac94fa121d6801da5f42e5393b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719552"
---
# <a name="dfsdiag-testsites"></a>Dfsdiag TestSites

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vérifie la configuration des sites des services de domaine Active Directory (AD DS) en vérifiant que les serveurs qui jouent le rôle de serveurs d’espaces de noms ou de cibles de dossier (lien) ont les mêmes associations de sites sur tous les contrôleurs de domaine.

## <a name="syntax"></a>Syntaxe  
  
```  
dfsdiag /TestSites </Machine:<server name>| /DFSpath:<namespace root or DFS folder> [/Recurse]> [/Full]  
```  
  
#### <a name="parameters"></a>Paramètres  
  
|Paramètre|Description|  
|-------|--------|  
|\/Usinage<server name>|Nom du serveur sur lequel vérifier l’Association de sites.|  
|\/DFSpath:<namespace root or DFS folder>|La racine de l’espace de noms ou le dossier de système de fichiers DFS (DFS) (lien) avec des cibles pour lesquelles vérifier l’Association de site.|  
|\/Recurse|Énumère et vérifie les associations de sites pour toutes les cibles de dossiers sous la racine d’espace de noms spécifiée.|  
|\/Sauvegarde|vérifie que AD DS et que le Registre du serveur contiennent les mêmes informations d’association de site.|  
  
## <a name="examples"></a>Exemples  
  
```  
dfsdiag /TestSites /Machine:MyServer  
```  
 
```  
dfsdiag /TestSites /DFSpath:\\Contoso.com\Namespace1\Folder1 /Full  
```  
  
```  
dfsdiag /TestSites /DFSpath:\\Contoso.com\Namespace2 /Recurse /Full  
```  
  
## <a name="additional-references"></a>Références supplémentaires  
  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  
-   [Dfsdiag](dfsdiag.md)  
  

