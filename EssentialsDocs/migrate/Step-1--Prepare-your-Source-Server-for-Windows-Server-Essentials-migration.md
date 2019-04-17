---
title: "Étape1: Préparer la migration de votre serveur Source pour WindowsServerEssentials"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 244c8a06-04c6-4863-8b52-974786455373
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 2efb1badde6d0ca11bc3b7526fdfb377d9f95d3f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="step-1-prepare-your-source-server-for-windows-server-essentials-migration"></a>Étape1: Préparer la migration de votre serveur Source pour WindowsServerEssentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Cette rubrique explique comment sauvegarder le serveur Source, évaluer l’intégrité du système de serveur Source, installer les service packs et les correctifs les plus récentes et vérifier la configuration réseau.  
  
## <a name="to-prepare-for-migration"></a>Pour préparer la migration  
 Effectuez les étapes préliminaires suivantes pour vous assurer que les paramètres et données sur votre serveur Source migrent correctement vers le serveur de Destination.  
  
1.  [Sauvegardez votre serveur Source](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_BackUpYourSourceServerToPrepareForMigration)  
  
2.  [Installer les service packs plus récents](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_InstallTheMostRecentServicePacksToPrepareForMigration)  
  
3.  [Supprimer le journal sur comme un paramètre de compte de service](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_DeleteSvcAcctSetting)  
  
4.  [Évaluer l’intégrité du serveur Source](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_EvaluateHealth)  
  
5.  [Créer un plan pour migrer des applications métier de](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_MigrateLOB)  
  
###  <a name="BKMK_BackUpYourSourceServerToPrepareForMigration"></a>Sauvegardez votre serveur Source  
 Sauvegardez votre serveur Source avant de commencer le processus de migration. Effectuer une sauvegarde permet de protéger vos données contre toute perte accidentelle si une erreur irrécupérable se produit lors de la migration.  
  
##### <a name="to-back-up-the-source-server"></a>Pour sauvegarder le serveur Source  
  
1.  Utilisez une des ressources dans le tableau suivant pour vous guider dans la réalisation d’une sauvegarde complète du serveur Source.  
  
2.  Vérifiez que la sauvegarde a été correctement exécuté. Pour tester l’intégrité de la sauvegarde, sélectionnez aléatoirement des fichiers de la sauvegarde, restaurez-les à un autre emplacement, puis vérifiez que les fichiers restaurés sont identiques aux fichiers d’origine.  
  
   |Produit|Ressource|
   |---|---|
   |WindowsSmallBusinessServer2003|[Sauvegarde et restauration de WindowsSmallBusinessServer2003](https://msdn.microsoft.com/library/cc875809.aspx) 
   |WindowsSmallBusinessServer2008|[Sauvegarde et restauration des données sur WindowsSmallBusinessServer2008](https://technet.microsoft.com/library/cc527505\(WS.10\).aspx)
   |Windows Server2008 Foundation|[Sauvegarde et récupération](https://technet.microsoft.com/library/cc754097\(WS.10\).aspx)  
   |WindowsSmallBusinessServer2011Essentials|[En savoir plus sur la définition de la sauvegarde du serveur](https://technet.microsoft.com/library/server-backup-support-1.aspx)
   |WindowsSmallBusinessServer2011 Standard|[Gestion de la sauvegarde du serveur](https://technet.microsoft.com/library/cc527488.aspx)  
   |Windows Server Essentials|[Gérer la sauvegarde et restauration dans WindowsServerEssentials](https://technet.microsoft.com/library/jj713536.aspx)

###  <a name="BKMK_InstallTheMostRecentServicePacksToPrepareForMigration"></a>Installer les service packs plus récents  
 Vous devez installer les dernières mises à jour et les service packs sur le serveur Source avant la migration.  
  
###  <a name="BKMK_DeleteSvcAcctSetting"></a>Supprimer le journal sur comme un paramètre de compte de service  
 Si vous migrez à partir de WindowsSmallBusinessServer2003 ou Windows Server2003, supprimez le **ouvrir une session en tant que service** paramètre stratégie de groupe du compte.  
  
##### <a name="to-delete-the-log-on-as-a-service-account-setting"></a>Pour supprimer le journal sur un paramètre de compte de service  
  
1.  Pour ouvrir le **gestion des stratégies de groupe** outil, cliquez sur **Démarrer**, cliquez sur **le panneau de configuration**, cliquez sur **outils d’administration**, puis cliquez sur **gestion des stratégies de groupe**.  
  
2.  Avec le bouton droit **stratégie des contrôleurs de domaine par défaut**, puis cliquez sur **modifier **.  
  
3.  Accédez à **ordinateur Configuration ordinateur\Paramètres Windows\Paramètres sécurité\Stratégies locales\Attribution des droits**.  
  
4.  Dans le volet d’informations, double-cliquez sur **ouvrir une session en tant que service**.  
  
5.  Désactivez le **définir ces paramètres de stratégie** case à cocher.  
  
6.  Supprimez \\\localhost\SYSVOL\\ < domainname\ > \scripts\SBS_LOGIN_SCRIPT.bat.  
  
###  <a name="BKMK_EvaluateHealth"></a>Évaluer l’intégrité du serveur Source  
 Il est important d’évaluer l’intégrité de votre serveur Source avant de commencer la migration. Utilisez les procédures suivantes pour vous assurer que les mises à jour sont actuelles, pour générer un rapport d’intégrité système et pour exécuter le WindowsServerSolutionsBest pratique Analyzer (BPA).  
  
#### <a name="download-and-install-critical-and-security-updates"></a>Téléchargement et installation critiques et sécurité des mises à jour  
 L’installation critiques et sécurité des mises à jour sur le serveur Source permet de s’assurer que votre migration sera réussie et contribue à protéger votre réseau pendant le processus de migration.  
  
###### <a name="to-check-for-the-latest-updates"></a>Pour vérifier les dernières mises à jour  
  
1.  À partir du serveur Source, cliquez sur **Démarrer**, cliquez sur **tous les programmes**, puis cliquez sur **mise à jour Windows**.  
  
2.  Cliquez sur **rechercher les mises à jour**.  
  
3.  Si les mises à jour sont trouvées, cliquez sur **installer les mises à jour**.  
  
#### <a name="run-the-best-practices-analyzer"></a>Exécuter le Best Practices Analyzer  
 Vous pouvez exécuter le Best Practices Analyzer (BPA) pour vérifier qu’il n’y a aucun problème sur votre serveur, réseau ou domaine avant de commencer le processus de migration. L’analyseur BPA collecte les informations de configuration à partir des sources suivantes:  
  
-   ActiveDirectory WindowsManagementInstrumentation (WMI)  
  
-   Le Registre  
  
-   Internet Information Services (IIS)  
  
###### <a name="to-use-the-bpa-to-analyze-your-source-server"></a>Pour utiliser l’outil BPA pour analyser votre serveur Source  
  
1.  Le tableau suivant fournit des liens vers du MicrosoftDownload Center où vous pouvez télécharger et installer Best Practices Analyzer (BPA) pour le serveur Source.  
  
   |Si votre serveur Source est en cours d’exécution|Vous pouvez obtenir les outils BPA à partir de|
   |---|---|
   |Windows SBS2003|[Site Web de MicrosoftWindowsSmallBusinessServer2003 Best Practices Analyzer](https://www.microsoft.com/download/details.aspx?id=5334)
   |Windows SBS2008|[Site Web de MicrosoftWindowsSmallBusinessServer2008 Best Practices Analyzer](https://www.microsoft.com/download/details.aspx?id=6231)  
   |Windows SBS2011Essentials ou Windows SBS2011 Standard|[Site Web de WindowsServerSolutionsBestPractices Analyzer](https://www.microsoft.com/download/details.aspx?id=15556) 
   |WindowsServerEssentials ou Windows Server2012|Le tableau de bord du serveur  
  
2.  Une fois le téléchargement terminé, cliquez sur **Démarrer**, pointez sur **tous les programmes**, puis cliquez sur **outil SBS Best Practices Analyzer**.  
  
    > [!NOTE]
    >  Recherchez les mises à jour avant d’analyser le serveur.  
  
3.  Dans le volet de navigation, cliquez sur **démarrer une analyse**.  
  
     Si votre serveur Source exécute WindowsServerEssentials, procédez comme suit:  
  
    1.  Ouvrez une session sur le serveur de Destination en tant qu’administrateur, puis ouvrez le tableau de bord.  
  
    2.  Dans le tableau de bord, cliquez sur le **périphériques** onglet.  
  
    3.  Dans le <**Server** >**tâches** volet, cliquez sur **Best Practices Analyzer**.  
  
4.  Dans le volet d’informations, tapez l’étiquette d’analyse, puis cliquez sur **numérisations**. L’étiquette d’analyse est le nom du rapport d’analyse, par exemple, **SBS BPA Scan 1 Jul2013**.  
  
5.  Une fois l’analyse terminée, cliquez sur **afficher un rapport de cette analyse Best Practices**.  
  
 Une fois que l’outil BPA collecte des informations sur la configuration du serveur, il vérifie que les informations sont correctes et présente ensuite aux administrateurs avec une liste des informations et problèmes triés par niveau de gravité. La liste décrit chaque problème et fournit une recommandation ou une solution possible. Trois types de rapports sont disponibles:  
  
|Type de rapport|Description
|-----------------|----------------- 
|Liste des rapports|Affiche les rapports dans une liste unidimensionnelle. 
|Rapports d’arborescence|Affiche les rapports dans une liste hiérarchique.

Pour afficher la description et les solutions d’un problème, cliquez sur le problème dans le rapport. Affectent pas tous les problèmes signalés par l’outil BPA migration, mais vous devez résoudre autant de problèmes que possible garantir la réussite de la migration.  
  
####  <a name="BKMK_SynchronizeTheSourceServerTimeWithAnExternalTimeSource"></a>Synchroniser l’heure du serveur Source avec une source de temps externe  
 L’heure sur le serveur Source doit être définie sur dans les cinq minutes l’heure sur le serveur de Destination, et la date et le fuseau horaire doivent être identiques sur les deux serveurs. Le serveur Source est en cours d’exécution sur un ordinateur virtuel, la date et heure fuseau horaire sur le serveur hôte doivent correspondre à celui du serveur Source et le serveur de Destination. Pour vous assurer que WindowsServerEssentials est installé avec succès, vous devez synchroniser l’heure du serveur Source vers le serveur de protocole NTP (Network Time Protocol) sur Internet.  
  
###### <a name="to-synchronize-the-source-server-time-with-the-ntp-server"></a>Pour synchroniser l’heure du serveur Source sur le serveur NTP  
  
1.  Ouvrez une session sur le serveur Source avec un compte d’administrateur de domaine et le mot de passe.  
  
2.  Cliquez sur **Démarrer**, cliquez sur **exécuter**, type **cmd** dans la zone de texte, puis appuyez sur ENTRÉE.  
  
3.  À l’invite de commandes, tapez w32tm /config /syncfromflags: domhier//reliable: aucune /update et appuyez sur ENTRÉE.  
  
4.  À l’invite de commandes, tapez net stop w32time et appuyez sur ENTRÉE.  
  
5.  À l’invite de commandes, tapez net start w32time et appuyez sur ENTRÉE.  
  
> [!IMPORTANT]
>  Pendant l’installation de WindowsServerEssentials, vous avez la possibilité pour vérifier l’heure sur le serveur de Destination et la modifier, si nécessaire. Assurez-vous que l’heure est réglée à cinq minutes l’heure est définie sur le serveur Source. Une fois l’installation terminée, le serveur de Destination se synchronise avec le serveur NTP. Tous les ordinateurs joints au domaine, y compris le serveur Source, synchronisent sur le serveur de Destination, qui assume le rôle de domaine principal maître d’émulateur PDC (contrôleur de domaine principal).  
  
###  <a name="BKMK_MigrateLOB"></a>Créer un plan pour migrer des applications métier de  
 Une application de (LOB) de métier est une application informatique critique vitale pour une entreprise. Applications métiers incluent la gestion de la comptabilité, la chaîne d’approvisionnement et la planification des ressources.  
  
 Lorsque vous envisagez de migrer vos applications cœur de métier, consultez les fournisseurs d’application cœur de métier pour déterminer la méthode appropriée pour chaque application. Vous devez également localiser le média qui est utilisé pour installer les applications cœur de métier sur le serveur de Destination.  
  
> [!NOTE]
>  Si WindowsSmallBusinessServer2011Essentials SDK vous permet de développer un complément d’intégrité système ou alerte et vous souhaitez continuer à utiliser ce complément avec WindowsServerEssentials, vous devez également mettre à jour le complément et le déployer sur le serveur de Destination.  
  
  
### <a name="create-a-plan-to-migrate-email-hosted-on-windows-sbs-2011-windows-sbs-2008-and-windows-sbs-2003"></a>Créer un plan pour migrer la messagerie électronique hébergée sur Windows SBS2011, Windows SBS2008 et Windows SBS2003  
 Dans Windows SBS2011, Windows SBS2008 et Windows SBS2003, la messagerie électronique est fournie par le biais de MicrosoftExchange Server. Toutefois, WindowsServerEssentials ne fournit pas un service de messagerie boîte de réception. Si vous utilisez actuellement un serveur qui exécute Windows SBS2011, Windows SBS2008 ou Windows SBS2003 pour héberger votre courrier d’entreprise s, vous devez migrer vers une alternative locale ou hébergée solution.  
  
> [!NOTE]
>  Une fois que vous mettez à jour et préparez votre serveur Source pour la migration, nous vous recommandons de créer une sauvegarde du serveur mis à jour avant de poursuivre le processus de migration.  
  
#### <a name="migrate-email-to-microsoft-office-365"></a>Migrer la messagerie électronique vers MicrosoftOffice 365  
 Si vous avez choisi d’utiliser MicrosoftOffice 365 comme solution de messagerie pour votre domaine, suivez les instructions de [migrer de toutes les boîtes aux lettres vers le Cloud avec une Migration Exchange à basculement](http://help.outlook.com/140/ms.exch.ecp.emailmigrationwizardexchangelearnmore.aspx) pour démarrer la migration de courrier électronique vers Office 365. Nous vous recommandons d’effectuer la migration de courrier électronique avant d’installer WindowsServerEssentials.  
  
> [!NOTE]
>  L’étape de suppression du serveur Exchange local sur le serveur Source est obligatoire si vous comptez intégrer WindowsServerEssentials à Office 365. Pour plus d’informations sur la façon de migrer des dossiers publics Exchange Server vers Office 365, consultez le blog [MicrosoftExchange2013 Public Folders Migration Scripts for Office 365](http://blogs.technet.com/b/fmustafa/archive/2013/04/11/microsoft-exchange-2013-public-folders-migration-scripts-for-office-365.aspx).  
>   
>  Une fois l’installation terminée, vous devez activer la fonctionnalité d’intégration d’Office 365dans WindowsServerEssentials en exécutant la **intégration à MicrosoftOffice 365** tâche.  
  
> [!IMPORTANT]
>  Pour permettre à l’outil de migration Office 365 pour se connecter au serveur Exchange qui est en cours d’exécution sur le serveur Source, vous devez activer RPC sur HTTP sur le serveur Source. Pour plus d’informations sur l’activation de RPC sur HTTP, consultez [procédure de déploiement de RPC sur HTTP dans Small Business Server2003 (Standard ou Premium) première](https://technet.microsoft.com/library/bb123622%28EXCHG.65%29.aspx). Si vous ne pouvez pas exécuter correctement l’outil de migration Office 365après avoir activé RPC sur HTTP, examinez le **ValidPorts** paramètre dans le Registre au niveau de HKEY_LOCAL_MACHINE\Software\Microsoft\Rpc\RpcProxy et vérifiez que le nom de domaine complet (FQDN) pour le serveur Source est répertorié. Si le nom de domaine complet n’est pas répertorié, ajoutez-le manuellement à l’aide de l’exemple suivant:  
>   
>  à distance. *Contoso*.com:6001-6002; à distance. *Contoso*.com: 6004 (remplacez *contoso* avec le nom de votre domaine).  
  
#### <a name="migrate-email-to-another-on-premises-exchange-server"></a>Migrer la messagerie électronique vers un autre serveur d’Exchange sur site  
 Pour savoir comment migrer la messagerie électronique vers un autre sur Exchange Server local, consultez [intégrer un serveur On-Premises Exchange Server avec WindowsServerEssentials](https://technet.microsoft.com/library/jj200172.aspx). Nous vous recommandons de définir le nouveau serveur Exchange local une fois que vous installez WindowsServerEssentials, puis de terminer la migration de courrier électronique avant de rétrograder le serveur Source.  
  
> [!NOTE]
>  Le connecteur POP3 de WindowsSmallBusinessServer n’est pas inclus avec Exchange Server. Après avoir migré les données de messagerie vers un autre serveur Exchange, vous ne pouvez plus utiliser la fonctionnalité Connecteur POP3.  
  
> [!NOTE]
>  Une fois que vous mettez à jour et préparez votre serveur Source pour la migration, vous devez créer une sauvegarde du serveur mis à jour avant de poursuivre le processus de migration.  
  
## <a name="next-steps"></a>Étapes suivantes  
 Vous avez préparé votre serveur Source pour la migration vers WindowsServerEssentials.  Passez maintenant à [étape2: installer WindowsServerEssentials en tant que nouveau contrôleur de domaine réplica](Step-2--Install-Windows-Server-Essentials-as-a-new-replica-domain-controller.md).  

Pour afficher toutes les étapes, voir [migrer vers WindowsServerEssentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

