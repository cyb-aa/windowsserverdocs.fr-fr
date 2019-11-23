---
title: Dfsdiag TestDFSIntegrity
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 173ee832-26e1-4ec8-a23a-38a7d6229ac3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7f344e2d1fecc542efc39688f20165fd3e39a04a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378431"
---
# <a name="dfsdiag-testdfsintegrity"></a>Dfsdiag TestDFSIntegrity

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vérifie l’intégrité de l’espace de noms système de fichiers DFS \(DFS\) en effectuant les tests suivants :  
  
-   Vérifie la corruption des métadonnées DFS ou les incohérences entre les contrôleurs de domaine.  
  
-   Valide la configuration de l’énumération basée sur les\-d’accès afin de garantir la cohérence entre les métadonnées DFS et le partage du serveur d’espaces de noms.  
  
-   Détecte les dossiers DFS qui se chevauchent \(des liens\), des dossiers dupliqués et des dossiers avec des cibles de dossiers qui se chevauchent.  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
dfsdiag /TestDFSIntegrity /DFSRoot:<DFS root path> [/Recurse] [/Full]  
```  
  
### <a name="parameters"></a>Paramètres  
  
|Paramètre|Description|  
|-------|--------|  
|\/DFSRoot :<DFS root path>|Espace de noms DFS à diagnostiquer.|  
|\/recurse|Effectue les tests, y compris les liaisons d’espace de noms.|  
|\/complète|vérifie la cohérence des ACL de partage et NTFS, ainsi que la configuration côté client sur toutes les cibles de dossiers. Il vérifie également que la propriété Online est définie.|  
  
## <a name="BKMK_Examples"></a>Illustre  
À TBD, tapez :  
  
```  
dfsdiag /TestDFSIntegrity /DFSRoot:\\Contoso.com\MyNamespace /Recurse /Full  
```  
  
## <a name="additional-references"></a>Références supplémentaires  
  
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  
-   [Dfsdiag](dfsdiag.md)  
  

