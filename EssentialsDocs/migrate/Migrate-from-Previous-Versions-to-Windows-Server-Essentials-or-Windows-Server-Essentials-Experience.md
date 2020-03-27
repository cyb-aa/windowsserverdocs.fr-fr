---
title: Migrer vers Windows Server Essentials ou Windows Server Essentials Experience depuis des versions antérieures
description: Décrit comment utiliser Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 2974fb3a-5150-43fd-a73f-3e5074eb5d03
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 55b31785de6e17232a717d534fcb21a24d9052bd
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318898"
---
# <a name="migrate-from-previous-versions-to-windows-server-essentials-or-windows-server-essentials-experience"></a>Migrer vers Windows Server Essentials ou Windows Server Essentials Experience depuis des versions antérieures

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Ce guide explique comment effectuer une migration à partir de versions antérieures de Windows Small Business Server et Windows Server Essentials (y compris Windows Server Essentials, Windows Small Business Server 2011 standard, Windows Small Business Server 2011 Essentials, Windows Small Business Server 2008 et Windows Small Business Server 2003) vers Windows Server Essentials ou vers Windows Server 2012 R2 avec le rôle expérience Windows Server Essentials installé.  
  
 **Pour les environnements comprenant jusqu’à 25 utilisateurs et 50 appareils**, vous pouvez suivre les étapes de ce guide pour effectuer une migration à partir de versions précédentes de Windows SBS vers Windows Server Essentials.  
  
 **Pour les environnements comprenant jusqu’à 100 utilisateurs et 200 appareils**, vous pouvez suivre les mêmes instructions pour migrer vers les éditions standard ou Datacenter de windows server 2012 R2 avec le rôle expérience Windows Server Essentials installé.  
  
> [!NOTE]
>  Pour éviter des problèmes pendant la migration, l'équipe de développement produit Windows Server Essentials vous conseille vivement de lire ce document avant de commencer la migration.  
  
## <a name="terms-and-definitions"></a>Termes et définitions  
 **Serveur source :** serveur existant à partir duquel vous migrez les paramètres et les données.  
  
 **Serveur de destination :** nouveau serveur vers lequel vous migrez les paramètres et les données.  
  
## <a name="migration-process-summary"></a>Résumé du processus de migration  
 Ce guide de migration comporte les étapes suivantes :  
  
1. [Étape 1 : préparer votre serveur source pour la migration vers Windows Server Essentials](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md).  Vous devez vérifier que le serveur source et le réseau sont prêts pour la migration. Cette section vous guide lors de la sauvegarde du serveur source, de l’évaluation de l’intégrité du système du serveur source, de l’installation des Service Packs et correctifs les plus récents, et de la vérification de la configuration réseau.  
  
2. [Étape 2 : installez Windows Server Essentials en tant que nouveau contrôleur de domaine réplica](Step-2--Install-Windows-Server-Essentials-as-a-new-replica-domain-controller.md). Cette section décrit comment installer Windows Server Essentials ou Windows Server 2012 R2 Standard (avec le rôle expérience Windows Server Essentials activé) en tant que contrôleur de domaine.  
  
3. [Étape 3 : joindre des ordinateurs au nouveau serveur Windows Server Essentials](Step-3--Join-computers-to-the-new-Windows-Server-Essentials-server.md).  Cette section explique comment joindre des ordinateurs clients au nouveau serveur exécutant Windows Server Essentials et comment mettre à jour des paramètres de stratégie de groupe.  
  
4. [Étape 4 : déplacer les paramètres et les données vers le serveur de destination pour la migration vers Windows Server Essentials](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  Cette section fournit des informations sur la migration des paramètres et données à partir du serveur source.  
  
5. [Étape 5 : activer la redirection de dossiers sur le serveur de destination pour la migration vers Windows Server Essentials](Step-5--Enable-folder-redirection-on-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  Si la redirection de dossiers est activée sur le serveur source, vous pouvez l’activer sur le serveur de destination, puis supprimer l’ancien paramètre Stratégie de groupe de redirection de dossier.  
  
6. [Étape 6 : rétrograder et supprimer le serveur source du nouveau réseau Windows Server Essentials](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md).  Avant de supprimer le serveur source du réseau, vous devez imposer une mise à jour de stratégie de groupe et rétrograder le serveur source.  
  
7. [Étape 7 : effectuez les tâches de postconnexion pour la migration de Windows Server Essentials](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md).  Après avoir terminé la migration de tous les paramètres et données vers Windows Server Essentials, vous souhaiterez peut-être mapper les ordinateurs autorisés aux comptes d’utilisateur.  
  
8. [Étape 8 : exécutez la Best Practices Analyzer Windows Server Essentials](Step-8--Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md).  Une fois que vous avez terminé la migration des paramètres et des données vers Windows Server Essentials, vous devez exécuter Windows Server Essentials Best Practices Analyzer (BPA).  
  
   Certaines des procédures de migration exigent que vous ouvriez une fenêtre d’invite de commandes en tant qu’administrateur. Les procédures suivantes expliquent comment effectuer cette opération.  
  
###  <a name="to-open-a-command-prompt-window-on-the-source-server-as-an-administrator"></a><a name="BKMK_OpenACommandPromptAsAdmin"></a>Pour ouvrir une fenêtre d’invite de commandes sur le serveur source en tant qu’administrateur  
  
1.  Cliquez sur **Démarrer**.  
  
2.  Dans la zone de recherche, tapez **cmd**.  
  
3.  Dans la liste de résultats, cliquez avec le bouton droit sur **cmd**, puis cliquez sur **Exécuter en tant qu’administrateur**.  
  
#### <a name="to-open-a-command-prompt-window-on-the-destination-server-as-an-administrator"></a>Pour ouvrir une fenêtre d’invite de commandes sur le serveur de destination en tant qu’administrateur  
  
1.  Dans l'écran d'**accueil**, dans la zone de recherche, tapez **cmd**.  
  
2.  Dans la liste de résultats, cliquez avec le bouton droit sur **cmd**, puis cliquez sur **Exécuter en tant qu’administrateur**.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Migrer les données du serveur vers Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Migrer les données du serveur vers Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

