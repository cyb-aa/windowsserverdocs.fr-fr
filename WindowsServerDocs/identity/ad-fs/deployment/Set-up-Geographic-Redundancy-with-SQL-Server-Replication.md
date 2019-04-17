---
title: "Le programme d’installation de redondance géographique avec la réplication SQLServer"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: active-directory-federation-services
ms.author: billmath
ms.assetId: 7b9f9a4f-888c-4358-bacd-3237661b1935
ms.openlocfilehash: a9f29c1eb19a8241eac5afb5c87581e6c988c3c8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="setup-geographic-redundancy-with-sql-server-replication"></a>Le programme d’installation de redondance géographique avec la réplication SQLServer

>S’applique à: Windows Server2016, Windows Server2012R2

> [!IMPORTANT]  
> Si vous souhaitez créer une batterie ADFS et utiliser SQLServer pour stocker vos données de configuration, vous pouvez utiliser SQLServer2008 ou version supérieure.
  
Si vous utilisez SQLServer en tant que votre base de données de configuration ADFS, vous pouvez configurer la redondance du geo\ pour votre batterie ADFS à l’aide de la réplication SQLServer. Redondance Geo\ réplique les données entre deux sites géographiquement distants afin que les applications peuvent passer d’un site vers un autre. De cette façon, en cas de défaillance d’un site, vous pouvez encore avoir toutes les données de configuration disponibles sur le deuxième site. Pour plus d’informations, consultez la section «une redondance géographique du SQLServer» dans [batterie de serveur de fédération à l’aide de SQLServer](../design/Federation-Server-Farm-Using-SQL-Server.md).  
  
## <a name="prerequisites"></a>Conditions préalables  
Installer et configurer une batterie de serveurs SQL. Pour plus d’informations, voir [https://technet.microsoft.com/evalcenter/hh225126.aspx](https://technet.microsoft.com/evalcenter/hh225126.aspx). Sur le serveur SQL initiale, assurez-vous que le service de l’Agent SQLServer est en cours d’exécution et défini sur démarrage automatique.  
  
## <a name="create-the-second-replica-sql-server-for-geo-redundancy"></a>Créer le deuxième \(replica\) SQLServer pour la redondance du geo\  
  
1.  Installer SQLServer \ (pour plus d’informations, voir [https://technet.microsoft.com/evalcenter/hh225126.aspx](https://technet.microsoft.com/evalcenter/hh225126.aspx). Copiez les fichiers de script CreateDB.sql et SetPermissions.sql résultants vers le serveur SQL de réplica.  
  
2.  Service de l’Agent SQLServer est en cours d’exécution et défini sur démarrage automatique  
  
3.  Exécutez **Export-AdfsDeploymentSQLScript** sur le nœud principal ADFS pour créer des fichiers CreateDB.sql et SetPermissions.sql.  Par exemple:`PS:\>Export-AdfsDeploymentSQLScript -DestinationFolder . –ServiceAccountName CONTOSO\gmsa1$`.  
![Configurer une redondance géographique](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql2.png)
  
4.  Copiez les scripts sur votre serveur secondaire.  Ouvrez le script CreateDB.sql dans **SQL Management Studio** et cliquez sur **Execute**.
![Configurer une redondance géographique](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql4.png)

5. Ouvrez le script SetPermissions.sql dans **SQL Management Studio** et cliquez sur **Execute**.
![Configurer une redondance géographique](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql6.png) 

   

>[!NOTE]
>Vous pouvez également utiliser les éléments suivants à partir de la ligne de commande. 
>
>    `c:\>sqlcmd –i CreateDB.sql`  
>  
>    `c:\>sqlcmd –i SetPermissions.sql` 
> 
## <a name="create-publisher-settings-on-the-initial-sql-server"></a>Créer des paramètres de serveur de publication sur le serveur SQL initiale  
  
1.  À partir de SQLServer Management studio, sous **réplication**, cliquez avec le bouton droit **Publications locales** et choisissez **nouvelle Publication... **<ph x="4">
![</ph> Configurer une redondance géographique](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql7.png) </br>  

2.  Dans l’écran de l’Assistant Nouvelle Publication cliquez sur **suivant**.</br>
![Configurer une redondance géographique](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql8.png) </br> 
  
3.  Sur **distributeur** page, choisissez le serveur local en tant que serveur de distribution, cliquez sur **suivant**.  
![Configurer une redondance géographique](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql9.png) </br>   

4.  Sur le **instantané** dossier page, entrez \\\SQL1\repldata à la place de dossier par défaut. \ (Remarque: vous devrez peut-être créer cette yourself\ partage).  
![Configurer une redondance géographique](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql10.png) </br>   
  
5.  Choisissez **AdfsConfigurationV3** en tant que la base de données de publication et cliquez sur **suivant**.  
![Configurer une redondance géographique](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql11.png) </br>
  
6.  Sur **Type de composition**, choisissez **publication de fusion** et cliquez sur **suivant**.  
![Configurer une redondance géographique](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql12.png) </br>
  
7.  Sur **abonné Types**, choisissez **SQLServer2008 ou version ultérieure** et cliquez sur **suivant**.  
 ![Configurer une redondance géographique](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql13.png) </br> 

8.  Sur le **Articles** page Sélectionnez **tableaux** nœud pour sélectionner toutes les tables, puis **offrant-vérification SyncProperties** table \ (celle-ci ne doit pas être replicated\)</br>
![Configurer une redondance géographique](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql14.png) </br>    
  
9.  Sur le **Articles** page, sélectionnez **fonctions de définies par l’utilisateur** nœud pour sélectionner toutes les fonctions de définies par l’utilisateur et cliquez sur **suivant**...  
![Configurer une redondance géographique](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql15.png) </br>    

10. Sur le **Article questions** page, cliquez sur **suivant**.  
![Configurer une redondance géographique](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql16.png) </br>   

11. Sur le **filtrer les lignes de tableau**, cliquez sur **suivant**.  
![Configurer une redondance géographique](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql17.png) </br>   
12. Sur le **Agent de capture instantanée** page, choisissez les paramètres par défaut de l’exécution et de 14jours, cliquez sur **suivant**.  
![Configurer une redondance géographique](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql18.png) </br>   
Vous devrez créer un compte de domaine de l’agent SQL. Utilisez les étapes décrites dans [connexion SQL de configuration pour le compte de domaine CONTOSO\\sqlagent](Set-up-Geographic-Redundancy-with-SQL-Server-Replication.md#sqlagent) pour créer une connexion SQL pour ce nouvel utilisateur AD et affecter des autorisations spécifiques.  
  
13. Sur le **sécurité de l’Agent**, cliquez sur **paramètres de sécurité** et entrez l’username\/mot de passe d’un compte de domaine \ (pas un GMSA\) créé pour l’agent SQL et cliquez sur **OK**.  Cliquez sur **suivant**.  
![Configurer une redondance géographique](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql19.png) </br>  

14. Sur le **Actions de l’Assistant**, cliquez sur **suivant**.   
![Configurer une redondance géographique](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql20.png) </br>

15. Sur le **terminez l’Assistant** page, entrez un nom pour votre composition, cliquez sur **Terminer**. 
![Configurer une redondance géographique](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql21.png) </br>  

16. Une fois la publication créée, vous devez voir l’état de réussite.  Cliquez sur **fermer **.
![Configurer une redondance géographique](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql22.png) </br>  

17. Précédent dans SQLServer Management Studio, la nouvelle Publication avec le bouton droit et cliquez sur **lancer le moniteur de réplication**.  
![Configurer une redondance géographique](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql23.png) </br> 
  
## <a name="create-subscription-settings-on-the-replica-sql-server"></a>Créer des paramètres d’abonnement sur le réplica SQLServer  
Assurez-vous que vous avez créé les paramètres de l’éditeur sur le serveur SQL initiale comme décrit ci-dessus, puis procédez comme suit:  
  
1.  Sur le réplica SQLServer, à partir de SQLServer Management studio, sous **réplication**, cliquez avec le bouton droit **abonnements locaux** et choisissez **nouvel abonnement... **. ![Configurer une redondance géographique](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql24.png) </br>  

2.  Sur le **Assistant Nouvel abonnement**, cliquez sur **suivant**.
![Configurer une redondance géographique](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql25.png) </br>   
  
3.  Sur le **Publication**, sélectionnez l’éditeur dans la liste déroulante.  Développez **AdfsConfigurationV3** et sélectionnez le nom de la publication créé ci-dessus, cliquez sur **suivant**.  
![Configurer une redondance géographique](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql26.png) </br> 
  
4.  Sur le **emplacement de l’Agent de fusion** page, sélectionnez **exécuter chaque agent sur son abonné \(pull subscriptions\)** \(the default\) et cliquez sur **suivant**.  
![Configurer une redondance géographique](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql27.png) </br> Ceci, ainsi que du Type d’abonnement ci-dessous, détermine la logique de résolution de conflit. \ (Pour plus d’informations, voir [détecter et résoudre les conflits de réplication de fusion](https://technet.microsoft.com/library/ms151191.aspx). </br>
 
5.  Sur le **abonnés** page, sélectionnez **AdfsConfigurationV3** en tant que la base de données de l’abonné et cliquez sur **suivant**.  
![Configurer une redondance géographique](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql28.png) </br> 
  
6.  Sur le **sécurité de l’Agent de fusion**, cliquez sur **... **et entrez le nom d’utilisateur et le mot de passe d’un compte de domaine \ (pas un GMSA\) est créé pour l’agent SQL à l’aide de la zone de points de suspension et cliquez sur **suivant**.
![Configurer une redondance géographique](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql30.png) </br> 
  
7.  Sur **calendrier de synchronisation**, choisissez **exécuter en permanence** et cliquez sur **suivant**. 
![Configurer une redondance géographique](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql31.png) </br> 
 
8.  Sur **initialiser les abonnements**, cliquez sur **suivant**.  
![Configurer une redondance géographique](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql32.png) </br> 
  
9.  Sur **Type d’abonnement**, choisissez **Client** et cliquez sur **suivant**.  
  
    Conséquences sont documentés [ici](https://technet.microsoft.com/library/ms151191.aspx) et [ici](https://technet.microsoft.com/library/ms151170.aspx).  L’essentiel, nous prenons la résolution de conflit «première à wins publisher» simple et nous n’avez pas besoin de republier vers d’autres abonnés.  
![Configurer une redondance géographique](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql33.png) </br>
   
10. Sur le **Actions de l’Assistant**, vérifiez **créer l’abonnement** est activée, puis cliquez sur **suivant**. 
![Configurer une redondance géographique](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql34.png) </br>

11. Sur le **terminez l’Assistant**, cliquez sur **Terminer**. 
![Configurer une redondance géographique](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql35.png) </br>

12. Une fois que l’abonnement a terminé le processus de création, vous devez voir réussite. Cliquez sur **fermer **. 
![Configurer une redondance géographique](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql36.png) </br>
  
## <a name="verify-the-process-of-initialization-and-replication"></a>Vérifiez que le processus d’initialisation et de la réplication  
  
1.  Sur le serveur SQL principal, clic droit le **réplication** nœud et cliquez sur **lancer le moniteur de réplication**.  
  
2.  Dans **moniteur de réplication**, cliquez sur la publication.  
  
3.  Sur le **tous les abonnements** onglet, cliquez avec le bouton droit et **afficher les détails**.  
  
    Vous devez être en mesure de voir le nombre d’entrées sous **Actions** pour la réplication initiale.  
  
4.  En outre, vous pouvez rechercher sous le **SQLServer Agent\\Jobs** nœud pour voir les job\(s\) planifiée pour exécuter les opérations de l’abonnement à publication\ /.  Uniquement les travaux sont affichés, veillez donc à vérifier l’éditeur et l’abonné de résolution des problèmes.  Une tâche de clic droit et sélectionnez **afficher l’historique des** pour afficher l’historique de l’exécution et les résultats.  
  
## <a name="sqlagent"></a>Configurer la connexion SQL pour le compte de domaine CONTOSO\\sqlagent  
  
1.  Créez une connexion sur le serveur principal et le réplica SQLServer appelée CONTOSO\\sqlagent \ (le nom du nouvel utilisateur de domaine créé et configuré sur le **sécurité de l’Agent** page dans les procédures ci-dessus. \)  
  
2.  Dans SQLServer, la connexion vous créé et sélectionnez Propriétés, puis sous clic droit le **mappage de l’utilisateur** onglet, mapper ce journal dans **AdfsConfiguration** et **AdfsArtifact** bases de données avec les rôles publics et db\_genevaservice. Également mapper ce fichier journal dans à base de données de distribution et ajouter le rôle db\_owner pour la distribution et des tables adfsconfiguration.  Cela comme applicable sur le serveur principal et le réplica SQL server. Pour plus d’informations, voir [modèle de sécurité de l’Agent de réplication](https://technet.microsoft.com/library/ms151868.aspx).  
  
3.  Donner la lecture de compte de domaine correspondant et des autorisations en écriture sur le partage configuré en tant que distributeur.  Assurez-vous que vous définissez en lecture et écrivez à la fois sur les autorisations de partage et les autorisations de fichier local.  
  
## <a name="configure-ad-fs-nodes-to-point-to-the-sql-server-replica-farm"></a>Configurer node\(s\) ADFS pour pointer vers la batterie de serveurs de réplication SQLServer  
Maintenant que vous avez configuré la géo-redondance, les nœuds de batterie de serveurs ADFS peuvent être configurés pour pointer vers votre batterie de serveurs réplica SQLServer, à l’aide standard ADFS «fonctions de jointure» batterie de serveurs, à partir de l’interface utilisateur ADFS Configuration Assistant ou à l’aide de Windows PowerShell.  
  
Si vous utilisez l’interface utilisateur ADFS Configuration Assistant, sélectionnez **ajouter un serveur de fédération à une batterie de serveurs de fédération**. **Ne le faites pas** sélectionnez **créer le premier serveur de fédération dans une batterie de serveurs de fédération**.  
  
Si vous utilisez Windows PowerShell, exécutez **Add-AdfsFarmNode**. **Ne le faites pas** exécuter **Install\-AdfsFarm**.  
  
Lorsque vous y êtes invité, fournissez le nom d’hôte et de l’instance du réplica SQLServer, **pas** initiale SQL server.  
