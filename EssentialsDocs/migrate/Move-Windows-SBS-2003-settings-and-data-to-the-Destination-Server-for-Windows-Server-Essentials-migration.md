---
title: "Déplacer les paramètres de Windows SBS 2003 et des données pour la migration du serveur de Destination pour Windows Server Essentials"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 67087ccb-d820-4642-8ca2-7d2d38714014
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 74375d65845a7a5e9c2d6752bdee49993b8cc791
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="move-windows-sbs-2003-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>Déplacer les paramètres de Windows SBS 2003 et des données pour la migration du serveur de Destination pour Windows Server Essentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Déplacer les paramètres et données vers le serveur de Destination comme suit:

1.  [Copier les données sur le serveur de Destination](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_CopyData)  
  
2.  [Importer des comptes d’utilisateurs Active Directory vers le bord Windows Server Essentials (facultatif)](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_ImportADaccounts)  
  
3.  [Supprimer les anciens scripts d’ouverture de session (facultatifs)](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_RemoveScripts)  
  
4.  [Supprimer les anciens Active Directory objets stratégie de groupe (facultatif)](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_RemoveLegacyADGPO)  
  
5.  [Configurer le réseau](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_Configure)  
  
6.  [Mapper les ordinateurs autorisés aux comptes d’utilisateurs](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_MapPermittedComputers)  
 
##  <a name="BKMK_CopyData"></a>Copier les données sur le serveur de Destination  
 Avant de copier des données à partir du serveur Source vers le serveur de Destination, effectuez les tâches suivantes:  
  
-   Passez en revue la liste des dossiers partagés sur le serveur Source, y compris les autorisations pour chaque dossier. Créez ou personnalisez les dossiers sur le serveur de Destination pour correspondre à la structure de dossiers que vous migrez à partir du serveur Source.  
  
-   Passez en revue la taille de chaque dossier et assurez-vous que le serveur de Destination dispose de suffisamment d’espace.  
  
-   Vérifiez les dossiers partagés sur le serveur Source en lecture seule pour tous les utilisateurs donc aucune écriture ne puisse être réalisée sur le lecteur pendant que vous copiez les fichiers vers le serveur de Destination.  
  
#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>Pour copier les données à partir du serveur Source vers le serveur de Destination  
  
1.  Connectez-vous au serveur de Destination en tant qu’administrateur de domaine.  
  
2.  Cliquez sur **Démarrer**, type **cmd** dans la zone de recherche, puis appuyez sur ENTRÉE.  
  
3.  À l’invite de commandes, tapez la commande suivante et appuyez sur ENTRÉE:  
  
    `robocopy \\<SourceServerName> \<SharedSourceFolderName> \\<DestinationServerName> \<SharedDestinationFolderName> /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`  
  
     Où:
     - \ < SourceServerName\ > est le nom du serveur Source
     - \ < SharedSourceFolderName\ > est le nom du dossier partagé sur le serveur Source
     - \ < DestinationServerName\ > est le nom du serveur de Destination,
     - \ < SharedDestinationFolderName\ > est le dossier partagé sur le serveur de Destination vers lequel les données seront copiées.  
 
4.  Répétez l’étape précédente pour chaque dossier partagé que vous effectuez une migration à partir du serveur Source.  
  
##  <a name="BKMK_ImportADaccounts"></a>Importer des comptes d’utilisateurs Active Directory vers le bord Windows Server Essentials (facultatif)  
 Par défaut, tous les comptes d’utilisateur créés sur le serveur Source migrent automatiquement vers le tableau de bord dans Windows Server Essentials. Toutefois, la migration automatique d’un compte d’utilisateur Active Directory échouera si certaines propriétés ne répondent pas aux conditions de la migration. Vous pouvez utiliser l’applet de commande Windows PowerShell suivante pour importer des utilisateurs Active Directory.  
  
#### <a name="to-import-an-active-directory-user-account-to-the-windows-server-essentials-dashboard"></a>Pour importer un compte d’utilisateur Active Directory dans le tableau de bord Windows Server Essentials  
  
1.  Connectez-vous au serveur de Destination en tant qu’administrateur de domaine.  
  
2.  Ouvrez Windows PowerShell en tant qu’administrateur.  
  
3.  Exécutez l’applet de commande suivante, où `[AD username]` est le nom du compte d’utilisateur Active Directory que vous souhaitez importer:  
  
     `Import-WssUser SamAccountName [AD username]`  
  
##  <a name="BKMK_RemoveScripts"></a>Supprimer les anciens scripts d’ouverture de session (facultatifs)  
 Windows SBS 2003 utilise des scripts d’ouverture de session pour des tâches telles que l’installation de logiciels et la personnalisation des postes de travail.  Windows Server Essentials remplace les scripts d’ouverture de session Windows SBS 2003 par une combinaison de scripts d’ouverture de session et les objets de stratégie de groupe.  
  
> [!NOTE]
>  Si vous avez modifié les scripts d’ouverture de session Windows SBS 2003, vous devez renommer les scripts pour conserver vos personnalisations.  
  
> [!NOTE]
>  Les scripts d’ouverture de session Windows SBS 2003 s’appliquent uniquement aux comptes d’utilisateurs qui ont été ajoutés à l’aide de la **Assistant Ajout de nouveaux utilisateurs**.  
  
#### <a name="to-remove-the-windows-sbs-2003-logon-scripts"></a>Pour supprimer les scripts d’ouverture de session Windows SBS 2003  
  
1.  Cliquez sur **Démarrer**, pointez sur **outils d’administration**, puis cliquez sur **Active Directory Users and Computers**.  
  
2.  Dans **Active Directory Users and Computers**, développez votre réseau, puis cliquez sur **utilisateurs**.  
  
3.  Cliquez sur un nom d’utilisateur, cliquez sur **propriétés**, puis cliquez sur le **profil** onglet.  
  
4.  Supprimer le contenu de la **script d’ouverture de session** zone de texte, puis cliquez sur **OK**.  
  
5.  Répétez les étapes 3 et 4 pour chaque utilisateur.  
  
##  <a name="BKMK_RemoveLegacyADGPO"></a>Supprimer les anciens Active Directory objets stratégie de groupe (facultatif)  
 Les objets de stratégie de groupe (GPO) sont mis à jour pour Windows Server Essentials. Ils représentent un sur-ensemble de la stratégie de groupe Windows SBS 2003. Pour Windows Server Essentials, un certain nombre de filtres Windows Management Instrumentation (WMI) et de groupe Windows SBS 2003 doit être supprimé manuellement pour éviter tout conflit avec les filtres WMI et de stratégie de groupe Windows Server Essentials.  
  
> [!NOTE]
>  Si vous avez modifié les objets de stratégie de groupe d’origine Windows SBS 2003, vous devez enregistrer des copies à un autre emplacement et puis supprimez-les de Windows SBS 2003.  
  
#### <a name="to-remove-old-group-policy-objects-from-windows-sbs-2003"></a>Pour supprimer les anciens objets de stratégie de groupe à partir de Windows SBS 2003  
  
1.  Ouvrez une session sur le serveur Source avec un compte d’administrateur.  
  
2.  Cliquez sur **Démarrer**, puis cliquez sur **gestion de serveur**.  
  
3.  Dans le volet de navigation, cliquez sur **gestion avancée des**, cliquez sur **gestion des stratégies de groupe**, puis cliquez sur **forêt:***< YourDomainName\ >*.  
  
4.  Cliquez sur **domaines**, cliquez sur *< YourDomainName\ >*, puis cliquez sur **objets de stratégie de groupe**.  
  
5.  Avec le bouton droit **stratégie Small Business Server audit**, cliquez sur **supprimer**, puis cliquez sur **OK**.  
  
6.  Répétez l’étape 5 pour supprimer les objets de stratégie de groupe suivants qui s’appliquent à votre réseau:  
  
    -   Ordinateur Client de Small Business Server  
  
    -   Stratégie de mot de passe de domaine Small Business Server  
  
         Nous vous recommandons de que configurer la stratégie de mot de passe dans Windows Server Essentials pour mettre en œuvre des mots de passe forts. Pour configurer la stratégie de mot de passe, utilisez le tableau de bord, qui écrit la configuration de la stratégie de domaine par défaut. La configuration de stratégie de mot de passe n’est pas écrite dans l’objet Small Business Server domaine mot de passe de stratégie, comme il était le cas dans Windows SBS 2003.  
  
    -   Pare-feu de connexion Internet Small Business Server  
  
    -   Stratégie de verrouillage Small Business Server  
  
    -   Stratégie d’Assistance à distance Small Business Server  
  
    -   Pare-feu de Windows Small Business Server  
  
    -   Stratégie d’ordinateur Client Small Business Server Update Services  
  
         Cet objet de stratégie de groupe sera présent si vous effectuez une migration à partir de Windows SBS 2003 R2.  
  
    -   Small Business Server Update Services stratégie des paramètres communs  
  
         Cet objet de stratégie de groupe sera présent si vous effectuez une migration à partir de Windows SBS 2003 R2.  
  
    -   Stratégie d’ordinateur serveur Small Business Server Update Services  
  
         Cet objet de stratégie de groupe sera présent si vous effectuez une migration à partir de Windows SBS 2003 R2.  
  
7.  Vérifiez que tous les objets GPO sont supprimés.  
  
#### <a name="to-remove-wmi-filters-from-windows-sbs-2003"></a>Pour supprimer les filtres WMI de Windows SBS 2003  
  
1.  Ouvrez une session sur le serveur Source avec un compte d’administrateur.  
  
2.  Cliquez sur **Démarrer**, puis cliquez sur **gestion de serveur**.  
  
3.  Dans le volet de navigation, cliquez sur **gestion avancée des**, cliquez sur **gestion des stratégies de groupe**, puis cliquez sur **forêt:***< YourNetworkDomainName\ >*  
  
4.  Cliquez sur **domaines**, cliquez sur *< YourNetworkDomainName\ >*, puis cliquez sur **filtres WMI**.  
  
5.  Avec le bouton droit **PostSP2**, cliquez sur **supprimer**, puis cliquez sur **Oui**.  
  
6.  Avec le bouton droit **PreSP2**, cliquez sur **supprimer**, puis cliquez sur **Oui**.  
  
7.  Vérifiez que ces trois filtres WMI sont supprimés.  
  
##  <a name="BKMK_Configure"></a>Configurer le réseau  
  
#### <a name="to-configure-the-network"></a>Pour configurer le réseau  
  
1.  Sur le serveur de Destination, ouvrez le tableau de bord.  
  
2.  Sur le tableau de bord **accueil** , cliquez sur **le programme d’installation**, cliquez sur **configurer l’accès en tout lieu**, puis choisissez le **cliquez pour configurer l’accès en tout lieu** option.  
  
3.  Suivez les instructions de la **configurer l’accès en tout lieu** Assistant pour configurer votre routeur et nom de domaine.  
  
 Si votre routeur ne prend pas en charge l’infrastructure UPnP, ou si l’infrastructure UPnP est désactivée, une icône d’avertissement jaune peut apparaître en regard du nom du routeur. Vérifiez que les ports suivants sont ouverts et qu’ils sont dirigés vers l’adresse IP du serveur de Destination:  
  
-   Port 80: Le trafic HTTP Web  
  
-   Port 443: Le trafic Web HTTPS  
  
> [!NOTE]
>  Si vous avez configuré un serveur d’Exchange local sur un second serveur, vous devez vérifier le port 25 (SMTP) est également ouvert et qu’il est redirigé vers l’adresse IP du serveur Exchange local.  
  
##  <a name="BKMK_MapPermittedComputers"></a>Mapper les ordinateurs autorisés aux comptes d’utilisateurs  
 Dans Windows SBS 2003, si un utilisateur se connecte à l’accès Web à distance, tous les ordinateurs du réseau sont affichés. Cela peut inclure des ordinateurs que l’utilisateur n’est pas autorisé à accéder. Dans Windows Server Essentials, un utilisateur doit être explicitement affecté à un ordinateur pour qu’il s’affiche dans l’accès Web à distance. Chaque compte d’utilisateur qui est migré à partir de Windows SBS 2003 doit être mappé à un ou plusieurs ordinateurs.  
  
#### <a name="to-map-user-accounts-to-computers"></a>Pour mapper les comptes d’utilisateur aux ordinateurs  
  
1.  Sur le serveur de Destination, ouvrez le tableau de bord WindowsServerEssentials.  
  
2.  Dans la barre de navigation, cliquez sur **utilisateurs**.  
  
3.  Dans la liste des comptes d’utilisateur, cliquez sur un compte d’utilisateur, puis cliquez sur **permet d’afficher les propriétés du compte**.  
  
4.  Cliquez sur le **accès en tout lieu** onglet, puis cliquez sur **autoriser l’accès Web à distance et l’accès aux applications de services web. **.  
  
5.  Cliquez sur **dossiers partagés**, cliquez sur **ordinateurs**, cliquez sur **liens de la page d’accueil**, puis cliquez sur **appliquer**.  
  
6.  Cliquez sur le **accès à l’ordinateur** onglet, puis cliquez sur le nom de l’ordinateur auquel vous souhaitez autoriser l’accès.  
  
7.  Répétez les étapes 3, 4, 5 et 6 pour chaque compte d’utilisateur.  
  
> [!NOTE]
>  Vous n’avez pas besoin de modifier la configuration de l’ordinateur client. Il est configuré automatiquement.  
  
> [!NOTE]
>  Après avoir effectué la migration, si vous rencontrez un problème lorsque vous créez le premier compte d’utilisateur sur le serveur de Destination, supprimer le compte d’utilisateur que vous avez ajouté, puis recréez-le.
