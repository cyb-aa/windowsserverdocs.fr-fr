---
ms.assetid: e983d2ab-4153-41e7-b243-12cf7d71a552
title: "Batterie de serveurs de fédération à l’aide de SQL Server"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2333f79c733415833b1d54afc8c385700ac5581e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="federation-server-farm-using-sql-server"></a>Batterie de serveurs de fédération à l’aide de SQL Server

>S’applique à: Windows Server2016, Windows Server2012R2

Cette topologie pour ActiveDirectory Federation Services \(ADFS\) diffère de la batterie de serveurs de fédération à l’aide de la topologie de déploiement de base de données interne Windows \(WID\) dans la mesure où elle ne réplique pas les données pour chaque serveur de fédération dans la batterie de serveurs. Au lieu de cela, tous les serveurs de fédération de la batterie de serveurs peuvent lire et écrire des données dans une base de données commune qui est stocké sur un serveur exécutant MicrosoftSQLServer qui se trouve dans le réseau d’entreprise.  
  
> [!IMPORTANT]  
> Si vous souhaitez créer une batterie ADFS et utiliser SQLServer pour stocker vos données de configuration, vous pouvez utiliser SQLServer2008 et versions plus récentes, notamment SQLServer2012 et SQLServer2014.  
  
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
Les versions suivantes de SQL server sont prises en charge avec ADFS dans Windows Server2012R2:  
  
-   SQLServer2008 \ / R2  
  
-   SQL Server 2012  
  
-   SQLServer2014  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recommandations de mise en réseau et la sélection élective serveur  
Similaire à la batterie de serveurs de fédération avec topologie WID, tous les serveurs de fédération dans la batterie de serveurs sont configurés pour utiliser un nom de cluster \(DNS\) Domain Name System \ (qui représente le Service de fédération nom\) et l’adresse IP d’un cluster cadre de la configuration du cluster \(NLB\) équilibrage de charge réseau. Cela permet à l’hôte NLB d’allouer les demandes des clients vers les serveurs de fédération individuels. Serveurs proxy de fédération peut servir aux demandes de client de proxy pour la batterie de serveurs de fédération.  
  
L’illustration suivante montre comment la société fictive Contoso Pharmaceuticals déployé sa batterie de serveurs de fédération avec topologie SQLServer dans le réseau d’entreprise. Il montre également comment cette société configuré le réseau de périmètre avec accès à un serveur DNS, un hôte NLB supplémentaire qui utilise le même nom \(fs.contoso.com\) cluster DNS qui est utilisé sur le cluster NLB du réseau d’entreprise, et avec deux proxys d’application web \(wap1 and wap2\).  
  
![Batterie de serveurs à l’aide de SQL](media/SQLFarmADFSBlue.gif)  
  
Pour plus d’informations sur la configuration de votre environnement réseau pour une utilisation avec les serveurs de fédération ou proxys d’application web, consultez «Conditions requises pour la résolution de nom» section [configuration ADFS requise](AD-FS-Requirements.md) et [planifier l’Infrastructure Web Application Proxy (WAP)](https://technet.microsoft.com/library/dn383648.aspx).  
  
## <a name="high-availability-options-for-sql-server-farms"></a>Options de haute disponibilité pour les batteries de serveurs SQL  
Dans Windows Server2012R2, ADFS il existe deux options de nouveautées pour prendre en charge la haute disponibilité dans les batteries de serveurs ADFS à l’aide de SQLServer.  
  
-   Prise en charge des groupes de disponibilité AlwaysOn SQLServer  
  
-   Prise en charge pour une disponibilité élevée distribuée à l’aide de la réplication de fusion de SQLServer  
  
Cette section décrit chacune de ces options, quels sont les problèmes qu’ils respectivement résoudre et certaines considérations relatives à la clé pour déterminer les options de déploiement.  
  
> [!NOTE]  
> Batteries de serveurs ADFS qui utilisent la base de données interne Windows \(WID\) fournissent la redondance de base de données avec accès en lecture/écriture sur le nœud de serveur de fédération principal et l’accès en lecture seule sur les nœuds secondaires.  Cela peut servir dans géographiquement local ou une topologie distribuée géographiquement.  
>   
> Lorsque vous utilisez WID tenez compte des restrictions suivantes:  
>   
> -   Une batterie WID dispose d’un nombre maximal de serveurs de fédération 30 si vous avez moins de 100 approbations de partie confiance.  
> -   Une batterie WID ne prend pas en charge résolution détection ou artefact de relecture de jetons \ (partie du \(SAML\) protocol\ Security Assertion Markup Language).  
  
Le tableau suivant fournit un résumé pour l’utilisation d’une batterie WID.  
  
||||  
|-|-|-|  
||1 \-100 RP approbations|Plus de 100 RP approbations|  
|1 \-30 AD FS nœuds|WID pris en charge|Non pris en charge à l’aide de WID \-SQL requis|  
|Plus de 30 AD FS nœuds|Non pris en charge à l’aide de WID \-SQL requis|Non pris en charge à l’aide de WID \-SQL requis|  
  
### <a name="alwayson-availability-groups"></a>Groupes de disponibilité AlwaysOn  
**Vue d’ensemble**  
  
Groupes de disponibilité AlwaysOn ont été introduits dans SQLServer2012 et fournissent une nouvelle façon de créer une instance de SQLServer haute disponibilité.  Groupes de disponibilité AlwaysOn combinent des éléments de clustering et de la mise en miroir de base de données pour la redondance et de basculement à la couche d’instance SQL et la couche de base de données.  Contrairement aux options de haute disponibilité précédentes, les groupes de disponibilité AlwaysOn ne nécessitent pas un espace de stockage commun \ (ou Updatable de zone de stockage) à la couche de base de données.  
  
Un groupe de disponibilité se compose d’un réplica principal \ (un ensemble de databases\ principal en lecture-écriture) réplicas de disponibilité et d’un à quatre \ (ensembles de correspondante databases\ secondaire).  Le groupe de disponibilité prend en charge une copie unique en lecture-écriture \ (replica\ principal), les réplicas de disponibilité en lecture seule et un maximum de quatre.  Chaque réplica de disponibilité doit résider sur un nœud différent d’un seul cluster \(WSFC\) le Clustering de basculement Windows Server.  Pour plus d’informations sur la disponibilité AlwaysOn Voir groupes [vue d’ensemble de groupes de disponibilité AlwaysOn \(SQLServer\)](https://technet.microsoft.com/library/ff877884.aspx).  
  
Du point de vue des nœuds d’une batterie de serveurs ADFS SQLServer, le groupe de disponibilité AlwaysOn remplace l’instance SQLServer unique comme stratégie \ / base de données de l’artefact.  L’écouteur du groupe de disponibilité est le comportement du client \ (le service\ de jeton du sécurité de services ADFS) pour se connecter à SQL.  
  
Le diagramme suivant illustre une batterie ADFS SQLServer avec le groupe de disponibilité AlwaysOn.  
  
![Batterie de serveurs à l’aide de SQL](media/alwaysonavailabilitygroups.jpg)  
  
> [!NOTE]  
> Groupes de disponibilité AlwaysOn nécessitent que les instances de SQLServer résident sur des nœuds \(WSFC\) le Clustering de basculement Windows Server.  
  
> [!NOTE]  
> Réplica de disponibilité qu’un seul peut agir comme une cible de basculement automatique, les trois autres s’appuient sur les basculements manuelles.  
  
**Considérations relatives au déploiement de clé**  
  
Si vous prévoyez d’utiliser des groupes de disponibilité AlwaysOn en combinaison avec la réplication de fusion de SQLServer, prenez note des problèmes ci-dessous sous «Considérations sur le déploiement de la clé pour l’utilisation d’ADFS avec la réplication de fusion SQLServer».  En particulier, lorsqu’un groupe de disponibilité AlwaysOn contenant une base de données est un abonné de réplication échoue, l’abonnement de la réplication échoue. Pour reprendre la réplication, un administrateur de la réplication doit reconfigurer manuellement l’abonné.  Consultez la description de SQLServer de problème spécifique au [\(SQLServer\) abonnés de réplication et les groupes de disponibilité AlwaysOn](https://technet.microsoft.com/library/hh882436.aspx) et prendre en charge globale d’instructions pour les groupes de disponibilité AlwaysOn avec les options de réplication à [\(SQLServer\) la réplication, le suivi des modifications, la Capture de données modifiées et groupes de disponibilité AlwaysOn](https://technet.microsoft.com/library/hh403414.aspx).  
  
**La configuration ADFS à utiliser un groupe de disponibilité AlwaysOn**  
  
La configuration d’une batterie ADFS avec des groupes de disponibilité AlwaysOn requiert une petite modification à la procédure de déploiement d’ADFS:  
  
1.  Vous souhaitez sauvegarder les bases de données doivent être créés avant de pouvoir configurer les groupes de disponibilité AlwaysOn.  ADFS crée les bases de données dans le cadre du programme d’installation et la configuration initiale du premier nœud de service de fédération d’une nouvelle batterie ADFS SQLServer.  Dans le cadre de la configuration ADFS, vous devez spécifier une chaîne de connexion SQL, afin d’avoir à configurer le premier nœud de la batterie de serveurs ADFS pour se connecter directement à une instance SQL \ (il s’agit temporary\ uniquement).   Pour obtenir des instructions spécifiques sur la configuration d’une batterie ADFS, y compris la configuration d’un nœud de la batterie de serveurs ADFS avec une chaîne de connexion SQL server, voir [configurer un serveur de fédération](../../ad-fs/deployment/Configure-a-Federation-Server.md).  
  
2.  Une fois les bases de données ADFS ont été créées, affectez-les à des groupes de disponibilité AlwaysOn et créer l’écouteur TCPIP courantes à l’aide des outils SQLServer et traiter à [\(SQLServer\) la création et la Configuration de groupes de disponibilité](https://technet.microsoft.com/library/ff878265.aspx).  
  
3.  Enfin, utilisez PowerShell pour modifier les propriétés d’ADFS pour mettre à jour de la chaîne de connexion SQL pour utiliser l’adresse DNS de l’écouteur du groupe de disponibilité AlwaysOn.  
  
    Exemples de commandes PSH pour mettre à jour de la chaîne de connexion SQL pour la base de données de stratégie ADFS:  
  
    ```  
    PS:\>$temp= Get-WmiObject -namespace root/ADFS -class SecurityTokenService  
    PS:\>$temp.ConfigurationdatabaseConnectionstring=”data source=<SQLCluster\SQLInstance>; initial catalog=adfsconfiguration;integrated security=true”  
    PS:\>$temp.put()  
  
    ```  
  
4.  Exemples de commandes PSH pour mettre à jour de la chaîne de connexion SQL pour la base de données de stratégie ADFS:  
  
    ```  
    PS:\> Set-AdfsProperties –artifactdbconnection ”Data source=<SQLCluster\SQLInstance >;Initial Catalog=AdfsArtifactStore;Integrated Security=True”  
    ```  
  
### <a name="sql-server-merge-replication"></a>Réplication de fusion SQLServer  
La réplication de fusion également introduite dans SQLServer2012, permet la redondance de données de stratégie ADFS avec les caractéristiques suivantes:  
  
-   Lecture et d’écriture de fonctionnalité sur tous les nœuds \ (et pas seulement le primary\)  
  
-   Plus petites quantités de données répliquées de manière asynchrone pour éviter d’introduire des temps de latence au système  
  
Le diagramme suivant illustre un géographiquement redondants batteries de serveurs ADFS SQLServer avec la réplication de fusion \ (1 éditeur, 2subscribers\):  
  
![Batterie de serveurs à l’aide de SQL](media/ADFSSQLGeoRedundancy3.png)  
  
**Considérations relatives à la clé de déploiement pour l’utilisation d’ADFS avec la réplication de fusion SQLServer \ (Notez les nombres dans le diagramme above\)**  
  
-   La base de données du serveur de distribution n’est pas prise en charge pour une utilisation avec des groupes de disponibilité AlwaysOn ou la mise en miroir de base de données.  Voir SQLServer prennent en charge des instructions pour les groupes de disponibilité AlwaysOn avec les options de réplication à [\(SQLServer\) la réplication, le suivi des modifications, la Capture de données modifiées et groupes de disponibilité AlwaysOn](https://technet.microsoft.com/library/hh403414.aspx).  
  
-   Lorsqu’un groupe de disponibilité AlwaysOn contenant une base de données est un abonné de réplication échoue, l’abonnement de la réplication échoue. Pour reprendre la réplication, un administrateur de la réplication doit reconfigurer manuellement l’abonné.  Consultez la description de SQLServer de problème spécifique au [\(SQLServer\) abonnés de réplication et les groupes de disponibilité AlwaysOn](https://technet.microsoft.com/library/hh882436.aspx) et prendre en charge globale d’instructions pour les groupes de disponibilité AlwaysOn avec les options de réplication [\(SQLServer\) la réplication, le suivi des modifications, la Capture de données modifiées et groupes de disponibilité AlwaysOn](https://technet.microsoft.com/library/hh403414.aspx).  
  
Pour obtenir des instructions plus détaillées sur la façon de configurer ADFS à utiliser une réplication de fusion SQLServer, voir [le programme d’installation de redondance géographique avec la réplication SQLServer](https://technet.microsoft.com/en-us/library/dn632406.aspx).  
  
## <a name="see-also"></a>Voir aussi  
[Planifier votre topologie de déploiement ADFS](Plan-Your-AD-FS-Deployment-Topology.md)  
[Guide de conception ADFS dans Windows Server2012R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

