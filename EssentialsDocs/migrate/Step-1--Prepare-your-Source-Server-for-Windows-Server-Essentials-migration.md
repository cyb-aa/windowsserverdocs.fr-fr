---
title: 'Étape 1 : Préparer votre serveur source pour la migration vers Windows Server Essentials'
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 244c8a06-04c6-4863-8b52-974786455373
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: cb0cffdda0e0f1528887d3c94a1905a99c5c55c3
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318782"
---
# <a name="step-1-prepare-your-source-server-for-windows-server-essentials-migration"></a>Étape 1 : Préparer votre serveur source pour la migration vers Windows Server Essentials

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Cette section décrit comment sauvegarder le serveur source, évaluer l'intégrité du système du serveur source, installer les Service Packs et correctifs les plus récents et vérifier la configuration réseau.  

## <a name="to-prepare-for-migration"></a>Pour préparer la migration  
 Effectuez les étapes préliminaires suivantes afin de vous assurer que les paramètres et les données sur votre serveur source migrent correctement vers le serveur de destination.  

1.  [Sauvegarder votre serveur source](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_BackUpYourSourceServerToPrepareForMigration)  

2.  [Installer les service packs les plus récents](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_InstallTheMostRecentServicePacksToPrepareForMigration)  

3.  [Supprimer le paramètre ouvrir une session en tant que compte de service](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_DeleteSvcAcctSetting)  

4.  [Évaluer l’intégrité du serveur source](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_EvaluateHealth)  

5.  [Créer un plan pour migrer des applications métier](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_MigrateLOB)  

###  <a name="back-up-your-source-server"></a><a name="BKMK_BackUpYourSourceServerToPrepareForMigration"></a>Sauvegarder votre serveur source  
 Sauvegardez votre serveur source avant de commencer le processus de migration. L'exécution d'une sauvegarde permet de protéger vos données de toute perte accidentelle si une erreur irrécupérable survient lors de la migration.  

##### <a name="to-back-up-the-source-server"></a>Pour sauvegarder le serveur source  

1. Utilisez l'une des ressources du tableau suivant pour vous guider dans la réalisation d'une sauvegarde complète du serveur source.  

2. Vérifiez que la sauvegarde a été effectuée correctement. Pour tester l’intégrité de la sauvegarde, sélectionnez des fichiers aléatoires de la sauvegarde, restaurez-les à un autre emplacement, puis vérifiez que les fichiers restaurés sont identiques aux fichiers d’origine.  

   |Produit|Resource|
   |---|---|
   |Windows Small Business Server 2003|[Sauvegarde et restauration de Windows Small Business Server 2003](https://msdn.microsoft.com/library/cc875809.aspx) 
   |Windows Small Business Server 2008|[Sauvegarde et restauration des données sur Windows Small Business Server 2008](https://technet.microsoft.com/library/cc527505\(WS.10\).aspx)
   |Windows Server 2008 Foundation|[Sauvegarde et récupération](https://technet.microsoft.com/library/cc754097\(WS.10\).aspx)  
   |Windows Small Business Server 2011 Essentials|[En savoir plus sur la configuration de la sauvegarde du serveur](https://technet.microsoft.com/library/server-backup-support-1.aspx)
   |Windows Small Business Server 2011 Standard|[Gestion de la sauvegarde du serveur](https://technet.microsoft.com/library/cc527488.aspx)  
   |Windows Server Essentials|[Gérer la sauvegarde et la restauration dans Windows Server Essentials](https://technet.microsoft.com/library/jj713536.aspx)

###  <a name="install-the-most-recent-service-packs"></a><a name="BKMK_InstallTheMostRecentServicePacksToPrepareForMigration"></a>Installer les service packs les plus récents  
 Vous devez installer les mises à jour et les Service Packs les plus récents sur le serveur source avant d'effectuer la migration.  

###  <a name="delete-the-log-on-as-a-service-account-setting"></a><a name="BKMK_DeleteSvcAcctSetting"></a>Supprimer le paramètre ouvrir une session en tant que compte de service  
 Si vous effectuez une migration à partir de Windows Small Business Server 2003 ou Windows Server 2003, supprimez le paramètre de compte **Ouvrir une session en tant que service** de la stratégie de groupe.  

##### <a name="to-delete-the-log-on-as-a-service-account-setting"></a>Pour supprimer le paramètre ouvrir une session en tant que compte de service  

1.  Pour ouvrir l'outil **Gestion des stratégies de groupe**, cliquez sur **Démarrer**, **Panneau de configuration**, **Outils d'administration**, puis sur **Gestion des stratégies de groupe**.  

2.  Cliquez avec le bouton droit sur **Stratégie des contrôleurs de domaine par défaut**, puis cliquez sur **Modifier**.  

3.  Accédez à **Configuration ordinateur\Paramètres Windows\Paramètres de sécurité\Stratégies locales\Attribution des droits utilisateur**.  

4.  Dans le volet d'informations, double-cliquez sur **Ouvrir une session en tant que service**.  

5.  Décochez la case **Définir ces paramètres de stratégie**.  

6.  Supprimez \\\localhost\SYSVOL\\< nom_domaine\>\scripts\ SBS_LOGIN_SCRIPT. bat.  

###  <a name="evaluate-the-health-of-the-source-server"></a><a name="BKMK_EvaluateHealth"></a>Évaluer l’intégrité du serveur source  
 Il est important d'évaluer l'intégrité de votre serveur source avant de commencer la migration. Utilisez les procédures suivantes pour vous assurer que les mises à jour sont actuelles, pour générer un rapport d'intégrité du système et pour exécuter Windows Server Solutions Best Practice Analyzer (BPA).  

#### <a name="download-and-install-critical-and-security-updates"></a>Télécharger et installer les mises à jour critiques et de sécurité  
 L'installation des mises à jour critiques et de sécurité sur le serveur source permet d'assurer la réussite de votre migration et contribue à protéger votre réseau pendant le processus de migration.  

###### <a name="to-check-for-the-latest-updates"></a>Pour rechercher les dernières mises à jour  

1.  Sur le serveur source, cliquez sur **Démarrer**, sur **Tous les programmes**, puis sur **Windows Update**.  

2.  Cliquez sur **Rechercher les mises à jour**.  

3.  Si des mises à jour sont trouvées, cliquez sur **Installer les mises à jour**.  

#### <a name="run-the-best-practices-analyzer"></a>Exécuter l'outil Best Practices Analyzer  
 Vous pouvez exécuter l'outil Best Practices Analyzer (BPA) pour vérifier qu'il n'y a aucun problème sur votre serveur, réseau ou domaine avant de commencer le processus de migration. L'analyseur BPA collecte les informations de configuration auprès des sources suivantes :  

-   WMI (Windows Management Instrumentation) Active Directory  

-   Le Registre  

-   Internet Information Services (IIS)  

###### <a name="to-use-the-bpa-to-analyze-your-source-server"></a>Pour utiliser l'outil BPA pour analyser votre serveur source  

1. Le tableau suivant fournit des liens vers le Centre de téléchargement Microsoft à partir duquel vous pouvez télécharger et installer l'outil Best Practices Analyzer (BPA) pour le serveur source.  


   |             Si votre serveur source est en cours d’exécution             |                                                     vous pouvez récupérer les outils BPA à partir de                                                      |
   |----------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
   |                     Windows SBS 2003                     | [Site Web Best Practices Analyzer Microsoft Windows Small Business Server 2003](https://www.microsoft.com/download/details.aspx?id=5334) |
   |                     Windows SBS 2008                     | [Site Web Best Practices Analyzer Microsoft Windows Small Business Server 2008](https://www.microsoft.com/download/details.aspx?id=6231) |
   | Windows SBS 2011 Essentials ou Windows SBS 2011 Standard |          [Site Web Best Practices Analyzer des solutions Windows Server](https://www.microsoft.com/download/details.aspx?id=15556)           |
   |     Windows Server Essentials ou Windows Server 2012     |                                                          Tableau de bord du serveur                                                           |


2. Une fois le téléchargement terminé, cliquez sur **Démarrer**, pointez sur **Tous les programmes**, puis cliquez sur **Outil Best Practices Analyzer SBS**.  

   > [!NOTE]
   >  Recherchez les mises à jour avant d’analyser le serveur.  

3. Dans le volet de navigation, cliquez sur **Démarrer une analyse**.  

    Si votre serveur source exécute Windows Server Essentials, procédez comme suit :  

   1.  Connectez-vous au serveur de destination en tant qu'administrateur, puis ouvrez le tableau de bord.  

   2.  Dans le tableau de bord, cliquez sur l'onglet **Périphériques**.  

   3.  Dans le volet **tâches** <**Server** >, cliquez sur **Best Practices Analyzer**.  

4. Dans le volet d'informations, tapez le libellé de l'analyse, puis cliquez sur **Démarrer l'analyse**. Le libellé de l'analyse est le nom de l'état de l'analyse, par exemple, **SBS BPA Scan 1Jul2013**.  

5. Une fois l’analyse terminée, cliquez sur **Afficher un rapport de cette analyse Best Practices**.  

   Une fois la collecte d'informations sur la configuration du serveur effectuée, l'outil BPA vérifie que les informations sont correctes et présente ensuite aux administrateurs la liste des informations et des problèmes classés par niveau de gravité. La liste décrit chaque problème et fournit une recommandation ou une solution possible. Trois types de rapports sont disponibles :  

|Type de rapport|Description
|-----------------|----------------- 
|Rapports de liste|Affiche les rapports dans une liste unidimensionnelle. 
|Rapports d’arborescence|Affiche les rapports dans une liste hiérarchique.

Pour afficher la description et les solutions relatives à un problème, cliquez sur le problème dans le rapport. Les problèmes signalés par l'outil BPA n'affectent pas tous la migration, mais vous devez résoudre le plus grand nombre de problèmes pour garantir la réussite de la migration.  

####  <a name="synchronize-the-source-server-time-with-an-external-time-source"></a><a name="BKMK_SynchronizeTheSourceServerTimeWithAnExternalTimeSource"></a>Synchroniser l’heure du serveur source avec une source de temps externe  
 L’heure du serveur source doit être réglée, à cinq minutes près, sur l'heure du serveur de destination, et la date et le fuseau horaire doivent être identiques sur les deux serveurs. Si le serveur source est exécuté dans une machine virtuelle, la date, l’heure et le fuseau horaire du serveur hôte doivent correspondre à ceux du serveur source et du serveur de destination. Pour vous assurer que Windows Server Essentials est correctement installé, vous devez synchroniser l’heure du serveur source sur le serveur NTP (Network Time Protocol) sur Internet.  

###### <a name="to-synchronize-the-source-server-time-with-the-ntp-server"></a>Pour synchroniser l’heure du serveur source sur le serveur NTP  

1.  Connectez-vous au serveur source avec un compte d'administrateur de domaine et un mot de passe.  

2.  Cliquez sur **Démarrer**, sur **Exécuter**, tapez **cmd** dans la zone de texte, puis appuyez sur Entrée.  

3.  À l'invite de commandes, tapez w32tm /config /syncfromflags:domhier /reliable:no /update et appuyez sur Entrée.  

4.  À l’invite de commandes, tapez net stop w32time et appuyez sur Entrée.  

5.  À l’invite de commandes, tapez net start w32time et appuyez sur Entrée.  

> [!IMPORTANT]
>  Au cours de l’installation de Windows Server Essentials, vous avez la possibilité de vérifier l’heure sur le serveur de destination et de la modifier, si nécessaire. Vérifiez que l’heure est réglée à cinq minutes près sur l’heure réglée sur le serveur source. À la fin de l’installation, le serveur de destination est synchronisé sur le serveur NTP. Tous les ordinateurs appartenant à un domaine, y compris le serveur source, sont synchronisés sur le serveur de destination, qui prend le rôle de maître d’émulateur de contrôleur de domaine principal.  

###  <a name="create-a-plan-to-migrate-line-of-business-applications"></a><a name="BKMK_MigrateLOB"></a>Créer un plan pour migrer des applications métier  
 Une application métier est une application informatique critique vitale pour l'activité d'une entreprise. Les applications métiers incluent la gestion des comptes, la gestion de la chaîne logistique, ainsi que la planification des ressources.  

 Lorsque vous prévoyez de migrer vos applications métiers, consultez les fournisseurs d’applications métiers afin de définir la meilleure méthode de migration de chaque application. Vous devez également rechercher le support utilisé pour réinstaller les applications métiers sur le serveur de destination.  

> [!NOTE]
>  Si vous avez utilisé le kit de développement logiciel (SDK) Windows Small Business Server 2011 Essentials pour développer un complément d’intégrité système ou d’alerte personnalisé et que vous souhaitez continuer à utiliser le complément avec Windows Server Essentials, vous devez également mettre à jour le complément et le déployer sur le serveur de destination.  


### <a name="create-a-plan-to-migrate-email-hosted-on-windows-sbs-2011-windows-sbs-2008-and-windows-sbs-2003"></a>Créer un plan pour migrer la messagerie électronique hébergée sur Windows SBS 2011, Windows SBS 2008 et Windows SBS 2003  
 Dans Windows SBS 2011, Windows SBS 2008 et Windows SBS 2003, la messagerie électronique est fournie par Microsoft Exchange Server. Toutefois, Windows Server Essentials ne fournit pas de service de messagerie de boîte de réception. Si vous utilisez actuellement un serveur exécutant Windows SBS 2011, Windows SBS 2008 ou Windows SBS 2003 pour héberger le courrier électronique de votre entreprise, vous devez migrer vers une autre solution locale ou hébergée.  

> [!NOTE]
>  Une fois votre serveur source mis à jour et préparé pour la migration, nous vous recommandons de créer une sauvegarde du serveur mis à jour avant de poursuivre le processus de migration.  

#### <a name="migrate-email-to-microsoft-office-365"></a>Migrer la messagerie électronique vers Microsoft Office 365  
 Si vous avez choisi d’utiliser Microsoft Office 365 comme solution de messagerie électronique pour votre domaine, suivez les conseils de la rubrique [Migrer toutes les boîtes aux lettres vers le nuage à l’aide d’une migration Exchange de conversion](https://help.outlook.com/140/ms.exch.ecp.emailmigrationwizardexchangelearnmore.aspx) pour démarrer la migration de la messagerie électronique vers Office 365. Nous vous recommandons d’effectuer la migration de la messagerie électronique avant d’installer Windows Server Essentials.  

> [!NOTE]
>  L’étape de suppression du serveur Exchange local sur le serveur source est obligatoire si vous envisagez d’intégrer Windows Server Essentials à Office 365. Pour plus d’informations sur la migration de dossiers publics Exchange Server vers Office 365, consultez le billet de blog [Microsoft Exchange 2013 Public Folders Migration Scripts for Office 365](https://blogs.technet.com/b/fmustafa/archive/2013/04/11/microsoft-exchange-2013-public-folders-migration-scripts-for-office-365.aspx).  
>   
>  Une fois l’installation terminée, vous devez activer la fonctionnalité d’intégration Office 365 dans Windows Server Essentials en exécutant la tâche **intégrer avec Microsoft Office 365** .  

> [!IMPORTANT]
>  Pour permettre à l'outil de migration Office 365 de se connecter à Exchange Server qui est exécuté sur le serveur source, vous devez activer RPC sur HTTP sur le serveur source. Pour plus d’informations sur l’activation de RPC sur HTTP, consultez [Comment effectuer le premier déploiement de RPC sur HTTP dans Small Business Server 2003 (Standard ou Premium)](https://technet.microsoft.com/library/bb123622%28EXCHG.65%29.aspx). Si vous ne parvenez pas à exécuter correctement l'outil de migration Office 365 après avoir activé RPC sur HTTP, examinez le paramètre **ValidPorts** dans le Registre au niveau de HKEY_LOCAL_MACHINE\Software\Microsoft\Rpc\RpcProxy et vérifiez que le nom de domaine complet pour le serveur source est répertorié. Si le nom de domaine complet n'est pas répertorié, ajoutez-le manuellement comme dans l'exemple suivant :  
>   
>  remote. *contoso*.com:6001-6002;remote. *contoso*.com:6004 (remplacez *contoso* par le nom de votre domaine).  

#### <a name="migrate-email-to-another-on-premises-exchange-server"></a>Migrer la messagerie électronique vers un autre serveur Exchange local  
 Pour plus d’informations sur la migration de la messagerie électronique vers un autre serveur Exchange local, consultez [intégrer un serveur Exchange local à Windows Server Essentials](https://technet.microsoft.com/library/jj200172.aspx). Nous vous recommandons de configurer le nouveau serveur Exchange local après l’installation de Windows Server Essentials, puis de terminer la migration de la messagerie électronique avant de rétrograder le serveur source.  

> [!NOTE]
>  Le connecteur POP3 Windows Small Business Server n’est pas inclus avec Exchange Server. Après avoir migré les données de messagerie vers un autre serveur Exchange, vous ne pouvez plus utiliser la fonctionnalité du connecteur POP3.  

> [!NOTE]
>  Une fois votre serveur source mis à jour et préparé pour la migration, vous devez créer une sauvegarde du serveur mis à jour avant de poursuivre le processus de migration.  

## <a name="next-steps"></a>Étapes suivantes :  
 Vous avez préparé votre serveur source pour la migration vers Windows Server Essentials.  Passez maintenant à [l’étape 2 : installer Windows Server Essentials en tant que nouveau contrôleur de domaine réplica](Step-2--Install-Windows-Server-Essentials-as-a-new-replica-domain-controller.md).  

Pour afficher toutes les étapes, consultez [migrer vers Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

