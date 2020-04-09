---
title: Migrer Windows Small Business Server 2011 Standard vers Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: c8325f87-fd79-471b-bf70-3f052692c383
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d4f4dbb7063428aee7728071b8e62c5cc019b2af
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852492"
---
# <a name="migrate-windows-small-business-server-2011-standard-to-windows-server-essentials"></a>Migrer Windows Small Business Server 2011 Standard vers Windows Server Essentials

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Ce guide explique comment migrer un domaine Windows Small Business Server 2011 standard existant vers Windows Server&reg; 2012 Essentials sur un nouveau matériel, puis comment migrer les paramètres et les données. Ce guide explique également comment supprimer votre serveur existant du réseau Windows Server Essentials une fois la migration terminée.  
  
> [!NOTE]
>  Pour éviter des problèmes pendant la migration, l’équipe de développement de produit Windows Server Essentials vous conseille vivement de lire ce document avant de commencer la migration.  
> 
> [!NOTE]
> 
>  Pour migrer vos données de serveur vers la dernière version de Windows Server Essentials, consultez [migrer vers Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).  
> 
>  Pour migrer vos données de serveur vers la dernière version de Windows Server Essentials, consultez [migrer vers Windows Server Essentials](../migrate/Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).  

  
## <a name="additional-resources"></a>Ressources supplémentaires  
 Pour obtenir des liens vers des informations, des outils et des ressources de communauté supplémentaires qui peuvent vous guider lors du processus de migration, consultez [Migration vers Windows Small Business Server](https://go.microsoft.com/fwlink/?LinkId=217520).  
  
## <a name="terms-and-definitions"></a>Termes et définitions  
 **Serveur source :** Serveur existant à partir duquel vous migrez vos paramètres et données.  
  
 **Serveur de destination :** Nouveau serveur vers lequel vous migrez vos paramètres et données.  
  
## <a name="migration-process-summary"></a>Résumé du processus de migration  
 Ce guide de migration comporte les étapes suivantes :  
  

1.  [Préparez votre serveur source pour la migration vers Windows Server Essentials](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md).  Vous devez vérifier que le serveur source et le réseau sont prêts pour la migration. Cette section vous guide lors de la sauvegarde du serveur source, de l’évaluation de l’intégrité du système du serveur source, de l’installation des Service Packs et correctifs les plus récents, et de la vérification de la configuration réseau.  
  
2.  [Installez Windows Server Essentials en mode migration](Install-Windows-Server-Essentials-in-migration-mode.md).  Cette section décrit les étapes à suivre pour installer Windows Server Essentials sur le serveur de destination en mode migration.  
  
3.  [Joindre des ordinateurs au nouveau serveur Windows Server Essentials](Join-computers-to-the-new-Windows-Server-Essentials-server.md).  Cette section traite de la jonction des ordinateurs clients au nouveau réseau Windows Server Essentials et de la mise à jour des paramètres de stratégie de groupe.  
  
4.  [Déplacer les paramètres et données de SBS 2011 vers le serveur de destination](Move-Windows-SBS-2011-Standard-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  Cette section fournit des informations sur la migration des paramètres et données à partir du serveur source.  
  
5.  [Activez la redirection de dossiers sur le serveur de destination Windows Server Essentials](Enable-folder-redirection-on-the-Windows-Server-Essentials-Destination-Server.md).  Si la redirection de dossiers est activée sur le serveur source, vous pouvez l’activer sur le serveur de destination, puis supprimer l’ancien paramètre Stratégie de groupe de redirection de dossier.  
  
6.  [Rétrograder et supprimer le serveur source du nouveau réseau Windows Server Essentials](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md).  Avant de supprimer le serveur source du réseau, vous devez imposer une mise à jour de stratégie de groupe et rétrograder le serveur source.  
  
7.  [Effectuez des tâches de postconnexion pour la migration de Windows Server Essentials](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md).  Après avoir terminé la migration de tous les paramètres et données vers Windows Server Essentials, vous souhaiterez peut-être mapper les ordinateurs autorisés aux comptes d’utilisateur.  
  
8.  [Exécutez le Best Practices Analyzer Windows Server Essentials](Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md).  Une fois que vous avez terminé la migration des paramètres et des données vers Windows Server Essentials, vous devez exécuter Windows Server Essentials BPA.  
 
 Pour certaines procédures de migration, vous devez ouvrir une fenêtre d'invite de commandes en tant qu'administrateur.  
  
###  <a name="to-open-a-command-prompt-window-on-the-source-server-as-an-administrator"></a><a name="BKMK_OpenACommandPromptAsAdmin"></a>Pour ouvrir une fenêtre d’invite de commandes sur le serveur source en tant qu’administrateur  
  
1.  Cliquez sur **Démarrer**.  
  
2.  Dans la zone de recherche, tapez **cmd**.  
  
3.  Dans la liste de résultats, cliquez avec le bouton droit sur **cmd**, puis cliquez sur **Exécuter en tant qu’administrateur**.  
  
#### <a name="to-open-a-command-prompt-window-on-the-destination-server-as-an-administrator"></a>Pour ouvrir une fenêtre d'invite de commandes sur le serveur de destination en tant qu'administrateur  
  
1.  Dans l'écran d'**accueil**, dans la zone de recherche, tapez **cmd**.  
  
2.  Dans la liste de résultats, cliquez avec le bouton droit sur **cmd**, puis cliquez sur **Exécuter en tant qu’administrateur**.
