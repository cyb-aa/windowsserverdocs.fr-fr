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
ms.openlocfilehash: bbd3ec26e5fb0ce9857f2c9e5321300fb835b303
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834590"
---
# <a name="ad-fs-deployment-topology-considerations"></a>Considérations sur la topologie du déploiement d'AD FS

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique décrit les considérations importantes pour vous aider à planifier et concevoir les Services de fédération Active Directory \(AD FS\) topologie de déploiement à utiliser dans votre environnement de production. Cette rubrique est un point de départ pour examiner et évaluer les aspects qui influent sur les fonctionnalités ou capacités disponibles après le déploiement d’AD FS. Par exemple, en fonction de la base de données type que vous choisissez de stocker la base de données de configuration AD FS finalement déterminera si vous pouvez implémenter certaines Security Assertion Markup Language \(SAML\) fonctionnalités qui nécessitent SQL Serveur.  
  
## <a name="determining-which-type-of-adfs-configuration-database-to-use"></a>Détermination du type de base de données de configuration AD FS à utiliser  
AD FS utilise une base de données pour stocker la configuration et, dans certains cas, les données transactionnelles liées au Service de fédération. Vous pouvez utiliser le logiciel AD FS pour sélectionner intégrées\-dans la base de données interne Windows \(WID\) ou Microsoft SQL Server 2005 ou ultérieure pour stocker les données dans le Service de fédération.  
  
En règle générale, les deux types de base de données sont relativement équivalents. Toutefois, il existe certaines différences à connaître avant de commencer la lecture plus d’informations sur les différentes topologies de déploiement que vous pouvez utiliser avec AD FS. Le tableau suivant décrit les fonctionnalités et indique si elles sont prises en charge dans une base de données interne Windows et dans une base de données SQL Server.  
  
Fonctionnalités d’AD FS  
  
|Fonctionnalité|Prise en charge par la base de données interne Windows ?|Prise en charge par SQL Server ?|Pour en savoir plus sur cette fonctionnalité|  
|-----------|---------------------|----------------------------|---------------------------------------|  
|Déploiement d'une batterie de serveurs de fédération|Oui, avec une limite de 30 serveurs de fédération pour chaque batterie de serveurs|Oui. Vous pouvez déployer un nombre illimité de serveurs de fédération dans une batterie.|[Déterminer votre topologie de déploiement AD FS](Determine-Your-AD-FS-Deployment-Topology.md)|  
|Résolution d’artefacts SAML **Remarque :** Cette fonctionnalité n'est pas nécessaire pour les scénarios Microsoft Online Services, Microsoft Office 365, Microsoft Exchange ou Microsoft Office SharePoint.|Non|Oui|[Le rôle de la base de données de Configuration AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[Meilleures pratiques pour sécuriser la planification et déploiement d’AD FS](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)|  
|SAML\/WS\-détection de relecture de jetons de fédération|Non|Oui|[Le rôle de la base de données de Configuration AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[Meilleures pratiques pour sécuriser la planification et déploiement d’AD FS](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)|  
  
Fonctionnalités de base de données  
  
|Fonctionnalité|Prise en charge par la base de données interne Windows ?|Prise en charge par SQL Server ?|Pour en savoir plus sur cette fonctionnalité|  
|-----------|---------------------|----------------------------|---------------------------------------|  
|Redondance de base de données à l’aide d’extraire la réplication, où un ou plusieurs serveurs hébergeant une lecture\-seule copie des modifications de requête de base de données qui sont effectuées sur un serveur source qui héberge une lecture\/écrire la copie de la base de données|Oui|Non|[Le rôle de la base de données de Configuration AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)|  
|Redondance de base de données à l’aide de haute\-des solutions de disponibilité, telles que le basculement de clustering ou de mise en miroir \(au niveau de la couche base de données uniquement\) **Remarque :** Toutes les topologies de déploiement d’AD FS prend en charge le clustering au niveau de la couche de service AD FS.|Non|Oui|[Le rôle de la base de données de Configuration AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[Vue d’ensemble des Solutions de haute disponibilité](https://go.microsoft.com/fwlink/?LinkId=179853)|  
  
### <a name="sql-server-considerations"></a>Considérations concernant SQL Server  
Vous devez prendre en compte les facteurs suivants si vous choisissez SQL Server comme base de données de configuration pour le déploiement d'AD FS.  
  
-   **Fonctionnalités SAML et leur impact sur la taille et sur la croissance de la base de données**. Quand la résolution d'artefacts SAML ou la détection de relecture de jetons SAML est activée, AD FS stocke des informations dans la base de données de configuration SQL Server pour chaque jeton AD FS émis. La croissance de la base de données SQL Server qu'entraîne cette activité n'est pas considérée comme significative et dépend de la période de rétention de la relecture de jetons configurée. Chaque enregistrement d’artefact a une taille d’environ 30 kilo-octets \(Ko\).  
  
-   **Nombre de serveurs nécessaires pour votre déploiement**. Vous devez ajouter au moins un serveur supplémentaire \(et le nombre total de serveurs nécessaires pour déployer votre infrastructure AD FS\) qui agira comme un hôte dédié de l’instance de SQL Server. Si vous envisagez d'utiliser le clustering avec basculement ou la mise en miroir pour fournir les fonctionnalités de tolérance de panne et d'extensibilité pour la base de données de configuration SQL Server, au moins deux serveurs SQL sont nécessaires.  
  
### <a name="how-the-configuration-database-type-you-select-may-impact-hardware-resources"></a>Impact du type de base de données de configuration sélectionné sur les ressources matérielles  
L'impact sur les ressources matérielles d'un serveur de fédération déployé dans une batterie utilisant la base de données interne Windows n'est pas significatif par rapport à un serveur de fédération déployé dans une batterie utilisant la base de données SQL Server. Toutefois, gardez à l'esprit que quand vous utilisez la base de données interne Windows pour la batterie, chaque serveur de fédération appartenant à celle-ci doit stocker et gérer les changements de réplication pour sa copie locale de la base de données de configuration AD FS tout en effectuant les opérations normales nécessaires au service de fédération.  
  
À l'opposé, les serveurs de fédération déployés dans une batterie qui utilise la base de données SQL Server ne contiennent pas nécessairement une instance locale de la base de données de configuration AD FS. Ils sont donc susceptibles de solliciter un peu moins les ressources matérielles.  
  
## <a name="verifying-that-your-production-environment-can-support-an-ad-fs-deployment"></a>Vérification que votre environnement de production peut prendre en charge un déploiement d'AD FS  
Outre les serveurs de fédération que vous allez déployer, et selon la façon dont votre environnement de production est configuré, les serveurs suivants être amenés à fournir l’infrastructure nécessaire pour prendre en charge de votre nouveau déploiement d’AD FS :  
  
-   Contrôleur de domaine Active Directory  
  
-   Autorité de certification \(autorité de certification\)  
  
-   Serveur web destiné à héberger les métadonnées de fédération  
  
-   Équilibrage de charge réseau \(NLB\)  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
