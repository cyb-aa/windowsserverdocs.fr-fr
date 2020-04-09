---
title: Dfsdiag TestSites
description: La rubrique commandes Windows pour Dfsdiag TestSites, qui vérifie la configuration des sites des services de domaine Active Directory (AD DS) en vérifiant que les serveurs qui jouent le rôle de serveurs d’espaces de noms ou de cibles de dossiers (lien) ont les mêmes associations de sites sur tous les contrôleurs de domaine.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 39a0d415-7eb7-4a26-861b-7ff00c45dcda
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 80cc9095748dafb030b204130bfa2ccb61ec69ea
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846222"
---
# <a name="dfsdiag-testsites"></a>Dfsdiag TestSites

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vérifie la configuration des sites des services de domaine Active Directory (AD DS) en vérifiant que les serveurs qui jouent le rôle de serveurs d’espaces de noms ou de cibles de dossier (lien) ont les mêmes associations de sites sur tous les contrôleurs de domaine.

## <a name="syntax"></a>Syntaxe  
  
```  
dfsdiag /TestSites </Machine:<server name>| /DFSpath:<namespace root or DFS folder> [/Recurse]> [/Full]  
```  
  
#### <a name="parameters"></a>Paramètres  
  
|Paramètre|Description|  
|-------|--------|  
|Ordinateur \/:<server name>|Nom du serveur sur lequel vérifier l’Association de sites.|  
|\/DFSpath :<namespace root or DFS folder>|La racine de l’espace de noms ou le dossier de système de fichiers DFS (DFS) (lien) avec des cibles pour lesquelles vérifier l’Association de site.|  
|\/recurse|Énumère et vérifie les associations de sites pour toutes les cibles de dossiers sous la racine d’espace de noms spécifiée.|  
|\/complète|vérifie que AD DS et que le Registre du serveur contiennent les mêmes informations d’association de site.|  
  
## <a name="examples"></a><a name=BKMK_Examples></a>Illustre  
  
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
  

