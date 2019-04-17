---
ms.assetid: 5c8c6cc0-0d22-4f27-a111-0aa90db7d6c8
title: "Planifier votre topologie de déploiement ADFS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 7e41f7728c42912ec6ce680e1ed0c6a906a33392
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="plan-your-ad-fs-deployment-topology"></a>Planifier votre topologie de déploiement ADFS

>S’applique à: Windows Server2016, Windows Server2012R2

La première étape de planification d’un déploiement d’ActiveDirectory Federation Services \(ADFS\) consiste à déterminer la topologie de déploiement en fonction des besoins de votre organisation.  
  
Avant de lire cette rubrique, examinez comment les données ADFS sont stockées et répliquées vers d’autres serveurs de fédération dans une batterie de serveurs de fédération et assurez-vous que vous comprenez l’objectif d’et les méthodes de réplication qui peuvent être utilisées pour les données sous-jacentes qui sont stockées dans la base de données de configuration ADFS.  
  
Il existe deux types de base de données que vous pouvez utiliser pour stocker les données de configuration ADFS: \(WID\) base de données interne Windows et MicrosoftSQLServer. Pour plus d’informations, voir [le rôle de la base de données de Configuration ADFS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md). Passez en revue les avantages et les limites qui sont associés à l’aide de WID ou SQLServer en tant que la base de configuration ADFS, ainsi que les différents scénarios d’application qu’ils prennent en charge et puis effectuez votre sélection.  
  
> [!IMPORTANT]  
> Pour implémenter la redondance de base, l’équilibrage de charge et la possibilité de faire évoluer le Service de fédération \(if required\), nous vous recommandons de déployer au moins deux serveurs de fédération par batterie de serveurs de fédération pour tous les environnements de production, quel que soit le type de base de données que vous utiliserez.  
  
## <a name="determining-which-type-of-ad-fs-configuration-database-to-use"></a>Détermination du type de base de données de configuration ADFS à utiliser  
ADFS utilise une base de données pour stocker la configuration et, dans certains cas, les données transactionnelles liées au Service de fédération. Vous pouvez utiliser le logiciel ADFS pour sélectionner l’intégrée de base de données interne Windows \(WID\) ou MicrosoftSQLServer2008 ou version ultérieure pour stocker les données dans le Service de fédération.  
  
Pour la plupart des cas, les types de base de données de deux sont relativement équivalents. Toutefois, il existe certaines différences à connaître avant de commencer la lecture plus d’informations sur les différentes topologies de déploiement que vous pouvez utiliser avec ADFS. Le tableau suivant décrit les différences de fonctionnalités prises en charge entre une base de données WID et une base de données SQLServer.  
  
||Fonctionnalité|Prise en charge par WID?|Prise en charge par SQLServer?
| --- | --- | --- |--- |
|Fonctionnalités de ADFS|Déploiement de la batterie de serveurs de fédération|Oui. Une batterie WID dispose d’un nombre maximal de serveurs de fédération 30 si vous avez moins de 100 approbations de partie confiance.</br></br>Une batterie WID ne gère pas la relecture de jetons détection ou artefact résolution (partie du protocole SAML Security Assertion Markup Language ()). |Oui. Aucune limite n’est appliquée pour le nombre de serveurs de fédération que vous pouvez déployer dans une batterie  
|Fonctionnalités de ADFS|Résolution d’artefacts SAML </br></br>**Remarque:** cette fonctionnalité n’est pas requise pour les scénarios MicrosoftOnline Services, MicrosoftOffice 365, MicrosoftExchange ou MicrosoftOffice SharePoint.|N°|Oui  
|Fonctionnalités de ADFS|Détection de relecture de jetons SAML\/WS-Federation|N°|Oui  
|Fonctionnalités de base de données|Redondance de base de la base de données à l’aide de la réplication par réception, où un ou plusieurs serveurs hébergeant une copie en lecture seule des modifications de demande de base de données qui sont effectuées sur un serveur source qui héberge une copie en lecture/écriture de la base de données|Oui|N° 
|Fonctionnalités de base de données|Redondance de base de données à l’aide des solutions de disponibilité évolutifs, telles que le basculement clustering ou la mise en miroir \ (à l’only\ de couche base de données) **Remarque:** toutes les topologies de déploiement d’ADFS prend en charge le clustering au niveau de la couche de service ADFS.|N°|Oui  

  
## <a name="sql-server-considerations"></a>Considérations concernant SQLServer  
Vous devez envisager les facteurs suivants si vous sélectionnez SQLServer en tant que la base de données de configuration pour votre déploiement d’ADFS.  
  
-   **SAML fonctionnalités et leur impact sur la taille de la base de données et la croissance**. Résolution d’artefacts SAML ou de fonctionnalités de détection de relecture de jetons SAML sont activées, ADFS stocke des informations dans la base de données de configuration SQLServer pour chaque jeton ADFS émis. La croissance de la base de données SQLServer à la suite de cette activité n’est pas considérée comme significative et dépend de la période de rétention de relecture de jetons configurée. Chaque enregistrement d’artefact a une taille de \(KB\) environ 30kilo-octets.  
  
-   **Nombre de serveurs nécessaires pour votre déploiement**. Vous devez ajouter au moins un serveur supplémentaire \ (et le nombre total de serveurs nécessaires pour déployer votre infrastructure\ ADFS) qui agit comme un hôte dédié de l’instance de SQLServer. Si vous envisagez d’utiliser la mise en miroir ou de clustering de basculement pour fournir une tolérance de panne et l’extensibilité pour la base de données de configuration de SQLServer, un minimum de deux serveurs SQLServer est requis.  
  
## <a name="how-the-configuration-database-type-you-select-may-impact-hardware-resources"></a>Comment le type de base de données de configuration que vous sélectionnez peut-être affecter les ressources matérielles  
L’impact sur les ressources matérielles sur un serveur de fédération qui est déployé dans une batterie de serveurs à l’aide de WID par opposition à un serveur de fédération qui est déployé dans une batterie de serveurs à l’aide de la base de données SQLServer n’est pas significatif. Toutefois, il est important de prendre en compte que lorsque vous utilisez WID pour la batterie de serveurs, chaque serveur de fédération doit stocker, gérer et mettre à jour les changements de réplication pour sa copie locale de la base de données de configuration ADFS tout en continuant également pour les opérations normales qui nécessite le Service de fédération.  
  
En comparaison, les serveurs de fédération qui sont déployés dans une batterie de serveurs qui utilise la base de données SQLServer ne contiennent pas nécessairement une instance locale de la base de données de configuration ADFS. Par conséquent, ils peuvent solliciter un peu moins de ressources matérielles.  
  
## <a name="BKMK_1"></a>Où placer un serveur de fédération  
Sécurité des meilleures pratiques, placer des serveurs de fédération ADFS devant un pare-feu et les connecter à votre réseau d’entreprise pour éviter l’exposition à partir d’Internet. Cela est important, car les serveurs de fédération pour octroyer des jetons de sécurité. Par conséquent, ils doivent avoir le même niveau de protection comme contrôleur de domaine. Si un serveur de fédération est compromis, un utilisateur malveillant a la possibilité d’émettre des jetons d’accès complet à toutes les applications Web et aux serveurs de fédération qui sont protégées par ADFS.  
  
> [!NOTE]  
> Une sécurité optimale, évitez d’avoir vos serveurs de fédération directement accessibles sur Internet. Autorisez vos serveurs de fédération accès direct à Internet uniquement lors de la configuration d’un environnement de laboratoire de test ou lorsque votre organisation ne dispose pas d’un réseau de périmètre.  
  
Pour les réseaux d’entreprise, un pare-feu intranet\ est établi entre le réseau d’entreprise et le réseau de périmètre, et un pare-feu Internet est souvent établi entre le réseau de périmètre et Internet. Dans ce cas, le serveur de fédération qui se trouve à l’intérieur du réseau d’entreprise, et il n’est pas directement accessible par les clients Internet.  
  
> [!NOTE]  
> Les ordinateurs clients qui sont connectés au réseau d’entreprise peuvent communiquer directement avec le serveur de fédération via l’authentification Windows intégrée.  
  
Un serveur proxy de fédération doit être placé dans le réseau de périmètre avant de configurer vos serveurs pare-feu pour une utilisation avec AD FS.  
  
## <a name="supported-deployment-topologies"></a>Topologies de déploiement pris en charge  
Les rubriques suivantes décrivent les différentes topologies de déploiement que vous pouvez utiliser avec ADFS. Elles décrivent également les avantages et les limitations liées à chaque topologie de déploiement afin que vous pouvez sélectionner la topologie la plus appropriée à vos besoins spécifiques.  
  
-   [Batterie de serveurs de fédération avec WID](Federation-Server-Farm-Using-WID.md)  
  
-   [Batterie de serveurs de fédération avec WID et proxys](Federation-Server-Farm-Using-WID-and-Proxies.md)  
  
-   [Batterie de serveurs de fédération à l’aide de SQL Server](Federation-Server-Farm-Using-SQL-Server.md)  
  
## <a name="see-also"></a>Voir aussi  
[Guide de conception ADFS dans Windows Server2012R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

