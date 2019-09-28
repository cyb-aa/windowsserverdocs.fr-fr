---
title: Configurer la redondance géographique avec Réplication SQL Server
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: active-directory-federation-services
ms.author: billmath
ms.assetId: 7b9f9a4f-888c-4358-bacd-3237661b1935
ms.openlocfilehash: 16cf1a237043aa546d4fc24164045aa9f9a1e6ac
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359817"
---
# <a name="setup-geographic-redundancy-with-sql-server-replication"></a>Configurer la redondance géographique avec Réplication SQL Server


> [!IMPORTANT]  
> Si vous souhaitez créer une batterie de AD FS et utiliser SQL Server pour stocker vos données de configuration, vous pouvez utiliser SQL Server 2008 ou une version ultérieure.
  
Si vous utilisez SQL Server comme base de données de configuration de AD FS, vous pouvez configurer géo @ no__t-0redundancy pour votre batterie de serveurs AD FS à l’aide de la réplication SQL Server. Géo @ no__t-0redundancy réplique les données entre deux sites distants géographiquement, de sorte que les applications puissent basculer d’un site à un autre. Ainsi, en cas de défaillance d’un site, toutes les données de configuration peuvent toujours être disponibles sur le deuxième site. Pour plus d’informations, consultez la section « SQL Server de la redondance géographique » dans la [batterie de serveurs de Fédération à l’aide de SQL Server](../design/Federation-Server-Farm-Using-SQL-Server.md).  
  
## <a name="prerequisites"></a>Prérequis  
Installez et configurez une batterie de serveurs SQL Server. Pour plus d’informations, [https://technet.microsoft.com/evalcenter/hh225126.aspx](https://technet.microsoft.com/evalcenter/hh225126.aspx)consultez. Dans le SQL Server initial, assurez-vous que le service SQL Server Agent est en cours d’exécution et qu’il est défini sur démarrage automatique.  
  
## <a name="create-the-second-replica-sql-server-for-geo-redundancy"></a>Créez le second \(replica @ no__t-1 SQL Server pour géo @ no__t-2redundancy  
  
1. Pour plus d’informations sur l’installation de SQL Server \(for, consultez [https://technet.microsoft.com/evalcenter/hh225126.aspx](https://technet.microsoft.com/evalcenter/hh225126.aspx). Copiez les fichiers de script CreateDB. SQL et SetPermissions. SQL résultants sur le serveur SQL de réplication.  
  
2. S’assurer SQL Server Agent Service est en cours d’exécution et défini en démarrage automatique  
  
3. Exécutez **Export @ no__t-1AdfsDeploymentSQLScript** sur le nœud principal AD FS pour créer des fichiers CreateDB. SQL et SetPermissions. Sql.  Par exemple : `PS:\>Export-AdfsDeploymentSQLScript -DestinationFolder . –ServiceAccountName CONTOSO\gmsa1$`.  
   Redondance géographique de la @no__t 0Set @ no__t-1
  
4. Copiez les scripts sur votre serveur secondaire.  Ouvrez le script CreateDB. SQL dans **sql Management Studio** , puis cliquez sur **exécuter**.
   Redondance géographique de la @no__t 0Set @ no__t-1

5. Ouvrez le script SetPermissions. SQL dans **sql Management Studio** , puis cliquez sur **exécuter**.
   Redondance géographique de la @no__t 0Set @ no__t-1 

   

> [!NOTE]
> Vous pouvez également utiliser les éléments suivants à partir de la ligne de commande. 
> 
>    `c:\>sqlcmd –i CreateDB.sql`  
> 
>    `c:\>sqlcmd –i SetPermissions.sql` 
> 
> ## <a name="create-publisher-settings-on-the-initial-sql-server"></a>Créer les paramètres du serveur de publication sur le SQL Server initial  
  
1. À partir de SQL Server Management Studio, sous **réplication**, cliquez avec le bouton droit sur **publications locales** , puis choisissez **nouvelle publication...** 
    @ no__t-4Set up Geographic Redundancy @ no__t-5 </br>  

2. Dans l’écran de l’Assistant Nouvelle publication, cliquez sur **suivant**.</br>
   Redondance géographique de la @no__t 0Set @ no__t-1 </br> 
  
3. Dans la page serveur de **distribution** , choisissez serveur local en tant que serveur de distribution, puis cliquez sur **suivant**.  
   Redondance géographique de la @no__t 0Set @ no__t-1 </br>   

4. Dans la page dossier d' **instantanés** , entrez \\ \ SQL1\repldata à la place du dossier par défaut. \(REMARQUE : Vous devrez peut-être créer ce partage vous-même @ no__t-0.  
   Redondance géographique de la @no__t 0Set @ no__t-1 </br>   
  
5. Choisissez **AdfsConfigurationV3** comme base de données de publication, puis cliquez sur **suivant**.  
   Redondance géographique de la @no__t 0Set @ no__t-1 </br>
  
6. Dans **type de publication**, choisissez **publication de fusion** , puis cliquez sur **suivant**.  
   Redondance géographique de la @no__t 0Set @ no__t-1 </br>
  
7. Sur les **types d’abonnés**, choisissez **SQL Server 2008 ou version ultérieure** , puis cliquez sur **suivant**.  
   Redondance géographique de la @no__t 0Set @ no__t-1 </br> 

8. Dans la page **Articles** , sélectionnez le nœud **tables** pour sélectionner toutes les tables, puis non **@ no__t-3check SyncProperties** table \(this l’une d’elles ne doit pas être répliquée @ no__t-5</br>
   Redondance géographique de la @no__t 0Set @ no__t-1 </br>    
  
9. Sur la page **Articles** , sélectionnez nœud **fonctions définies** par l’utilisateur pour sélectionner toutes les fonctions définies par l’utilisateur, puis cliquez sur **suivant**.  
   Redondance géographique de la @no__t 0Set @ no__t-1 </br>    

10. Sur la page **problèmes d’article** , cliquez sur **suivant**.  
    Redondance géographique de la @no__t 0Set @ no__t-1 </br>   

11. Dans la page **Filtrer les lignes** de la table, cliquez sur **suivant**.  
    Redondance géographique de la @no__t 0Set @ no__t-1 </br>   
12. Sur la page **agent d’instantané** , choisissez les valeurs par défaut immédiat et 14 jours, puis cliquez sur **suivant**.  
    Redondance géographique de la @no__t 0Set @ no__t-1 </br>   
    Vous devrez peut-être créer un compte de domaine pour l’agent SQL. Utilisez les étapes décrites dans [configurer la connexion SQL pour le compte de domaine contoso @ no__t-1sqlagent](Set-up-Geographic-Redundancy-with-SQL-Server-Replication.md#sqlagent) pour créer une connexion SQL pour ce nouvel utilisateur AD et affecter des autorisations spécifiques.  
  
13. Dans la page sécurité de l' **agent** , cliquez sur **paramètres de sécurité** et entrez le nom d’utilisateur @ no__t-2password d’un compte de domaine \(not un GMSA @ no__t-4 créé pour l’agent SQL, puis cliquez sur **OK**.  Cliquez sur **Suivant**.  
    Redondance géographique de la @no__t 0Set @ no__t-1 </br>  

14. Sur la page actions de l' **Assistant** , cliquez sur **suivant**.   
    Redondance géographique de la @no__t 0Set @ no__t-1 </br>

15. Dans la page **terminer l’Assistant** , entrez un nom pour votre publication, puis cliquez sur **Terminer**. 
    Redondance géographique de la @no__t 0Set @ no__t-1 </br>  

16. Une fois la publication créée, vous devez voir l’état de la réussite.  Cliquez sur **Fermer**.
    Redondance géographique de la @no__t 0Set @ no__t-1 </br>  

17. Dans SQL Server Management Studio, cliquez avec le bouton droit sur la nouvelle publication, puis cliquez sur **lancer le moniteur de réplication**.  
    Redondance géographique de la @no__t 0Set @ no__t-1 </br> 
  
## <a name="create-subscription-settings-on-the-replica-sql-server"></a>Créer des paramètres d’abonnement sur le réplica SQL Server  
Assurez-vous que vous avez créé les paramètres du serveur de publication sur le SQL Server initial comme décrit ci-dessus, puis effectuez la procédure suivante :  
  
1. Sur le SQL Server de réplica, dans SQL Server Management Studio, sous **réplication**, cliquez avec le bouton droit sur **abonnements locaux** , puis choisissez **nouvel abonnement...** . Redondance géographique de la @no__t 0Set @ no__t-1 </br>  

2. Sur la page de l' **Assistant nouvel abonnement** , cliquez sur **suivant**.
   Redondance géographique de la @no__t 0Set @ no__t-1 </br>   
  
3. Dans la page **publication** , sélectionnez le serveur de publication dans la liste déroulante.  Développez **AdfsConfigurationV3** et sélectionnez le nom de la publication créée ci-dessus, puis cliquez sur **suivant**.  
   Redondance géographique de la @no__t 0Set @ no__t-1 </br> 
  
4. Dans la page **agent de fusion l’emplacement** , sélectionnez **exécuter chaque agent sur son abonné \(pull abonnements @ no__t-3** \(The par défaut @ no__t-5, puis cliquez sur **suivant**.  
   Redondance géographique de la @no__t 0Set @ no__t-1 </br> Cela, ainsi que le type d’abonnement ci-dessous, déterminent la logique de résolution des conflits. \(For plus d’informations, consultez [détecter et résoudre les conflits de réplication de fusion](https://technet.microsoft.com/library/ms151191.aspx). </br>
 
5. Sur la page **abonnés** , sélectionnez **AdfsConfigurationV3** comme base de données de l’abonné, puis cliquez sur **suivant**.  
   Redondance géographique de la @no__t 0Set @ no__t-1 </br> 
  
6. Dans la page **sécurité de agent de fusion** , cliquez sur **...** , puis entrez le nom d’utilisateur et le mot de passe d’un compte de domaine \(not un GMSA @ no__t-3 créé pour l’agent SQL à l’aide de la zone de sélection, puis cliquez sur **suivant**.
   Redondance géographique de la @no__t 0Set @ no__t-1 </br> 
  
7. Dans **planification**de la synchronisation, choisissez **exécuter en continu** , puis cliquez sur **suivant**. 
   Redondance géographique de la @no__t 0Set @ no__t-1 </br> 
 
8. Sur **initialiser les abonnements**, cliquez sur **suivant**.  
   Redondance géographique de la @no__t 0Set @ no__t-1 </br> 
  
9. Dans **type d’abonnement**, choisissez **client** , puis cliquez sur **suivant**.  
  
   Les implications sont documentées [ici](https://technet.microsoft.com/library/ms151191.aspx) et [ici](https://technet.microsoft.com/library/ms151170.aspx).  Pour l’essentiel, nous prenons la résolution de conflit « tout d’abord vers l’éditeur gagne » et nous n’avons pas besoin de republier sur d’autres abonnés.  
   Redondance géographique de la @no__t 0Set @ no__t-1 </br>
   
10. Sur la page actions de l' **Assistant** , assurez-vous que **l’option créer l’abonnement** est cochée, puis cliquez sur **suivant**. 
    Redondance géographique de la @no__t 0Set @ no__t-1 </br>

11. Dans la page **terminer l’Assistant** , cliquez sur **Terminer**. 
    Redondance géographique de la @no__t 0Set @ no__t-1 </br>

12. Une fois que l’abonnement a terminé le processus de création, vous devriez voir la réussite. Cliquez sur **Fermer**. 
    Redondance géographique de la @no__t 0Set @ no__t-1 </br>
  
## <a name="verify-the-process-of-initialization-and-replication"></a>Vérifier le processus d’initialisation et de réplication  
  
1.  Sur le serveur SQL Server principal, cliquez avec le bouton droit sur @ no__t-0click le nœud **réplication** , puis cliquez sur **lancer le moniteur de réplication**.  
  
2.  Dans le **moniteur de réplication**, cliquez sur la publication.  
  
3.  Dans l’onglet **tous les abonnements** , cliquez avec le bouton droit et **Affichez les détails**.  
  
    Vous devez être en mesure de voir de nombreuses entrées sous **actions** pour la réplication initiale.  
  
4.  En outre, vous pouvez consulter le nœud **SQL Server Agent @ no__t-1Jobs** pour voir le travail @ no__t-2S @ no__t-3 planifié pour exécuter les opérations de la publication @ no__t-4subscription.  Seules les tâches locales sont affichées. Veillez donc à vérifier sur le serveur de publication et sur l’abonné pour la résolution des problèmes.  Droite @ no__t-0click un travail, puis sélectionnez **afficher l’historique** pour afficher l’historique et les résultats de l’exécution.  
  
## <a name="sqlagent"></a>Configurer la connexion SQL pour le compte de domaine CONTOSO @ no__t-1sqlagent  
  
1.  Créez une nouvelle connexion sur le serveur principal et le réplica SQL Server appelée CONTOSO @ no__t-0sqlagent \(Le nom du nouvel utilisateur de domaine créé et configuré dans la page sécurité de l' **agent** dans les procédures ci-dessus. \)  
  
2.  Dans SQL Server, cliquez avec le bouton droit sur no__t-0click la connexion que vous avez créée, sélectionnez Propriétés, puis sous l’onglet Mappage de l' **utilisateur** , mappez cette connexion aux bases de données **AdfsConfiguration** et **AdfsArtifact** avec les rôles public et DB @ no__t-4genevaservice. Mappez également cette connexion à la base de données de distribution et ajoutez le rôle DB @ no__t-0owner pour les tables distribution et adfsconfiguration.  Procédez comme applicable sur le serveur SQL principal et le réplica SQL Server. Pour plus d’informations, voir [Replication Agent Security Model](https://technet.microsoft.com/library/ms151868.aspx).  
  
3.  Accordez au compte de domaine correspondant des autorisations en lecture et en écriture sur le partage configuré en tant que serveur de distribution.  Veillez à définir les autorisations de lecture et d’écriture à la fois sur les autorisations de partage et sur les autorisations de fichier local.  
  
## <a name="configure-ad-fs-nodes-to-point-to-the-sql-server-replica-farm"></a>Configurez AD FS node @ no__t-0 @ no__t-1 pour pointer vers la batterie de serveurs de réplication SQL Server  
Maintenant que vous avez configuré la redondance géographique, vous pouvez configurer les nœuds de la batterie de serveurs AD FS de façon à ce qu’ils pointent vers votre réplica SQL Server batterie de serveurs à l’aide des fonctionnalités de la batterie de serveurs AD FS « Join » standard, à partir de l’interface utilisateur de l’Assistant Configuration de AD FS ou de Windows PowerShell.  
  
Si vous utilisez l’interface utilisateur de l’Assistant Configuration de AD FS, sélectionnez **Ajouter un serveur de Fédération à une batterie de serveurs de Fédération**. **Ne** sélectionnez pas **créer le premier serveur de Fédération dans une batterie de serveurs de Fédération**.  
  
Si vous utilisez Windows PowerShell, exécutez **Add @ no__t-1AdfsFarmNode**. **N'** exécutez pas **install @ no__t-2AdfsFarm**.  
  
Lorsque vous y êtes invité, fournissez l’hôte et le nom d’instance du réplica SQL Server, **pas** le serveur SQL initial.  
