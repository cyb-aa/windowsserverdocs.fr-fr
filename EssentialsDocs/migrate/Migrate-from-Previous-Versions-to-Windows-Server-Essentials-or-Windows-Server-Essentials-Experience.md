---
title: "Migrer des Versions antérieures à Windows Server Essentials ou expérience Windows Server Essentials"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/20116
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2974fb3a-5150-43fd-a73f-3e5074eb5d03
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 213ee4304d9d4ebdb7580f7f78fdaca78aa454c9
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="migrate-from-previous-versions-to-windows-server-essentials-or-windows-server-essentials-experience"></a>Migrer des Versions antérieures à Windows Server Essentials ou expérience Windows Server Essentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Ce guide décrit comment migrer à partir de versions précédentes de Windows Small Business Server et Windows Server Essentials (y compris Windows Server Essentials, Windows Small Business Server 2011 Standard, Windows Small Business Server 2011 Essentials, Windows Small Business Server 2008 et Windows Small Business Server 2003) vers Windows Server Essentials ou Windows Server 2012 R2 avec le rôle expérience Windows Server Essentials installé.  
  
 **Pour les environnements comptant jusqu'à 25 utilisateurs et 50 appareils**, vous pouvez suivre les étapes décrites dans ce guide pour migrer à partir de versions antérieures de Windows SBS vers Windows Server Essentials.  
  
 **Pour les environnements comptant jusqu'à 100 utilisateurs et 200 appareils**, vous pouvez suivre les mêmes recommandations pour migrer vers les éditions Standard ou Datacenter de Windows Server 2012 R2 avec le rôle expérience Windows Server Essentials installé.  
  
> [!NOTE]
>  Pour éviter des problèmes lors de la migration, l’équipe de développement de produit Windows Server Essentials vous recommande vivement de lire ce document avant de commencer la migration.  
  
## <a name="terms-and-definitions"></a>Termes et définitions  
 **Serveur source** serveur existant à partir duquel vous migrez vos paramètres et données.  
  
 **Serveur de destination** le nouveau serveur vers lequel vous migrez vos paramètres et données.  
  
## <a name="migration-process-summary"></a>Résumé du processus de migration  
 Ce guide de migration inclut les étapes suivantes:  
  
1.  [Étape 1: Préparer la migration de votre serveur Source pour Windows Server Essentials](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md).  Vous devez vous assurer que votre serveur Source et le réseau sont prêts pour la migration. Cette section vous guide par le biais de la sauvegarde du serveur Source, l’évaluation de l’intégrité du système de serveur Source, installer les service packs et les correctifs les plus récentes et vérification de la configuration réseau.  
  
2.  [Étape 2: Installer Windows Server Essentials en tant que nouveau contrôleur de domaine réplica](Step-2--Install-Windows-Server-Essentials-as-a-new-replica-domain-controller.md). Cette section décrit comment installer Windows Server Essentials ou Windows Server 2012 R2 Standard (avec le rôle expérience Windows Server Essentials activé) en tant qu’un contrôleur de domaine.  
  
3.  [Étape 3: Joindre des ordinateurs au nouveau serveur Windows Server Essentials](Step-3--Join-computers-to-the-new-Windows-Server-Essentials-server.md).  Cette section explique comment joindre des ordinateurs clients vers le nouveau serveur exécutant Windows Server Essentials et mise à jour les paramètres de stratégie de groupe.  
  
4.  [Étape 4: Déplacer les paramètres et données pour la migration du serveur de Destination pour Windows Server Essentials](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  Cette section fournit des informations sur la migration des données et paramètres à partir du serveur Source.  
  
5.  [Étape 5: Activer la redirection de dossiers sur la migration du serveur de Destination pour Windows Server Essentials](Step-5--Enable-folder-redirection-on-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  Si la redirection de dossiers est activée sur le serveur Source, vous pouvez activer la redirection de dossiers sur le serveur de Destination, puis supprimez l’ancien paramètre de stratégie de groupe de Redirection de dossiers.  
  
6.  [Étape 6: Rétrograder et supprimer le serveur Source du nouveau réseau Windows Server Essentials](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md).  Avant de pouvoir supprimer le serveur Source à partir du réseau, vous devez forcer une mise à jour de stratégie de groupe et rétrograder le serveur Source.  
  
7.  [Étape 7: Effectuer les tâches de post-migration pour la migration de Windows Server Essentials](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md).  Après avoir terminé la migration de tous les paramètres et données vers Windows Server Essentials, vous voudrez peut-être mapper les ordinateurs autorisés aux comptes d’utilisateurs.  
  
8.  [Étape 8: Exécuter Windows Server Essentials Best Practices Analyzer](Step-8--Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md).  Après avoir terminé la migration des paramètres et données vers Windows Server Essentials, vous devez exécuter le Windows Server Essentials Best Practices Analyzer (BPA).  
  
 Plusieurs des procédures de migration, vous devez ouvrir une fenêtre d’invite de commandes en tant qu’administrateur. Les procédures suivantes expliquent comment effectuer cette opération.  
  
###  <a name="BKMK_OpenACommandPromptAsAdmin"></a>Pour ouvrir une fenêtre d’invite de commandes sur le serveur Source en tant qu’administrateur  
  
1.  Cliquez sur **Démarrer**.  
  
2.  Dans la zone de recherche, tapez **cmd**.  
  
3.  Dans la liste des résultats, cliquez sur **cmd**, puis cliquez sur **exécuter en tant qu’administrateur**.  
  
#### <a name="to-open-a-command-prompt-window-on-the-destination-server-as-an-administrator"></a>Pour ouvrir une fenêtre d’invite de commandes sur le serveur de Destination en tant qu’administrateur  
  
1.  Sur le **Démarrer** écran, dans la zone de recherche, tapez **cmd**.  
  
2.  Dans la liste des résultats, cliquez sur **cmd**, puis cliquez sur **exécuter en tant qu’administrateur**.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Migrer les données de serveur vers Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Migrer les données de serveur vers Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

