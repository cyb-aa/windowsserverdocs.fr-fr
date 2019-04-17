---
title: "Migrer Windows Server Essentials vers un nouveau matériel"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f695ae90-3160-407b-bebf-9e460f22c86d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 61106439a63a75143a9cca0989c70370adfedd38
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="migrate-windows-server-essentials-to-new-hardware"></a>Migrer Windows Server Essentials vers un nouveau matériel

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Ce guide décrit comment migrer un domaine Windows Server® 2012 Essentials existant vers Windows Server Essentials sur un nouveau matériel, puis comment migrer les paramètres et données. Ce guide décrit également comment supprimer votre serveur existant à partir du réseau de Windows Server Essentials après avoir terminé la migration.  
  
> [!NOTE]
>  Pour éviter tout problème pendant la migration, l’équipe de développement de produit Windows Server Essentials vous recommande vivement de lire ce document avant de commencer la migration.  
  
> [!NOTE]

>  Pour migrer les données de votre serveur vers la dernière version de Windows Server Essentials, consultez [migrer vers Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).  

  
## <a name="additional-resources"></a>Ressources supplémentaires  
 Pour des liens vers des informations supplémentaires, des outils et ressources de la Communauté qui peuvent vous guider tout au long du processus de migration, visitez le [Migration de Windows Small Business Server](https://go.microsoft.com/fwlink/?LinkId=217520) site Web.  
  
## <a name="terms-and-definitions"></a>Termes et définitions  
 **Serveur source:** serveur existant à partir duquel vous migrez vos paramètres et données.  
  
 **Serveur de destination:** le nouveau serveur vers lequel vous migrez vos paramètres et données.  
  
## <a name="migration-process-summary"></a>Résumé du processus de migration  
 Ce Guide de Migration inclut les étapes suivantes:  
  

1.  [Préparer la migration de votre serveur Source pour Windows Server Essentials](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md).  Vous devez vous assurer que votre serveur Source et le réseau sont prêts pour la migration. Cette section vous guide par le biais de la sauvegarde du serveur Source, l’évaluation de l’intégrité du système de serveur Source, installer les service packs et les correctifs les plus récentes et vérification de la configuration réseau.  
  
2.  [Installer Windows Server Essentials en mode migration](Install-Windows-Server-Essentials-in-migration-mode.md).  Cette section décrit les étapes à suivre pour installer Windows Server Essentials sur le serveur de Destination en mode migration.  
  
3.  [Joindre des ordinateurs au nouveau serveur Windows Server Essentials](Join-computers-to-the-new-Windows-Server-Essentials-server.md).  Cette section aborde la jonction des ordinateurs clients pour le nouveau serveur Windows Server Essentials et la mise à jour les paramètres de stratégie de groupe.  
  
4.  [Déplacer les paramètres et les données pour la migration du serveur de Destination pour Windows Server Essentials](Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  Cette section fournit des informations sur la migration des données et paramètres à partir du serveur Source.  
  
5.  [Configurer la redirection de dossiers sur le serveur de Destination Windows Server Essentials](Configure-folder-redirection-on-the-Windows-Server-Essentials-Destination-Server.md).  Si la redirection de dossiers est activée sur le serveur Source, vous pouvez activer la redirection de dossiers sur le serveur de Destination, puis supprimez l’ancien paramètre de stratégie de groupe de Redirection de dossiers.  
  
6.  [Rétrograder et supprimer le serveur Source du nouveau réseau Windows Server Essentials](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md).  Avant de pouvoir supprimer le serveur Source à partir du réseau, vous devez forcer une mise à jour de stratégie de groupe et rétrograder le serveur Source.  
  
7.  [Effectuer des tâches de post-migration pour la migration de Windows Server Essentials](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md).  Après avoir terminé la migration de tous les paramètres et données vers Windows Server Essentials, vous voudrez peut-être mapper les ordinateurs autorisés aux comptes d’utilisateurs.  
  
8.  [Exécuter Windows Server Essentials Best Practices Analyzer](Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md).  Après avoir terminé la migration des paramètres et données vers Windows Server Essentials, vous devez télécharger et exécuter l’outil BPA Windows Server Essentials.  
  
 Plusieurs des procédures de migration, vous devez ouvrir une fenêtre d’invite de commandes en tant qu’administrateur.  
  
#### <a name="to-open-a-command-prompt-window-as-an-administrator"></a>Pour ouvrir une fenêtre d’invite de commandes en tant qu’administrateur  
  
1.  Sur le **Démarrer** écran, dans la zone de recherche, tapez **cmd**.  
  
2.  Dans la liste des résultats, cliquez sur **cmd**, puis cliquez sur **exécuter en tant qu’administrateur**.
