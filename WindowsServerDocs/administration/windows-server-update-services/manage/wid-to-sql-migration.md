---
title: Migration de la base de données WSUS de l’WID (base de données interne Windows) vers SQL
description: 'Rubrique Windows Server Update Service (WSUS) : procédure de migration de la base de données WSUS (SUSDB) d’une instance de base de données interne Windows vers une instance locale ou distante de SQL Server.'
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: get-started article
ms.assetid: 90e3464c-49d8-4861-96db-ee6f8a09g7dr
author: coreyp-at-msft
ms.author: coreyp
manager: dougkim
ms.date: 07/25/2018
ms.openlocfilehash: 0977aa1fd9a6848bd7b85bb592b6a82556277e72
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361580"
---
>S’applique à : Windows Server 2012, Windows Server 2012 R2, Windows Server 2016

# <a name="migrating-the-wsus-database-from-wid-to-sql"></a>Migration de la base de données WSUS de WID vers SQL

Procédez comme suit pour migrer la base de données WSUS (SUSDB) d’une instance de base de données interne Windows vers une instance locale ou distante de SQL Server.

## <a name="prerequisites"></a>Prérequis

- Instance SQL. Il peut s’agir de **MSSQLSERVER** par défaut ou d’une instance personnalisée.
- SQL Server Management Studio
- WSUS avec rôle WID installé
- IIS (cela est normalement inclus lorsque vous installez WSUS via Gestionnaire de serveur). Il n’est pas déjà installé, il doit être.

## <a name="migrating-the-wsus-database"></a>Migration de la base de données WSUS

### <a name="stop-the-iis-and-wsus-services-on-the-wsus-server"></a>Arrêter les services IIS et WSUS sur le serveur WSUS

À partir de PowerShell (avec élévation de privilèges), exécutez :

```powershell
    Stop-Service IISADMIN
    Stop-Service WsusService
```

### <a name="detach-susdb-from-the-windows-internal-database"></a>Détacher SUSDB de la base de données interne Windows

#### <a name="using-sql-management-studio"></a>Utilisation de SQL Management Studio

1. Cliquez avec le bouton droit sur **SUSDB** - @ no__t-2 **tâches** - @ no__t-5 Cliquez sur **détacher**: ![image1 @ no__t-8
2. Cochez la case **Supprimer les connexions existantes** , puis cliquez sur **OK** (facultatif, si des connexions actives existent).
    ![image2 @ no__t-1

#### <a name="using-command-prompt"></a>Utilisation de l’invite de commandes

> [!IMPORTANT]
> Ces étapes montrent comment détacher la base de données WSUS (SUSDB) de l’instance de base de données interne Windows à l’aide de l’utilitaire **sqlcmd** . Pour plus d’informations sur l’utilitaire **sqlcmd** , consultez l' [utilitaire sqlcmd](https://go.microsoft.com/fwlink/?LinkId=81183).
> 1. Ouvrir une invite de commandes avec élévation de privilèges
> 2. Exécutez la commande SQL suivante pour détacher la base de données WSUS (SUSDB) de l’instance de base de données interne Windows à l’aide de l’utilitaire **sqlcmd** :

```batchfile
        sqlcmd -S \\.\pipe\Microsoft##WID\tsql\query
        use master
        GO
        alter database SUSDB set single_user with rollback immediate
        GO
        sp_detach_db SUSDB
        GO
```

### <a name="copy-the-susdb-files-to-the-sql-server"></a>Copiez les fichiers SUSDB dans le SQL Server

1. Copiez **SUSDB. mdf** et **SUSDB\_log.ldf** à partir du dossier de données WID ( **% systemdrive%** \* * Windows\WID\Data * *) vers le dossier de données de l’instance SQL.

> [!TIP]
> Par exemple, si votre dossier d’instance SQL est **C:\Program Files\Microsoft SQL Server\MSSQL12. MSSQLSERVER\MSSQL**et le dossier de données wid est **C:\Windows\WID\Data,** copiez les fichiers SUSDB à partir de **C:\WINDOWS\WID\DATA** dans **C:\Program Files\Microsoft SQL Server\MSSQL12. MSSQLSERVER\MSSQL\Data**

### <a name="attach-susdb-to-the-sql-instance"></a>Attacher SUSDB à l’instance SQL

1. Dans **SQL Server Management Studio**, sous le nœud **instance** , cliquez avec le bouton droit sur **bases de données**, puis cliquez sur **attacher**.
    ![image3](images/image3.png)
2. Dans la **zone attacher des bases de** données, sous **bases de données à attacher**, cliquez sur le bouton **Ajouter** et recherchez le fichier **SUSDB. mdf** (copié à partir du dossier WID), puis cliquez sur **OK**.
    ![image4 @ no__t-1 ![image5 @ no__t-3

> [!TIP]
> Cela peut également être effectué à l’aide de Transact-SQL.  Pour obtenir des instructions, consultez la [documentation SQL relative à l’attachement d’une base de données](https://docs.microsoft.com/sql/relational-databases/databases/attach-a-database) .
>
> Exemple (à l’aide des chemins d’accès de l’exemple précédent) :
> ```sql
>    USE master;
>    GO
>    CREATE DATABASE SUSDB
>    ON
>        (FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL12.MSSQLSERVER\MSSQL\Data\SUSDB.mdf'),
>        (FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL12.MSSQLSERVER\MSSQL\Log\SUSDB_Log.ldf')
>        FOR ATTACH;
>    GO
>```

### <a name="verify-sql-server-and-database-logins-and-permissions"></a>Vérifier les connexions et les autorisations de SQL Server et de base de données

#### <a name="sql-server-login-permissions"></a>Autorisations de connexion SQL Server

Après avoir attaché le SUSDB, vérifiez que **NT AUTHORITY\NETWORK SERVICE** dispose des autorisations de connexion à l’instance de SQL Server en procédant comme suit :

1. Accédez à SQL Server Management Studio
2. Ouverture de l’instance
3. Cliquer sur **sécurité**
4. Cliquer sur **connexions**

Le compte **NT AUTHORITY\NETWORK SERVICE** doit figurer dans la liste. Si ce n’est pas le cas, vous devez l’ajouter en ajoutant un nouveau nom de connexion.

> [!IMPORTANT]
> Si l’instance SQL se trouve sur un autre ordinateur que WSUS, le compte d’ordinateur du serveur WSUS doit être indiqué au format **[FQDN] \\ [WSUSComputerName] $** .  Si ce n’est pas le cas, vous pouvez utiliser les étapes ci-dessous, en remplaçant **NT AUTHORITY\NETWORK SERVICE** par le compte d’ordinateur du serveur WSUS ( **[FQDN] \\ [WSUSComputerName] $** ), ***en plus d'*** accorder des droits à l' **autorité NT \ SERVICE réseau**

##### <a name="adding-nt-authoritynetwork-service-and-granting-it-rights"></a>Ajout de NT AUTHORITY\NETWORK SERVICE et octroi d’autorisations

1. Cliquez avec le bouton droit sur **connexions** , puis cliquez sur **nouvelle connexion...**
    ![image6 @ no__t-1
2. Sur la page **général** , renseignez le **nom de connexion** (**NT AUTHORITY\NETWORK SERVICE**) et définissez la **base de données par défaut** sur SUSDB.
    ![image7 @ no__t-1
3. Dans la page **rôles du serveur** , assurez-vous que **public** et **sysadmin** sont sélectionnés.
    ![image8 @ no__t-1
4. Sur la page mappage de l' **utilisateur** :
    - Sous **utilisateurs mappés à cette connexion**: sélectionnez **SUSDB**
    - Sous @no__t-appartenance au rôle 0Database pour : SUSDB @ no__t-0, vérifiez que les éléments suivants sont vérifiés :
        - **publique**
        - **WebService** ![image9 @ no__t-2
5. Cliquez sur **OK**.

Vous devez maintenant voir **NT AUTHORITY\NETWORK SERVICE** sous connexions.
![image10 @ no__t-1

#### <a name="database-permissions"></a>Autorisations de base de données

1. Cliquez avec le bouton droit sur le SUSDB
2. Sélectionner les **Propriétés**
3. Cliquez sur **autorisations**

Le compte **NT AUTHORITY\NETWORK SERVICE** doit figurer dans la liste.

1. Si ce n’est pas le cas, ajoutez le compte.
2. Dans la zone de texte Login Name (nom de connexion), entrez l’ordinateur WSUS au format suivant :
    > [**FQDN] \\ [WSUSComputerName] $**
3. Vérifiez que la **base de données par défaut** est définie sur **SUSDB**.

    > [!TIP]
    > Dans l’exemple suivant, le nom de domaine complet est **Contosto.com** et le nom de l’ordinateur WSUS est **WsusMachine**:
    >
    > ![image11](images/image11.png)

4. Dans la page **mappage** de l’utilisateur, sélectionnez la base de données **SUSDB** sous **« utilisateurs mappés à cette connexion »** .
5. Vérifiez **WebService** sous l’appartenance du rôle de base de données **» pour : SUSDB "**  : ![image12 @ no__t-2
6. Cliquez sur **OK** pour enregistrer les paramètres.
    > [!NOTE]
    > Vous devrez peut-être redémarrer le service SQL pour que les modifications prennent effet.

### <a name="edit-the-registry-to-point-wsus-to-the-sql-server-instance"></a>Modifiez le registre de façon à faire pointer WSUS vers l’instance SQL Server

> [!IMPORTANT]
> Suivez attentivement les étapes décrites dans cette section. De graves problèmes peuvent se produire si vous modifiez le Registre de façon incorrecte. Avant de le modifier, [sauvegardez le Registre afin de pouvoir le restaurer](https://support.microsoft.com/en-us/help/322756) en cas de problème.

1. Cliquez sur **Démarrer**, puis sur **Exécuter**. Tapez **regedit**, puis cliquez sur **OK**.
2. Recherchez la clé suivante : **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\UpdateServices\Server\Setup\SqlServerName**
3. Dans la zone de texte **valeur** , tapez **[servername] \\ [nom_instance]** , puis cliquez sur **OK**. Si le nom de l’instance est l’instance par défaut, tapez **[servername]** .
4. Recherchez la clé suivante : **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Update rôle Services\Server\Setup\Installed Services\UpdateServices-WidDatabase** ![image13 @ no__t-2
5. Renommez la clé en **updateservices-Database** ![image41 @ no__t-2

    > [!NOTE]
    > Si vous ne mettez pas à jour cette clé, **WsusUtil** tentera de traiter le wid plutôt que l’instance SQL vers laquelle vous avez effectué la migration.

### <a name="start-the-iis-and-wsus-services-on-the-wsus-server"></a>Démarrer les services IIS et WSUS sur le serveur WSUS

À partir de PowerShell (avec élévation de privilèges), exécutez :

```powershell
    Start-Service IISADMIN
    Start-Service WsusService
```

> [!NOTE]
> Si vous utilisez la console WSUS, fermez et redémarrez-la.

## <a name="uninstalling-the-wid-role-not-recommended"></a>Désinstallation du rôle WID (non recommandé)

> [!WARNING]
> La suppression du rôle WID entraîne également la suppression d’un dossier de base de données ( **%systemdrive%\Program Files\Update Services\Database**) qui contient les scripts requis par WSUSutil. exe pour les tâches consécutives à l’installation. Si vous choisissez de désinstaller le rôle WID, veillez à sauvegarder le dossier **%systemdrive%\Program Files\Update Services\Database** au préalable.

À l’aide de PowerShell :

```powershell
Uninstall-WindowsFeature -Name 'Windows-Internal-Database'
```

Une fois le rôle WID supprimé, vérifiez que la clé de Registre suivante est présente : **Rôle HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Update Services\Server\Setup\Installed Services\UpdateServices-Database**