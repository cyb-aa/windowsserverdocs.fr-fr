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
ms.openlocfilehash: 585d0195b096056ba769f4e9a08d5c4d2156b96a
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191452"
---
# <a name="federation-server-farm-using-sql-server"></a>Batterie de serveurs de fédération utilisant SQL Server

Cette topologie pour Active Directory Federation Services \(AD FS\) diffère de la batterie de serveurs de fédération à l’aide de la base de données interne Windows \(WID\) topologie de déploiement qui elle ne réplique pas les données à chaque serveur de fédération dans la batterie de serveurs. Au lieu de cela, tous les serveurs de fédération dans la batterie de serveurs peuvent lire et écrire des données dans une base de données commune qui est stocké sur un serveur exécutant Microsoft SQL Server qui se trouve dans le réseau d’entreprise.  
  
> [!IMPORTANT]  
> Si vous souhaitez créer une batterie de serveurs AD FS et utiliser SQL Server pour stocker vos données de configuration, vous pouvez utiliser SQL Server 2008 et les versions plus récentes, y compris SQL Server 2012 et SQL Server 2014.  
  
## <a name="deployment-considerations"></a>Considérations relatives au déploiement  
Cette section décrit divers points de vue sur le public visé, les avantages et les limites qui sont associés à cette topologie de déploiement.  
  
### <a name="who-should-use-this-topology"></a>Qui doit utiliser cette topologie ?  
  
-   Grandes organisations avec plus de 100 relations d’approbation qui doivent fournir leurs utilisateurs internes et les utilisateurs externes avec l’authentification unique\-sur \(SSO\) accès aux applications fédérées ou de services  
  
-   Les organisations ayant déjà utilisent SQL Server et souhaitent tirer parti de leurs outils existants et de savoir-faire  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quels sont les avantages de l’utilisation de cette topologie ?  
  
-   Prise en charge pour un plus grand nombre de relations d’approbation \(plus de 100\)  
  
-   Prise en charge pour la détection de relecture de jetons \(une fonctionnalité de sécurité\) et résolution d’artefacts \(dans le cadre de la Security Assertion Markup Language \(SAML\) protocole 2.0\)  
  
-   Outils de support pour tous les avantages de SQL Server, telles que la mise en miroir de base de données, clustering de basculement, reporting et gestion  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quelles sont les limitations de l’utilisation de cette topologie ?  
  
-   Cette topologie ne fournit pas de redondance de base de données par défaut. Bien qu’une batterie de serveurs de fédération avec topologie WID réplique automatiquement la base de données WID sur chaque serveur de fédération dans la batterie de serveurs, la batterie de serveurs de fédération avec topologie SQL Server contient une seule copie de la base de données  
  
> [!NOTE]  
> SQL Server prend en charge plusieurs données différentes et les options de redondance d’application, y compris le clustering de basculement, la mise en miroir de base de données et les différents types de réplication SQL Server.  
  
Le Microsoft Information Technology \(informatique\) service utilise la mise en miroir de base de données SQL Server dans haute\-sécurité \(synchrone\) mode et le clustering de basculement pour fournir une élevée\- prise en charge de la disponibilité de l’instance de SQL Server. SQL Server transactionnelle \(homologue\-à\-homologue\) et la réplication de fusion n’ont pas été testés par l’équipe de produit AD FS chez Microsoft. Pour plus d’informations sur SQL Server, consultez [vue d’ensemble des Solutions haute disponibilité](https://go.microsoft.com/fwlink/?LinkId=179853) ou [en sélectionnant le Type de réplication appropriée](https://go.microsoft.com/fwlink/?LinkId=214648).  
  
### <a name="supported-sql-server-versions"></a>Versions prises en charge de SQL Server  
Les versions suivantes de SQL server sont prises en charge avec AD FS dans Windows Server 2012 R2 :  
  
-   SQL Server 2008 \/ R2  
  
-   SQL Server 2012  
  
-   SQL Server 2014  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recommandations de mise en page de positionnement et de réseau serveur  
Comme pour la batterie de serveurs de fédération avec topologie WID, tous les serveurs de fédération dans la batterie de serveurs sont configurés pour utiliser un seul cluster Domain Name System \(DNS\) nom \(qui représente le nom du Service de fédération\)et adresse IP du cluster d’un cadre de l’équilibrage de charge réseau \(NLB\) configuration du cluster. Cela permet à l’hôte NLB d’allouer les demandes des clients vers les serveurs de fédération individuels. Serveurs proxy de fédération permet aux demandes des clients de proxy pour la batterie de serveurs de fédération.  
  
L’illustration suivante montre comment la société fictive Contoso Pharmaceuticals a déployé sa batterie de serveurs de fédération avec topologie SQL Server dans le réseau d’entreprise. Il montre également comment cette entreprise a configuré le réseau de périmètre avec un accès à un serveur DNS, un hôte NLB supplémentaire qui utilise le même nom DNS de cluster \(fs.contoso.com\) qui est utilisé sur le cluster d’équilibrage de charge réseau de réseau d’entreprise et avec deux serveurs web proxys d’application \(wap1 et wap2\).  
  
![batterie de serveurs à l’aide de SQL](media/SQLFarmADFSBlue.gif)  
  
Pour plus d’informations sur la configuration de votre environnement réseau pour une utilisation avec les serveurs de fédération ou de proxys d’application web, consultez « Exigences de résolution de nom » section [configuration AD FS requise](AD-FS-Requirements.md) et [planifier le Web Infrastructure du Proxy d’application (WAP)](https://technet.microsoft.com/library/dn383648.aspx).  
  
## <a name="high-availability-options-for-sql-server-farms"></a>Options de haute disponibilité pour les batteries de serveurs SQL  
Dans Windows Server 2012 R2, AD FS il existe deux nouvelles options pour prendre en charge la haute disponibilité dans les batteries de serveurs AD FS à l’aide de SQL Server.  
  
-   Prise en charge des groupes de disponibilité AlwaysOn SQL Server  
  
-   Prise en charge pour une disponibilité élevée géographiquement distribuée à l’aide de la réplication de fusion SQL Server  
  
Cette section décrit chacune de ces options, les problèmes qu’ils résolvent respectivement et certains points clés pour déterminer les options de déploiement.  
  
> [!NOTE]  
> Batteries de serveurs AD FS qui utilisent la base de données interne Windows \(WID\) assurer la redondance de base de données en lecture\/l’accès en écriture sur le nœud de serveur de fédération principal et lire\-accéder uniquement sur les nœuds secondaires.  Cela peut servir dans géographiquement local ou dans une topologie distribuée géographiquement.  
>   
> Lorsque vous utilisez WID tenez compte des limitations suivantes :  
>   
> -   Une batterie de serveurs WID a une limite de 30 serveurs de fédération si vous avez 100 ou moins de confiance.  
> -   Une batterie de serveurs WID ne prend pas en charge la résolution de détection ou d’artefact de relecture de jetons \(dans le cadre de la Security Assertion Markup Language \(SAML\) protocole\).  
  
Le tableau suivant fournit un résumé de l’aide d’une batterie de serveurs WID.  
  
||||  
|-|-|-|  
||1 \- 100 approbations de partie de confiance|Plus de 100 approbations de partie de confiance|  
|1 \- 30 AD FS nœuds|WID pris en charge|Non pris en charge à l’aide de WID \- SQL requis|  
|Nœuds de plus de 30 AD FS|Non pris en charge à l’aide de WID \- SQL requis|Non pris en charge à l’aide de WID \- SQL requis|  
  
### <a name="alwayson-availability-groups"></a>Groupes de disponibilité AlwaysOn  
**Vue d’ensemble**  
  
Groupes de disponibilité AlwaysOn ont été introduits dans SQL Server 2012 et offrent un nouveau moyen pour créer une instance de SQL Server haute disponibilité.  Groupes de disponibilité AlwaysOn combinent des éléments de clustering et de mise en miroir de base de données pour la redondance et de basculement à la couche d’instance SQL et la couche de base de données.  Contrairement aux options de haute disponibilité précédentes, groupes de disponibilité AlwaysOn nécessitent un espace de stockage commun \(ou réseau de stockage\) au niveau de la couche de base de données.  
  
Un groupe de disponibilité se compose d’un réplica principal \(un ensemble de lecture\-écrire des bases de données primaires\) réplicas de disponibilité et un à quatre \(définit des bases de données secondaire correspondantes\).  Le groupe de disponibilité prend en charge une lecture unique\-écrire copie \(le réplica principal\), en lecture et un à quatre\-uniquement les réplicas de disponibilité.  Chaque réplica de disponibilité doit résider sur un autre nœud d’un seul serveur de Clustering de basculement Windows \(WSFC\) cluster.  Pour plus d’informations sur la disponibilité AlwaysOn groupes, consultez [vue d’ensemble des groupes de disponibilité AlwaysOn \(SQL Server\)](https://technet.microsoft.com/library/ff877884.aspx).  
  
Du point de vue des nœuds d’une batterie de serveurs AD FS SQL Server, le groupe de disponibilité AlwaysOn remplace l’instance de SQL Server unique en tant que la stratégie \/ base de données de l’artefact.  L’écouteur de groupe de disponibilité est le comportement du client \(le service de jeton de sécurité AD FS\) utilise pour se connecter à SQL.  
  
Le diagramme suivant illustre une batterie de serveurs AD FS SQL avec le groupe de disponibilité AlwaysOn.  
  
![batterie de serveurs à l’aide de SQL](media/alwaysonavailabilitygroups.jpg)  
  
> [!NOTE]  
> Groupes de disponibilité AlwaysOn nécessitent que les instances de SQL Server résident sur le Clustering de basculement Windows Server \(WSFC\) nœuds.  
  
> [!NOTE]  
> Qu’un seul réplica de disponibilité peut agir comme une cible de basculement automatique, les trois autres se fie aux basculements manuels.  
  
**Considérations relatives au déploiement de clés**  
  
Si vous envisagez d’utiliser des groupes de disponibilité AlwaysOn en association avec la réplication de fusion SQL Server, veuillez noter les problèmes décrits sous « Clé considérations relatives au déploiement à l’aide d’AD FS avec la réplication de fusion SQL Server » ci-dessous.  En particulier, lorsqu’un groupe de disponibilité AlwaysOn contenant une base de données est un abonné de réplication bascule, l’abonnement de réplication échoue. Pour reprendre la réplication, un administrateur de réplication doit reconfigurer manuellement l’abonné.  Consultez la description de SQL Server de problème spécifique au [abonnés de réplication et groupes de disponibilité AlwaysOn \(SQL Server\) ](https://technet.microsoft.com/library/hh882436.aspx) et prennent en charge globale d’instructions pour les groupes de disponibilité AlwaysOn avec options de réplication à [réplication, suivi des modifications, Capture de données modifiées et groupes de disponibilité AlwaysOn \(SQL Server\)](https://technet.microsoft.com/library/hh403414.aspx).  
  
**Configuration d’AD FS pour utiliser un groupe de disponibilité AlwaysOn**  
  
Configuration d’une batterie de serveurs AD FS avec des groupes de disponibilité AlwaysOn nécessite une légère modification à la procédure de déploiement d’AD FS :  
  
1.  Vous souhaitez sauvegarder les bases de données doivent être créés avant de pouvoir configurer les groupes de disponibilité AlwaysOn.  AD FS crée ses bases de données dans le cadre du programme d’installation et la configuration initiale du premier nœud de service de fédération d’une nouvelle batterie de serveurs AD FS SQL Server.  Dans le cadre de la configuration AD FS, vous devez spécifier une chaîne de connexion SQL, afin d’avoir à configurer le premier nœud de batterie de serveurs AD FS pour vous connecter directement à une instance SQL \(cela n’est que temporaire\).   Pour obtenir des conseils spécifiques sur la configuration d’une batterie de serveurs AD FS, notamment la configuration d’un nœud de batterie de serveurs AD FS avec une chaîne de connexion SQL server, consultez [configurer un serveur de fédération](../../ad-fs/deployment/Configure-a-Federation-Server.md).  
  
2.  Une fois que les bases de données AD FS ont été créées, affectez-les à des groupes de disponibilité AlwaysOn et créer l’écouteur TCP/IP courantes à l’aide des outils SQL Server et de traiter au [la création et Configuration des groupes de disponibilité \(deSQLServer\) ](https://technet.microsoft.com/library/ff878265.aspx).  
  
3.  Enfin, utilisez PowerShell pour modifier les propriétés AD FS pour mettre à jour la chaîne de connexion SQL pour utiliser l’adresse DNS de l’écouteur du groupe de disponibilité AlwaysOn.  
  
    Exemples de commandes PSH pour mettre à jour la chaîne de connexion SQL pour la base de données de configuration AD FS :  
  
    ```  
    PS:\>$temp= Get-WmiObject -namespace root/ADFS -class SecurityTokenService  
    PS:\>$temp.ConfigurationdatabaseConnectionstring=”data source=<SQLCluster\SQLInstance>; initial catalog=adfsconfiguration;integrated security=true”  
    PS:\>$temp.put()  
  
    ```  
  
4.  Exemples de commandes PSH pour mettre à jour la chaîne de connexion SQL pour la base de données de service de la résolution AD FS artefact :  
  
    ```  
    PS:\> Set-AdfsProperties –artifactdbconnection ”Data source=<SQLCluster\SQLInstance >;Initial Catalog=AdfsArtifactStore;Integrated Security=True”  
    ```  
  
### <a name="sql-server-merge-replication"></a>Réplication de fusion SQL Server  
A également introduit dans SQL Server 2012, permet à la réplication de fusion pour la redondance de données de stratégie AD FS avec les caractéristiques suivantes :  
  
-   Lire et écrire la fonctionnalité sur tous les nœuds \(pas seulement sur le serveur principal\)  
  
-   Petites quantités de données répliquées de manière asynchrone pour éviter d’introduire une latence au système  
  
Le diagramme suivant illustre un géographiquement redondant batteries de serveurs AD FS SQL Server avec la réplication de fusion \(1 serveur de publication, 2 abonnés\):  
  
![batterie de serveurs à l’aide de SQL](media/ADFSSQLGeoRedundancy3.png)  
  
**Considérations relatives au déploiement de clé pour l’utilisation d’AD FS avec la réplication de fusion SQL Server \(Notez les numéros dans le diagramme ci-dessus\)**  
  
-   La base de données du serveur de distribution n’est pas pris en charge pour une utilisation avec les groupes de disponibilité AlwaysOn ou la mise en miroir de base de données.  Voir la section SQL Server prennent en charge les instructions pour les groupes de disponibilité AlwaysOn avec les options de réplication à [réplication, suivi des modifications, Capture de données modifiées et groupes de disponibilité AlwaysOn \(SQL Server\)](https://technet.microsoft.com/library/hh403414.aspx).  
  
-   Lorsqu’un groupe de disponibilité AlwaysOn contenant une base de données est un abonné de réplication bascule, l’abonnement de réplication échoue. Pour reprendre la réplication, un administrateur de réplication doit reconfigurer manuellement l’abonné.  Consultez la description de SQL Server de problème spécifique au [abonnés de réplication et groupes de disponibilité AlwaysOn \(SQL Server\) ](https://technet.microsoft.com/library/hh882436.aspx) et prennent en charge globale d’instructions pour les groupes de disponibilité AlwaysOn avec options de réplication [réplication, suivi des modifications, Capture de données modifiées et groupes de disponibilité AlwaysOn \(SQL Server\)](https://technet.microsoft.com/library/hh403414.aspx).  
  
Pour obtenir des instructions plus détaillées sur la façon de configurer AD FS à utiliser une réplication de fusion SQL Server, consultez [le programme d’installation à la redondance géographique avec la réplication SQL Server](https://technet.microsoft.com/library/dn632406.aspx).  
  
## <a name="see-also"></a>Voir aussi  
[Planifier votre topologie de déploiement d’AD FS](Plan-Your-AD-FS-Deployment-Topology.md)  
[Guide de conception AD FS dans Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

