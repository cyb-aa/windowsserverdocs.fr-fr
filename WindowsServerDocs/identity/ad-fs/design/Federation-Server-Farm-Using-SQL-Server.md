---
ms.assetid: e983d2ab-4153-41e7-b243-12cf7d71a552
title: Batterie de serveurs de fédération utilisant SQL Server
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c6c37b6f98b3ac80179b0ae3b7e2a0c1e34a4975
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867881"
---
# <a name="federation-server-farm-using-sql-server"></a>Batterie de serveurs de fédération utilisant SQL Server

Cette topologie pour \(services ADFS AD FS\) diffère de la batterie de serveurs de Fédération\) utilisant la topologie \(de déploiement de base de données interne Windows en ce qu’elle ne réplique pas les données vers chaque serveur de Fédération de la batterie. Au lieu de cela, tous les serveurs de Fédération de la batterie peuvent lire et écrire des données dans une base de données commune qui est stockée sur un serveur exécutant Microsoft SQL Server qui se trouve dans le réseau d’entreprise.  
  
> [!IMPORTANT]  
> Si vous souhaitez créer une batterie de AD FS et utiliser SQL Server pour stocker vos données de configuration, vous pouvez utiliser SQL Server 2008 et versions plus récentes, y compris SQL Server 2012 et SQL Server 2014.  
  
## <a name="deployment-considerations"></a>Points à prendre en considération pour le déploiement  
Cette section décrit les différentes considérations à prendre en compte concernant le public concerné, les avantages et les limitations associés à cette topologie de déploiement.  
  
### <a name="who-should-use-this-topology"></a>Qui doit utiliser cette topologie ?  
  
-   Grandes organisations avec plus de 100 relations d’approbation qui doivent fournir à leurs utilisateurs internes et externes un accès\- \(SSO\) à authentification unique aux applications ou services fédérés  
  
-   Les organisations qui utilisent déjà SQL Server et souhaitent tirer parti de leurs outils et compétences existants  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quels sont les avantages de l’utilisation de cette topologie ?  
  
-   Prise en charge d’un grand nombre \(de relations d’approbation supérieures à 100\)  
  
-   Prise en charge de la détection \(de relecture\) de jetons une \(fonctionnalité de sécurité et la\) résolution d’artefacts de la Security Assertion Markup Language \(protocole SAML 2,0\)  
  
-   Prise en charge de l’ensemble des avantages de SQL Server, tels que la mise en miroir de bases de données, le clustering de basculement, la création de rapports et les outils de gestion  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quelles sont les limitations de l’utilisation de cette topologie ?  
  
-   Cette topologie ne fournit pas de redondance de base de données par défaut. Bien qu’une batterie de serveurs de Fédération avec la topologie WID réplique automatiquement la base de données WID sur chaque serveur de Fédération de la batterie de serveurs, la batterie de serveurs de Fédération avec SQL Server topologie contient une seule copie de la base de données.  
  
> [!NOTE]  
> SQL Server prend en charge de nombreuses options de redondance des données et des applications, notamment le clustering de basculement, la mise en miroir de bases de données et plusieurs types différents de réplication SQL Server.  
  
Le service informatique des \(technologies de l’information Microsoft utilise SQL Server la mise en miroir\) de bases de données en mode synchrone haute\- \-sécurité \(et le\) clustering de basculement pour fournir une haute la prise en charge de la disponibilité pour l’instance SQL Server. SQL Server l' \(homologue\-transactionnel à\-l'\) homologue et la réplication de fusion n’ont pas été testés par l’équipe de produit AD FS chez Microsoft. Pour plus d’informations sur SQL Server, consultez [vue d’ensemble des solutions de haute disponibilité](https://go.microsoft.com/fwlink/?LinkId=179853) ou [sélection du type de réplication approprié](https://go.microsoft.com/fwlink/?LinkId=214648).  
  
### <a name="supported-sql-server-versions"></a>Versions de SQL Server prises en charge  
Les versions suivantes de SQL Server sont prises en charge avec AD FS dans Windows Server 2012 R2 :  
  
-   SQL Server 2008 \/ R2  
  
-   SQL Server 2012  
  
-   SQL Server 2014  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recommandations relatives à l’emplacement du serveur et à la disposition du réseau  
À l’instar de la batterie de serveurs de Fédération avec la topologie wid, tous les serveurs de Fédération de la batterie de serveurs sont configurés\) pour \(utiliser un nom DNS du système \(de noms de domaine de cluster qui représente le nom\)duserviceFS(FederationService)et une adresse IP de cluster dans le cadre de la configuration \(du\) cluster NLB d’équilibrage de charge réseau. Cela permet à l’hôte NLB d’allouer des demandes de client aux serveurs de Fédération individuels. Les serveurs proxys de Fédération peuvent être utilisés pour effectuer un proxy des demandes des clients vers la batterie de serveurs de Fédération.  
  
L’illustration suivante montre comment la société fictive contoso Pharmaceuticals a déployé sa batterie de serveurs de Fédération avec SQL Server topologie dans le réseau d’entreprise. Il montre également comment cette société a configuré le réseau de périmètre avec un accès à un serveur DNS, un hôte NLB supplémentaire qui utilise le même \(nom\) DNS de cluster FS.contoso.com qui est utilisé sur le cluster d’équilibrage de la charge réseau d’entreprise et deux sites Web proxys \(d’application WAP1 et\)WAP2.  
  
![batterie de serveurs utilisant SQL](media/SQLFarmADFSBlue.gif)  
  
Pour plus d’informations sur la configuration de votre environnement de mise en réseau pour une utilisation avec des serveurs de Fédération ou des proxys d’application Web, consultez la section « exigences relatives à la résolution de noms » dans [AD FS configuration requise](AD-FS-Requirements.md) et [planifier l’infrastructure du proxy d’application Web. (WAP)](https://technet.microsoft.com/library/dn383648.aspx).  
  
## <a name="high-availability-options-for-sql-server-farms"></a>Options de haute disponibilité pour les batteries de SQL Server  
Dans Windows Server 2012 R2, AD FS deux nouvelles options permettent de prendre en charge la haute disponibilité dans les batteries de AD FS à l’aide de SQL Server.  
  
-   Prise en charge de SQL Server groupes de disponibilité AlwaysOn  
  
-   Prise en charge de la haute disponibilité distribuée géographiquement à l’aide de la réplication de fusion SQL Server  
  
Cette section décrit chacune de ces options, les problèmes qu’ils résolvent respectivement, et certaines considérations importantes pour déterminer les options à déployer.  
  
> [!NOTE]  
> AD FS batteries de serveurs qui utilisent le \(schéma\) de base de données interne Windows fournissent\/une redondance des données de base avec un accès en\-lecture/écriture sur le nœud du serveur de Fédération principal et un accès en lecture seule sur les nœuds secondaires.  Il peut être utilisé dans une topologie géographiquement locale ou distribuée géographiquement.  
>   
> Lorsque vous utilisez WID, tenez compte des limitations suivantes :  
>   
> -   Une batterie de serveurs WID a une limite de 30 serveurs de Fédération si vous avez 100 ou moins d’approbations de partie de confiance.  
> -   Une batterie de serveurs wid ne prend pas en charge la détection de \(relecture de jetons ou\) la\)résolution d’artefacts du protocole SAML Security Assertion Markup Language \(.  
  
Le tableau suivant fournit un résumé de l’utilisation d’une batterie de serveurs WID.  
  
||||  
|-|-|-|  
||1 \- 100 confiances RP|Plus de 100 confiances RP|  
|1 \- 30 nœuds de AD FS|WID pris en charge|Non pris en charge \- avec SQL wid requis|  
|Plus de 30 nœuds de AD FS|Non pris en charge \- avec SQL wid requis|Non pris en charge \- avec SQL wid requis|  
  
### <a name="alwayson-availability-groups"></a>Groupes de disponibilité AlwaysOn  
**Vue d’ensemble**  
  
Les groupes de disponibilité AlwaysOn ont été introduits dans SQL Server 2012 et offrent un nouveau moyen de créer une instance de SQL Server à haute disponibilité.  Les groupes de disponibilité AlwaysOn associent des éléments de clustering et de mise en miroir de bases de données à des fins de redondance et de basculement au niveau de la couche d’instance SQL et de la couche  Contrairement aux options de haute disponibilité précédentes, les groupes de disponibilité AlwaysOn \(n’ont pas besoin\) d’un stockage commun ou d’un réseau de zone de stockage au niveau de la couche base de données.  
  
Un groupe de disponibilité est constitué d’un \(réplica principal d’un ensemble de bases de données\) primaires en lecture\-/écriture et de un \(à quatre\)réplicas de disponibilité des bases de données secondaires correspondantes.  Le groupe de disponibilité prend en charge\-un seul \(accès en lecture\)seule copier le réplica principal et\-un à quatre réplicas de disponibilité en lecture seule.  Chaque réplica de disponibilité doit résider sur un nœud différent d’un cluster \(WSFC\) de clustering de basculement Windows Server unique.  Pour plus d’informations sur les groupes de disponibilité AlwaysOn, consultez [vue d’ensemble de \(groupes de disponibilité AlwaysOn SQL Server\)](https://technet.microsoft.com/library/ff877884.aspx).  
  
Du point de vue des nœuds d’une batterie de serveurs AD FS SQL Server, le groupe de disponibilité AlwaysOn remplace l' \/ instance de SQL Server unique en tant que base de données des artefacts de la stratégie.  L’écouteur du groupe de disponibilité est \(celui utilisé par le service\) de jeton de sécurité AD FS pour se connecter à SQL.  
  
Le diagramme suivant illustre une batterie de serveurs AD FS SQL Server avec le groupe de disponibilité AlwaysOn.  
  
![batterie de serveurs utilisant SQL](media/alwaysonavailabilitygroups.jpg)  
  
> [!NOTE]  
> Les groupes de disponibilité AlwaysOn requièrent que les instances de SQL Server résident sur \(des\) nœuds WSFC de clustering de basculement Windows Server.  
  
> [!NOTE]  
> Un seul réplica de disponibilité peut agir comme cible de basculement automatique, les trois autres s’appuient sur des basculements manuels.  
  
**Considérations importantes relatives au déploiement**  
  
Si vous envisagez d’utiliser des groupes de disponibilité AlwaysOn conjointement avec SQL Server réplication de fusion, notez les problèmes décrits dans la section « Considérations sur le déploiement de clés pour l’utilisation de AD FS avec la réplication de fusion SQL Server » ci-dessous.  En particulier, lorsqu’un groupe de disponibilité AlwaysOn contenant une base de données qui est un abonné de réplication bascule, l’abonnement de réplication échoue. Pour reprendre la réplication, un administrateur de réplication doit reconfigurer manuellement l’abonné.  Consultez la SQL Server Description du problème spécifique sur les [abonnés de réplication\) et \(](https://technet.microsoft.com/library/hh882436.aspx) les instructions de groupes de disponibilité AlwaysOn SQL Server et de support global pour les groupes de disponibilité AlwaysOn avec des options de réplication sur [ Réplication, change Tracking, capture de données modifiées et \(groupes de disponibilité AlwaysOn\)SQL Server](https://technet.microsoft.com/library/hh403414.aspx).  
  
**Configuration de AD FS pour utiliser un groupe de disponibilité AlwaysOn**  
  
La configuration d’une batterie de serveurs AD FS avec des groupes de disponibilité AlwaysOn nécessite une légère modification de la procédure de déploiement AD FS :  
  
1.  Les bases de données que vous souhaitez sauvegarder doivent être créées avant de pouvoir configurer les groupes de disponibilité AlwaysOn.  AD FS crée ses bases de données dans le cadre de l’installation et de la configuration initiale du premier nœud de service de Fédération d’une nouvelle batterie de serveurs SQL Server AD FS.  Dans le cadre de la configuration de AD FS, vous devez spécifier une chaîne de connexion SQL. vous devrez donc configurer le premier nœud de la batterie de serveurs AD FS pour qu' \(il se connecte\)directement à une instance SQL, ce qui est uniquement temporaire.   Pour obtenir des conseils spécifiques sur la configuration d’une batterie de serveurs AD FS, notamment la configuration d’un nœud de batterie de serveurs AD FS avec une chaîne de connexion SQL Server, consultez [configurer un serveur de Fédération](../../ad-fs/deployment/Configure-a-Federation-Server.md).  
  
2.  Une fois les bases de données de AD FS créées, affectez-les à des groupes de disponibilité AlwaysOn et créez l’écouteur TCP/IP commun à l’aide des outils et processus de SQL Server lors de la [création et de la \(configuration des groupes de disponibilité SQL Server\) ](https://technet.microsoft.com/library/ff878265.aspx).  
  
3.  Enfin, utilisez PowerShell pour modifier les propriétés de AD FS afin de mettre à jour la chaîne de connexion SQL afin d’utiliser l’adresse DNS de l’écouteur du groupe de disponibilité AlwaysOn.  
  
    Exemples de commandes PSH pour mettre à jour la chaîne de connexion SQL pour la base de données de configuration AD FS :  
  
    ```  
    PS:\>$temp= Get-WmiObject -namespace root/ADFS -class SecurityTokenService  
    PS:\>$temp.ConfigurationdatabaseConnectionstring=”data source=<SQLCluster\SQLInstance>; initial catalog=adfsconfiguration;integrated security=true”  
    PS:\>$temp.put()  
  
    ```  
  
4.  Exemples de commandes PSH pour mettre à jour la chaîne de connexion SQL pour le AD FS base de données du service de résolution d’artefact :  
  
    ```  
    PS:\> Set-AdfsProperties –artifactdbconnection ”Data source=<SQLCluster\SQLInstance >;Initial Catalog=AdfsArtifactStore;Integrated Security=True”  
    ```  
  
### <a name="sql-server-merge-replication"></a>Réplication de fusion SQL Server  
Également introduite dans SQL Server 2012, la réplication de fusion permet la redondance des données de stratégie AD FS avec les caractéristiques suivantes :  
  
-   Capacité de lecture et d’écriture sur \(tous les nœuds non seulement sur le serveur principal\)  
  
-   Des quantités de données plus petites sont répliquées de façon asynchrone afin d’éviter d’introduire une latence dans le système  
  
Le diagramme suivant montre une AD FS de batteries de SQL Server géographiquement redondantes \(avec un serveur de publication\)de réplication de fusion 1, 2 abonnés :  
  
![batterie de serveurs utilisant SQL](media/ADFSSQLGeoRedundancy3.png)  
  
**Considérations relatives au déploiement de clés pour l’utilisation de \(AD FS avec SQL Server numéros de note de réplication de fusion dans le diagramme ci-dessus\)**  
  
-   La base de données de distribution n’est pas prise en charge pour une utilisation avec groupes de disponibilité AlwaysOn ou la mise en miroir de bases de données.  Consultez SQL Server des instructions de prise en charge pour les groupes de disponibilité AlwaysOn avec des options de réplication à [la réplication, change Tracking, la capture de données modifiées et les SQL Server \(\)groupes de disponibilité AlwaysOn](https://technet.microsoft.com/library/hh403414.aspx).  
  
-   Quand un groupe de disponibilité AlwaysOn contenant une base de données qui est un abonné de réplication bascule, l’abonnement de réplication échoue. Pour reprendre la réplication, un administrateur de réplication doit reconfigurer manuellement l’abonné.  Consultez la SQL Server Description du problème spécifique sur les [abonnés de réplication\) et \(](https://technet.microsoft.com/library/hh882436.aspx) les instructions de groupes de disponibilité AlwaysOn SQL Server et de support global pour les groupes de disponibilité AlwaysOn avec des options [de réplication Réplication, change Tracking, capture de données modifiées et \(groupes de disponibilité AlwaysOn\)SQL Server](https://technet.microsoft.com/library/hh403414.aspx).  
  
Pour obtenir des instructions plus détaillées sur la configuration de AD FS pour utiliser une réplication de fusion de SQL Server, consultez Configuration de la [redondance géographique avec réplication SQL Server](https://technet.microsoft.com/library/dn632406.aspx).  
  
## <a name="see-also"></a>Voir aussi  
[Planifier votre topologie de déploiement d’AD FS](Plan-Your-AD-FS-Deployment-Topology.md)  
[Guide de conception AD FS dans Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

