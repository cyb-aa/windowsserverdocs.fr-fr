---
ms.assetid: 4ef052f0-61a9-4912-b780-5c96187c850f
title: Considérations sur la topologie du déploiement d'AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 260d86c0feae0179620ece09e06f12729691b5a3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359222"
---
# <a name="ad-fs-deployment-topology-considerations"></a>Considérations sur la topologie du déploiement d'AD FS

Cette rubrique décrit des éléments importants à prendre en compte pour vous aider à planifier et à concevoir les Services ADFS topologie de déploiement \(AD FS @ no__t-1 à utiliser dans votre environnement de production. Cette rubrique est un point de départ pour examiner et évaluer les considérations qui affectent les fonctionnalités ou fonctionnalités qui seront disponibles après le déploiement de AD FS. Par exemple, selon le type de base de données que vous choisissez de stocker, AD FS base de données de configuration détermine si vous pouvez implémenter certaines fonctionnalités Security Assertion Markup Language \(SAML @ no__t-1 qui requièrent SQL Server.  

## <a name="determining-which-type-of-ad-fs-configuration-database-to-use"></a>Détermination du type de AD FS base de données de configuration à utiliser  
AD FS utilise une base de données pour stocker la configuration et, dans certains cas, les données transactionnelles associées au service FS (Federation Service). Vous pouvez utiliser le logiciel AD FS pour sélectionner la base de données interne Windows intégrée @ no__t-0in \(WID @ no__t-2 ou Microsoft SQL Server 2005 ou une version ultérieure pour stocker les données dans le service FS (Federation Service).  

En règle générale, les deux types de base de données sont relativement équivalents. Toutefois, il existe quelques différences à connaître avant de commencer à lire des informations supplémentaires sur les différentes topologies de déploiement que vous pouvez utiliser avec AD FS. Le tableau suivant décrit les différences entre les fonctionnalités prises en charge entre une base de données WID et une base de données SQL Server.  

Fonctionnalités de AD FS  

|Fonctionnalité|Prise en charge par la base de données interne Windows ?|Prise en charge par SQL Server ?|Pour en savoir plus sur cette fonctionnalité|  
|-----------|---------------------|----------------------------|---------------------------------------|  
|Déploiement d'une batterie de serveurs de fédération|Oui, avec une limite de 30 serveurs de Fédération pour chaque batterie|Oui. Vous pouvez déployer un nombre illimité de serveurs de fédération dans une batterie.|[Déterminer votre topologie de déploiement d’AD FS](Determine-Your-AD-FS-Deployment-Topology.md)|  
|**Remarque relative** à la résolution d’artefacts SAML : Cette fonctionnalité n'est pas nécessaire pour les scénarios Microsoft Online Services, Microsoft Office 365, Microsoft Exchange ou Microsoft Office SharePoint.|Non|Oui|[Rôle de la base de données de configuration AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[Meilleures pratiques pour sécuriser la planification et le déploiement d’AD FS](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)|  
|SAML @ no__t-0WS @ no__t-1Federation détection de relecture de jetons|Non|Oui|[Rôle de la base de données de configuration AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[Meilleures pratiques pour sécuriser la planification et le déploiement d’AD FS](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)|  

Fonctionnalités de base de données  

|Fonctionnalité|Prise en charge par la base de données interne Windows ?|Prise en charge par SQL Server ?|Pour en savoir plus sur cette fonctionnalité|  
|-----------|---------------------|----------------------------|---------------------------------------|  
|Redondance de base de données de base à l’aide de la réplication par extraction, où un ou plusieurs serveurs hébergeant une copie Read @ no__t-0only de la base de données demandent des modifications qui sont effectuées sur un serveur source hébergeant une copie Read @ no__t-1write de la base de données|Oui|Non|[Rôle de la base de données de configuration AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)|  
|Redondance de base de données à l’aide de solutions @ no__t-0availability élevées, telles que le clustering de basculement ou la mise en miroir \(AT la couche de base de données uniquement @ no__t-2 **Remarque :** Toutes les topologies de déploiement AD FS prennent en charge le clustering au niveau de la couche de service AD FS.|Non|Oui|[Rôle de la base de données de configuration AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[Présentation des solutions de haute disponibilité](https://go.microsoft.com/fwlink/?LinkId=179853)|  

### <a name="sql-server-considerations"></a>Considérations concernant SQL Server  
Vous devez tenir compte des faits de déploiement suivants si vous sélectionnez SQL Server comme base de données de configuration pour votre déploiement AD FS.  

-   **Fonctionnalités SAML et leur impact sur la taille et sur la croissance de la base de données**. Quand la résolution d’artefacts SAML ou la détection de relecture de jetons SAML sont activées, AD FS stocke les informations dans la base de données de configuration SQL Server pour chaque jeton AD FS émis. La croissance de la base de données SQL Server à la suite de cette activité n’est pas considérée comme significative et dépend de la période de rétention de la relecture de jetons configurée. Chaque enregistrement d’artefact a une taille d’environ 30 kilo-octets @no__t-taille 0 Ko @ no__t-1.  

-   **Nombre de serveurs nécessaires pour votre déploiement**. Vous devez ajouter au moins un serveur supplémentaire \(to le nombre total de serveurs nécessaires au déploiement de votre AD FS infrastructure @ no__t-1 qui agira comme un hôte dédié de l’instance SQL Server. Si vous envisagez d’utiliser le clustering de basculement ou la mise en miroir pour fournir une tolérance de panne et une évolutivité pour la base de données de configuration SQL Server, vous devez disposer d’au moins deux serveurs SQL Server.  

### <a name="how-the-configuration-database-type-you-select-may-impact-hardware-resources"></a>Impact du type de base de données de configuration sélectionné sur les ressources matérielles  
L’impact sur les ressources matérielles d’un serveur de Fédération déployé dans une batterie de serveurs à l’aide de WID par opposition à un serveur de Fédération déployé dans une batterie de serveurs à l’aide de la base de données SQL Server n’est pas significatif. Toutefois, il est important de tenir compte du fait que lorsque vous utilisez le schéma WID pour la batterie de serveurs, chaque serveur de Fédération de cette batterie de serveurs doit stocker, gérer et gérer les modifications de réplication pour sa copie locale de la base de données de configuration de AD FS, tout en continuant à fournir le opérations requises par l’service FS (Federation Service).  

En comparaison, les serveurs de Fédération déployés dans une batterie qui utilise la base de données SQL Server ne contiennent pas nécessairement une instance locale de la base de données de configuration AD FS. Ils sont donc susceptibles de solliciter un peu moins les ressources matérielles.  

## <a name="verifying-that-your-production-environment-can-support-an-ad-fs-deployment"></a>Vérification que votre environnement de production peut prendre en charge un déploiement d'AD FS  
En plus des serveurs de Fédération que vous allez déployer, et en fonction de la configuration de votre environnement de production existant, les serveurs supplémentaires suivants peuvent être nécessaires pour fournir l’infrastructure nécessaire pour prendre en charge votre nouveau AD FS déploiement :  

-   Contrôleur de domaine Active Directory  

-   @No__t de l’autorité de certification-0CA @ no__t-1  

-   Serveur web destiné à héberger les métadonnées de fédération  

-   Équilibrage de la charge réseau \(NLB @ no__t-1  

## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
