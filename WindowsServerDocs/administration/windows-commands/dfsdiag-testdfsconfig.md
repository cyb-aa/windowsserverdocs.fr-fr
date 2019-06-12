---
title: Dfsdiag TestDFSConfig
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 922b78b87f3bb66765b87348a3bf136e14c9e837
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436128"
---
# <a name="dfsdiag-testdfsconfig"></a>Dfsdiag TestDFSConfig

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vérifie la configuration d’un système de fichiers distribués \(DFS\) espace de noms en effectuant les actions suivantes :  
  
-   vérifie que le service DFS Namespace est en cours d’exécution et que son type de démarrage est défini sur automatique sur tous les serveurs d’espace de noms.  
  
-   vérifie que la configuration du Registre DFS est cohérente entre les serveurs d’espace de noms.  
  
-   Valide les dépendances suivantes sur les serveurs d’espace de noms en cluster qui exécutent Windows Server 2008 ou version ultérieure :  
  
    -   Dépendance de ressource Namespace racine sur la ressource de nom réseau.  
  
    -   Dépendance de ressource de nom de ressource d’adresse IP du réseau.  
  
    -   Dépendance de ressource Namespace racine sur la ressource de disque physique.  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
dfsdiag /TestDFSConfig /DFSRoot:<namespace>  
```  
  
### <a name="parameters"></a>Paramètres  
  
|       Paramètre       |               Description               |
|-----------------------|-----------------------------------------|
| \/RacineDFS :<namespace> | L’espace de noms \(racine DFS\) à diagnostiquer. |
  
## <a name="BKMK_Examples"></a>Exemples  
À TBD, tapez :  
  
```  
dfsdiag /TestDFSConfig /DFSRoot:\\Contoso.com\MyNamespace  
```  
  
## <a name="additional-references"></a>Références supplémentaires  
  
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

