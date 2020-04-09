---
ms.assetid: 5c8c6cc0-0d22-4f27-a111-0aa90db7d6c8
title: Planifier votre topologie de déploiement d’AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 53364e076a8c3b7d95e8c834a5a7621071ed6061
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858672"
---
# <a name="plan-your-ad-fs-deployment-topology"></a>Planifier votre topologie de déploiement d’AD FS

La première étape de la planification d’un déploiement de Services ADFS \(AD FS\) consiste à déterminer la topologie de déploiement appropriée pour répondre aux besoins de votre organisation.  
  
Avant de lire cette rubrique, passez en revue la façon dont AD FS données sont stockées et répliquées sur d’autres serveurs de Fédération dans une batterie de serveurs de Fédération et assurez-vous que vous comprenez l’objectif de et les méthodes de réplication qui peuvent être utilisées pour les données sous-jacentes stockées dans la base de données de configuration AD FS.  
  
Il existe deux types de base de données que vous pouvez utiliser pour stocker des données de configuration AD FS : base de données interne Windows \(WID\) et Microsoft SQL Server. Pour plus d'informations, consultez [Rôle de la base de données de configuration AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md). Passez en revue les différents avantages et limitations associés à l’utilisation de WID ou de SQL Server comme base de données de configuration de AD FS, ainsi que les différents scénarios d’application qu’ils prennent en charge, puis effectuez votre sélection.  
  
> [!IMPORTANT]  
> Pour implémenter la redondance de base, l’équilibrage de charge et l’option de mise à l’échelle des service FS (Federation Service) \(si nécessaire\), nous vous recommandons de déployer au moins deux serveurs de Fédération par batterie de serveurs de Fédération pour tous les environnements de production, quel que soit le type de base de données que vous allez utiliser.  
  
## <a name="determining-which-type-of-adfs-configuration-database-to-use"></a>Détermination du type de base de données de configuration AD FS à utiliser  
AD FS utilise une base de données pour stocker la configuration et, dans certains cas, les données transactionnelles associées au service FS (Federation Service). Vous pouvez utiliser le logiciel AD FS pour sélectionner le\-créé dans la base de données interne Windows \(\), ou Microsoft SQL Server 2008 ou version ultérieure, pour stocker les données dans le service FS (Federation Service).  
  
En règle générale, les deux types de base de données sont relativement équivalents. Toutefois, il existe quelques différences à connaître avant de commencer à lire des informations supplémentaires sur les différentes topologies de déploiement que vous pouvez utiliser avec AD FS. Le tableau suivant décrit les fonctionnalités et indique si elles sont prises en charge dans une base de données interne Windows et dans une base de données SQL Server.  
  
||Composant|Prise en charge par la base de données interne Windows ?|Prise en charge par SQL Server ?
| --- | --- | --- |--- |
|Fonctionnalités de AD FS|Déploiement d'une batterie de serveurs de fédération|Oui. Une batterie de serveurs WID a une limite de 30 serveurs de Fédération si vous avez 100 ou moins d’approbations de partie de confiance.</br></br>Une batterie de serveurs WID ne prend pas en charge la détection de relecture de jetons ou la résolution d’artefacts (partie du protocole SAML (Security Assertion Markup Language). |Oui. Vous pouvez déployer un nombre illimité de serveurs de fédération dans une batterie.  
|Fonctionnalités de AD FS|Résolution d'artefacts SAML </br></br>**Remarque :** Cette fonctionnalité n’est pas requise pour les scénarios Microsoft Online Services, Microsoft Office 365, Microsoft Exchange ou Microsoft Office SharePoint.|Non|Oui  
|Fonctionnalités de AD FS|Détection de relecture de jetons de Fédération\/WS\-|Non|Oui  
|Fonctionnalités de base de données|Redondance de base de données de base à l’aide de la réplication par extraction, où un ou plusieurs serveurs hébergeant une seule copie de la base de données\-copient uniquement les modifications apportées à la requête sur un serveur source hébergeant une copie en lecture\/écriture de la base de données.|Oui|Non 
|Fonctionnalités de base de données|Redondance de base de données utilisant des solutions de haute disponibilité\-, telles que le clustering de basculement ou la mise en miroir \(au niveau de la couche base de données uniquement\) **Remarque :** toutes les topologies de déploiement AD FS prennent en charge le clustering au niveau de la couche de service AD FS.|Non|Oui  

  
## <a name="sql-server-considerations"></a>Considérations concernant SQL Server  
Vous devez prendre en compte les facteurs suivants si vous choisissez SQL Server comme base de données de configuration pour le déploiement d'AD FS.  
  
-   **Fonctionnalités SAML et leur impact sur la taille et sur la croissance de la base de données**. Quand la résolution d'artefacts SAML ou la détection de relecture de jetons SAML est activée, AD FS stocke des informations dans la base de données de configuration SQL Server pour chaque jeton AD FS émis. La croissance de la base de données SQL Server qu'entraîne cette activité n'est pas considérée comme significative et dépend de la période de rétention de la relecture de jetons configurée. Chaque enregistrement d’artefact a une taille d’environ 30 kilo-octets \(Ko\).  
  
-   **Nombre de serveurs nécessaires pour votre déploiement**. Vous devez ajouter au moins un serveur supplémentaire \(au nombre total de serveurs nécessaires pour déployer votre infrastructure AD FS\) qui agira en tant qu’hôte dédié de l’instance de SQL Server. Si vous envisagez d'utiliser le clustering avec basculement ou la mise en miroir pour fournir les fonctionnalités de tolérance de panne et d'extensibilité pour la base de données de configuration SQL Server, au moins deux serveurs SQL sont nécessaires.  
  
## <a name="how-the-configuration-database-type-you-select-may-impact-hardware-resources"></a>Impact du type de base de données de configuration sélectionné sur les ressources matérielles  
L'impact sur les ressources matérielles d'un serveur de fédération déployé dans une batterie utilisant la base de données interne Windows n'est pas significatif par rapport à un serveur de fédération déployé dans une batterie utilisant la base de données SQL Server. Toutefois, gardez à l'esprit que quand vous utilisez la base de données interne Windows pour la batterie, chaque serveur de fédération appartenant à celle-ci doit stocker et gérer les changements de réplication pour sa copie locale de la base de données de configuration AD FS tout en effectuant les opérations normales nécessaires au service de fédération.  
  
À l'opposé, les serveurs de fédération déployés dans une batterie qui utilise la base de données SQL Server ne contiennent pas nécessairement une instance locale de la base de données de configuration AD FS. Ils sont donc susceptibles de solliciter un peu moins les ressources matérielles.  
  
## <a name="where-to-place-a-federation-server"></a><a name="BKMK_1"></a>Où placer un serveur de Fédération  
En guise de meilleure pratique de sécurité, placez AD FS serveurs de Fédération devant un pare-feu et connectez-les à votre réseau d’entreprise pour empêcher l’exposition à partir d’Internet. Cela est important, car les serveurs de Fédération ont une autorisation complète pour accorder des jetons de sécurité. Par conséquent, ils doivent avoir le même niveau de protection qu’un contrôleur de domaine. Si un serveur de Fédération est compromis, un utilisateur malveillant a la possibilité d’émettre des jetons d’accès complets à toutes les applications Web et aux serveurs de Fédération protégés par AD FS.  
  
> [!NOTE]  
> Pour des raisons de sécurité, évitez d’accéder directement à vos serveurs de Fédération sur Internet. Envisagez de donner à vos serveurs de Fédération un accès direct à Internet uniquement lorsque vous configurez un environnement de laboratoire de test ou que votre organisation ne dispose pas d’un réseau de périmètre.  
  
Pour les réseaux d’entreprise types, un pare-feu de\-intranet est établi entre le réseau d’entreprise et le réseau de périmètre, et un pare-feu Internet\-est souvent établi entre le réseau de périmètre et Internet. Dans ce cas, le serveur de Fédération se trouve dans le réseau d’entreprise et n’est pas directement accessible par les clients Internet.  
  
> [!NOTE]  
> Les ordinateurs clients qui sont connectés au réseau d’entreprise peuvent communiquer directement avec le serveur de Fédération par le biais de l’authentification intégrée de Windows.  
  
Un proxy de serveur de Fédération doit être placé dans le réseau de périmètre avant de configurer vos serveurs pare-feu pour une utilisation avec AD FS.  
  
## <a name="supported-deployment-topologies"></a>Topologies de déploiement prises en charge  
Les rubriques suivantes décrivent les différentes topologies de déploiement que vous pouvez utiliser avec AD FS. Elles expliquent également les avantages et les limites de chaque topologie de déploiement, ce qui vous permet de choisir la topologie la plus appropriée pour votre entreprise.  
  
-   [Batterie de serveurs de fédération utilisant la base de données interne Windows](Federation-Server-Farm-Using-WID.md)  
  
-   [Batterie de serveurs de fédération utilisant la base de données interne Windows et des proxys](Federation-Server-Farm-Using-WID-and-Proxies.md)  
  
-   [Batterie de serveurs de fédération utilisant SQL Server](Federation-Server-Farm-Using-SQL-Server.md)  
  
## <a name="see-also"></a>Voir aussi  
[Guide de conception AD FS dans Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

