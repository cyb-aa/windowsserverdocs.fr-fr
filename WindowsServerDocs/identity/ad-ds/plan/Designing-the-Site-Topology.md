---
ms.assetid: eeb919de-e21e-48d8-8186-e42adec6933f
title: Conception de la topologie de Site
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 3f16b5b941ef9c3bd8f4bf742d432afc1b3f559a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="designing-the-site-topology"></a>Conception de la topologie de Site

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Une topologie de site du service annuaire est une représentation logique de votre réseau physique. Conception d’une topologie de site pour les Services de domaine ActiveDirectory (ADDS) implique la planification de l’emplacement des contrôleurs de domaine et les sites de la conception, des sous-réseaux, des liens de sites et ponts entre liens de site pour vous assurer de routage efficace du trafic de requête et la réplication.  
  
Conception d’une topologie de site vous aide à acheminer de manière efficace les requêtes du client et le trafic de réplication ActiveDirectory. Une topologie de site bien conçue permet à votre organisation de bénéficier des avantages suivants:  
  
-   Réduire le coût de la réplication des données ActiveDirectory.  
  
-   Réduire les tâches administratives qui sont nécessaires pour mettre à jour de la topologie de site.  
  
-   Planifier la réplication qui permet des emplacements avec les liaisons de réseau lente ou accès à distance pour répliquer des données ActiveDirectory pendant les heures creuses.  
  
-   Optimiser la capacité des ordinateurs clients de localiser les ressources le plus proche, tels que les contrôleurs de domaine et serveurs de système de fichiers distribués (DFS). Cette contribue à réduire le trafic réseau sur le réseau étendu lente liaisons réseau (étendu WAN), d’améliore les processus d’ouverture de session et la fermeture de session et accélérer les opérations de téléchargement de fichier.  
  
Avant de commencer à concevoir votre topologie de site, vous devez comprendre la structure de votre réseau physique. En outre, vous devez d’abord concevoir votre structure logique ActiveDirectory, y compris la hiérarchie d’administration, du plan de forêt et plan de domaine pour chaque forêt. Vous devez également effectuer la conception de votre infrastructure système DNS (Domain Name) pour les services ADDS. Pour plus d’informations sur la conception de votre structure logique ActiveDirectory et votre infrastructure DNS, voir [conception de la logique Structure pour Windows Server2008 ADDS](https://technet.microsoft.com/library/cc770806.aspx).  
  
Après avoir effectué votre conception de topologie de site, vous devez vérifier que vos contrôleurs de domaine répondent à la configuration matérielle requise pour Windows Server2008 Standard, Windows Server2008 Enterprise et Datacenter de Windows Server2008.  
  
## <a name="in-this-guide"></a>Dans ce guide  
  
-   [Présentation de topologie de Site ActiveDirectory](../../ad-ds/plan/Understanding-Active-Directory-Site-Topology.md)  
  
-   [Collecte des informations de réseau](../../ad-ds/plan/Collecting-Network-Information.md)  
  
-   [Planification du Placement des contrôleurs de domaine](../../ad-ds/plan/Planning-Domain-Controller-Placement.md)  
  
-   [Création d’une conception de Site](../../ad-ds/plan/Creating-a-Site-Design.md)  
  
-   [Création d’une conception de lien de Site](../../ad-ds/plan/Creating-a-Site-Link-Design.md)  
  
-   [Création d’une conception de pont lien de sites](../../ad-ds/plan/Creating-a-Site-Link-Bridge-Design.md)  
  
-   [Recherche de ressources supplémentaires pour la conception de topologie de Site Windows Server2008 ActiveDirectory](../../ad-ds/plan/Finding-Additional-Resources-for-Windows-Server-2008-Active-Directory-Site-Topology-Design.md)  
  


