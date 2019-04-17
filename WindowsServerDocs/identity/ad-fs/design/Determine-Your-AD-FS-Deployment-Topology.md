---
ms.assetid: f67b0bc9-e5af-4891-9da0-d9be539af42d
title: "Déterminer votre topologie de déploiement ADFS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3300c16be6d516d7ec0bf4d0c3a025e59e6126b6
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="determine-your-ad-fs-deployment-topology"></a>Déterminer votre topologie de déploiement ADFS

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

La première étape de planification d’un déploiement d’ActiveDirectory Federation Services \(ADFS\) est pour déterminer la topologie de déploiement pour répondre à la seule \(SSO\) connexion sur les besoins de votre organisation. Les rubriques de cette section décrivent les différentes topologies de déploiement que vous pouvez utiliser avec ADFS. Elles décrivent également les avantages et les limitations liées à chaque topologie de déploiement afin que vous pouvez sélectionner la topologie la plus appropriée à vos besoins spécifiques.  
  
Avant de lire cette rubrique de topologie de déploiement, nous vous recommandons abord d'effectuer les tâches dans l’ordre indiqué dans le tableau suivant.  
  
|Tâche recommandée|Description|Référence|  
|--------------------|---------------|-------------|  
|Examinez comment les données ADFS sont stockées et répliquées vers d’autres serveurs de fédération dans une batterie de serveurs de fédération.|Comprendre l’objectif d’et les méthodes de réplication qui peuvent être utilisées pour les données sous-jacentes qui sont stockées dans la base de données de configuration ADFS. Cette rubrique présente les concepts de la base de données de configuration et décrit les types de base de deux données: \(WID\) base de données interne Windows et MicrosoftSQLServer.|[Le rôle de la base de données de Configuration ADFS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)|  
|Sélectionnez le type de base de données de configuration ADFS que vous allez déployer dans votre organisation.|Passez en revue les avantages et les limites qui sont associés à l’aide de WID ou SQLServer en tant que la base de configuration ADFS, ainsi que les différents scénarios d’application pris en charge.|[Considérations sur la topologie déploiement ADFS](AD-FS-Deployment-Topology-Considerations.md)|  
  
> [!NOTE]  
> Pour implémenter la redondance de base, l’équilibrage de charge et la possibilité de faire évoluer le Service de fédération \(if required\), nous vous recommandons de déployer au moins deux serveurs de fédération par batterie de serveurs de fédération pour tous les environnements de production, quel que soit le type de base de données que vous utiliserez.  
  
Lorsque vous avez consulté le contenu dans le tableau précédent, passez aux rubriques suivantes dans cette section:  
  
-   [Serveur de fédération autonome à l’aide de WID](Stand-Alone-Federation-Server-Using-WID.md)  
  
-   [Batterie de serveurs de fédération avec WID](Federation-Server-Farm-Using-WID-2012.md)  
  
-   [Batterie de serveurs de fédération avec WID et proxys](Federation-Server-Farm-Using-WID-and-Proxies-2012.md)  
  
-   [Batterie de serveurs de fédération à l’aide de SQL Server](Federation-Server-Farm-Using-SQL-Server-2012.md)  
  
Après avoir terminé la sélection de votre topologie de déploiement ADFS, nous vous recommandons de consulter la rubrique [planification de la capacité du serveur ADFS](Planning-for-AD-FS-Server-Capacity.md) pour déterminer le nombre recommandé de serveurs que vous devez déployer pour prendre en charge cette topologie.  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception ADFS dans Windows Server2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

