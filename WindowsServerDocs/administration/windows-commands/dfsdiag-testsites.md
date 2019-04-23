---
title: Dfsdiag TestSites
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6486c79cfa58bb262fd3161ad0801e84185b6629
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873970"
---
# <a name="dfsdiag-testsites"></a>Dfsdiag TestSites

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vérifie la configuration des Services de domaine active directory \(AD DS\) sites en vérifiant que les serveurs qui agissent comme serveurs de l’espace de noms ou un dossier \(lien\) cibles possèdent les mêmes associations de site sur tous les domaine contrôleurs.  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
dfsdiag /TestSites </Machine:<server name>| /DFSpath:<namespace root or DFS folder> [/Recurse]> [/Full]  
```  
  
### <a name="parameters"></a>Paramètres  
  
|Paramètre|Description|  
|-------|--------|  
|\/Ordinateur :<server name>|Le nom du serveur sur lequel vérifier l’association du site.|  
|\/DFSpath :<namespace root or DFS folder>|L’espace de noms racine ou le système de fichiers distribués \(DFS\) dossier \(lien\) avec des cibles pour lequel vérifier l’association du site.|  
|\/Recurse|Énumère et vérifie les associations de site pour toutes les cibles de dossier sous la racine de l’espace de noms spécifié.|  
|\/complet|vérifie que les services AD DS et le Registre du serveur contiennent les mêmes informations d’association de site.|  
  
## <a name="BKMK_Examples"></a>Exemples  
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
  
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

