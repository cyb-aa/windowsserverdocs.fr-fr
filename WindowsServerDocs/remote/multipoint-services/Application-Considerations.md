---
title: Considérations relatives aux applications
description: Informations de compatibilité pour les applications sur MultiPoint services
ms.custom: na
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 445e6184-4e1e-4f10-ad3c-042f2a6c2f5f
author: evaseydl
manager: scottman
ms.author: evas
ms.date: 08/04/2016
ms.openlocfilehash: 21531273b1dd6d643df3f816a880a0efb3117c70
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405127"
---
# <a name="application-considerations"></a>Considérations relatives aux applications
  
## <a name="application-compatibility"></a>Compatibilité des applications

Toutes les applications que vous souhaitez exécuter sur un système MultiPoint services doivent respecter les conditions suivantes :
  
- Elle doit être installée et exécutée sur Windows Server 2016 
- Elle doit prendre en charge les sessions afin que chaque utilisateur puisse exécuter une instance de l’application dans un système MultiPoint.
  
Si l’application spécifie cette exigence, nous vous recommandons d’essayer d’installer l’application et de l’utiliser dans une session Bureau à distance. 

## <a name="addressing-application-compatibility-problems"></a>Résolution des problèmes de compatibilité des applications  
MultiPoint services offre la possibilité d’associer des stations à des instances complètes des éditions Windows 10 entreprise exécutant virtuellement sur le même ordinateur hôte. Pour les applications critiques qui n’exécutent pas plusieurs instances pour plusieurs utilisateurs, ou qui ne sont pas installées sur un système d’exploitation 64 bits, il peut s’agir d’une solution. Le déploiement de postes de travail de cette manière requiert l’utilisation de l’onglet bureaux virtuels dans le gestionnaire MultiPoint pour effectuer les opérations suivantes :  
  
-   Activer les bureaux virtuels  
-   Créer un modèle de bureau  
-   Personnaliser le modèle avec l’application à problème  
-   Associer des stations au modèle personnalisé  

Chaque station démarrant à partir du même modèle, toutes les modifications sont effacées à chaque démarrage de l’ordinateur.  
  
>[!NOTE] 
>Il est important de vérifier les conditions de licence pour les applications que vous souhaitez exécuter sur une MultiPoint. Bien que vous installiez une seule application de copie, peut nécessiter une licence par utilisateur.  
  
