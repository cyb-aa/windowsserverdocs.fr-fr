---
title: Configuration d’un déploiement de AD FS avec groupes de disponibilité AlwaysOn
author: billmath
ms.author: billmath
manager: daveba
ms.date: 01/20/2020
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 8660bcab5719029936588738352e542828081ce8
ms.sourcegitcommit: 371e59315db0cca5bdb713264a62b215ab43fd0f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82192615"
---
# <a name="setting-up-an-ad-fs-deployment-with-alwayson-availability-groups"></a>Configuration d’un déploiement de AD FS avec groupes de disponibilité AlwaysOn
Une topologie géo-distribuée à haute disponibilité fournit les éléments suivants :
* Élimination d’un point de défaillance unique : avec les fonctionnalités de basculement, vous pouvez obtenir une infrastructure ADFS à haut niveau de disponibilité même si l’un des centres de données dans une partie d’un globe tombe en panne.
* Amélioration des performances : vous pouvez utiliser le déploiement suggéré pour fournir une infrastructure ADFS à hautes performances.

AD FS peut être configuré pour un scénario géo-distribué à haut niveau de disponibilité.
Le guide suivant présente une vue d’ensemble des AD FS avec les groupes de disponibilité SQL Always on et fournit des considérations et des conseils sur le déploiement.

## <a name="overview---alwayson-availability-groups"></a>Vue d’ensemble-groupes de disponibilité AlwaysOn

Pour plus d’informations sur les groupes de disponibilité AlwaysOn, consultez [vue d’ensemble des groupes de disponibilité AlwaysOn (SQL Server)](https://technet.microsoft.com/library/ff877884.aspx)

Du point de vue des nœuds d’une batterie de serveurs AD FS SQL Server, le groupe de disponibilité AlwaysOn remplace l’instance de SQL Server unique en tant que base de données de stratégie/d’artefacts.L’écouteur du groupe de disponibilité est celui utilisé par le client (le service de jeton de sécurité AD FS) pour se connecter à SQL.
Le diagramme suivant illustre une batterie de serveurs AD FS SQL Server avec le groupe de disponibilité AlwaysOn.

![batterie de serveurs utilisant SQL](media/ad-fs-always-on/SQLoverview.png)

Un groupe de disponibilité Always On est une ou plusieurs bases de données utilisateur qui basculent ensemble. Un groupe de disponibilité se compose d’un réplica de disponibilité principal et d’un à quatre réplicas secondaires qui sont gérés par le biais d’SQL Server le déplacement des données basées sur les journaux pour la protection des données sans avoir besoin de stockage partagé. Chaque réplica est hébergé par une instance de SQL Server sur un nœud différent du WSFC. Le groupe de disponibilité et un nom de réseau virtuel correspondant sont inscrits en tant que ressources dans le cluster WSFC.

Un écouteur de groupe de disponibilité sur le nœud du réplica principal répond aux demandes entrantes des clients pour se connecter au nom du réseau virtuel et, selon les attributs de la chaîne de connexion, il redirige chaque requête vers l’instance de SQL Server appropriée.
En cas de basculement, au lieu de transférer la propriété des ressources physiques partagées à un autre nœud, WSFC est utilisé pour reconfigurer un réplica secondaire sur une autre instance de SQL Server pour qu’il devienne le réplica principal du groupe de disponibilité. La ressource de nom de réseau virtuel du groupe de disponibilité est ensuite transférée à cette instance.
À un moment donné, une seule instance de SQL Server peut héberger le réplica principal des bases de données d’un groupe de disponibilité, tous les réplicas secondaires associés doivent résider sur une instance distincte et chaque instance doit résider sur des nœuds physiques distincts.

> [!NOTE] 
> Si des ordinateurs s’exécutent sur Azure, configurez les machines virtuelles Azure pour permettre à la configuration de l’écouteur de communiquer avec les groupes de disponibilité AlwaysOn. Pour plus d’informations, sur les [machines virtuelles : écouteur SQL Always on](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener).

Pour obtenir une vue d’ensemble supplémentaire des groupes de disponibilité AlwaysOn, consultez [vue d’ensemble des groupes de disponibilité Always on (SQL Server)](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server?view=sql-server-ver15).

> [!NOTE] 
> Si l’organisation nécessite un basculement entre plusieurs centres de données, il est recommandé de créer une base de données d’artefacts dans chaque centre de données et d’activer un cache d’arrière-plan qui réduit la latence pendant le traitement des demandes. Suivez les instructions de cette procédure pour [affiner le paramétrage de SQL et réduire la latence](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/adfs-sql-latency).

## <a name="deployment-guidance"></a>Guide de déploiement

1. <b>Tenez compte de la base de données appropriée pour les objectifs du déploiement AD FS.</b> AD FS utilise une base de données pour stocker la configuration et, dans certains cas, les données transactionnelles liées au service FS (Federation Service). Vous pouvez utiliser AD FS logiciel pour sélectionner la base de données interne Windows (WID) ou Microsoft SQL Server 2008 ou une version ultérieure pour stocker les données dans le service de Fédération.
Le tableau suivant décrit les différences entre les fonctionnalités prises en charge entre un WID et une base de données SQL.


| Category      | Fonctionnalité       | Pris en charge par WID  | Pris en charge par SQL |
| ------------------ |:-------------:| :---:|:---: |
| Fonctionnalités de AD FS     | Déploiement d'une batterie de serveurs de fédération | Oui  | Oui |
| Fonctionnalités de AD FS     | Résolution d’artefacts SAML. Remarque : cela n’est pas courant pour les applications SAML     |   Non | Oui  |
| Fonctionnalités de AD FS | Détection de relecture de jetons SAML/WS-Federation. Remarque : requis uniquement lorsque AD FS reçoit des jetons d’un fournisseurs externe. Cela n’est pas obligatoire si AD FS n’agit pas comme un partenaire de Fédération.      |    Non  | Oui |
| Fonctionnalités de base de données     |   Redondance de base de données de base à l’aide de la réplication par extraction, où un ou plusieurs serveurs hébergeant une copie en lecture seule de la base de données demandent des modifications apportées à un hôte de serveur source une copie en lecture/écriture de la base de données    |   Non  | Non   |
| Fonctionnalités de base de données | Redondance de base de données à l’aide de solutions de haute disponibilité, telles que le clustering ou la mise en miroir (au niveau de la couche de base de données)      |    Non  | Oui |
| Fonctionnalités supplémentaires | Scénario facteurs OAuth     |   Oui  | Oui |

Si vous êtes une grande organisation avec plus de 100 relations d’approbation qui doivent fournir à leurs utilisateurs internes et externes un accès par authentification unique aux applications ou services de Fédération, SQL est l’option recommandée.

Si vous êtes une organisation avec 100 de plus ou moins de relations d’approbation configurées, le schéma WID fournit la redondance des données et du service de Fédération (où chaque serveur de Fédération réplique les modifications sur les autres serveurs de Fédération de la même batterie de serveurs). WID ne prend pas en charge la détection de relecture de jetons ou la résolution d’artefacts et a une limite de 30 serveurs de Fédération.
Pour plus d’informations sur la planification de votre déploiement, rendez-vous [ici](https://docs.microsoft.com/windows-server/identity/ad-fs/design/planning-your-deployment).

## <a name="sql-server-high-availability-solutions"></a>Solutions de haute disponibilité SQL Server
Si vous utilisez SQL Server comme base de données de configuration de AD FS, vous pouvez configurer la géo-redondance pour votre batterie de serveurs AD FS à l’aide de la réplication SQL Server. La géo-redondance réplique les données entre deux sites distants géographiquement, de sorte que les applications puissent basculer d’un site à un autre. Ainsi, en cas de défaillance d’un site, toutes les données de configuration peuvent toujours être disponibles sur le deuxième site. 
Si SQL est la base de données appropriée pour vos objectifs de déploiement, poursuivez avec ce guide de déploiement.

Ce guide vous aidera à effectuer les opérations suivantes :
* Déployer AD FS
* Configurez AD FS pour utiliser une groupes de disponibilité AlwaysOn
* Installer le rôle de clustering de basculement
* Exécuter les tests de validation de cluster
* Activer les groupes de disponibilité Always On
* Sauvegarder des bases de données AD FS
* Créer des groupes de disponibilité AlwaysOn
* Ajouter des bases de données sur le second nœud
* Joindre un réplica de disponibilité à un groupe de disponibilité
* Mettre à jour la chaîne de connexion SQL

## <a name="deploy-ad-fs"></a>Déployer AD FS

> [!NOTE] 
> Si des ordinateurs s’exécutent sur Azure, les machines virtuelles doivent être configurées de manière spécifique pour permettre à l’écouteur de communiquer avec le groupe Always On disponibilité du. Pour plus d’informations sur la configuration, consultez [configurer un équilibreur de charge pour un groupe de disponibilité sur des machines virtuelles Azure SQL Server](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener)


Ce guide de déploiement présentera une batterie de serveurs à deux nœuds avec deux serveurs SQL comme exemple.
Pour déployer AD FS suivez les liens initiaux ci-dessous pour installer le service de rôle AD FS. Pour configurer pour un groupe AoA, il y aura des étapes supplémentaires pour le rôle.
-   [Joindre un ordinateur à un domaine](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/join-a-computer-to-a-domain)
-   [Inscrire un certificat SSL pour AD FS](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/enroll-an-ssl-certificate-for-ad-fs)
-   [Installer le service de rôle AD FS](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/install-the-ad-fs-role-service)


## <a name="configuring-ad-fs-to-use-an-alwayson-availability-group"></a>Configuration de AD FS pour utiliser un groupe de disponibilité AlwaysOn

La configuration d’une batterie de serveurs AD FS avec des groupes de disponibilité AlwaysOn nécessite une légère modification de la procédure de déploiement AD FS. Vérifiez que chaque instance de serveur exécute la même version de SQL. Pour afficher la liste complète des conditions préalables requises, des restrictions et des recommandations pour groupes de disponibilité Always On, lisez [ce qui suit](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability?view=sql-server-2017#PrerequisitesForDbs).

1.  Les bases de données que vous souhaitez sauvegarder doivent être créées avant de pouvoir configurer les groupes de disponibilité AlwaysOn.  AD FS crée ses bases de données dans le cadre de l’installation et de la configuration initiale du premier nœud de service de Fédération d’une nouvelle batterie de serveurs SQL Server AD FS.  Spécifiez le nom d’hôte de base de données pour la batterie de serveurs existante à l’aide de SQL Server. Dans le cadre de la configuration de la AD FS, vous devez spécifier une chaîne de connexion SQL. vous devrez donc configurer la première batterie AD FS pour qu’elle se connecte directement à une instance SQL (uniquement temporaire). Pour obtenir des conseils spécifiques sur la configuration d’une batterie de serveurs AD FS, notamment la configuration d’un nœud de batterie de serveurs AD FS avec une chaîne de connexion SQL Server, consultez [configurer un serveur de Fédération](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/configure-a-federation-server).

![spécifier une batterie de serveurs](media/ad-fs-always-on/deploymentSpecifyFarm.png)

2.  Vérifiez la connectivité à la base de données à l’aide de SSMS, puis connectez-vous au nom d’hôte de la base de données ciblée. Si vous ajoutez un autre nœud à la batterie de serveurs de Fédération, connectez-vous à la base de données ciblée.
3.  Spécifiez le certificat SSL pour la batterie de serveurs AD FS.

![spécifier le certificat SSL](media/ad-fs-always-on/deploymentSpecifySSL.png)

4.  Connectez la batterie de serveurs à un compte de service ou à gMSA.

![spécifier le compte de service](media/ad-fs-always-on/deploymentSpecifyServiceAccount.png)

5.  Effectuez la configuration et l’installation de la batterie de serveurs AD FS.

> [!NOTE] 
> SQL Server doit être exécuté sous un compte de domaine pour l’installation de Always On groupes de disponibilité. Par défaut, il est exécuté en tant que système local.

## <a name="install-the-failover-clustering-role"></a>Installer le rôle de clustering de basculement
Le rôle de cluster de basculement Windows Server fournit des informations supplémentaires sur les clusters de basculement Windows Server.
1.  Démarrez le Gestionnaire de serveur.
2.  Dans le menu gérer, sélectionnez Ajouter des rôles et des fonctionnalités.
3.  Dans la page avant de commencer, sélectionnez suivant.
4.  Dans la page Sélectionner le type d’installation, sélectionnez installation basée sur un rôle ou une fonctionnalité, puis sélectionnez suivant.
5.  Dans la page Sélectionner le serveur de destination, sélectionnez le serveur SQL Server sur lequel vous voulez installer la fonctionnalité, puis sélectionnez suivant.

![serveurs de destination](media/ad-fs-always-on/clusteringDestinationServer.png)

6.  Dans la page Sélectionner des rôles serveurs, sélectionnez Suivant.
7.  Dans la page Sélectionner des fonctionnalités, cochez la case Clustering de basculement.

![sélectionner la fonctionnalité de clustering](media/ad-fs-always-on/clusteringFeature.png)

8.  Dans la page confirmer les sélections d’installation, sélectionnez Installer.
La fonctionnalité de clustering de basculement ne nécessite pas le redémarrage du serveur.
9.  Une fois l’installation terminée, sélectionnez Fermer.
10. Répétez cette procédure sur chaque serveur à ajouter comme nœud de cluster.

## <a name="run-cluster-validation-tests"></a>Exécuter les tests de validation de cluster
1.  Sur un ordinateur doté des outils de gestion du cluster de basculement installés à partir des outils d'administration de serveur distant, ou sur un serveur sur lequel vous avez installé la fonctionnalité de clustering de basculement, démarrez le Gestionnaire du cluster de basculement. Pour effectuer cette opération sur un serveur, démarrez Gestionnaire de serveur, puis, dans le menu Outils, sélectionnez Gestionnaire du cluster de basculement.
2.  Dans le volet Gestionnaire du cluster de basculement, sous gestion, sélectionnez valider la configuration.
3.  Dans la page Avant de commencer, sélectionnez Suivant.
4.  Dans la page Sélectionner des serveurs ou un cluster, dans la zone entrer le nom, entrez le nom NetBIOS ou le nom de domaine complet d’un serveur que vous prévoyez d’ajouter en tant que nœud de cluster de basculement, puis sélectionnez Ajouter. Répétez cette étape pour chaque serveur à ajouter. Pour ajouter plusieurs serveurs à la fois, séparez les noms d’une virgule ou d’un point-virgule. Par exemple, entrez les noms au format server1.contoso.com, server2.contoso.com. Quand vous avez terminé, cliquez sur Suivant.

![image sélectionner les serveurs](media/ad-fs-always-on/clusterValidationServers.png)

5. Dans la page Options de test, sélectionnez exécuter tous les tests (recommandé), puis cliquez sur suivant.
6. Dans la page confirmation, sélectionnez suivant.
La page de validation affiche l'état des tests en cours d'exécution.
7. Dans la page Résumé, procédez de l'une ou l'autre des façons suivantes :
- Si les résultats indiquent que les tests se sont terminés correctement et que la configuration est adaptée au clustering, et que vous souhaitez créer le cluster immédiatement, assurez-vous que la case à cocher créer le cluster maintenant en utilisant les nœuds validés est activée, puis sélectionnez Terminer. Ensuite, passez à l’étape 4 de la [procédure créer le cluster de basculement](https://docs.microsoft.com/windows-server/failover-clustering/create-failover-cluster#create-the-failover-cluster).

![valider l’image de configuration](media/ad-fs-always-on/clusterValidationResults.png)

-   Si les résultats indiquent que des avertissements ou des échecs se sont produits, sélectionnez Afficher le rapport pour afficher les détails et déterminer les problèmes qui doivent être corrigés. Notez qu'un avertissement dans le cadre d'un test de validation indique que l'aspect en question du cluster de basculement peut être pris en charge, mais qu'il n'est peut-être pas conforme aux meilleures pratiques.

> [!NOTE]
> Si vous recevez un avertissement pour le test Valider la réservation persistante des espaces de stockage, voir le billet de blog qui explique qu’[un avertissement de validation du cluster de basculement Windows indique que vos disques ne prennent pas en charge les réservations persistantes pour les espaces de stockage](https://blogs.msdn.microsoft.com/clustering/2013/05/24/validate-storage-spaces-persistent-reservation-test-results-with-warning/) pour plus d’informations.
> Pour plus d'informations sur les tests de validation du matériel, voir [Valider le matériel pour un cluster de basculement](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)).

## <a name="create-the-failover-cluster"></a>Création d’un cluster de basculement

Pour effectuer cette étape, assurez-vous que le compte d'utilisateur avec lequel vous ouvrez la session remplit les conditions décrites dans la section [Vérifier les conditions préalables](https://docs.microsoft.com/windows-server/failover-clustering/create-failover-cluster#verify-the-prerequisites) de cette rubrique.
1.  Démarrez le Gestionnaire de serveur.
2.  Dans le menu Outils, sélectionnez Gestionnaire du cluster de basculement.
3.  Dans le volet Gestionnaire du cluster de basculement, sous gestion, sélectionnez créer un cluster.
L'Assistant Création d'un cluster s'ouvre.
4.  Dans la page Avant de commencer, sélectionnez Suivant.
5.  Si la page Sélectionner les serveurs s’affiche, dans la zone entrer le nom, entrez le nom NetBIOS ou le nom de domaine complet d’un serveur que vous envisagez d’ajouter en tant que nœud de cluster de basculement, puis sélectionnez Ajouter. Répétez cette étape pour chaque serveur à ajouter. Pour ajouter plusieurs serveurs à la fois, séparez les noms d’une virgule ou d’un point-virgule. Par exemple, entrez les noms au format server1.contoso.com; server2.contoso.com. Quand vous avez terminé, cliquez sur Suivant.

![créer un cluster et sélectionner des serveurs](media/ad-fs-always-on/createClusterServers.png)

> [!NOTE]
> Si vous avez choisi de créer le cluster immédiatement après l’exécution de la validation dans la [procédure de validation](https://docs.microsoft.com/windows-server/failover-clustering/create-failover-cluster#validate-the-configuration)de la configuration, la page Sélectionner les serveurs ne s’affiche pas. Les nœuds qui ont été validés sont ajoutés automatiquement à l'Assistant Création d'un cluster, si bien que vous n'avez pas besoin de les entrer à nouveau.

6.  Si vous avez passé l'étape de validation antérieure, la page Avertissement de validation s'affiche. Nous vous recommandons vivement d'exécuter la validation du cluster. Microsoft assure uniquement la prise en charge des clusters qui ont réussi tous les tests de validation. Pour exécuter les tests de validation, sélectionnez Oui, puis cliquez sur suivant. Exécutez l’Assistant validation d’une configuration comme décrit dans [valider la configuration](https://docs.microsoft.com/windows-server/failover-clustering/create-failover-cluster#validate-the-configuration).
7.  Dans la page Point d'accès pour l'administration du cluster, procédez comme suit :
-   Dans la zone Nom du cluster, entrez le nom que vous voulez utiliser pour administrer le cluster. Avant cela, prenez connaissance des informations suivantes :
 -  Pendant la création du cluster, ce nom est inscrit en tant qu'objet ordinateur de cluster (aussi appelé objet nom de cluster ou CNO) dans AD DS. Si vous spécifiez un nom NetBIOS pour le cluster, le CNO est créé à l'emplacement où résident les objets ordinateur des nœuds du cluster. Il peut s'agir soit du conteneur Ordinateurs par défaut, soit d'une UO.
 -  Pour spécifier un autre emplacement pour le CNO, vous pouvez entrer le nom unique d'une UO dans la zone Nom du cluster. Par exemple : CN=ClusterName, OU=Clusters, DC=Contoso, DC=com.
 -  Si un administrateur du domaine a prédéfini le CNO dans une UO différente de celle où résident les nœuds du cluster, spécifiez le nom unique fourni par l'administrateur du domaine.
- Si la carte réseau du serveur n'a pas été configurée pour utiliser DHCP, vous devez configurer une ou plusieurs adresses IP statiques pour le cluster de basculement. Cochez la case correspondant à chaque réseau que vous voulez utiliser pour la gestion du cluster. Sélectionnez le champ adresse en regard d’un réseau sélectionné, puis entrez l’adresse IP que vous souhaitez affecter au cluster. Cette adresse IP (et les autres éventuelles) est associée au nom de cluster dans le système DNS (Domain Name System).
- Quand vous avez terminé, cliquez sur Suivant.

8.  Dans la page Confirmation, examinez les paramètres. Par défaut, la case Ajouter la totalité du stockage disponible au cluster est cochée. Décochez cette case dans l'un des cas suivants :
-   Vous souhaitez configurer le stockage à un moment ultérieur.
-   Vous envisagez de créer des espaces de stockage en cluster via le Gestionnaire du cluster de basculement ou les applets de commande de clustering de basculement Windows PowerShell, et vous n'avez pas encore créé d'espaces de stockage dans Services de fichiers et de stockage. Pour plus d'informations, voir [Déployer des espaces de stockage en cluster](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)).
9.  Sélectionnez suivant pour créer le cluster de basculement.
10. Dans la page Résumé, vérifiez que le cluster de basculement a bien été créé. Si des avertissements ou des erreurs se sont produits, affichez le résumé de la sortie ou sélectionnez Afficher le rapport pour afficher le rapport complet. Sélectionnez Terminer.
11. Pour vérifier que le cluster a été créé, assurez-vous que le nom du cluster figure bien sous Gestionnaire du cluster de basculement dans l'arborescence de navigation. Vous pouvez développer le nom du cluster, puis sélectionner des éléments sous nœuds, stockage ou réseaux pour afficher les ressources associées.
Notez que la réplication du nom du cluster dans DNS peut prendre un certain temps. Une fois l’inscription et la réplication DNS réussies, si vous sélectionnez tous les serveurs dans Gestionnaire de serveur, le nom du cluster doit être indiqué en tant que serveur avec l’état de facilité de gestion en ligne.

![création du cluster terminée](media/ad-fs-always-on/createClusterComplete.png)

## <a name="enable-always-on-availability-groups-with-sql-server-configuration-manager"></a>Activer les groupes de disponibilité Always on avec Gestionnaire de configuration SQL Server

1.  Connectez-vous au nœud de cluster de basculement Windows Server (WSFC) qui héberge l’instance SQL Server dans laquelle vous souhaitez activer Always On groupes de disponibilité.
2.  Dans le menu Démarrer, pointez sur tous les programmes, sur Microsoft SQL Server, sur outils de configuration, puis cliquez sur Gestionnaire de configuration SQL Server.
3.  Dans Gestionnaire de configuration SQL Server, cliquez sur services SQL Server, cliquez avec le bouton<instance name>droit sur SQL Server <instance name> (), où est le nom d’une instance de serveur local pour laquelle vous souhaitez activer les groupes de disponibilité Always on, puis cliquez sur Propriétés.
4.  Sélectionnez l’onglet Haute disponibilité Always On.
5.  Vérifiez que le champ Nom du cluster de basculement Windows contient le nom du cluster de basculement local. Si ce champ est vide, cette instance de serveur ne prend actuellement pas en charge les groupes de disponibilité Always On. Soit l’ordinateur local n’est pas un nœud de cluster, soit le cluster WSFC a été arrêté, soit cette édition de SQL Server qui ne prend pas en charge groupes de disponibilité Always On.
6.  Cochez la case Activer les groupes de disponibilité Always On , puis cliquez sur OK.
Le Gestionnaire de configuration SQL Server enregistre votre modification. Ensuite, vous devez redémarrer manuellement le service SQL Server. Cela vous permet de choisir l'heure de redémarrage la plus adaptée aux besoins de l'entreprise. Lors du redémarrage du service SQL Server, Always On est activé et la propriété du serveur IsHadrEnabled prend la valeur 1.

![activer AoA](media/ad-fs-always-on/enableAoAGroup.png)

## <a name="back-up-ad-fs-databases"></a>Sauvegarder des bases de données AD FS
Sauvegardez la configuration AD FS et les bases de données des artefacts avec les journaux de transactions complets. Placez la sauvegarde dans la destination choisie.
Sauvegardez l’artefact ADFS et les bases de données de configuration.
- Tâches > sauvegarde > complète > ajouter à un fichier de sauvegarde > OK pour créer

![sauvegarder le serveur](media/ad-fs-always-on/backUpADFS.png)

## <a name="create-new-availability-group"></a>Créer un nouveau groupe de disponibilité

1.  Dans l'Explorateur d'objets, connectez-vous à l'instance de serveur qui héberge le réplica principal.
2.  Développez le nœud Haute disponibilité AlwaysOn et le nœud Groupes de disponibilité .
3.  Pour lancer l'Assistant Nouveau groupe de disponibilité, sélectionnez la commande Assistant Nouveau groupe de disponibilité.
4.  La première fois que vous exécutez cet Assistant, une page Introduction apparaît. Pour ignorer cette page dans le futur, vous pouvez cliquer sur Ne plus afficher cette page. Après avoir lu cette page, cliquez sur Suivant.
5.  Sur la page Spécifier les options du groupe de disponibilité, entrez le nom du nouveau groupe de disponibilité dans le champ Nom du groupe de disponibilité. Ce nom doit être un identificateur de SQL Server valide qui est unique sur le cluster et dans l’ensemble de votre domaine. La longueur maximale d'un nom de groupe de disponibilité est de 128 caractères. e
6.  Ensuite, spécifiez le type de cluster. Les types de cluster possibles dépendent de la version de SQL Server et du système d’exploitation. Choisissez WSFC, EXTERNAL ou NONE. Pour plus d’informations, consultez la page [spécifier le nom du groupe de disponibilité](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/specify-availability-group-name-page?view=sql-server-ver15)

![nommer le groupe et le cluster AoA](media/ad-fs-always-on/createAoAName.png)

7.  Sur la page Sélectionner des bases de données, la grille répertorie les bases de données utilisateur sur l'instance de serveur connectée qui peuvent devenir des bases de données de disponibilité. Sélectionnez une ou plusieurs des bases de données répertoriées pour participer au nouveau groupe de disponibilité. Ces bases de données seront initialement les bases de données primaires initiales.
Pour chaque base de données répertoriée, la colonne Taille affiche la taille de la base de données, si elle est connue. La colonne État indique si une base de données particulière satisfait aux [conditions préalables](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability?view=sql-server-ver15) pour les bases de données de disponibilité. Si les conditions préalables requises ne sont pas remplies, une courte description de l'état indique la raison pour laquelle la base de données est inéligible ; par exemple, si elle n'utilise pas le mode de récupération complet. Pour plus d'informations, cliquez sur la description de l'état.
Si vous modifiez une base de données pour la rendre éligible, cliquez sur Actualiser pour mettre à jour la grille de bases de données.
Si la base de données contient une clé principale de base de données, entrez le mot de passe de la clé principale de base de données dans la colonne Mot de passe.

![sélectionner des bases de données pour AoA](media/ad-fs-always-on/createAoASelectDb.png)

8. dans la page spécifier les réplicas, spécifiez et configurez un ou plusieurs réplicas pour le nouveau groupe de disponibilité. Cette page contient quatre onglets. Le tableau suivant présente ces onglets. Pour plus d’informations, consultez la rubrique [page spécifier les réplicas (Assistant Nouveau groupe de disponibilité : Assistant Ajout de réplica)](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/specify-replicas-page-new-availability-group-wizard-add-replica-wizard?view=sql-server-ver15) .

| Onglet      | Brève description       |
| ------------------ |:-------------:|
| Réplicas     | Utilisez cet onglet pour spécifier chaque instance de SQL Server qui hébergera un réplica secondaire. Notez que l'instance de serveur à laquelle vous êtes actuellement connecté doit héberger le réplica principal. |
| Points de terminaison     | Utilisez cet onglet pour vérifier tous les points de terminaison de mise en miroir de bases de données existants et également, si ce point de terminaison manque sur une instance de serveur dont les comptes de service utilisent l'authentification Windows, pour créer le point de terminaison automatiquement.|
| Préférences de sauvegarde | Utilisez cet onglet pour spécifier vos préférences de sauvegarde pour le groupe de disponibilité dans son ensemble, ainsi que les priorités de sauvegarde pour les différents réplicas de disponibilité.      |
| Écouteur     | Utilisez cet onglet pour créer un écouteur de groupe de disponibilité. Par défaut, l'assistant ne crée pas d'écouteur.      |

![spécifier les détails du réplica](media/ad-fs-always-on/createAoAchooseReplica.png)

9. Sur la page Sélectionner la synchronisation de données initiale, choisissez comment vous souhaitez que vos nouvelles bases de données secondaires soient créées et jointes au groupe de disponibilité. Choisissez l’une des options suivantes :
-   Amorçage automatique
 - SQL Server crée automatiquement les réplicas secondaires pour chaque base de données du groupe. L’amorçage automatique nécessite que le chemin des données et des fichiers journaux soit le même sur chaque instance de SQL Server qui participe au groupe. Disponible sur SQL Server 2016 (13. x) et versions ultérieures. Consultez [initialiser automatiquement les groupes de disponibilité Always on](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/automatically-initialize-always-on-availability-group?view=sql-server-ver15).
- Sauvegarde complète de la base de données et des journaux
 - Sélectionnez cette option si votre environnement répond aux conditions requises pour démarrer automatiquement la synchronisation initiale des données (pour plus d’informations, consultez [conditions préalables requises, restrictions et recommandations, plus haut dans cette rubrique)](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio?view=sql-server-ver15#Prerequisites).
Si vous sélectionnez Complet, après avoir créé le groupe de disponibilité, l'assistant sauvegarde chaque base de données primaire et son journal des transactions sur un partage réseau et restaure les sauvegardes sur chaque instance de serveur qui héberge un réplica secondaire. L'assistant joint ensuite chaque base de données secondaire au groupe de disponibilité.
Dans le champ Spécifier un emplacement réseau partagé accessible par tous les réplicas :, spécifiez un partage de sauvegarde dans lequel l'intégralité de l'instance de serveur qui héberge les réplicas dispose d'un accès en lecture-écriture. Pour plus d'informations, consultez Conditions préalables requises, plus haut dans cette rubrique. Au cours de l’étape de validation, l’Assistant effectue un test pour vérifier que l’emplacement réseau fourni est valide. Le test crée sur le réplica principal une base de données portant le nom « BackupLocDb_ » suivi d’un GUID, sauvegarde la base de données à l’emplacement réseau fourni, puis la restaure sur les réplicas secondaires. Si l’Assistant ne parvient pas à supprimer cette base de données, son historique de sauvegarde et le fichier de sauvegarde, vous pouvez les supprimer en toute sécurité.
- Joindre uniquement
 - Si vous avez préparé manuellement les bases de données secondaires sur les instances de serveur qui hébergeront les réplicas secondaires, vous pouvez sélectionner cette option. L'assistant joindra les bases de données secondaires existantes au groupe de disponibilité.
- Ignorer la synchronisation de données initiale
 - Sélectionnez cette option si vous souhaitez utiliser votre propre base de données et sauvegardes de journaux de vos bases de données primaires. Pour plus d’informations, consultez [Démarrer le déplacement des données sur une base de données secondaire Always on (SQL Server)](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server?view=sql-server-ver15).

![choisir l’option de synchronisation des données](media/ad-fs-always-on/createAoADataSync.png)

9.  La page Validation vérifie si les valeurs que vous avez spécifiées dans cet Assistant répondent aux exigences de l'Assistant Nouveau groupe de disponibilité. Pour effectuer un changement, cliquez sur Précédent pour revenir à une page antérieure de l'assistant pour modifier une ou plusieurs valeurs. Cliquez sur Suivant pour revenir à la page Validation, puis cliquez sur Réexécuter la validation.

10. Sur la page Résumé, examinez vos choix pour le nouveau groupe de disponibilité. Pour apporter une modification, cliquez sur Précédent pour revenir à la page appropriée. Après avoir apporté la modification, cliquez sur Suivant pour revenir à la page Résumé.

> [!NOTE] 
> Lorsque le compte de service SQL Server d’une instance de serveur qui hébergera un nouveau réplica de disponibilité n’existe pas déjà en tant que compte de connexion, l’Assistant Nouveau groupe de disponibilité doit créer la connexion. Dans la page Résumé, l'Assistant affiche des informations pour la connexion qui doit être créée. Si vous cliquez sur Terminer, l'Assistant crée cette connexion pour le compte de service SQL Server et accorde l'autorisation de connexion CONNECT.
> Si vous êtes satisfait de vos sélections, cliquez éventuellement sur Script pour créer un script des étapes que l'assistant devra exécuter. Ensuite, pour créer et configurer le nouveau groupe de disponibilité, cliquez sur Terminer.

11. La page État d'avancement affiche l'état d'avancement des étapes de création du groupe de disponibilité (configuration de points de terminaison, création du groupe de disponibilité et jointure du réplica secondaire au groupe).
12. Lorsque ces étapes sont terminées, la page Résultats affiche le résultat de chaque étape. Si toutes ces étapes aboutissent, le nouveau groupe de disponibilité est entièrement configuré. Si l'une des étapes se solde par une erreur, vous devrez peut-être effectuer la configuration manuellement ou faire appel à l'assistant pour l'étape qui a échoué. Pour plus d'informations sur la cause d'une erreur donnée, cliquez sur le lien « Erreur » associé dans la colonne Résultat.
À la fin de l'Assistant, cliquez sur Fermer pour le quitter.

![validation terminée](media/ad-fs-always-on/createAoAValidation.png)

## <a name="add-databases-on-secondary-node"></a>Ajouter des bases de données sur le nœud secondaire

1.  Restaurez la base de données d’artefacts par le biais de l’interface utilisateur sur le nœud secondaire à l’aide des fichiers de sauvegarde créés.
![restaurer via l’interface utilisateur](media/ad-fs-always-on/restoreDB.png)

2. Restaurez la base de données dans un état de NON-récupération.
![restauration avec non-récupération](media/ad-fs-always-on/restoreNonRecovery.png)

3. Répétez le processus pour restaurer la base de données de configuration.

## <a name="join-availability-replica-to-an-availability-group"></a>Joindre le réplica de disponibilité à un groupe de disponibilité

1.  Dans l'Explorateur d'objets, connectez-vous à l'instance de serveur qui héberge le réplica secondaire, puis cliquez sur le nom du serveur pour développer son arborescence.
2.  Développez le nœud Haute disponibilité AlwaysOn et le nœud Groupes de disponibilité .
3.  Sélectionnez le groupe de disponibilité du réplica secondaire auquel vous êtes connecté.
4.  Cliquez avec le bouton droit sur le réplica secondaire, puis cliquez sur Joindre au groupe de disponibilité.
5.  Cette opération ouvre la boîte de dialogue Joindre le réplica au groupe de disponibilité.
6.  Pour joindre le réplica secondaire au groupe de disponibilité, cliquez sur OK.

![joindre le réplica secondaire](media/ad-fs-always-on/jointoAoA.png)

## <a name="update-the-sql-connection-string"></a>Mettre à jour la chaîne de connexion SQL
Enfin, utilisez PowerShell pour modifier les propriétés de AD FS afin de mettre à jour la chaîne de connexion SQL afin d’utiliser l’adresse DNS de l’écouteur du groupe de disponibilité AlwaysOn.
Exécutez la modification de la base de données de configuration sur chaque nœud et redémarrez le service ADFS sur tous les nœuds ADFS. La valeur initiale du catalogue change en fonction de la version de la batterie de serveurs.

```
PS:\>$temp= Get-WmiObject -namespace root/ADFS -class SecurityTokenService
PS:\>$temp.ConfigurationdatabaseConnectionstring=”data source=<SQLCluster\SQLInstance>; initial catalog=adfsconfiguration;integrated security=true”
PS:\>$temp.put()
PS:\> Set-AdfsProperties –artifactdbconnection ”Data source=<SQLCluster\SQLInstance >;Initial Catalog=AdfsArtifactStore;Integrated Security=True”
```
