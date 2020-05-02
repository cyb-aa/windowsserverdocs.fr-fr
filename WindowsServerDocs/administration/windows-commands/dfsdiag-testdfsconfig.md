---
title: Dfsdiag TestDFSConfig
description: Rubrique de référence pour Dfsdiag TestDFSConfig, qui vérifie la configuration d’un espace de noms système de fichiers DFS (DFS).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 106aeeb9-ea79-4e6e-829c-eca06309bab2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 057b0662fddb7148837be16380d190cdb37382c5
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719591"
---
# <a name="dfsdiag-testdfsconfig"></a>Dfsdiag TestDFSConfig

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vérifie la configuration d’un espace de noms système de fichiers DFS (DFS) en effectuant les actions suivantes :  
  
-   Vérifie que le service d’espace de noms DFS est en cours d’exécution et que son type de démarrage est défini sur automatique sur tous les serveurs d’espaces de noms.  
  
-   Vérifie que la configuration du Registre DFS est cohérente entre les serveurs d’espaces de noms.  
  
-   Valide les dépendances suivantes sur les serveurs d’espaces de noms en cluster qui exécutent Windows Server 2008 ou une version ultérieure :  
  
    -   Dépendance de la ressource racine de l’espace de noms sur la ressource de nom réseau.  
  
    -   Dépendance de ressource de nom de réseau sur la ressource d’adresse IP.  
  
    -   Dépendance de la ressource racine de l’espace de noms sur la ressource de disque physique.

## <a name="syntax"></a>Syntaxe  
  
```  
dfsdiag /TestDFSConfig /DFSRoot:<namespace>  
```  
  
#### <a name="parameters"></a>Paramètres  
  
|       Paramètre       |               Description               |
|-----------------------|-----------------------------------------|
| /DFSRoot:`<namespace>` | Espace de noms (racine DFS) à diagnostiquer. |
  
## <a name="examples"></a>Exemples  
  
```  
dfsdiag /TestDFSConfig /DFSRoot:\\Contoso.com\MyNamespace  
```  
  
## <a name="additional-references"></a>Références supplémentaires  
  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  
-   [Dfsdiag](dfsdiag.md)  
  

