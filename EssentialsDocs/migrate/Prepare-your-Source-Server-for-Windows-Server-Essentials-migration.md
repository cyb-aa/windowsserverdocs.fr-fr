---
title: Préparer votre serveur source pour Windows Server Essentials migration1
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f5861ae9-77cb-4d37-b4c5-8f0757213385
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 503b8edc645b43da1dc5c5fb37547e8e0245d4a2
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318816"
---
# <a name="prepare-your-source-server-for-windows-server-essentials-migration1"></a>Préparer votre serveur source pour Windows Server Essentials migration1

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Effectuez les étapes préliminaires suivantes afin de vous assurer que les paramètres et les données sur votre serveur source migrent correctement vers le serveur de destination.  
  
#### <a name="to-prepare-for-migration"></a>Pour préparer la migration  
  

1.  [Sauvegarder votre serveur source](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_BackUpYourSourceServerToPrepareForMigration)  
  
2.  [Installer les service packs les plus récents](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_InstallTheMostRecentServicePacksToPrepareForMigration)  
  
3.  [Évaluer l’intégrité du serveur source](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_UseWindowsBestPracticeAnalyzer)  
  
4.  [Exécuter l’outil de préparation de la migration sur le serveur source](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_MPT)  
  
5.  [Créer un plan pour migrer des applications métier](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_PlanToMigrateLineOfBusinessApplications)  

1.  [Sauvegarder votre serveur source](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_BackUpYourSourceServerToPrepareForMigration)  
  
2.  [Installer les service packs les plus récents](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_InstallTheMostRecentServicePacksToPrepareForMigration)  
  
3.  [Évaluer l’intégrité du serveur source](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_UseWindowsBestPracticeAnalyzer)  
  
4.  [Exécuter l’outil de préparation de la migration sur le serveur source](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_MPT)  
  
5.  [Créer un plan pour migrer des applications métier](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_PlanToMigrateLineOfBusinessApplications)  

  
###  <a name="back-up-your-source-server"></a><a name="BKMK_BackUpYourSourceServerToPrepareForMigration"></a>Sauvegarder votre serveur source  
 Sauvegardez votre serveur source avant de commencer le processus de migration. L'exécution d'une sauvegarde permet de protéger vos données de toute perte accidentelle si une erreur irrécupérable survient lors de la migration.  
  
##### <a name="to-back-up-the-source-server"></a>Pour sauvegarder le serveur source  
  
1.  Effectuez une sauvegarde complète du serveur source. Pour plus d’informations sur la sauvegarde de Windows Small Business Server 2011 Essentials, consultez [en savoir plus sur la configuration de la sauvegarde du serveur](https://technet.microsoft.com/library/server-backup-support-1.aspx).  
  
2.  Vérifiez que la sauvegarde a été effectuée correctement. Pour tester l’intégrité de la sauvegarde, sélectionnez des fichiers aléatoires de la sauvegarde, restaurez-les à un autre emplacement, puis vérifiez que les fichiers restaurés sont identiques aux fichiers d’origine.  
  
###  <a name="install-the-most-recent-service-packs"></a><a name="BKMK_InstallTheMostRecentServicePacksToPrepareForMigration"></a>Installer les service packs les plus récents  
 Vous devez installer les mises à jour et les Service Packs les plus récents sur le serveur source avant d'effectuer la migration.  
  
###  <a name="evaluate-the-health-of-the-source-server"></a><a name="BKMK_UseWindowsBestPracticeAnalyzer"></a>Évaluer l’intégrité du serveur source  
 Il est important d'évaluer l'intégrité de votre serveur source avant de commencer la migration. Utilisez les procédures suivantes pour vous assurer que les mises à jour sont actuelles, pour générer un rapport d'intégrité du système et pour exécuter Windows Server Solutions Best Practice Analyzer (BPA).  
  
#### <a name="download-and-install-critical-and-security-updates"></a>Télécharger et installer les mises à jour critiques et de sécurité  
 L'installation des mises à jour critiques et de sécurité sur le serveur source permet d'assurer la réussite de votre migration et contribue à protéger votre réseau pendant le processus de migration.  
  
###### <a name="to-check-for-the-latest-updates"></a>Pour rechercher les dernières mises à jour  
  
1.  Sur le serveur source, cliquez sur **Démarrer**, sur **Tous les programmes**, puis sur **Windows Update**.  
  
2.  Cliquez sur **Rechercher les mises à jour**.  
  
3.  Si des mises à jour sont trouvées, cliquez sur **Installer les mises à jour**.  
  
#### <a name="check-the-alert-viewer-for-critical-errors"></a>Rechercher les erreurs critiques dans l'Afficheur des alertes  
 Vous pouvez examiner l'Afficheur des alertes dans le tableau de bord pour rechercher les erreurs critiques.  
  
#### <a name="run-the-windows-server-solutions-best-practices-analyzer"></a>Exécutez Windows Server Solutions Best Practices Analyzer  
 Vous pouvez exécuter Windows Server Solutions Best Practices Analyzer (BPA) pour vérifier qu'il n'y a aucun problème sur votre serveur, réseau ou domaine avant de commencer le processus de migration. L'analyseur BPA collecte les informations de configuration auprès des sources suivantes :  
  
-   WMI (Windows Management Instrumentation) Active Directory®  
  
-   Le Registre  
  
-   Métabase des services Internet (IIS)  
  
###### <a name="to-use-the-windows-server-solutions-bpa-to-analyze-your-source-server"></a>Pour utiliser l'outil BPA des solutions Windows Server afin d'analyser votre serveur source  
  
1. Téléchargez et installez [Windows Server Solutions Best Practices Analyzer](https://www.microsoft.com/download/details.aspx?id=15556) à partir du Centre de téléchargement Microsoft.  
  
2. Une fois le téléchargement terminé, cliquez sur **Démarrer**, pointez sur **Tous les programmes**, puis cliquez sur **Outil Best Practices Analyzer SBS**.  
  
   > [!NOTE]
   >  Recherchez les mises à jour avant d’analyser le serveur.  
  
3. Dans le volet de navigation, cliquez sur **Démarrer une analyse**.  
  
4. Dans le volet d'informations, tapez le libellé de l'analyse, puis cliquez sur **Démarrer l'analyse**. Le libellé d’analyse correspond au nom du rapport d’analyse (**SBS BPA Scan 1Jul2012**, par exemple).  
  
5. Une fois l’analyse terminée, cliquez sur **Afficher un rapport de cette analyse Best Practices**.  
  
   Une fois la collecte d'informations sur la configuration du serveur effectuée, l'outil BPA des solutions Windows Server vérifie que les informations sont correctes et présente ensuite aux administrateurs la liste des informations et des problèmes classés par niveau de gravité. La liste décrit chaque problème et fournit une recommandation ou une solution possible. Trois types de rapports sont disponibles :  
  
|Type de rapport|Description|  
|-----------------|-----------------|  
|Rapports de liste|Affiche les rapports dans une liste unidimensionnelle.|  
|Rapports d’arborescence|Affiche les rapports dans une liste hiérarchique.|  
|Autres rapports|Affiche des rapports tels qu’un journal des exécutions.|  
  
 Pour afficher la description et les solutions relatives à un problème, cliquez sur le problème dans le rapport. Tous les problèmes signalés par Windows SBS 2011 Essentials BPA n’affectent pas la migration, mais vous devez résoudre autant de problèmes que possible pour garantir la réussite de la migration.  
  
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
  
###  <a name="run-the-migration-preparation-tool-on-the-source-server"></a><a name="BKMK_MPT"></a>Exécuter l’outil de préparation de la migration sur le serveur source  
 Avant d'effectuer une installation en mode de migration, vous devez exécuter l'outil de préparation de la migration sur le serveur source. Cet outil est conçu pour préparer le serveur source et le domaine à migrer vers Windows Server Essentials.  
  
> [!IMPORTANT]
>  Sauvegardez votre serveur source avant d’exécuter l'outil de préparation de la migration. Toutes les modifications que l'outil de préparation de la migration effectue dans le schéma sont irréversibles. Si vous rencontrez des problèmes au cours de la migration, la seule manière de restaurer l’état que le serveur source avait avant l'exécution de l'outil est de restaurer le serveur depuis une sauvegarde du système.  
  
 Pour exécuter l’outil de préparation de la migration, vous devez être membre du groupe Administrateurs de l’entreprise, du groupe Administrateurs du schéma et du groupe Admins du domaine.  
  
##### <a name="to-verify-that-you-have-the-appropriate-permissions-to-run-the-tool-on-the-source-server"></a>Pour vérifier que vous disposez des autorisations appropriées pour exécuter l'outil sur le serveur source  
  
1. Sur le serveur source, cliquez sur **Démarrer**, **Outils d’administration**, puis sur **Utilisateurs et ordinateurs Active Directory**.  
  
2. Dans l'arborescence de la console, développez votre domaine, puis cliquez sur **Utilisateurs**.  
  
3. Cliquez avec le bouton droit de la souris sur le compte d’administrateur que vous utilisez pour la migration, puis cliquez sur **Propriétés**.  
  
4. Cliquez sur l’onglet **Membre de**, puis vérifiez que les groupes Administrateurs d’entreprise, Administrateurs du schéma et Admins du domaine sont répertoriés dans la zone de texte **Membre de**.  
  
5. Si les groupes ne sont pas répertoriés, cliquez sur **Ajouter**, puis ajoutez chaque groupe qui n’est pas répertorié.  
  
   > [!NOTE]
   > - Il se peut que vous receviez une notification d'erreur d'autorisation d'accès si le service Netlogon n'est pas démarré.  
   >   -   Vous devez vous déconnecter du serveur et vous reconnecter pour que les modifications prennent effet.  
  
    Vous pouvez utiliser la dernière version de l'Agent de mise à jour automatique Windows Update pour vous assurer que le processus de mise à jour du serveur fonctionne correctement.  
  
   Vous pouvez utiliser la dernière version de l'Agent de mise à jour automatique Windows Update pour vous assurer que le processus de mise à jour du serveur fonctionne correctement.  
  
   Avant de pouvoir installer Windows Update Agent sur le serveur source, vous devez d’abord installer Windows PowerShell 2,0 et Microsoft Baseline Configuration Analyzer 2,0.  
  
-   Pour télécharger et installer Windows PowerShell 2,0, voir l' [article 968929](https://go.microsoft.com/fwlink/p/?LinkId=241483) de la base de connaissances Microsoft.  
  
-   Pour télécharger et installer Microsoft Baseline Configuration Analyzer 2,0, voir [Microsoft Baseline Configuration analyzer 2,0](https://go.microsoft.com/fwlink/p/?LinkId=241484) dans le centre de téléchargement Microsoft.  
  
-   Pour télécharger et installer la dernière version de l’Agent Windows Update, consultez l’[article 949104](https://go.microsoft.com/fwlink/p/?LinkId=237493) de la Base de connaissances Microsoft.  
  
##### <a name="to-install-and-run-the-migration-preparation-tool-on-the-source-server"></a>Pour installer et exécuter l'outil de préparation de la migration sur le serveur source.  
  
1. Insérez Windows Server Essentials le DVD1 dans le lecteur de DVD sur le serveur source.  
  
2. Ouvrez l'Explorateur Windows, accédez au dossier **\support\tools** du DVD, puis double-cliquez sur le fichier **sourcetool.msi**.  
  
   > [!NOTE]
   > - Si l'outil de préparation de la migration est déjà installé sur le serveur, exécutez-le à partir du menu **Démarrer**.  
   >   -   Afin de vous assurer d'obtenir le meilleur environnement de migration possible, il est recommandé de toujours choisir d’installer la mise à jour la plus récente.  
  
    L’Assistant installe l’outil de préparation de la migration sur le serveur source. Lorsque l’installation est terminée, l’outil de préparation de la migration s’exécute automatiquement et installe les dernières mises à jour.  
  
3. Dans l'Outil de préparation de la migration, sélectionnez **J'ai une sauvegarde et suis prêt à continuer**, puis cliquez sur **Suivant**.  
  
   > [!WARNING]
   >  Si vous recevez un message d’erreur relatif à l’installation d’un correctif, reportez-vous à la rubrique méthode 2 : renommer le dossier Catroot2 de l' [article 822798](https://go.microsoft.com/FWLink/p/?LinkID=118672) de la base de connaissances Microsoft.  
  
    L'outil de préparation de la migration prépare le domaine source pour la migration en étendant le schéma Active Directory. Une fois la tâche terminée, cliquez sur **Suivant** pour continuer.  
  
4. Une fois la préparation du domaine source effectuée, l'outil de préparation de la migration analyse le serveur source afin d'identifier deux types de problèmes potentiels.  
  
   - **Erreurs** Problèmes détectés sur le serveur source qui peuvent bloquer la migration ou provoquer l’échec de la migration. Suivez les instructions figurant dans le message d'erreur afin de corriger les erreurs, puis cliquez sur **Relancer l'analyse**.  
  
   - **Avertissements** Problèmes détectés sur le serveur source qui peuvent provoquer des problèmes fonctionnels pendant la migration. Il est fortement recommandé de suivre les instructions figurant dans le message d'erreur afin de résoudre les problèmes avant la migration.  
  
     Une fois tous les problèmes reconnus et corrigés, cliquez sur **Suivant**.  
  
5. Dans l'outil de préparation de la migration, cliquez sur **Terminer**.  
  
6. Une fois l’exécution de l’outil de préparation de la migration terminée, vous pouvez être invité à redémarrer le serveur source avant de commencer la migration vers Windows Server Essentials.  
  
> [!NOTE]
>  Vous devez effectuer une exécution réussie de l’outil de préparation de la migration sur le serveur source dans les deux semaines qui suivent l’installation de Windows Server Essentials sur le serveur de destination. Dans le cas contraire, l’installation de Windows Server Essentials sur le serveur de destination sera bloquée. Si tel est le cas, vous devez exécuter de nouveau l'outil de préparation de la migration sur le serveur source.  
  
###  <a name="create-a-plan-to-migrate-line-of-business-applications"></a><a name="BKMK_PlanToMigrateLineOfBusinessApplications"></a>Créer un plan pour migrer des applications métier  
 Une application métier est une application informatique critique vitale pour l'activité d'une entreprise. Les applications métiers incluent la gestion des comptes, la gestion de la chaîne logistique, ainsi que la planification des ressources.  
  
 Lorsque vous prévoyez de migrer vos applications métiers, consultez les fournisseurs d’applications métiers afin de définir la meilleure méthode de migration de chaque application. Vous devez également rechercher le support utilisé pour réinstaller les applications métiers sur le serveur de destination.  
  
> [!NOTE]
>  Si vous avez utilisé le kit de développement logiciel (SDK) Windows Small Business Server 2011 Essentials pour développer un complément d’intégrité système ou d’alerte personnalisé et que vous souhaitez continuer à utiliser le complément avec Windows Server Essentials, vous devez également mettre à jour le complément et le déployer sur le serveur de destination.  
  
