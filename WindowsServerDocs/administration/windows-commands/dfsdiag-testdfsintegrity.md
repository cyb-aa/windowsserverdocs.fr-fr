---
title: Dfsdiag TestDFSIntegrity
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 9a79e034f7c60be89266eb29dcd69e8f73b2aafe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837090"
---
# <a name="dfsdiag-testdfsintegrity"></a>Dfsdiag TestDFSIntegrity

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vérifie l’intégrité du système de fichiers distribués \(DFS\) espace de noms en effectuant les tests suivants :  
  
-   Vérifie la corruption des métadonnées DFS ou des incohérences entre les contrôleurs de domaine.  
  
-   Valide la configuration d’accès\-en fonction d’énumération pour vous assurer qu’il est cohérent entre les métadonnées DFS et le partage de serveur d’espace de noms.  
  
-   Détecte les dossiers DFS qui se chevauchent \(liens\), des dossiers en double et des dossiers avec chevauchement des cibles de dossier.  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
dfsdiag /TestDFSIntegrity /DFSRoot:<DFS root path> [/Recurse] [/Full]  
```  
  
### <a name="parameters"></a>Paramètres  
  
|Paramètre|Description|  
|-------|--------|  
|\/RacineDFS :<DFS root path>|L’espace de noms DFS à diagnostiquer.|  
|\/Recurse|Effectue le test, notamment que l’espace de noms interlinks.|  
|\/complet|vérifie la cohérence du partage et de configuration de côté les ACL NTFS et le client sur toutes les cibles de dossier. Il vérifie également que la propriété en ligne est définie.|  
  
## <a name="BKMK_Examples"></a>Exemples  
À TBD, tapez :  
  
```  
dfsdiag /TestDFSIntegrity /DFSRoot:\\Contoso.com\MyNamespace /Recurse /Full  
```  
  
## <a name="additional-references"></a>Références supplémentaires  
  
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

