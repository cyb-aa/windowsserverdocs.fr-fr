---
ms.assetid: 6618b3ce-0e94-4009-b887-d8e05453358b
title: Batterie de serveurs de fédération utilisant SQL Server
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 862cbc74833e2d4e9f385ba961b58a1f703e6611
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853132"
---
# <a name="federation-server-farm-using-sql-server"></a>Batterie de serveurs de fédération utilisant SQL Server

Cette topologie pour Services ADFS \(AD FS\) diffère de celle de la batterie de serveurs de Fédération utilisant la base de données interne Windows \(la topologie de déploiement, car elle ne réplique pas les données sur chaque serveur de Fédération de la batterie.\) Au lieu de cela, tous les serveurs de Fédération de la batterie peuvent lire et écrire des données dans une base de données commune qui est stockée sur un serveur exécutant Microsoft SQL Server qui se trouve dans le réseau d’entreprise.  
  
## <a name="deployment-considerations"></a>Considérations relatives au déploiement  
Cette section décrit les différentes considérations à prendre en compte concernant le public concerné, les avantages et les limitations associés à cette topologie de déploiement.  
  
### <a name="who-should-use-this-topology"></a>Qui doit utiliser cette topologie ?  
  
-   Grandes organisations avec plus de 100 relations d’approbation qui doivent fournir à la fois à leurs utilisateurs internes et à leurs utilisateurs externes un\-d’authentification unique sur \(SSO\) l’accès à des applications ou services fédérés  
  
-   Les organisations qui utilisent déjà SQL Server et souhaitent tirer parti de leurs outils et compétences existants  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quels sont les avantages de l’utilisation de cette topologie ?  
  
-   Prise en charge d’un grand nombre de relations d’approbation \(plus de 100\)  
  
-   La prise en charge de la détection de relecture de jetons \(une fonctionnalité de sécurité\) et la résolution d’artefact \(partie du Security Assertion Markup Language \(protocole SAML\) 2,0\)  
  
-   Prise en charge de l’ensemble des avantages de SQL Server, tels que la mise en miroir de bases de données, le clustering de basculement, la création de rapports et les outils de gestion  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quelles sont les limitations de l’utilisation de cette topologie ?  
  
-   Cette topologie ne fournit pas de redondance de base de données par défaut. Bien qu’une batterie de serveurs de Fédération avec la topologie WID réplique automatiquement la base de données WID sur chaque serveur de Fédération de la batterie de serveurs, la batterie de serveurs de Fédération avec SQL Server topologie contient une seule copie de la base de données.  
  
> [!NOTE]  
> SQL Server prend en charge de nombreuses options de redondance des données et des applications, notamment le clustering de basculement, la mise en miroir de bases de données et plusieurs types différents de réplication SQL Server.  
  
Microsoft Information Technology \(service informatique\) utilise SQL Server la mise en miroir de bases de données dans un haut\-niveau de sécurité \(le mode\) synchrone et le clustering de basculement pour fournir une prise en charge de la haute\-de la disponibilité pour l’instance SQL Server. SQL Server\-d’homologues \(transactionnelles à\-\) d’homologues et que la réplication de fusion n’a pas été testée par l’équipe de produit AD FS chez Microsoft. Pour plus d’informations sur SQL Server, consultez [vue d’ensemble des solutions de haute disponibilité](https://go.microsoft.com/fwlink/?LinkId=179853) ou [sélection du type de réplication approprié](https://go.microsoft.com/fwlink/?LinkId=214648).  
  
### <a name="supported-sql-server-versions"></a>Versions de SQL Server prises en charge  
Les versions suivantes de SQL Server sont prises en charge avec AD FS installé avec Windows Server 2012 :  
  
-   SQL Server 2008 \/ R2  
  
-   SQL Server 2012  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recommandations relatives à l’emplacement du serveur et à la disposition du réseau  
À l’instar de la batterie de serveurs de Fédération avec la topologie WID, tous les serveurs de Fédération de la batterie de serveurs sont configurés pour utiliser un système de noms de domaine de cluster \(nom de\) DNS \(qui représente le nom de service FS (Federation Service)\) et une adresse IP de cluster dans le cadre de l’équilibrage de charge réseau \(la configuration du cluster\) NLB. Cela permet à l’hôte NLB d’allouer des demandes de client aux serveurs de Fédération individuels. Les serveurs proxys de Fédération peuvent être utilisés pour effectuer un proxy des demandes des clients vers la batterie de serveurs de Fédération.  
  
L’illustration suivante montre comment la société fictive contoso Pharmaceuticals a déployé sa batterie de serveurs de Fédération avec SQL Server topologie dans le réseau d’entreprise. Il montre également comment cette société a configuré le réseau de périmètre avec un accès à un serveur DNS, un hôte NLB supplémentaire qui utilise le même nom DNS de cluster \(fs.contoso.com\) utilisé sur le cluster NLB du réseau d’entreprise, et avec deux serveurs proxy de Fédération \(fsp1 et fsp2\).  
  
![batterie de serveurs utilisant SQL](media/FarmSQLProxies.gif)  
  
Pour plus d’informations sur la configuration de votre environnement de mise en réseau pour une utilisation avec des serveurs de Fédération ou des serveurs proxys de Fédération, consultez [Configuration requise pour la résolution de noms pour les serveurs de Fédération](Name-Resolution-Requirements-for-Federation-Servers.md) ou [exigences de résolution de noms pour les serveurs proxys de Fédération](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
