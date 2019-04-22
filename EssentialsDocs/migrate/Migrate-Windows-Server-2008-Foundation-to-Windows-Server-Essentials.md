---
title: Migrer Windows Server 2008 Foundation vers Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f22fc0a4-cb82-4e60-afe6-2d03145745e7
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 22721833d3ba97c7860c949764d565415bbc7cab
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813760"
---
# <a name="migrate-windows-server-2008-foundation-to-windows-server-essentials"></a>Migrer Windows Server 2008 Foundation vers Windows Server Essentials

>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Ce guide décrit comment migrer un domaine Windows Server 2008 Foundation existant vers Windows Server® 2012 Essentials sur un nouveau matériel, puis comment migrer les paramètres et données. Ce guide décrit également comment supprimer votre serveur existant à partir du réseau de Windows Server Essentials une fois que vous avez terminé la migration.  
  
> [!NOTE]
>  Pour éviter tout problème pendant la migration, l’équipe de développement de produit Windows Server Essentials vous conseille vivement de lire ce document avant de commencer la migration.  
  
## <a name="additional-resources"></a>Ressources supplémentaires  
 Pour obtenir des liens vers des informations, des outils et des ressources de communauté supplémentaires qui peuvent vous guider lors du processus de migration, consultez [Migration vers Windows Small Business Server](https://go.microsoft.com/fwlink/?LinkId=217520).  
  
## <a name="terms-and-definitions"></a>Termes et définitions  
 **Serveur source :** serveur existant à partir duquel vous migrez vos paramètres et données.  
  
 **Serveur de destination :** nouveau serveur vers lequel vous migrez vos paramètres et données.  
  
## <a name="migration-process-summary"></a>Résumé du processus de migration  
 Ce guide de migration comporte les étapes suivantes :  
  

1.  [Préparer la migration de votre serveur Source pour Windows Server Essentials](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md).  Vous devez vérifier que le serveur source et le réseau sont prêts pour la migration. Cette section vous guide lors de la sauvegarde du serveur source, de l’évaluation de l’intégrité du système du serveur source, de l’installation des Service Packs et correctifs les plus récents, et de la vérification de la configuration réseau.  
  
2.  [Installer Windows Server Essentials en mode migration](Install-Windows-Server-Essentials-in-migration-mode.md).  Cette section décrit les étapes à suivre pour installer Windows Server Essentials sur le serveur de Destination en mode migration.  
  
3.  [Joindre des ordinateurs au nouveau réseau Windows Server Essentials](Join-computers-to-the-new-Windows-Server-Essentials-network.md).  Cette section traite de jonction des ordinateurs clients au nouveau réseau Windows Server Essentials et de mise à jour des paramètres de stratégie de groupe.  
  
4.  [Déplacer les paramètres de Windows Server 2008 Foundation et les données vers le serveur de Destination](Move-Windows-Server-2008-Foundation-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  Cette section fournit des informations sur la migration des paramètres et données à partir du serveur source.  
  
5.  [Rétrograder et supprimer le serveur Source du nouveau réseau Windows Server Essentials](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md).  Avant de supprimer le serveur source du réseau, vous devez imposer une mise à jour de stratégie de groupe et rétrograder le serveur source.  
  
6.  [Effectuer des tâches de post-migration pour la migration de Windows Server Essentials](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md).  Une fois que vous avez terminé la migration de tous les paramètres et données vers Windows Server Essentials, vous souhaiterez mapper les ordinateurs autorisés aux comptes d’utilisateur.  
  
7.  [Exécuter Windows Server Essentials Best Practices Analyzer](Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md).  Une fois que vous avez terminé la migration des paramètres et données vers Windows Server Essentials, vous devez exécuter l’outil BPA de Windows Server Essentials.  

1.  [Préparer la migration de votre serveur Source pour Windows Server Essentials](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md).  Vous devez vérifier que le serveur source et le réseau sont prêts pour la migration. Cette section vous guide lors de la sauvegarde du serveur source, de l’évaluation de l’intégrité du système du serveur source, de l’installation des Service Packs et correctifs les plus récents, et de la vérification de la configuration réseau.  
  
2.  [Installer Windows Server Essentials en mode migration](../migrate/Install-Windows-Server-Essentials-in-migration-mode.md).  Cette section décrit les étapes à suivre pour installer Windows Server Essentials sur le serveur de Destination en mode migration.  
  
3.  [Joindre des ordinateurs au nouveau réseau Windows Server Essentials](../migrate/Join-computers-to-the-new-Windows-Server-Essentials-network.md).  Cette section traite de jonction des ordinateurs clients au nouveau réseau Windows Server Essentials et de mise à jour des paramètres de stratégie de groupe.  
  
4.  [Déplacer les paramètres de Windows Server 2008 Foundation et les données vers le serveur de Destination](../migrate/Move-Windows-Server-2008-Foundation-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  Cette section fournit des informations sur la migration des paramètres et données à partir du serveur source.  
  
5.  [Rétrograder et supprimer le serveur Source du nouveau réseau Windows Server Essentials](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md).  Avant de supprimer le serveur source du réseau, vous devez imposer une mise à jour de stratégie de groupe et rétrograder le serveur source.  
  
6.  [Effectuer des tâches de post-migration pour la migration de Windows Server Essentials](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md).  Une fois que vous avez terminé la migration de tous les paramètres et données vers Windows Server Essentials, vous souhaiterez mapper les ordinateurs autorisés aux comptes d’utilisateur.  
  
7.  [Exécuter Windows Server Essentials Best Practices Analyzer](../migrate/Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md).  Une fois que vous avez terminé la migration des paramètres et données vers Windows Server Essentials, vous devez exécuter l’outil BPA de Windows Server Essentials.  

  
 Pour certaines procédures de migration, vous devez ouvrir une fenêtre d'invite de commandes en tant qu'administrateur.  
  
###  <a name="BKMK_OpenACommandPromptAsAdmin"></a> Pour ouvrir une fenêtre d’invite de commandes sur le serveur Source en tant qu’administrateur  
  
1.  Cliquez sur **Démarrer**.  
  
2.  Dans la zone de recherche, tapez **cmd**.  
  
3.  Dans la liste de résultats, cliquez avec le bouton droit sur **cmd**, puis cliquez sur **Exécuter en tant qu’administrateur**.  
  
#### <a name="to-open-a-command-prompt-window-on-the-destination-server-as-an-administrator"></a>Pour ouvrir une fenêtre d'invite de commandes sur le serveur de destination en tant qu'administrateur  
  
1.  Dans l'écran d' **accueil** , dans la zone de recherche, tapez **cmd**.  
  
2.  Dans la liste de résultats, cliquez avec le bouton droit sur **cmd**, puis cliquez sur **Exécuter en tant qu’administrateur**.
