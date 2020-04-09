---
ms.assetid: f67b0bc9-e5af-4891-9da0-d9be539af42d
title: Déterminer votre topologie de déploiement d'AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 0336c54357745217c30e14afc0824f97d476b45d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855152"
---
# <a name="determine-your-ad-fs-deployment-topology"></a>Déterminer votre topologie de déploiement d'AD FS

La première étape de la planification d’un déploiement de Services ADFS \(AD FS\) consiste à déterminer la topologie de déploiement appropriée pour répondre à la\-de l’authentification unique sur \(les besoins\) de l’authentification unique de votre organisation. Les rubriques de cette section décrivent les différentes topologies de déploiement que vous pouvez utiliser avec AD FS. Elles expliquent également les avantages et les limites de chaque topologie de déploiement, ce qui vous permet de choisir la topologie la plus appropriée pour votre entreprise.  
  
Avant de lire ces rubriques, prenez le temps d'effectuer les tâches mentionnées dans le tableau suivant, dans l'ordre indiqué.  
  
|Tâche recommandée|Description|Référence|  
|--------------------|---------------|-------------|  
|Passez en revue la façon dont AD FS données sont stockées et répliquées sur d’autres serveurs de Fédération dans une batterie de serveurs de Fédération.|Assimilez la finalité de la base de données de configuration AD FS, ainsi que les méthodes de réplication utilisables pour les données sous-jacentes qui y sont stockées. Cette rubrique présente les concepts de la base de données de configuration et décrit les deux types de base de données : base de données interne Windows \(WID\) et Microsoft SQL Server.|[Rôle de la base de données de configuration AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)|  
|Sélectionner le type de base de données de configuration AD FS à déployer dans votre organisation.|Passez en revue les avantages et les limites de l'utilisation de la base de données interne ou de SQL Server comme base de données de configuration AD FS, ainsi que les différents scénarios d'application pris en charge.|[Considérations sur la topologie du déploiement d’AD FS](AD-FS-Deployment-Topology-Considerations.md)|  
  
> [!NOTE]  
> Pour implémenter la redondance de base, l’équilibrage de charge et l’option de mise à l’échelle des service FS (Federation Service) \(si nécessaire\), nous vous recommandons de déployer au moins deux serveurs de Fédération par batterie de serveurs de Fédération pour tous les environnements de production, quel que soit le type de base de données que vous allez utiliser.  
  
Après avoir consulté le tableau précédent, passez aux rubriques suivantes de cette section :  
  
-   [Serveur de fédération autonome utilisant la base de données interne Windows](Stand-Alone-Federation-Server-Using-WID.md)  
  
-   [Batterie de serveurs de fédération utilisant la base de données interne Windows](Federation-Server-Farm-Using-WID-2012.md)  
  
-   [Batterie de serveurs de fédération utilisant la base de données interne Windows et des proxys](Federation-Server-Farm-Using-WID-and-Proxies-2012.md)  
  
-   [Batterie de serveurs de fédération utilisant SQL Server](Federation-Server-Farm-Using-SQL-Server-2012.md)  
  
Une fois que vous avez fini de sélectionner votre topologie de déploiement AD FS, nous vous recommandons de consulter la rubrique [Planning for AD FS Server Capacity](Planning-for-AD-FS-Server-Capacity.md) pour déterminer le nombre recommandé de serveurs que vous devrez déployer pour prendre en charge cette topologie.  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

