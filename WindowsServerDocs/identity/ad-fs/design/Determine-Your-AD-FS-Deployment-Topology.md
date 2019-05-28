---
ms.assetid: f67b0bc9-e5af-4891-9da0-d9be539af42d
title: Déterminer votre topologie de déploiement d'AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 06cc4bd37905f6bb7afbc513ffce216104654aba
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191479"
---
# <a name="determine-your-ad-fs-deployment-topology"></a>Déterminer votre topologie de déploiement d'AD FS

La première étape de planification d’un déploiement d’Active Directory Federation Services \(AD FS\) consiste à déterminer la topologie de déploiement pour répondre à l’authentification unique\-sur \(SSO\) a besoin de votre organisation. Les rubriques de cette section décrivent les différentes topologies de déploiement que vous pouvez utiliser avec AD FS. Elles expliquent également les avantages et les limites de chaque topologie de déploiement, ce qui vous permet de choisir la topologie la plus appropriée pour votre entreprise.  
  
Avant de lire ces rubriques, prenez le temps d'effectuer les tâches mentionnées dans le tableau suivant, dans l'ordre indiqué.  
  
|Tâche recommandée|Description|Référence|  
|--------------------|---------------|-------------|  
|Examinez comment les données AD FS sont stockées et répliquées sur d’autres serveurs de fédération dans une batterie de serveurs de fédération.|Assimilez la finalité de la base de données de configuration AD FS, ainsi que les méthodes de réplication utilisables pour les données sous-jacentes qui y sont stockées. Cette rubrique présente les concepts de la base de données de configuration et décrit les deux types de base de données : Base de données interne Windows \(WID\) et Microsoft SQL Server.|[Rôle de la base de données de configuration AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)|  
|Sélectionner le type de base de données de configuration AD FS à déployer dans votre organisation.|Passez en revue les avantages et les limites de l'utilisation de la base de données interne ou de SQL Server comme base de données de configuration AD FS, ainsi que les différents scénarios d'application pris en charge.|[Considérations sur la topologie du déploiement d’AD FS](AD-FS-Deployment-Topology-Considerations.md)|  
  
> [!NOTE]  
> Pour implémenter la redondance de base, l’équilibrage de charge et l’option Mettre à l’échelle le Service de fédération \(si nécessaire\), nous vous recommandons de déployer au moins deux serveurs de fédération par batterie de serveurs de fédération pour tous les environnements de production, quel que soit le type de base de données que vous allez utiliser.  
  
Après avoir consulté le tableau précédent, passez aux rubriques suivantes de cette section :  
  
-   [Serveur de fédération autonome utilisant la base de données interne Windows](Stand-Alone-Federation-Server-Using-WID.md)  
  
-   [Batterie de serveurs de fédération utilisant la base de données interne Windows](Federation-Server-Farm-Using-WID-2012.md)  
  
-   [Batterie de serveurs de fédération utilisant la base de données interne Windows et des proxys](Federation-Server-Farm-Using-WID-and-Proxies-2012.md)  
  
-   [Batterie de serveurs de fédération utilisant SQL Server](Federation-Server-Farm-Using-SQL-Server-2012.md)  
  
Une fois que vous avez sélectionné votre topologie de déploiement AD FS, nous vous recommandons de consulter la rubrique [planification de la capacité de serveur AD FS](Planning-for-AD-FS-Server-Capacity.md) pour déterminer le nombre recommandé de serveurs dont vous aurez besoin pour déployer pour prendre en charge de cette topologie.  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

