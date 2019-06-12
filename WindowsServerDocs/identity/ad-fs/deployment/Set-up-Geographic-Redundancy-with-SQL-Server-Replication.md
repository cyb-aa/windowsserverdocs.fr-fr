---
title: Le programme d’installation à la redondance géographique avec la réplication SQL Server
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: active-directory-federation-services
ms.author: billmath
ms.assetId: 7b9f9a4f-888c-4358-bacd-3237661b1935
ms.openlocfilehash: 00880d06835fad08538f634fdf2868146fc23b1a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442921"
---
# <a name="setup-geographic-redundancy-with-sql-server-replication"></a>Le programme d’installation à la redondance géographique avec la réplication SQL Server


> [!IMPORTANT]  
> Si vous souhaitez créer une batterie de serveurs AD FS et utiliser SQL Server pour stocker vos données de configuration, vous pouvez utiliser SQL Server 2008 ou version ultérieure.
  
Si vous utilisez SQL Server en tant que votre base de données de configuration AD FS, vous pouvez configurer géo\-redondance pour votre batterie de serveurs AD FS à l’aide de la réplication SQL Server. Geo\-redondance réplique les données entre deux sites éloignés afin que les applications peuvent passer d’un site vers un autre. De cette façon, en cas de défaillance d’un site, vous pouvez toujours avoir toutes les données de configuration disponibles sur le deuxième site. Pour plus d’informations, consultez la section « redondance géographique de le SQL Server » dans [batterie de serveur de fédération à l’aide de SQL Server](../design/Federation-Server-Farm-Using-SQL-Server.md).  
  
## <a name="prerequisites"></a>Prérequis  
Installez et configurez une batterie de serveurs SQL. Pour plus d’informations, consultez [ https://technet.microsoft.com/evalcenter/hh225126.aspx ](https://technet.microsoft.com/evalcenter/hh225126.aspx). Sur le serveur SQL initiale, assurez-vous que le service SQL Server Agent est en cours d’exécution et configuré pour démarrer automatiquement.  
  
## <a name="create-the-second-replica-sql-server-for-geo-redundancy"></a>Créez le deuxième \(réplica\) SQL Server pour géo\-redondance  
  
1. Installer SQL Server \(pour plus d’informations, consultez [ https://technet.microsoft.com/evalcenter/hh225126.aspx ](https://technet.microsoft.com/evalcenter/hh225126.aspx). Copiez les fichiers de script CreateDB.sql et SetPermissions.sql résultant vers le serveur SQL de réplica.  
  
2. Vérifiez le service SQL Server Agent est en cours d’exécution et configuré pour démarrer automatiquement  
  
3. Exécutez **exporter\-AdfsDeploymentSQLScript** sur le nœud principal AD FS pour créer des fichiers CreateDB.sql et SetPermissions.sql.  Par exemple :`PS:\>Export-AdfsDeploymentSQLScript -DestinationFolder . –ServiceAccountName CONTOSO\gmsa1$`.  
   ![Configurer une redondance géographique](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql2.png)
  
4. Copiez les scripts à votre serveur secondaire.  Ouvrez le script CreateDB.sql dans **SQL Management Studio** et cliquez sur **Execute**.
   ![Configurer une redondance géographique](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql4.png)

5. Ouvrez le script SetPermissions.sql dans **SQL Management Studio** et cliquez sur **Execute**.
   ![Configurer une redondance géographique](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql6.png) 

   

> [!NOTE]
> Vous pouvez également utiliser la commande suivante dans la ligne de commande. 
> 
>    `c:\>sqlcmd –i CreateDB.sql`  
> 
>    `c:\>sqlcmd –i SetPermissions.sql` 
> 
> ## <a name="create-publisher-settings-on-the-initial-sql-server"></a>Créer des paramètres de serveur de publication sur le serveur SQL initiale  
  
1. À partir de SQL Server Management studio, sous **réplication**, avec le bouton droit cliquez sur **Publications locales** et choisissez **nouvelle Publication... ** 
    ![Configurer une redondance géographique](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql7.png) </br>  

2. Dans l’écran de l’Assistant Nouvelle Publication cliquez **suivant**.</br>
   ![Configurer une redondance géographique](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql8.png) </br> 
  
3. Sur **distributeur** page, choisissez le serveur local en tant que serveur de distribution, puis cliquez sur **suivant**.  
   ![Configurer une redondance géographique](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql9.png) </br>   

4. Sur le **instantané** dossier, entrez \\\SQL1\repldata à la place de dossier par défaut. \(REMARQUE : Vous devrez peut-être créer ce partage vous-même\).  
   ![Configurer une redondance géographique](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql10.png) </br>   
  
5. Choisissez **AdfsConfigurationV3** comme la base de données de publication et cliquez sur **suivant**.  
   ![Configurer une redondance géographique](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql11.png) </br>
  
6. Sur **Type de Publication**, choisissez **publication de fusion** et cliquez sur **suivant**.  
   ![Configurer une redondance géographique](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql12.png) </br>
  
7. Sur **Types d’abonnés**, choisissez **SQL Server 2008 ou version ultérieure** et cliquez sur **suivant**.  
   ![Configurer une redondance géographique](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql13.png) </br> 

8. Sur le **Articles** page Sélectionnez **Tables** nœud pour sélectionner toutes les tables, puis **Annuler\-vérifier SyncProperties** table \(celle-ci ne doit pas être répliquée\)</br>
   ![Configurer une redondance géographique](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql14.png) </br>    
  
9. Sur le **Articles** page, sélectionnez **fonctions utilisateur** nœud pour sélectionner toutes les fonctions de définies par l’utilisateur sur **suivant**...  
   ![Configurer une redondance géographique](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql15.png) </br>    

10. Sur le **problèmes liés aux articles** page, cliquez sur **suivant**.  
    ![Configurer une redondance géographique](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql16.png) </br>   

11. Sur le **filtrer les lignes de Table** , cliquez sur **suivant**.  
    ![Configurer une redondance géographique](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql17.png) </br>   
12. Sur le **Agent d’instantané** , choisissez les valeurs par défaut d’immédiat et 14 jours, cliquez sur **suivant**.  
    ![Configurer une redondance géographique](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql18.png) </br>   
    Vous devrez peut-être créer un compte de domaine de l’agent SQL. Utilisez les étapes de [connexion SQL de configurer pour le compte de domaine CONTOSO\\sqlagent](Set-up-Geographic-Redundancy-with-SQL-Server-Replication.md#sqlagent) pour créer une connexion SQL pour ce nouvel utilisateur Active Directory et affecter des autorisations spécifiques.  
  
13. Sur le **sécurité de l’Agent** , cliquez sur **paramètres de sécurité** et entrez le nom d’utilisateur\/mot de passe d’un compte de domaine \(pas un compte GMSA\) créé pour l’agent SQL et Cliquez sur **OK**.  Cliquez sur **Suivant**.  
    ![Configurer une redondance géographique](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql19.png) </br>  

14. Sur le **Actions de l’Assistant** , cliquez sur **suivant**.   
    ![Configurer une redondance géographique](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql20.png) </br>

15. Sur le **terminer l’Assistant** page, entrez un nom pour votre publication, cliquez sur **Terminer**. 
    ![Configurer une redondance géographique](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql21.png) </br>  

16. Une fois que la publication est créée, vous devez voir l’état de réussite.  Cliquez sur **Fermer**.
    ![Configurer une redondance géographique](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql22.png) </br>  

17. Nouveau dans SQL Server Management Studio, cliquez sur la nouvelle Publication et cliquez sur **lancer le moniteur de réplication**.  
    ![Configurer une redondance géographique](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql23.png) </br> 
  
## <a name="create-subscription-settings-on-the-replica-sql-server"></a>Créer des paramètres d’abonnement sur le réplica SQL Server  
Assurez-vous que vous avez créé les paramètres de serveur de publication sur le serveur SQL initiale comme décrit ci-dessus et puis effectuez la procédure suivante :  
  
1. Sur le réplica SQL Server, à partir de SQL Server Management studio, sous **réplication**, avec le bouton droit cliquez sur **abonnements locaux** et choisissez **nouvel abonnement...** . ![Configurer une redondance géographique](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql24.png) </br>  

2. Sur le **Assistant Nouvel abonnement** , cliquez sur **suivant**.
   ![Configurer une redondance géographique](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql25.png) </br>   
  
3. Sur le **Publication** , sélectionnez le serveur de publication à partir de la liste déroulante.  Développez **AdfsConfigurationV3** et sélectionnez le nom de la publication créée ci-dessus, puis cliquez sur **suivant**.  
   ![Configurer une redondance géographique](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql26.png) </br> 
  
4. Sur le **emplacement de l’Agent de fusion** page, sélectionnez **exécuter chaque agent sur son abonné \(abonnements par extraction\)**  \(la valeur par défaut\) et cliquez sur  **Suivant**.  
   ![Configurer une redondance géographique](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql27.png) </br> En même temps que le Type d’abonnement ci-dessous, ce contrôle détermine la logique de résolution de conflit. \(Pour plus d’informations, consultez [détecter et résoudre les conflits de réplication de fusion](https://technet.microsoft.com/library/ms151191.aspx). </br>
 
5. Sur le **abonnés** page, sélectionnez **AdfsConfigurationV3** comme la base de données abonné et cliquez sur **suivant**.  
   ![Configurer une redondance géographique](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql28.png) </br> 
  
6. Sur le **sécurité de l’Agent de fusion** , cliquez sur **...**  et entrez le nom d’utilisateur et le mot de passe d’un compte de domaine \(pas un compte GMSA\) créé pour l’agent SQL à l’aide de la boîte de points de suspension et cliquez sur **suivant**.
   ![Configurer une redondance géographique](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql30.png) </br> 
  
7. Sur **calendrier de synchronisation**, choisissez **exécuter en continu** et cliquez sur **suivant**. 
   ![Configurer une redondance géographique](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql31.png) </br> 
 
8. Sur **initialiser les abonnements**, cliquez sur **suivant**.  
   ![Configurer une redondance géographique](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql32.png) </br> 
  
9. Sur **Type d’abonnement**, choisissez **Client** et cliquez sur **suivant**.  
  
   Implications sont documentées [ici](https://technet.microsoft.com/library/ms151191.aspx) et [ici](https://technet.microsoft.com/library/ms151170.aspx).  En fait, nous prenons la résolution de conflit « première pour l’éditeur gagne » simple et nous n’avez pas besoin de republier vers d’autres abonnés.  
   ![Configurer une redondance géographique](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql33.png) </br>
   
10. Sur le **Actions de l’Assistant** , vérifiez **créer l’abonnement** est activée et cliquez sur **suivant**. 
    ![Configurer une redondance géographique](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql34.png) </br>

11. Sur le **terminer l’Assistant** , cliquez sur **Terminer**. 
    ![Configurer une redondance géographique](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql35.png) </br>

12. Une fois que l’abonnement a terminé le processus de création, vous devez voir la réussite. Cliquez sur **Fermer**. 
    ![Configurer une redondance géographique](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql36.png) </br>
  
## <a name="verify-the-process-of-initialization-and-replication"></a>Vérifiez que le processus d’initialisation et de réplication  
  
1.  Sur le serveur SQL principal, avec le bouton droit\-cliquez sur le **réplication** nœud et cliquez sur **lancer le moniteur de réplication**.  
  
2.  Dans **moniteur de réplication**, cliquez sur la publication.  
  
3.  Sur le **tous les abonnements** , avec le bouton droit sur et **afficher les détails**.  
  
    Vous devez être en mesure de voir le nombre d’entrées sous **Actions** pour la réplication initiale.  
  
4.  En outre, vous pouvez consulter sous le **Agent SQL Server\\travaux** nœud pour afficher le travail\(s\) planifiée pour s’exécuter les opérations de la publication\/abonnement.  Uniquement les travaux locaux sont affichés, veillez donc à vérifier sur le serveur de publication et sur l’abonné pour le dépannage.  Droite\-cliquez sur une tâche et sélectionnez **afficher l’historique** pour afficher l’historique de l’exécution et les résultats.  
  
## <a name="sqlagent"></a>Configurer la connexion SQL pour le compte de domaine CONTOSO\\sqlagent  
  
1.  Créez une nouvelle connexion sur le serveur principal et un réplica SQL Server nommée CONTOSO\\sqlagent \(le nom du nouvel utilisateur de domaine créé et configuré sur le **sécurité de l’Agent** page dans les procédures ci-dessus.\)  
  
2.  Dans SQL Server, avec le bouton droit\-cliquez sur la connexion que vous avez créé et sélectionnez Propriétés, puis sous la **mappage de l’utilisateur** mapper cette connexion à, onglet **AdfsConfiguration** et **AdfsArtifact**  bases de données avec le public et db\_genevaservice rôles. Également mapper cette connexion à la base de données de distribution et ajouter la base de données\_rôle de propriétaire pour les tables de distribution et adfsconfiguration.  Pour cela le cas échéant sur le serveur principal et le réplica SQL server. Pour plus d’informations, voir [Replication Agent Security Model](https://technet.microsoft.com/library/ms151868.aspx).  
  
3.  Donnez à la lecture de compte de domaine correspondant et autorisations en écriture sur le partage configuré en tant que serveur de distribution.  Assurez-vous que vous définissez en lecture et en écrivez à la fois sur les autorisations de partage et les autorisations de fichiers local.  
  
## <a name="configure-ad-fs-nodes-to-point-to-the-sql-server-replica-farm"></a>Configurer le nœud AD FS\(s\) pour pointer vers la batterie de serveurs de réplication SQL Server  
Maintenant que vous avez configuré la géo-redondance, les nœuds de batterie de serveurs AD FS peuvent être configurés pour pointer vers votre batterie de serveurs réplica SQL Server, à l’aide du standard AD FS « fonctions de jointure » batterie de serveurs, à partir de l’interface utilisateur des Assistant Configuration de AD FS ou à l’aide de Windows PowerShell.  
  
Si vous utilisez l’interface utilisateur de AD FS Configuration Assistant, sélectionnez **ajouter un serveur de fédération à une batterie de serveurs de fédération**. **Ne le faites pas** sélectionnez **créer le premier serveur de fédération dans une batterie de serveurs de fédération**.  
  
Si vous utilisez Windows PowerShell, exécutez **ajouter\-AdfsFarmNode**. **Ne le faites pas** exécuter **installer\-AdfsFarm**.  
  
Lorsque vous y êtes invité, fournissez le nom d’hôte et une instance du réplica SQL Server, **pas** initiale SQL server.  
