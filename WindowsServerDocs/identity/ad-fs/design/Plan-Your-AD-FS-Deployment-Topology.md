---
ms.assetid: 5c8c6cc0-0d22-4f27-a111-0aa90db7d6c8
title: Planifier votre topologie de déploiement d’AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 7e41f7728c42912ec6ce680e1ed0c6a906a33392
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821710"
---
# <a name="plan-your-ad-fs-deployment-topology"></a>Planifier votre topologie de déploiement d’AD FS

>S'applique à : Windows Server 2016, Windows Server 2012 R2

La première étape de planification d’un déploiement d’Active Directory Federation Services \(AD FS\) consiste à déterminer la topologie de déploiement pour répondre aux besoins de votre organisation.  
  
Avant de lire cette rubrique, examinez comment les données AD FS sont stockées et répliquées sur d’autres serveurs de fédération dans une batterie de serveurs de fédération et assurez-vous que vous comprenez l’objectif d’et les méthodes de réplication qui peuvent être utilisées pour les données sous-jacentes qui sont stockées dans les services AD FS con base de données de la configuration.  
  
Il existe deux types de base de données que vous pouvez utiliser pour stocker les données de configuration AD FS : Base de données interne Windows \(WID\) et Microsoft SQL Server. Pour plus d'informations, consultez [Rôle de la base de données de configuration AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md). Passez en revue les avantages et les limitations qui sont associées à l’aide de WID ou SQL Server en tant que la base de configuration AD FS, ainsi que les différents scénarios d’applications qu’ils prennent en charge et que vous effectuez votre sélection.  
  
> [!IMPORTANT]  
> Pour implémenter la redondance de base, l’équilibrage de charge et l’option Mettre à l’échelle le Service de fédération \(si nécessaire\), nous vous recommandons de déployer au moins deux serveurs de fédération par batterie de serveurs de fédération pour tous les environnements de production, quel que soit le type de base de données que vous allez utiliser.  
  
## <a name="determining-which-type-of-adfs-configuration-database-to-use"></a>Détermination du type de base de données de configuration AD FS à utiliser  
AD FS utilise une base de données pour stocker la configuration et, dans certains cas, les données transactionnelles liées au Service de fédération. Vous pouvez utiliser le logiciel AD FS pour sélectionner intégrées\-dans la base de données interne Windows \(WID\) ou Microsoft SQL Server 2008 ou version ultérieure pour stocker les données dans le Service de fédération.  
  
En règle générale, les deux types de base de données sont relativement équivalents. Toutefois, il existe certaines différences à connaître avant de commencer la lecture plus d’informations sur les différentes topologies de déploiement que vous pouvez utiliser avec AD FS. Le tableau suivant décrit les fonctionnalités et indique si elles sont prises en charge dans une base de données interne Windows et dans une base de données SQL Server.  
  
||Fonctionnalité|Prise en charge par la base de données interne Windows ?|Prise en charge par SQL Server ?
| --- | --- | --- |--- |
|Fonctionnalités d’AD FS|Déploiement d'une batterie de serveurs de fédération|Oui. Une batterie de serveurs WID a une limite de 30 serveurs de fédération si vous avez 100 ou moins de confiance.</br></br>Une batterie de serveurs WID ne prend pas en charge la relecture de jetons de détection ou artefact résolution (partie du protocole Security Assertion Markup Language (SAML)). |Oui. Vous pouvez déployer un nombre illimité de serveurs de fédération dans une batterie.  
|Fonctionnalités d’AD FS|Résolution d'artefacts SAML </br></br>**Remarque :** Cette fonctionnalité n'est pas nécessaire pour les scénarios Microsoft Online Services, Microsoft Office 365, Microsoft Exchange ou Microsoft Office SharePoint.|Non|Oui  
|Fonctionnalités d’AD FS|SAML\/WS\-détection de relecture de jetons de fédération|Non|Oui  
|Fonctionnalités de base de données|Redondance de base de données à l’aide d’extraire la réplication, où un ou plusieurs serveurs hébergeant une lecture\-seule copie des modifications de requête de base de données qui sont effectuées sur un serveur source qui héberge une lecture\/écrire la copie de la base de données|Oui|Non 
|Fonctionnalités de base de données|Redondance de base de données à l’aide de haute\-des solutions de disponibilité, telles que le basculement de clustering ou de mise en miroir \(au niveau de la couche base de données uniquement\) **Remarque :** Toutes les topologies de déploiement d’AD FS prend en charge le clustering au niveau de la couche de service AD FS.|Non|Oui  

  
## <a name="sql-server-considerations"></a>Considérations concernant SQL Server  
Vous devez prendre en compte les facteurs suivants si vous choisissez SQL Server comme base de données de configuration pour le déploiement d'AD FS.  
  
-   **Fonctionnalités SAML et leur impact sur la taille et sur la croissance de la base de données**. Quand la résolution d'artefacts SAML ou la détection de relecture de jetons SAML est activée, AD FS stocke des informations dans la base de données de configuration SQL Server pour chaque jeton AD FS émis. La croissance de la base de données SQL Server qu'entraîne cette activité n'est pas considérée comme significative et dépend de la période de rétention de la relecture de jetons configurée. Chaque enregistrement d’artefact a une taille d’environ 30 kilo-octets \(Ko\).  
  
-   **Nombre de serveurs nécessaires pour votre déploiement**. Vous devez ajouter au moins un serveur supplémentaire \(et le nombre total de serveurs nécessaires pour déployer votre infrastructure AD FS\) qui agira comme un hôte dédié de l’instance de SQL Server. Si vous envisagez d'utiliser le clustering avec basculement ou la mise en miroir pour fournir les fonctionnalités de tolérance de panne et d'extensibilité pour la base de données de configuration SQL Server, au moins deux serveurs SQL sont nécessaires.  
  
## <a name="how-the-configuration-database-type-you-select-may-impact-hardware-resources"></a>Impact du type de base de données de configuration sélectionné sur les ressources matérielles  
L'impact sur les ressources matérielles d'un serveur de fédération déployé dans une batterie utilisant la base de données interne Windows n'est pas significatif par rapport à un serveur de fédération déployé dans une batterie utilisant la base de données SQL Server. Toutefois, gardez à l'esprit que quand vous utilisez la base de données interne Windows pour la batterie, chaque serveur de fédération appartenant à celle-ci doit stocker et gérer les changements de réplication pour sa copie locale de la base de données de configuration AD FS tout en effectuant les opérations normales nécessaires au service de fédération.  
  
À l'opposé, les serveurs de fédération déployés dans une batterie qui utilise la base de données SQL Server ne contiennent pas nécessairement une instance locale de la base de données de configuration AD FS. Ils sont donc susceptibles de solliciter un peu moins les ressources matérielles.  
  
## <a name="BKMK_1"></a>Où placer un serveur de fédération  
Sécurité de meilleures pratiques, placer des serveurs de fédération AD FS devant un pare-feu et les connecter à votre réseau d’entreprise pour empêcher l’exposition à partir d’Internet. Ceci est important, car les serveurs de fédération sont autorisés à octroyer des jetons de sécurité. Par conséquent, ils doivent avoir le même niveau de protection qu’un contrôleur de domaine. Si un serveur de fédération est compromis, un utilisateur malveillant a la possibilité d’émettre des jetons d’accès complet à toutes les applications Web et aux serveurs de fédération qui sont protégés par AD FS.  
  
> [!NOTE]  
> En tant qu’une sécurité optimale, évitez d’avoir vos serveurs de fédération directement accessibles sur Internet. Donnez vos serveurs de fédération un accès Internet direct uniquement lors de la configuration d’un environnement de laboratoire de test ou lorsque votre organisation ne dispose pas d’un réseau de périmètre.  
  
Pour les réseaux d’entreprise, un intranet\-accessible sur le pare-feu est établie entre le réseau d’entreprise et le réseau de périmètre et un Internet\-accessible sur le pare-feu est souvent établi entre le réseau de périmètre et le Internet. Dans ce cas, le serveur de fédération se trouve à l’intérieur du réseau d’entreprise, et il n’est pas directement accessible par les clients Internet.  
  
> [!NOTE]  
> Les ordinateurs clients qui sont connectés au réseau d’entreprise peuvent communiquer directement avec le serveur de fédération via l’authentification intégrée Windows.  
  
Un serveur proxy de fédération doit être placé dans le réseau de périmètre avant de configurer vos serveurs pare-feu pour une utilisation avec AD FS.  
  
## <a name="supported-deployment-topologies"></a>Topologies de déploiement pris en charge  
Les rubriques suivantes décrivent les différentes topologies de déploiement que vous pouvez utiliser avec AD FS. Elles expliquent également les avantages et les limites de chaque topologie de déploiement, ce qui vous permet de choisir la topologie la plus appropriée pour votre entreprise.  
  
-   [Batterie de serveurs de fédération avec WID](Federation-Server-Farm-Using-WID.md)  
  
-   [Batterie de serveurs de fédération avec WID et proxys](Federation-Server-Farm-Using-WID-and-Proxies.md)  
  
-   [Batterie de serveurs de fédération à l’aide de SQL Server](Federation-Server-Farm-Using-SQL-Server.md)  
  
## <a name="see-also"></a>Voir aussi  
[Guide de conception AD FS dans Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

