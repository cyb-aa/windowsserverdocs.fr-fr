---
title: "Préparer votre serveur Source pour WindowsServerEssentials migration1"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f5861ae9-77cb-4d37-b4c5-8f0757213385
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 929c7506c78667646e429c4f28df7e5642c575ab
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="prepare-your-source-server-for-windows-server-essentials-migration1"></a>Préparer votre serveur Source pour WindowsServerEssentials migration1

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Effectuez les étapes préliminaires suivantes pour vous assurer que les paramètres et données sur votre serveur Source migrent correctement vers le serveur de Destination.  
  
#### <a name="to-prepare-for-migration"></a>Pour préparer la migration  
  

1.  [Sauvegardez votre serveur Source](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_BackUpYourSourceServerToPrepareForMigration)  
  
2.  [Installer les service packs plus récents](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_InstallTheMostRecentServicePacksToPrepareForMigration)  
  
3.  [Évaluer l’intégrité du serveur Source](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_UseWindowsBestPracticeAnalyzer)  
  
4.  [Exécuter l’outil de préparation de Migration sur le serveur Source](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_MPT)  
  
5.  [Créer un plan pour migrer des applications métier de](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_PlanToMigrateLineOfBusinessApplications)  

1.  [Sauvegardez votre serveur Source](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_BackUpYourSourceServerToPrepareForMigration)  
  
2.  [Installer les service packs plus récents](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_InstallTheMostRecentServicePacksToPrepareForMigration)  
  
3.  [Évaluer l’intégrité du serveur Source](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_UseWindowsBestPracticeAnalyzer)  
  
4.  [Exécuter l’outil de préparation de Migration sur le serveur Source](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_MPT)  
  
5.  [Créer un plan pour migrer des applications métier de](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_PlanToMigrateLineOfBusinessApplications)  

  
###  <a name="BKMK_BackUpYourSourceServerToPrepareForMigration"></a>Sauvegardez votre serveur Source  
 Sauvegardez votre serveur Source avant de commencer le processus de migration. Effectuer une sauvegarde permet de protéger vos données contre toute perte accidentelle si une erreur irrécupérable se produit lors de la migration.  
  
##### <a name="to-back-up-the-source-server"></a>Pour sauvegarder le serveur Source  
  
1.  Effectuer une sauvegarde complète du serveur Source. Pour plus d’informations sur la sauvegarde de WindowsSmallBusinessServer2011Essentials, consultez [en savoir plus sur la définition de la sauvegarde du serveur](https://technet.microsoft.com/library/server-backup-support-1.aspx).  
  
2.  Vérifiez que la sauvegarde a été correctement exécuté. Pour tester l’intégrité de la sauvegarde, sélectionnez aléatoirement des fichiers de la sauvegarde, restaurez-les à un autre emplacement, puis vérifiez que les fichiers restaurés sont identiques aux fichiers d’origine.  
  
###  <a name="BKMK_InstallTheMostRecentServicePacksToPrepareForMigration"></a>Installer les service packs plus récents  
 Vous devez installer les dernières mises à jour et les service packs sur le serveur Source avant la migration.  
  
###  <a name="BKMK_UseWindowsBestPracticeAnalyzer"></a>Évaluer l’intégrité du serveur Source  
 Il est important d’évaluer l’intégrité de votre serveur Source avant de commencer la migration. Utilisez les procédures suivantes pour vous assurer que les mises à jour sont actuelles, pour générer un rapport d’intégrité système et pour exécuter le WindowsServerSolutionsBest pratique Analyzer (BPA).  
  
#### <a name="download-and-install-critical-and-security-updates"></a>Téléchargement et installation critiques et sécurité des mises à jour  
 L’installation critiques et sécurité des mises à jour sur le serveur Source permet de s’assurer que votre migration sera réussie et contribue à protéger votre réseau pendant le processus de migration.  
  
###### <a name="to-check-for-the-latest-updates"></a>Pour vérifier les dernières mises à jour  
  
1.  À partir du serveur Source, cliquez sur **Démarrer**, cliquez sur **tous les programmes**, puis cliquez sur **mise à jour Windows**.  
  
2.  Cliquez sur **rechercher les mises à jour**.  
  
3.  Si les mises à jour sont trouvées, cliquez sur **installer les mises à jour**.  
  
#### <a name="check-the-alert-viewer-for-critical-errors"></a>Vérification de l’Afficheur des alertes pour les erreurs critiques  
 Vous pouvez vérifier l’Afficheur des alertes sur le tableau de bord pour rechercher les erreurs critiques.  
  
#### <a name="run-the-windows-server-solutions-best-practices-analyzer"></a>Exécuter WindowsServerSolutionsBestPractices Analyzer  
 Vous pouvez exécuter le WindowsServerSolutionsBestPractices Analyzer (BPA) pour vérifier qu’il n’y a aucun problème sur votre serveur, réseau ou domaine avant de commencer le processus de migration. L’analyseur BPA collecte les informations de configuration à partir des sources suivantes:  
  
-   Directory® active WindowsManagementInstrumentation (WMI)  
  
-   Le Registre  
  
-   La métabase Internet Information Services (IIS)  
  
###### <a name="to-use-the-windows-server-solutions-bpa-to-analyze-your-source-server"></a>Pour utiliser l’outil BPA des Solutions Windows Server pour analyser votre serveur Source  
  
1.  Téléchargez et installez le [WindowsServerSolutionsBestPractices Analyzer](https://www.microsoft.com/en-us/download/details.aspx?id=15556) à du MicrosoftDownload Center.  
  
2.  Une fois le téléchargement terminé, cliquez sur **Démarrer**, pointez sur **tous les programmes**, puis cliquez sur **outil SBS Best Practices Analyzer**.  
  
    > [!NOTE]
    >  Recherchez les mises à jour avant d’analyser le serveur.  
  
3.  Dans le volet de navigation, cliquez sur **démarrer une analyse**.  
  
4.  Dans le volet d’informations, tapez l’étiquette d’analyse, puis cliquez sur **numérisations**. L’étiquette d’analyse est le nom du rapport d’analyse, par exemple, **SBS BPA Scan 1 Jul2012**.  
  
5.  Une fois l’analyse terminée, cliquez sur **afficher un rapport de cette analyse Best Practices**.  
  
 Une fois la collecte d’informations sur la configuration du serveur, l’outil BPA des Solutions Windows Server vérifie que les informations sont correctes et présente ensuite aux administrateurs avec une liste des informations et problèmes triés par niveau de gravité. La liste décrit chaque problème et fournit une recommandation ou une solution possible. Trois types de rapports sont disponibles:  
  
|Type de rapport|Description|  
|-----------------|-----------------|  
|Liste des rapports|Affiche les rapports dans une liste unidimensionnelle.|  
|Rapports d’arborescence|Affiche les rapports dans une liste hiérarchique.|  
|Autres rapports|Affiche les rapports tels qu’un journal d’exécution.|  
  
 Pour afficher la description et les solutions d’un problème, cliquez sur le problème dans le rapport. Migration d’affectent pas tous les problèmes signalés par l’outil BPA Windows SBS2011Essentials, mais vous devez résoudre autant de problèmes que possible pour garantir la réussite de la migration.  
  
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
  
###  <a name="BKMK_MPT"></a>Exécuter l’outil de préparation de Migration sur le serveur Source  
 Vous ne pouvez pas effectuer une installation en mode de migration sans exécuter d’abord l’outil de préparation de Migration sur le serveur Source. Cet outil est conçu pour préparer votre serveur Source et le domaine à migrer vers WindowsServerEssentials.  
  
> [!IMPORTANT]
>  Sauvegardez votre serveur Source avant d’exécuter l’outil de préparation de Migration. Toutes les modifications de l’outil de préparation de Migration effectue dans le schéma sont irréversibles. Si vous rencontrez des problèmes lors de la migration, la seule façon rétablir l’état du serveur Source qu'avait avant l’exécution de l’outil consiste à restaurer le serveur à partir d’une sauvegarde du système.  
  
 Pour exécuter l’outil de préparation de Migration, vous devez être membre du groupe Administrateurs de l’entreprise, le groupe Administrateurs du schéma et Admins du domaine.  
  
##### <a name="to-verify-that-you-have-the-appropriate-permissions-to-run-the-tool-on-the-source-server"></a>Pour vérifier que vous disposez des autorisations appropriées pour exécuter l’outil sur le serveur Source  
  
1.  Sur le serveur Source, cliquez sur **Démarrer**, cliquez sur **outils d’administration**, puis cliquez sur **ActiveDirectory Users and Computers**.  
  
2.  Dans l’arborescence de la console, développez votre domaine, puis cliquez sur **utilisateurs**.  
  
3.  Cliquez sur le compte administrateur que vous utilisez pour la migration, puis cliquez sur **propriétés**.  
  
4.  Cliquez sur le **membre de** onglet et vérifiez que les administrateurs de l’entreprise, administrateurs du schéma et Admins du domaine sont répertorié dans le **membre** zone de texte.  
  
5.  Si les groupes ne sont pas répertoriés, cliquez sur **ajouter**, puis ajoutez chaque groupe qui n’est pas répertorié.  
  
    > [!NOTE]
    >  -   Vous pouvez recevoir une erreur d’autorisation si le service Netlogon n’est pas démarré.  
    > -   Vous devez fermer la session et ouvrez une session sur le serveur pour que les modifications prennent effet.  
  
     Vous pouvez utiliser la dernière version de l’Agent Windows Update pour vous assurer que le processus de mise à jour serveur fonctionne correctement.  
  
 Vous pouvez utiliser la dernière version de l’Agent Windows Update pour vous assurer que le processus de mise à jour serveur fonctionne correctement.  
  
 Vous pouvez installer l’Agent Windows Update sur le serveur Source, vous devez d’abord installer WindowsPowerShell2.0 et MicrosoftBaseline Configuration Analyzer 2.0.  
  
-   Pour télécharger et installer WindowsPowerShell2.0, voir [article 968929](https://go.microsoft.com/fwlink/p/?LinkId=241483) dans la Base de connaissances Microsoft.  
  
-   Pour télécharger et installer MicrosoftBaseline Configuration Analyzer 2.0, voir la [MicrosoftBaseline Configuration Analyzer 2.0](https://go.microsoft.com/fwlink/p/?LinkId=241484) à du MicrosoftDownload Center.  
  
-   Pour télécharger et installer la dernière version de l’Agent de mise à jour de Windows, voir [article 949104](https://go.microsoft.com/fwlink/p/?LinkId=237493) dans la Base de connaissances Microsoft.  
  
##### <a name="to-install-and-run-the-migration-preparation-tool-on-the-source-server"></a>Pour installer et exécuter l’outil de préparation de Migration sur le serveur Source  
  
1.  Insérez le DVD1 de WindowsServerEssentials dans le lecteur de DVD sur le serveur Source.  
  
2.  Ouvrez l’Explorateur Windows, accédez à la **\support\tools** dossier du DVD, puis double-cliquez sur le **sourcetool.msi** fichier.  
  
    > [!NOTE]
    >  -   Si l’outil de préparation de Migration est déjà installé sur le serveur, exécutez l’outil à partir de la **Démarrer** menu.  
    > -   Pour vous assurer que vous êtes prêt pour la meilleure expérience de migration possibles, il est recommandé de toujours choisir d’installer la mise à jour plus récente.  
  
     L’Assistant installe l’outil de préparation de Migration sur le serveur Source. Lorsque l’installation est terminée, l’outil de préparation de Migration s’exécute automatiquement et installe les dernières mises à jour.  
  
3.  Dans l’outil de préparation de Migration, sélectionnez **je disposer d’une sauvegarde et suis prêt à continuer**, puis cliquez sur **suivant**.  
  
    > [!WARNING]
    >  Si vous recevez un message d’erreur relatif à l’installation d’un correctif, voir la méthode 2: renommer le dossier Catroot2 dans [article 822798](https://go.microsoft.com/FWLink/p/?LinkID=118672) dans la Base de connaissances Microsoft.  
  
     L’outil de préparation de la Migration prépare le domaine source pour la migration en étendant le schéma ActiveDirectory. Une fois la tâche terminée, cliquez sur **suivant** pour continuer.  
  
4.  Après la préparation du domaine source, l’outil de préparation de la Migration analyse le serveur Source pour identifier les deux types de problèmes potentiels.  
  
    -   **Erreurs** problèmes détectés sur le serveur Source pouvant bloquer la migration ou provoquer son échec. Suivez les instructions dans le message d’erreur pour résoudre les problèmes, puis cliquez sur **relancer l’analyse**.  
  
    -   **Avertissements** problèmes détectés sur le serveur Source qui peut provoquer des problèmes fonctionnels pendant la migration. Il est fortement recommandé de suivre les instructions fournies dans le message d’erreur pour résoudre les problèmes avant de procéder à la migration.  
  
     Une fois reconnus et corrigés tous les problèmes, cliquez sur **suivant**.  
  
5.  Dans l’outil de préparation de Migration, cliquez sur **Terminer**.  
  
6.  Une fois l’outil de préparation de la Migration terminée, vous pouvez être invité à redémarrer le serveur Source avant de commencer la migration vers WindowsServerEssentials.  
  
> [!NOTE]
>  Vous devez effectuer une exécution réussie de l’outil de préparation de Migration sur le serveur Source dans les deux semaines suivant l’installation de WindowsServerEssentials sur le serveur de Destination. Dans le cas contraire, l’installation de WindowsServerEssentials sur le serveur de Destination sera bloquée. Si cela se produit, vous devez exécuter l’outil de préparation de Migration sur le serveur Source.  
  
###  <a name="BKMK_PlanToMigrateLineOfBusinessApplications"></a>Créer un plan pour migrer des applications métier de  
 Une application de (LOB) de métier est une application informatique critique vitale pour une entreprise. Applications métiers incluent la gestion de la comptabilité, la chaîne d’approvisionnement et la planification des ressources.  
  
 Lorsque vous envisagez de migrer vos applications cœur de métier, consultez les fournisseurs d’application cœur de métier pour déterminer la méthode appropriée pour chaque application. Vous devez également localiser le média qui est utilisé pour installer les applications cœur de métier sur le serveur de Destination.  
  
> [!NOTE]
>  Si WindowsSmallBusinessServer2011Essentials SDK vous permet de développer un complément d’intégrité système ou alerte et vous souhaitez continuer à utiliser ce complément avec WindowsServerEssentials, vous devez également mettre à jour le complément et le déployer sur le serveur de Destination.  
  
