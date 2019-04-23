---
title: Considérations relatives aux applications
description: Informations de compatibilité pour les applications sur MultiPoint Services
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 400f87c09f1b2e897d67f94e9b7ac12ae0a1e799
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839830"
---
# <a name="application-considerations"></a>Considérations relatives aux applications
  
## <a name="application-compatibility"></a>Compatibilité des applications

N’importe quelle application que vous souhaitez exécuter sur un système MultiPoint Services doit remplir les conditions suivantes :
  
- Il doit installer et exécuter sur Windows Server 2016 
- Il doit être conscient de la session pour chaque utilisateur peut exécuter une instance de l’application dans un système MultiPoint.
  
Si l’application ne spécifie pas cette exigence, nous recommandons à essayer d’installer l’application et l’utiliser dans une session Bureau à distance. 

## <a name="addressing-application-compatibility-problems"></a>Adressage des problèmes de compatibilité des applications  
MultiPoint Services offre la possibilité d’associer des stations avec les instances complètes des éditions de Windows 10 entreprise en cours d’exécution pratiquement sur le même ordinateur hôte. Pour les applications critiques qui ne seront exécutera pas plusieurs instances pour plusieurs utilisateurs, ou n’installera pas sur un système d’exploitation 64 bits, cela peut être une solution. Déploiement de postes de travail de cette façon nécessite à l’aide de l’onglet de bureaux virtuels dans le gestionnaire MultiPoint pour :  
  
-   Activer les bureaux virtuels  
-   Créer un modèle de bureau  
-   Personnaliser le modèle avec l’application de problème  
-   Associer des stations avec le modèle personnalisé  

Chaque station démarre à partir du même modèle, donc toutes les modifications sont effacées à chaque démarrage de l’ordinateur.  
  
>[!NOTE] 
>Il est important de vérifier les licences requises pour les applications que vous souhaitez exécuter sur un MultiPoint. Bien que vous installez les applications une seule copie peuvent nécessiter de licence par utilisateur.  
  
