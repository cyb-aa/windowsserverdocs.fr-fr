---
title: Migration de la base de données WSUS (base de données Windows interne) WID to SQL
description: Rubrique de Windows Server Update Service (WSUS) - la migration de la base de données WSUS (SUSDB) à partir d’une instance de base de données interne Windows vers une instance locale ou distante de SQL Server.
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ed6f695947fc17d2e96b5282b3a67a221bb0140d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858030"
---
>S’applique à : Windows Server 2012, Windows Server 2012 R2, Windows Server 2016

# <a name="migrating-the-wsus-database-from-wid-to-sql"></a>Migration de la base de données WSUS de WID vers SQL

Procédez comme suit pour migrer la base de données WSUS (SUSDB) à partir d’une instance de base de données interne Windows vers une instance locale ou distante de SQL Server.

## <a name="prerequisites"></a>Prérequis

- Instance SQL. Cela peut être la valeur par défaut **MSSQLServer** ou une Instance personnalisée.
- SQL Server Management Studio
- WSUS avec rôle WID installé
- IIS (c’est normalement inclus lorsque vous installez WSUS par le biais du Gestionnaire de serveur). Il n’est pas déjà installé, il devra être.

## <a name="migrating-the-wsus-database"></a>Migration de la base de données WSUS

### <a name="stop-the-iis-and-wsus-services-on-the-wsus-server"></a>Arrêtez les services IIS et WSUS sur le serveur WSUS

À partir de PowerShell (avec élévation de privilèges), exécutez :

```powershell
    Stop-Service IISADMIN
    Stop-Service WsusService
```

### <a name="detach-susdb-from-the-windows-internal-database"></a>Détacher SUSDB à partir de la base de données interne Windows

#### <a name="using-sql-management-studio"></a>À l’aide de SQL Management Studio

1. Avec le bouton droit **SUSDB** - &gt; **tâches** - &gt; cliquez sur **détachement**: ![image1](images/image1.png)
2. Vérifiez **supprimer les connexions existantes** et cliquez sur **OK** (facultatif, si les connexions sont actives).
    ![image2](images/image2.png)

#### <a name="using-command-prompt"></a>Utilisation de l’invite de commandes

> [!IMPORTANT]
> Ces étapes montrent comment détacher la base de données WSUS (SUSDB) à partir de l’instance de base de données interne Windows à l’aide de la **sqlcmd** utilitaire. Pour plus d’informations sur la **sqlcmd** utilitaire, consultez [utilitaire sqlcmd](https://go.microsoft.com/fwlink/?LinkId=81183).
1. Ouvrez une invite de commandes avec élévation de privilèges
2. Exécutez la commande SQL suivante pour détacher la base de données WSUS (SUSDB) à partir de l’instance de base de données interne Windows à l’aide de la **sqlcmd** utilitaire :

```batchfile
        sqlcmd -S \\.\pipe\Microsoft##WID\tsql\query
        use master
        GO
        alter database SUSDB set single_user with rollback immediate
        GO
        sp_detach_db SUSDB
        GO
```

### <a name="copy-the-susdb-files-to-the-sql-server"></a>Copiez les fichiers SUSDB à SQL Server

1. Copie **SUSDB.mdf** et **SUSDB\_log.ldf** à partir du dossier de données WID (**% lecteur_système %**\** Windows\WID\Data **) pour les données d’Instance SQL Dossier.

> [!TIP]
> Par exemple, si votre dossier de l’Instance SQL est **C:\Program Files\Microsoft SQL Server\MSSQL12. MSSQLSERVER\MSSQL**, et le dossier de données de WID **C:\Windows\WID\Data,** copiez les fichiers SUSDB du **C:\Windows\WID\Data** à **C:\Program Files\Microsoft SQL Server \MSSQL12. MSSQLSERVER\MSSQL\Data**

### <a name="attach-susdb-to-the-sql-instance"></a>Attacher SUSDB à l’Instance SQL

1. Dans **SQL Server Management Studio**, sous le **Instance** nœud, avec le bouton droit **bases de données**, puis cliquez sur **attacher**.
    ![image3](images/image3.png)
2. Dans le **attacher les bases de données** boîte de **bases de données à attacher**, cliquez sur le **ajouter** bouton et recherchez le **SUSDB.mdf** fichier (copiés à partir de la Dossier WID), puis cliquez sur **OK**.
    ![image4](images/image4.png) ![image5](images/image5.png)

> [!TIP]
> Cela peut également être effectuée à l’aide de Transact-Sql.  Veuillez consulter la [documentation SQL permettant d’attacher une base de données](https://docs.microsoft.com/sql/relational-databases/databases/attach-a-database) pour ses instructions.
>
> Exemple (à l’aide de chemins d’accès à partir de l’exemple précédent) :
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

### <a name="verify-sql-server-and-database-logins-and-permissions"></a>Vérifiez que SQL Server et les connexions de base de données et les autorisations

#### <a name="sql-server-login-permissions"></a>Autorisations de connexion SQL Server

Après avoir attaché le SUSDB, vérifiez que **NT AUTHORITY\NETWORK SERVICE** a des autorisations de connexion à l’instance de SQL Server en procédant comme suit :

1. Accédez à SQL Server Management Studio
2. Ouverture de l’Instance
3. Cliquez sur **sécurité**
4. Cliquez sur **connexions**

Le **NT AUTHORITY\NETWORK SERVICE** compte doit être répertorié. Si elle n’est pas le cas, vous devez l’ajouter en ajoutant le nouveau nom de connexion.

> [!IMPORTANT]
> Si l’Instance SQL se trouve sur un autre ordinateur à partir de WSUS, le compte d’ordinateur du serveur WSUS doit être répertorié dans le format **[FQDN]\\[WSUSComputerName] $**.  Si non, les étapes ci-dessous peuvent être utilisés pour l’ajouter, en remplaçant **NT AUTHORITY\NETWORK SERVICE** avec un compte d’ordinateur du serveur WSUS (**[FQDN]\\[WSUSComputerName] $**) il s’agirait ***outre*** l’octroi des droits à **NT AUTHORITY\NETWORK SERVICE**

##### <a name="adding-nt-authoritynetwork-service-and-granting-it-rights"></a>Ajout de NT AUTHORITY\NETWORK SERVICE et lui accorder des droits

1. Bouton droit sur **connexions** et cliquez sur **nouvelle connexion...**
    ![image6](images/image6.png)
2. Sur le **général** page, renseignez le **nom de connexion** (**NT AUTHORITY\NETWORK SERVICE**) et définissez le **base de données par défaut** à SUSDB.
    ![image7](images/image7.png)
3. Sur le **rôles serveur** , vérifiez **public** et **sysadmin** sont sélectionnés.
    ![image8](images/image8.png)
4. Sur le **mappage de l’utilisateur** page :
    - Sous **utilisateurs mappés à cette connexion**: sélectionnez **SUSDB**
    - Sous **appartenance au rôle de la base de données : SUSDB**, vérifiez les éléments suivants sont activés :
        - **public**
        - **webService** ![image9](images/image9.png)
5. Cliquez sur **OK**.

Vous devriez maintenant voir **NT AUTHORITY\NETWORK SERVICE** sous connexions.
![image10](images/image10.png)

#### <a name="database-permissions"></a>Autorisations de base de données

1. Avec le bouton droit de la SUSDB
2. Sélectionnez **propriétés**
3. Cliquez sur **autorisations**

Le **NT AUTHORITY\NETWORK SERVICE** compte doit être répertorié.

1. Si elle n’est pas le cas, ajoutez le compte.
2. Dans la zone de texte Nom de connexion, entrez l’ordinateur WSUS dans le format suivant :
    > [**FQDN]\\[WSUSComputerName] $**
3. Vérifiez que le **base de données par défaut** a la valeur **SUSDB**.

    > [!TIP]
    > Dans l’exemple suivant, le nom de domaine complet est **Contosto.com** et le nom d’ordinateur WSUS est **WsusMachine**:
    >
    > ![image11](images/image11.png)

4. Sur le **mappage de l’utilisateur** page, sélectionnez le **SUSDB** de base de données sous **« Utilisateurs mappés à cette connexion. »**
5. Vérifiez **webservice** sous le **« appartenance au rôle de la base de données : SUSDB »**: ![image12](images/image12.png)
6. Cliquez sur **OK** pour enregistrer les paramètres.
    > [!NOTE]
    > Vous devrez peut-être redémarrer le Service SQL pour que les modifications prennent effet.

### <a name="edit-the-registry-to-point-wsus-to-the-sql-server-instance"></a>Modifier le Registre au serveur WSUS de point pour l’Instance SQL Server

> [!IMPORTANT]
> Suivez attentivement les étapes décrites dans cette section. De graves problèmes peuvent se produire si vous modifiez le Registre de façon incorrecte. Avant de le modifier, [sauvegarder le Registre pour la restauration](https://support.microsoft.com/en-us/help/322756) en cas de problèmes.

1. Cliquez sur **Démarrer**, puis sur **Exécuter**. Tapez **regedit**, puis cliquez sur **OK**.
2. Recherchez la clé suivante : **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\UpdateServices\Server\Setup\SqlServerName**
3. Dans le **valeur** zone de texte, tapez **[nomserveur]\\[NOM_INSTANCE]**, puis cliquez sur **OK**. Si le nom d’instance est l’instance par défaut, tapez **[nomserveur]**.
4. Recherchez la clé suivante : **Rôle de Services\Server\Setup\Installed HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Update Services\UpdateServices-WidDatabase** ![image13](images/image13.png)
5. Renommer la clé à **UpdateServices-base de données** ![image41](images/image14.png)

    > [!NOTE]
    > Si vous ne modifiez pas cette clé, puis **WsusUtil** va tenter de traiter le WID plutôt que l’Instance SQL à laquelle vous avez migré.

### <a name="start-the-iis-and-wsus-services-on-the-wsus-server"></a>Démarrez les services IIS et WSUS sur le serveur WSUS

À partir de PowerShell (avec élévation de privilèges), exécutez :

```powershell
    Start-Service IISADMIN
    Start-Service WsusService
```

> [!NOTE]
> Si vous utilisez la Console WSUS, fermez et redémarrez-le.

## <a name="uninstalling-the-wid-role-not-recommended"></a>Désinstallation du rôle WID (non recommandé)

> [!WARNING]
> Suppression du rôle WID supprime également un dossier de base de données (**%SystemDrive%\Program Files\Update Services\Database**) qui contient les scripts requis par WSUSUtil.exe pour les tâches de post-installation. Si vous choisissez de désinstaller le rôle WID, assurez-vous que vous sauvegardez la **%SystemDrive%\Program Files\Update Services\Database** dossier au préalable.

À l’aide de PowerShell :

```powershell
Uninstall-WindowsFeature -Name 'Windows-Internal-Database'
```

Une fois le rôle de travail est supprimé, vérifiez que la clé de Registre suivante est présente : **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Update Services\Server\Setup\Installed rôle Services\UpdateServices-base de données**