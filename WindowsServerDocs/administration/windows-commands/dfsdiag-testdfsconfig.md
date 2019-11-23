---
title: Dfsdiag TestDFSConfig
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 106aeeb9-ea79-4e6e-829c-eca06309bab2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8008e02d588edaa6fe7700a331c43f9680d89431
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378422"
---
# <a name="dfsdiag-testdfsconfig"></a>Dfsdiag TestDFSConfig

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vérifie la configuration d’un système de fichiers DFS \(espace de noms DFS\) en effectuant les actions suivantes :  
  
-   vérifie que le service d’espace de noms DFS est en cours d’exécution et que son type de démarrage est défini sur automatique sur tous les serveurs d’espaces de noms.  
  
-   vérifie que la configuration du Registre DFS est cohérente entre les serveurs d’espaces de noms.  
  
-   Valide les dépendances suivantes sur les serveurs d’espaces de noms en cluster qui exécutent Windows Server 2008 ou une version ultérieure :  
  
    -   Dépendance de la ressource racine de l’espace de noms sur la ressource de nom réseau.  
  
    -   Dépendance de ressource de nom de réseau sur la ressource d’adresse IP.  
  
    -   Dépendance de la ressource racine de l’espace de noms sur la ressource de disque physique.  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
dfsdiag /TestDFSConfig /DFSRoot:<namespace>  
```  
  
### <a name="parameters"></a>Paramètres  
  
|       Paramètre       |               Description               |
|-----------------------|-----------------------------------------|
| \/DFSRoot :<namespace> | Espace de noms \(\) racine DFS à diagnostiquer. |
  
## <a name="BKMK_Examples"></a>Illustre  
À TBD, tapez :  
  
```  
dfsdiag /TestDFSConfig /DFSRoot:\\Contoso.com\MyNamespace  
```  
  
## <a name="additional-references"></a>Références supplémentaires  
  
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  
-   [Dfsdiag](dfsdiag.md)  
  

