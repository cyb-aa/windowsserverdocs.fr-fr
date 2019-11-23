---
title: Dfsdiag TestSites
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 39a0d415-7eb7-4a26-861b-7ff00c45dcda
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: af72da64dd20d4b37824355a494cb8f97f597b28
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378382"
---
# <a name="dfsdiag-testsites"></a>Dfsdiag TestSites

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vérifie la configuration des services de domaine Active Directory \(AD DS les sites\) en vérifiant que les serveurs qui jouent le rôle de serveurs d’espaces de noms ou de \(de\) de lien de dossiers ou de dossiers ont les mêmes associations de sites sur tous les contrôleurs de domaine.  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
dfsdiag /TestSites </Machine:<server name>| /DFSpath:<namespace root or DFS folder> [/Recurse]> [/Full]  
```  
  
### <a name="parameters"></a>Paramètres  
  
|Paramètre|Description|  
|-------|--------|  
|Ordinateur \/:<server name>|Nom du serveur sur lequel vérifier l’Association de sites.|  
|\/DFSpath :<namespace root or DFS folder>|La racine de l’espace de noms ou le système de fichiers DFS \(dossier DFS\) \(lien\) avec les cibles pour lesquelles vérifier l’Association de site.|  
|\/recurse|Énumère et vérifie les associations de sites pour toutes les cibles de dossiers sous la racine d’espace de noms spécifiée.|  
|\/complète|vérifie que AD DS et que le Registre du serveur contiennent les mêmes informations d’association de site.|  
  
## <a name="BKMK_Examples"></a>Illustre  
À TBD, tapez :  
  
```  
dfsdiag /TestSites /Machine:MyServer  
```  
  
À TBD, tapez :  
  
```  
dfsdiag /TestSites /DFSpath:\\Contoso.com\Namespace1\Folder1 /Full  
```  
  
À TBD, tapez :  
  
```  
dfsdiag /TestSites /DFSpath:\\Contoso.com\Namespace2 /Recurse /Full  
```  
  
## <a name="additional-references"></a>Références supplémentaires  
  
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  
-   [Dfsdiag](dfsdiag.md)  
  

