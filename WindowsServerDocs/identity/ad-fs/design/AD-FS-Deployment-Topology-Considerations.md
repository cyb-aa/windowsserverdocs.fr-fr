---
ms.assetid: 4ef052f0-61a9-4912-b780-5c96187c850f
title: "Considérations sur la topologie déploiement ADFS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: eee14ee7bb50e1a82f35caf9fbacda0b86d3a1ad
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="ad-fs-deployment-topology-considerations"></a>Considérations sur la topologie déploiement ADFS

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Cette rubrique décrit les considérations importantes pour vous aider à planifier et concevoir la topologie de déploiement \(ADFS\) qui ActiveDirectory Federation Services à utiliser dans votre environnement de production. Cette rubrique est un point de départ pour examiner et évaluer les aspects qui influent sur les fonctionnalités ou capacités disponibles après le déploiement d’ADFS. Par exemple, en fonction de la base de données type que vous choisissez pour stocker la base de données de configuration ADFS en fin de compte détermine si vous pouvez implémenter certaines fonctionnalités \(SAML\) Security Assertion Markup Language qui nécessitent SQLServer.  
  
## <a name="determining-which-type-of-ad-fs-configuration-database-to-use"></a>Détermination du type de base de données de configuration ADFS à utiliser  
ADFS utilise une base de données pour stocker la configuration et, dans certains cas, les données transactionnelles liées au Service de fédération. Vous pouvez utiliser le logiciel ADFS pour sélectionner l’intégrée de base de données interne Windows \(WID\) ou MicrosoftSQLServer version2005 ou ultérieure pour stocker les données dans le Service de fédération.  
  
Pour la plupart des cas, les types de base de données de deux sont relativement équivalents. Toutefois, il existe certaines différences à connaître avant de commencer la lecture plus d’informations sur les différentes topologies de déploiement que vous pouvez utiliser avec ADFS. Le tableau suivant décrit les différences de fonctionnalités prises en charge entre une base de données WID et une base de données SQLServer.  
  
Fonctionnalités de ADFS  
  
|Fonctionnalité|Prise en charge par WID?|Prise en charge par SQLServer?|Plus d’informations sur cette fonctionnalité|  
|-----------|---------------------|----------------------------|---------------------------------------|  
|Déploiement de la batterie de serveurs de fédération|Oui, avec une limite de cinq serveurs de fédération par batterie.|Oui. Aucune limite n’est appliquée pour le nombre de serveurs de fédération que vous pouvez déployer dans une batterie|[Déterminer votre topologie de déploiement ADFS](Determine-Your-AD-FS-Deployment-Topology.md)|  
|Résolution d’artefacts SAML **Remarque:** cette fonctionnalité n’est pas requise pour les scénarios MicrosoftOnline Services, MicrosoftOffice 365, MicrosoftExchange ou MicrosoftOffice SharePoint.|N°|Oui|[Le rôle de la base de données de Configuration ADFS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[Meilleures pratiques pour sécuriser la planification et déploiement d’ADFS](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)|  
|Détection de relecture de jetons SAML\/WS-Federation|N°|Oui|[Le rôle de la base de données de Configuration ADFS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[Meilleures pratiques pour sécuriser la planification et déploiement d’ADFS](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)|  
  
Fonctionnalités de base de données  
  
|Fonctionnalité|Prise en charge par WID?|Prise en charge par SQLServer?|Plus d’informations sur cette fonctionnalité|  
|-----------|---------------------|----------------------------|---------------------------------------|  
|Redondance de base de la base de données à l’aide de la réplication par réception, où un ou plusieurs serveurs hébergeant une copie en lecture seule des modifications de demande de base de données qui sont effectuées sur un serveur source qui héberge une copie en lecture/écriture de la base de données|Oui|N°|[Le rôle de la base de données de Configuration ADFS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)|  
|Redondance de base de données à l’aide des solutions de disponibilité évolutifs, telles que le basculement clustering ou la mise en miroir \ (à l’only\ de couche base de données) **Remarque:** toutes les topologies de déploiement d’ADFS prend en charge le clustering au niveau de la couche de service ADFS.|N°|Oui|[Le rôle de la base de données de Configuration ADFS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[Vue d’ensemble des Solutions haute disponibilité](https://go.microsoft.com/fwlink/?LinkId=179853)|  
  
### <a name="sql-server-considerations"></a>Considérations concernant SQLServer  
Vous devez envisager les facteurs suivants si vous sélectionnez SQLServer en tant que la base de données de configuration pour votre déploiement d’ADFS.  
  
-   **SAML fonctionnalités et leur impact sur la taille de la base de données et la croissance**. Résolution d’artefacts SAML ou de fonctionnalités de détection de relecture de jetons SAML sont activées, ADFS stocke des informations dans la base de données de configuration SQLServer pour chaque jeton ADFS émis. La croissance de la base de données SQLServer à la suite de cette activité n’est pas considérée comme significative et dépend de la période de rétention de relecture de jetons configurée. Chaque enregistrement d’artefact a une taille de \(KB\) environ 30kilo-octets.  
  
-   **Nombre de serveurs nécessaires pour votre déploiement**. Vous devez ajouter au moins un serveur supplémentaire \ (et le nombre total de serveurs nécessaires pour déployer votre infrastructure\ ADFS) qui agit comme un hôte dédié de l’instance de SQLServer. Si vous envisagez d’utiliser la mise en miroir ou de clustering de basculement pour fournir une tolérance de panne et l’extensibilité pour la base de données de configuration de SQLServer, un minimum de deux serveurs SQLServer est requis.  
  
### <a name="how-the-configuration-database-type-you-select-may-impact-hardware-resources"></a>Comment le type de base de données de configuration que vous sélectionnez peut-être affecter les ressources matérielles  
L’impact sur les ressources matérielles sur un serveur de fédération qui est déployé dans une batterie de serveurs à l’aide de WID par opposition à un serveur de fédération qui est déployé dans une batterie de serveurs à l’aide de la base de données SQLServer n’est pas significatif. Toutefois, il est important de prendre en compte que lorsque vous utilisez WID pour la batterie de serveurs, chaque serveur de fédération doit stocker, gérer et mettre à jour les changements de réplication pour sa copie locale de la base de données de configuration ADFS tout en continuant également pour les opérations normales qui nécessite le Service de fédération.  
  
En comparaison, les serveurs de fédération qui sont déployés dans une batterie de serveurs qui utilise la base de données SQLServer ne contiennent pas nécessairement une instance locale de la base de données de configuration ADFS. Par conséquent, ils peuvent solliciter un peu moins de ressources matérielles.  
  
## <a name="verifying-that-your-production-environment-can-support-an-ad-fs-deployment"></a>Vérifier que votre environnement de production peut prendre en charge un déploiement d’ADFS  
Outre les serveurs de fédération que vous allez déployer et selon la configuration de votre environnement de production est, les serveurs suivants être amenés à fournir l’infrastructure nécessaire pour prendre en charge de votre nouveau déploiement d’ADFS:  
  
-   Contrôleur de domaine ActiveDirectory  
  
-   Autorité de certification \(CA\)  
  
-   Serveur Web pour les métadonnées de fédération hôte  
  
-   \(NLB\) équilibrage de charge réseau  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception ADFS dans Windows Server2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
