---
ms.assetid: eeb919de-e21e-48d8-8186-e42adec6933f
title: Conception de la topologie de site
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: e7b6267946217d5c5fb57496eb6bf54911b61e8a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822595"
---
# <a name="designing-the-site-topology"></a>Conception de la topologie de site

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Une topologie de site de service d’annuaire est une représentation logique de votre réseau physique. La conception d’une topologie de site pour Active Directory Domain Services (AD DS) implique la planification de l’emplacement des contrôleurs de domaine et la conception de sites, de sous-réseaux, de liens de sites et de ponts entre liens de sites pour garantir un routage efficace du trafic de requêtes et de réplications.  
  
La conception d’une topologie de site vous permet de router efficacement les requêtes des clients et de Active Directory le trafic de réplication. Une topologie de site bien conçue permet à votre organisation de bénéficier des avantages suivants :  
  
-   Réduire le coût de la réplication des données Active Directory.  
  
-   Réduire les efforts administratifs requis pour gérer la topologie du site.  
  
-   Planifier la réplication qui active les emplacements avec des liaisons réseau lentes ou d’accès à distance pour répliquer Active Directory données pendant les heures creuses.  
  
-   Optimisez la capacité des ordinateurs clients à localiser les ressources les plus proches, telles que les contrôleurs de domaine et les serveurs de système de fichiers DFS (DFS). Cela permet de réduire le trafic réseau sur les liaisons de réseau étendu (WAN) lentes, d’améliorer les processus d’ouverture et de fermeture de session et d’accélérer les opérations de téléchargement de fichiers.  
  
Avant de commencer à concevoir la topologie de votre site, vous devez comprendre la structure de votre réseau physique. En outre, vous devez d’abord concevoir votre structure logique Active Directory, y compris la hiérarchie administrative, le plan de forêt et le plan de domaine pour chaque forêt. Vous devez également effectuer la conception de votre infrastructure DNS (Domain Name System) pour AD DS. Pour plus d’informations sur la conception de votre structure logique et de votre infrastructure DNS Active Directory, consultez [conception de la structure logique pour Windows Server 2008 AD DS](https://technet.microsoft.com/library/cc770806.aspx).  
  
Une fois que vous avez terminé la conception de la topologie de votre site, vous devez vérifier que vos contrôleurs de domaine satisfont à la configuration matérielle requise pour Windows Server 2008 standard, Windows Server 2008 Enterprise et Windows Server 2008 Datacenter.  
  
## <a name="in-this-guide"></a>Dans ce guide  
  
-   [Présentation de la topologie de site Active Directory](../../ad-ds/plan/Understanding-Active-Directory-Site-Topology.md)  
  
-   [Collecte d’informations réseau](../../ad-ds/plan/Collecting-Network-Information.md)  
  
-   [Planification de l’emplacement des contrôleurs de domaine](../../ad-ds/plan/Planning-Domain-Controller-Placement.md)  
  
-   [Création d’une conception de site](../../ad-ds/plan/Creating-a-Site-Design.md)  
  
-   [Création d’une conception de lien de sites](../../ad-ds/plan/Creating-a-Site-Link-Design.md)  
  
-   [Création d’une conception de pont lien de sites](../../ad-ds/plan/Creating-a-Site-Link-Bridge-Design.md)  
  
-   [Recherche de ressources supplémentaires pour la conception de la topologie de site Active Directory de Windows Server 2008](../../ad-ds/plan/Finding-Additional-Resources-for-Windows-Server-2008-Active-Directory-Site-Topology-Design.md)  
  


