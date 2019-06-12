---
ms.assetid: 4ef052f0-61a9-4912-b780-5c96187c850f
title: Considérations sur la topologie du déploiement d'AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cf646dedef85add8607c7940275e3c3fae90a661
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445342"
---
# <a name="ad-fs-deployment-topology-considerations"></a>Considérations sur la topologie du déploiement d'AD FS

Cette rubrique décrit les considérations importantes pour vous aider à planifier et concevoir les Services de fédération Active Directory \(AD FS\) topologie de déploiement à utiliser dans votre environnement de production. Cette rubrique est un point de départ pour examiner et évaluer les aspects qui influent sur les fonctionnalités ou capacités disponibles après le déploiement d’AD FS. Par exemple, en fonction de la base de données type que vous choisissez de stocker la base de données de configuration AD FS finalement déterminera si vous pouvez implémenter certaines Security Assertion Markup Language \(SAML\) fonctionnalités qui nécessitent SQL Serveur.  

## <a name="determining-which-type-of-ad-fs-configuration-database-to-use"></a>Détermination du type de base de données de configuration AD FS à utiliser  
AD FS utilise une base de données pour stocker la configuration et, dans certains cas, les données transactionnelles liées au Service de fédération. Vous pouvez utiliser le logiciel AD FS pour sélectionner intégrées\-dans la base de données interne Windows \(WID\) ou Microsoft SQL Server 2005 ou ultérieure pour stocker les données dans le Service de fédération.  

En règle générale, les deux types de base de données sont relativement équivalents. Toutefois, il existe certaines différences à connaître avant de commencer la lecture plus d’informations sur les différentes topologies de déploiement que vous pouvez utiliser avec AD FS. Le tableau suivant décrit les différences de fonctionnalités prises en charge entre une base de données WID et une base de données SQL Server.  

Fonctionnalités d’AD FS  

|Fonctionnalité|Prise en charge par la base de données interne Windows ?|Prise en charge par SQL Server ?|Pour en savoir plus sur cette fonctionnalité|  
|-----------|---------------------|----------------------------|---------------------------------------|  
|Déploiement d'une batterie de serveurs de fédération|Oui, avec une limite de 30 serveurs de fédération pour chaque batterie de serveurs|Oui. Vous pouvez déployer un nombre illimité de serveurs de fédération dans une batterie.|[Déterminer votre topologie de déploiement d’AD FS](Determine-Your-AD-FS-Deployment-Topology.md)|  
|Résolution d’artefacts SAML **Remarque :** Cette fonctionnalité n'est pas nécessaire pour les scénarios Microsoft Online Services, Microsoft Office 365, Microsoft Exchange ou Microsoft Office SharePoint.|Non|Oui|[Rôle de la base de données de configuration AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[Meilleures pratiques pour sécuriser la planification et le déploiement d’AD FS](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)|  
|SAML\/WS\-détection de relecture de jetons de fédération|Non|Oui|[Rôle de la base de données de configuration AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[Meilleures pratiques pour sécuriser la planification et le déploiement d’AD FS](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)|  

Fonctionnalités de base de données  

|Fonctionnalité|Prise en charge par la base de données interne Windows ?|Prise en charge par SQL Server ?|Pour en savoir plus sur cette fonctionnalité|  
|-----------|---------------------|----------------------------|---------------------------------------|  
|Redondance de base de données à l’aide d’extraire la réplication, où un ou plusieurs serveurs hébergeant une lecture\-seule copie des modifications de requête de base de données qui sont effectuées sur un serveur source qui héberge une lecture\/écrire la copie de la base de données|Oui|Non|[Rôle de la base de données de configuration AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)|  
|Redondance de base de données à l’aide de haute\-des solutions de disponibilité, telles que le basculement de clustering ou de mise en miroir \(au niveau de la couche base de données uniquement\) **Remarque :** Toutes les topologies de déploiement d’AD FS prend en charge le clustering au niveau de la couche de service AD FS.|Non|Oui|[Rôle de la base de données de configuration AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[Vue d’ensemble des Solutions de haute disponibilité](https://go.microsoft.com/fwlink/?LinkId=179853)|  

### <a name="sql-server-considerations"></a>Considérations concernant SQL Server  
Vous devez envisager les facteurs suivants si vous sélectionnez SQL Server en tant que la base de données de configuration pour votre déploiement AD FS.  

-   **Fonctionnalités SAML et leur impact sur la taille et sur la croissance de la base de données**. Lors de la résolution d’artefacts SAML ou de fonctionnalités de détection de relecture de jetons SAML sont activées, AD FS stocke des informations dans la base de données de configuration de SQL Server pour chaque jeton AD FS qui est émise. La croissance de la base de données SQL Server à la suite de cette activité n’est pas considérée comme significative et dépend de la période de rétention de relecture des jetons configurés. Chaque enregistrement d’artefact a une taille d’environ 30 kilo-octets \(Ko\).  

-   **Nombre de serveurs nécessaires pour votre déploiement**. Vous devez ajouter au moins un serveur supplémentaire \(et le nombre total de serveurs nécessaires pour déployer votre infrastructure AD FS\) qui agira comme un hôte dédié de l’instance de SQL Server. Si vous envisagez d’utiliser la mise en miroir ou de clustering de basculement pour fournir une tolérance de panne et d’évolutivité pour la base de données de configuration de SQL Server, un minimum de deux serveurs SQL Server est requis.  

### <a name="how-the-configuration-database-type-you-select-may-impact-hardware-resources"></a>Impact du type de base de données de configuration sélectionné sur les ressources matérielles  
L’impact sur les ressources matérielles sur un serveur de fédération qui est déployé dans une batterie de serveurs à l’aide de WID par opposition à un serveur de fédération qui est déployé dans une batterie de serveurs à l’aide de la base de données SQL Server n’est pas significatif. Toutefois, il est important de savoir que lorsque vous utilisez WID pour la batterie de serveurs, chaque serveur de fédération dans cette batterie de serveurs doit stocker, gérer et mettre à jour les modifications de réplication pour sa copie locale de la base de données de configuration AD FS tout en continuant également fournir la normale opérations nécessaires au Service de fédération.  

En comparaison, les serveurs de fédération sont déployés dans une batterie de serveurs qui utilise la base de données SQL Server ne contiennent pas nécessairement une instance locale de la base de données de configuration AD FS. Ils sont donc susceptibles de solliciter un peu moins les ressources matérielles.  

## <a name="verifying-that-your-production-environment-can-support-an-ad-fs-deployment"></a>Vérification que votre environnement de production peut prendre en charge un déploiement d'AD FS  
Outre les serveurs de fédération que vous allez déployer, et selon la façon dont votre environnement de production est configuré, les serveurs suivants être amenés à fournir l’infrastructure nécessaire pour prendre en charge de votre nouveau déploiement d’AD FS :  

-   Contrôleur de domaine Active Directory  

-   Autorité de certification \(autorité de certification\)  

-   Serveur web destiné à héberger les métadonnées de fédération  

-   Équilibrage de charge réseau \(NLB\)  

## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
