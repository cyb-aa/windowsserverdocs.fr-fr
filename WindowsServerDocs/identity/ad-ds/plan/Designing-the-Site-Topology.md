---
ms.assetid: eeb919de-e21e-48d8-8186-e42adec6933f
title: Conception de la topologie de site
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e1d9323ceda478369973f959687d46c9ca3cb88f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888490"
---
# <a name="designing-the-site-topology"></a>Conception de la topologie de site

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Une topologie de site du service annuaire est une représentation logique de votre réseau physique. Conception d’une topologie de site pour les Services de domaine Active Directory (AD DS) implique la planification de placement des contrôleurs de domaine et les sites de conception, des sous-réseaux, des liens de sites et ponts entre liens de site pour garantir un routage efficace du trafic de requêtes et de réplication.  
  
Conception d’une topologie de site vous permet de router efficacement des requêtes des clients et le trafic de réplication Active Directory. Une topologie de site bien conçue permet à votre organisation de bénéficier des avantages suivants :  
  
-   Réduire le coût de réplication des données Active Directory.  
  
-   Réduire les efforts d’administration qui sont requises pour gérer la topologie de site.  
  
-   Planifier la réplication qui permet des emplacements avec des liaisons réseau lentes ou à distance pour répliquer les données d’Active Directory pendant les heures creuses.  
  
-   Optimiser la capacité des ordinateurs clients de localiser les ressources le plus proche, tels que les contrôleurs de domaine et serveurs de système de fichiers distribués (DFS, Distributed File System). Contribue à réduire le trafic réseau sur une large zone lente liaisons réseau (étendu WAN), améliorer les processus d’ouverture de session et de fermeture de session et d’accélérer les opérations de téléchargement de fichier.  
  
Avant de commencer à concevoir votre topologie de site, vous devez comprendre la structure de votre réseau physique. En outre, vous devez tout d’abord concevoir votre structure Active Directory logique, y compris la hiérarchie d’administration, le plan de la forêt et le plan de domaine pour chaque forêt. Vous devez également effectuer la conception de votre infrastructure système DNS (Domain Name) pour AD DS. Pour plus d’informations sur la conception de votre structure Active Directory logique et une infrastructure DNS, consultez [concevoir la logique Structure pour Windows Server 2008 AD DS](https://technet.microsoft.com/library/cc770806.aspx).  
  
Après avoir terminé votre conception de topologie de site, vous devez vérifier que vos contrôleurs de domaine répondent à la configuration matérielle requise pour Windows Server 2008 Standard, Windows Server 2008 Enterprise et Windows Server 2008 Datacenter.  
  
## <a name="in-this-guide"></a>Dans ce guide  
  
-   [Comprendre la topologie de Site Active Directory](../../ad-ds/plan/Understanding-Active-Directory-Site-Topology.md)  
  
-   [Collecte d’informations réseau](../../ad-ds/plan/Collecting-Network-Information.md)  
  
-   [Planification du Placement des contrôleurs de domaine](../../ad-ds/plan/Planning-Domain-Controller-Placement.md)  
  
-   [Création d’un Site](../../ad-ds/plan/Creating-a-Site-Design.md)  
  
-   [Création d’une conception de lien de Site](../../ad-ds/plan/Creating-a-Site-Link-Design.md)  
  
-   [Création d’un pont lien de sites](../../ad-ds/plan/Creating-a-Site-Link-Bridge-Design.md)  
  
-   [Recherche de ressources supplémentaires pour la conception de topologie de Site Windows Server 2008 Active Directory](../../ad-ds/plan/Finding-Additional-Resources-for-Windows-Server-2008-Active-Directory-Site-Topology-Design.md)  
  


