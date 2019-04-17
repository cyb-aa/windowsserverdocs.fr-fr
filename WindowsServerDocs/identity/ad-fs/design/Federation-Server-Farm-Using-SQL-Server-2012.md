---
ms.assetid: 6618b3ce-0e94-4009-b887-d8e05453358b
title: "Batterie de serveurs de fédération à l’aide de SQL Server"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a0fff975b9cb278e59686323d2bd72e641597573
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="federation-server-farm-using-sql-server"></a>Batterie de serveurs de fédération à l’aide de SQL Server

>S’applique à: Windows Server2012

Cette topologie pour ActiveDirectory Federation Services \(ADFS\) diffère de la batterie de serveurs de fédération à l’aide de la topologie de déploiement de base de données interne Windows \(WID\) dans la mesure où elle ne réplique pas les données pour chaque serveur de fédération dans la batterie de serveurs. Au lieu de cela, tous les serveurs de fédération de la batterie de serveurs peuvent lire et écrire des données dans une base de données commune qui est stocké sur un serveur exécutant MicrosoftSQLServer qui se trouve dans le réseau d’entreprise.  
  
## <a name="deployment-considerations"></a>Considérations relatives au déploiement  
Cette section décrit des considérations sur le public, les avantages et les limites qui sont associés à cette topologie de déploiement.  
  
### <a name="who-should-use-this-topology"></a>Qui doit utiliser cette topologie?  
  
-   Grandes entreprises avec plus de 100relations d’approbation qui doivent fournir leurs utilisateurs internes et les utilisateurs externes connexion \(SSO\) accès unique à l’application fédérée ou des services  
  
-   Les organisations ayant déjà utilisent SQLServer et souhaitent tirer parti de leurs outils existants et de l’expertise  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quels sont les avantages de l’utilisation de cette topologie?  
  
-   Prise en charge pour un plus grand nombre de relations d’approbation \(more than 100\)  
  
-   Prise en charge pour la détection de relecture de jetons \(a security feature\) et résolution d’artefacts \ (dans le cadre de la \(SAML\) Security Assertion Markup Language 2.0 protocol\)  
  
-   Outils de support pour tous les avantages de SQLServer, telles que la mise en miroir de base de données, le clustering de basculement, rapports et gestion  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quelles sont les limites de l’utilisation de cette topologie?  
  
-   Cette topologie ne fournit pas de redondance de base de données par défaut. Même si une batterie de serveurs de fédération avec topologie WID réplique automatiquement la base de données WID sur chaque serveur de fédération de la batterie, la batterie de serveurs de fédération avec topologie SQLServer contient une copie de la base de données  
  
> [!NOTE]  
> SQLServer prend en charge les nombreuses différentes données et les options de la redondance d’application, y compris le clustering de basculement, la mise en miroir de base de données et différents types de réplication SQLServer.  
  
Le service MicrosoftInformation Technology \(IT\) utilise la mise en miroir de base de données SQLServer en mode de \(synchronous\) de sécurité évolutifs et clustering de basculement pour prendre en charge de disponibilité évolutifs pour l’instance SQLServer. \(Peer\-to\-peer\) transactionnelle SQLServer et la réplication de fusion n’ont pas été testés par l’équipe du produit ADFS chez Microsoft. Pour plus d’informations sur SQLServer, voir [vue d’ensemble des Solutions haute disponibilité](https://go.microsoft.com/fwlink/?LinkId=179853) ou [en sélectionnant le Type approprié de réplication](https://go.microsoft.com/fwlink/?LinkId=214648).  
  
### <a name="supported-sql-server-versions"></a>Versions prises en charge de SQLServer  
Les versions suivantes de SQL server sont prises en charge avec ADFS installé avec Windows Server2012:  
  
-   SQLServer2008 \ / R2  
  
-   SQL Server 2012  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recommandations de mise en réseau et la sélection élective serveur  
Similaire à la batterie de serveurs de fédération avec topologie WID, tous les serveurs de fédération dans la batterie de serveurs sont configurés pour utiliser un nom de cluster \(DNS\) Domain Name System \ (qui représente le Service de fédération nom\) et l’adresse IP d’un cluster cadre de la configuration du cluster \(NLB\) équilibrage de charge réseau. Cela permet à l’hôte NLB d’allouer les demandes des clients vers les serveurs de fédération individuels. Serveurs proxy de fédération peut servir aux demandes de client de proxy pour la batterie de serveurs de fédération.  
  
L’illustration suivante montre comment la société fictive Contoso Pharmaceuticals déployé sa batterie de serveurs de fédération avec topologie SQLServer dans le réseau d’entreprise. Il montre également comment cette société configuré le réseau de périmètre avec accès à un serveur DNS, un hôte NLB supplémentaire qui utilise le même nom \(fs.contoso.com\) cluster DNS qui est utilisé sur le cluster NLB du réseau d’entreprise, et deux serveurs proxys de fédération \(fsp1 and fsp2\).  
  
![Batterie de serveurs à l’aide de SQL](media/FarmSQLProxies.gif)  
  
Pour plus d’informations sur la configuration de votre environnement réseau pour une utilisation avec les serveurs de fédération ou serveurs proxy de fédération, voir [Name Resolution Requirements for Federation Servers](Name-Resolution-Requirements-for-Federation-Servers.md) ou [Name Resolution Requirements for Federation Server Proxies](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception ADFS dans Windows Server2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
