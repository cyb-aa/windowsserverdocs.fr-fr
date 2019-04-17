---
title: "Gérer la sauvegarde de l’ordinateur Client dans WindowsServerEssentials"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b4776e8-9504-4b98-ae80-11da797d9819
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d184c97e47f04b9d7434aaeb0d328761bcfac1c0
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="manage-client-computer-backup-in-windows-server-essentials"></a>Gérer la sauvegarde de l’ordinateur Client dans WindowsServerEssentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials
 
 Cette rubrique contient des informations sur les tâches de sauvegarde courantes pour les ordinateurs clients que vous pouvez accomplir à l’aide du tableau de bord WindowsServerEssentials et comprend les sections suivantes:  
  
-   [Fonctionne de l’Assistant Réparation de la sauvegarde de base de données](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Comprendre les paramètres de sauvegarde d’ordinateur](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Configurer la sauvegarde pour un ordinateur client](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Modifier l’heure de sauvegarde planifiée](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Modifier la stratégie de rétention de sauvegarde d’ordinateur](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Rétablir les paramètres par défaut de sauvegarde](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [Utiliser les outils de réparation et de récupération](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_7)  
  
-   [Réparer la base de données de sauvegarde](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [Désactiver la sauvegarde d’un ordinateur](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [Exécuter la tâche de nettoyage de sauvegarde](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_10)  
  
-   [Afficher les alertes dans la barre des tâches sur un ordinateur client](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_11)  
  
-   [Afficher l’état de sauvegarde à partir du Launchpad](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_12)  
  
-   [Arrêter une sauvegarde en cours d’exécution à partir du Launchpad](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_13)  
  
-   [Démarrer une sauvegarde à partir du Launchpad](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_14)  
  
-   [Fonctionne de la sauvegarde de l’ordinateur](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_15)  
  
-   [Conseils pour éviter la perte de données altération de la base de données de sauvegarde de client](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_16)  
  
-   [Restaurer des fichiers ou dossiers à partir d’une sauvegarde de l’ordinateur client](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_17)  
  
-   [Historique des fichiers de présentation](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_FileHistory)  
  
##  <a name="BKMK_1"></a>Fonctionne de l’Assistant Réparation de la sauvegarde de base de données  
 Si WindowsServerEssentials détecte des erreurs dans votre base de données de sauvegarde, il vous envoie une notification d’intégrité et l’état d’alerte devient rouge, ce qui indique une situation critique.  
  
 Dans le tableau de bord WindowsServerEssentials, cliquez sur l’icône d’état de l’alerte pour afficher la notification d’erreur de sauvegarde de la base de données. La notification inclut un **réparation** bouton, ce qui démarre l’Assistant Réparation de la sauvegarde de base de données. L’Assistant peut prendre plusieurs heures.  
  
 L’Assistant examine votre base de données de sauvegarde de l’ordinateur sélectionné et tente de réparer la base de données si des erreurs sont détectées. Dans certains cas, l’Assistant ne peut pas réparer la base de données de sauvegarde complète. Avant de lancer l’Assistant, vérifiez que tous les disques durs externes que vous utilisez sur votre serveur sont connectés au serveur, activé et fonctionne correctement. L’erreur de sauvegarde de la base de données peut être résolue automatiquement via la reconnexion d’un disque dur manquant.  
  
> [!CAUTION]
>  Vous devez sauvegarder la base de données avant de tenter de réparer. Selon le type des erreurs détectées dans la base de données de sauvegarde, l’Assistant ne peut pas être en mesure de récupérer les sauvegardes. Certaines ou toutes les sauvegardes soit définitivement perdues.  
  
##  <a name="BKMK_2"></a>Comprendre les paramètres de sauvegarde d’ordinateur  
 Une fois la sauvegarde est configurée pour les ordinateurs clients, vous pouvez spécifier une autre fenêtre de temps pour la sauvegarde. De même, vous pouvez spécifier une durée de rétention de sauvegarde plus longue ou plus courte que la valeur par défaut.  
  
 Le **restaurer les paramètres par défaut** option vous permet de réinitialiser la stratégie de rétention et de la fenêtre de sauvegarde pour les valeurs par défaut fournies durant la configuration initiale de la sauvegarde.  
  
 Les valeurs par défaut sont:  
  
|Paramètre de sauvegarde|Par défaut|Description|  
|--------------------|-------------|-----------------|  
|Heure de début|6 H 00|Spécifie l’heure de démarrage de la sauvegarde quotidienne. Il est recommandé que vous le définissez sur une heure lorsque les utilisateurs ne sont pas de leurs ordinateurs.|  
|Heure de fin|9:00 AM|Spécifie l’heure de la sauvegarde doit être terminée. Si la sauvegarde ne requiert pas la durée spécifiée, elle utilise uniquement le temps nécessaire pour sauvegarder correctement l’ordinateur.|  
|Conserver les sauvegardes quotidiennes|5jours|Spécifie le nombre de jours pendant lesquels les sauvegardes quotidiennes sont conservées. Par exemple, le paramètre par défaut est utilisé, 5sauvegardes quotidiennes sont conservées. Le jour 6eet chaque jour par la suite, le fichier de sauvegarde quotidiens plus anciens est supprimé.|  
|Conserver les sauvegardes hebdomadaires|4semaines|Spécifie le nombre de semaines pendant lesquelles la dernière sauvegarde de la semaine est conservée. Par exemple, les paramètres par défaut est utilisé, 4sauvegardes hebdomadaires sont conservées. La semaine 5 et chaque semaine par la suite, la sauvegarde hebdomadaire la plus ancienne est supprimée.|  
|Conserver les sauvegardes mensuelles|6derniers mois|Spécifie le nombre de mois pendant lesquels la dernière sauvegarde du mois est conservée. Par exemple, le paramètre par défaut utilisé, 6sauvegardes mensuelles sont conservées. Le 7 et chaque mois par la suite, la sauvegarde mensuelle la plus ancienne est supprimée.|  
  
##  <a name="BKMK_3"></a>Configurer la sauvegarde pour un ordinateur client  
 Si la sauvegarde est désactivée, vous pouvez définir la sauvegarde de l’ordinateur à partir du tableau de bord. Lorsque vous définissez la sauvegarde pour un ordinateur, vous pouvez choisir pour tout sauvegarder sur l’ordinateur ou de sélectionner les volumes et les dossiers que vous souhaitez sauvegarder.  
  
> [!NOTE]
>  L’ordinateur doit être en ligne vous permettant de configurer la sauvegarde.  
  
> [!IMPORTANT]
>   WindowsServerEssentials ne prend pas en charge sauvegarde et restauration des disques dynamiques sur les ordinateurs clients.  
  
#### <a name="to-set-up-backup-for-a-client-computer"></a>Pour configurer la sauvegarde pour un ordinateur client  
  
1.  Ouvrez le **tableau de bord**, puis cliquez sur le **périphériques** onglet.  
  
2.  Cliquez sur le nom de l’ordinateur client que vous souhaitez configurer la sauvegarde, puis dans le **tâches** volet, cliquez sur **configurer la sauvegarde pour cet ordinateur**.  
  
    > [!NOTE]
    >  Si la sauvegarde est déjà configurée pour l’ordinateur client, **personnaliser la sauvegarde pour cet ordinateur** est répertorié dans le **tâches** volet à la place de **configurer la sauvegarde pour cet ordinateur**.  
  
3.  Dans l’Assistant Configurer la sauvegarde, vous pouvez choisir de sauvegarder tous les dossiers ou sélectionner certains dossiers que vous souhaitez sauvegarder. Suivez les instructions de l’Assistant.  
  
4.  Cliquez sur **fermer** lorsque la sauvegarde est configurée pour l’ordinateur.  
  
### <a name="critical-system-files"></a>Fichiers système critiques  
 Lorsque vous installez le système d’exploitation Windows, le programme d’installation crée des dossiers sur votre lecteur système où il place les fichiers nécessaires pour démarrer et exécuter le système. Fichiers système critiques incluent les fichiers .dll, .exe, .ocx et .sys les extensions de fichier. Certains de ces fichiers sont des polices True Type. En outre, les fichiers d’état système, par exemple le Registre s système, sont requis pour le système d’exploitation s’exécuter correctement.  
  
### <a name="find-the-file-you-are-looking-for"></a>Recherchez le fichier que vous recherchez  
 Vous pouvez restaurer tous les dossiers pour un ordinateur, plusieurs fichiers et dossiers, ou un seul fichier ou un dossier à partir d’une sauvegarde existante.  
  
 Après avoir sélectionné la sauvegarde que vous souhaitez utiliser pour la restauration, le fichier de sauvegarde est lu et tous les fichiers et dossiers sont affichés. Vous pouvez explorer le le fichier ou dossier spécifique à restaurer en double-cliquant sur le dossier de niveau supérieur, puis faire défiler la hiérarchie de dossiers jusqu'à ce que vous recherchez le fichier que vous recherchez.  
  
### <a name="why-am-i-unable-to-select-some-items"></a>Pourquoi ne puis-je pas sélectionner certains éléments?  
 La case à cocher dans le menu de sélection de la **sélectionner les éléments à sauvegarder page** peut indiquer un état différent pour chaque dossier. Lorsque la case à cocher est:  
  
-   **Sélectionné**, le dossier associé et le contenu du dossier est sélectionné pour la sauvegarde.  
  
-   **Non sélectionné**, le dossier associé et le contenu du dossier est exclu de la sauvegarde.  
  
-   **Solid**, le dossier associé est sélectionné pour la sauvegarde, mais un ou plusieurs éléments de ce dossier sont exclus de la sauvegarde.  
  
 Si vous ne pouvez pas sélectionner un dossier spécifique:  
  
-   Le volume peut être configuré pour la sauvegarde, mais il peut être en mode hors connexion. Cela est courant pour les lecteurs USB amovibles. Les volumes qui sont hors connexion sont affichés en gris.  
  
-   Vous pouvez uniquement sauvegarder les données à partir d’un lecteur local formaté en tant que système de fichiers NTFS. Lecteurs au format FAT (y compris FAT32) ou des systèmes de fichiers ReFS n’apparaissent pas dans la liste des lecteurs à sauvegarder.  
  
> [!IMPORTANT]
>  Volume Shadow Copy Service (VSS) ne gère pas la création d’un cliché instantané d’un volume virtuel et le volume hôte dans le même jeu d’instantanés. VSS prend en charge à la création de clichés instantanés sur un disque dur virtuel (VHD), si la sauvegarde du volume virtuel est nécessaire. Pour plus d’informations, voir [maintenance et sauvegarde des disques durs virtuels](https://go.microsoft.com/fwlink/p/?LinkId=256577).  
  
##  <a name="BKMK_4"></a>Modifier l’heure de sauvegarde planifiée  
 Le processus de sauvegarde doit être planifié pendant une période lorsque moins de personnes possible utilisent leurs ordinateurs en réseau. Il s’agit généralement en soirée ou heures du matin. La durée par défaut pour la sauvegarde est 6:00 PM 9 h 00. Le serveur tente de sauvegarder des ordinateurs clients uniquement durant la fenêtre de temps planifiée.  
  
 Toutefois, si votre entreprise est actif pendant ces heures creuses en règle générale, vous voudrez peut-être modifier ces paramètres par défaut.  
  
#### <a name="to-change-the-time-that-backup-is-scheduled-to-run"></a>Pour modifier l’heure de sauvegarde planifiée pour s’exécuter  
  
1.  Ouvrez le serveur **tableau de bord**, puis cliquez sur **périphériques**.  
  
2.  Dans **tâches périphériques**, cliquez sur **les paramètres de l’historique des fichiers et de personnaliser la sauvegarde ordinateur**.  
  
    > [!NOTE]
    >  Dans WindowsServerEssentials, cette tâche a été renommée **les tâches de sauvegarde des ordinateurs clients**.  
  
3.  Dans **paramètres sauvegarde de l’ordinateur Client et les outils**, dans le **sauvegarde de l’ordinateur** onglet, vous pouvez modifier les heures de début et de fin pour répondre à vos besoins.  
  
4.  Cliquez sur **appliquer**, puis cliquez sur **OK**.  
  
##  <a name="BKMK_5"></a>Modifier la stratégie de rétention de sauvegarde d’ordinateur  
 Les sauvegardes quotidiennes de tous vos ordinateurs s’accumulent sur votre serveur, au fil du temps. Pour vous aider à gérer ces sauvegardes, WindowsServerEssentials peut vous aider à gérer la base de données des sauvegardes d’ordinateurs. Vous pouvez configurer le nombre de sauvegardes à conserver pour tous vos ordinateurs.  
  
 La stratégie de rétention de sauvegarde détermine la durée pendant laquelle une sauvegarde est conservée avant d’être supprimé au cours du processus de nettoyage de sauvegarde. Nettoyage des sauvegardes s’exécute à 11 h 59tous les samedis. Il supprime toutes les sauvegardes exclues par la stratégie de rétention de sauvegarde. Les valeurs par défaut pour la stratégie de rétention de sauvegarde sont:  
  
-   **Conserver les sauvegardes quotidiennes pendant 5jours.** La première sauvegarde de la journée est conservée sous forme de la sauvegarde quotidienne. Après 5jours, la sauvegarde quotidienne la plus ancienne est supprimée durant le processus de nettoyage.  
  
-   **Conserver les sauvegardes hebdomadaires pendant 4semaines.** La première sauvegarde de la semaine est conservée sous forme de sauvegarde hebdomadaire. Après 4semaines, la sauvegarde hebdomadaire la plus ancienne est supprimée durant le processus de nettoyage.  
  
-   **Conserver les sauvegardes mensuelles pendant 6mois.** La première sauvegarde du mois est conservée sous forme de la sauvegarde mensuelle. Après 6mois, la sauvegarde mensuelle la plus ancienne est supprimée durant le processus de nettoyage  
  
#### <a name="to-change-the-computer-backup-retention-policy"></a>Pour modifier la stratégie de rétention de sauvegarde d’ordinateur  
  
1.  Ouvrez le **tableau de bord**.  
  
2.  Cliquez sur **périphériques**.  
  
3.  Dans le **tâches périphériques** volet, cliquez sur **les paramètres de l’historique des fichiers et de personnaliser la sauvegarde ordinateur**.  
  
    > [!NOTE]
    >  Dans WindowsServerEssentials, cette tâche a été renommée **tâches de sauvegarde d’ordinateur Client**.  
  
4.  Dans **paramètres sauvegarde de l’ordinateur Client et les outils**, dans le **stratégie de rétention de sauvegarde d’ordinateur Client** section, apportez les modifications à la stratégie de rétention qui répond à vos besoins.  
  
5.  Lorsque vous avez terminé, cliquez sur **OK** pour appliquer les modifications et fermer la boîte de dialogue. La stratégie de rétention mise à jour sera appliquée la prochaine fois que la sauvegarde s’exécute le nettoyage.  
  
    > [!NOTE]
    >  La stratégie de rétention mise à jour s’applique à tous les ordinateurs clients sur votre réseau qui sont configurés pour la sauvegarde.  
  
##  <a name="BKMK_6"></a>Rétablir les paramètres par défaut de sauvegarde  
 Une fois la sauvegarde est configurée pour les ordinateurs clients, l’administrateur réseau avez peut-être spécifié une autre fenêtre de temps. De même, l’administrateur a peut-être spécifié une durée de rétention de sauvegarde plus longue ou plus courte que la valeur par défaut. Le **rétablir les paramètres par défaut** bouton vous permet de réinitialiser la stratégie de rétention et de la fenêtre de sauvegarde pour les valeurs par défaut fournies durant la configuration initiale de la sauvegarde.  
  
 Les valeurs par défaut sont:  
  
-   Heure de début de la sauvegarde: 18 h 00  
  
-   Heure de fin: 9 h 00  
  
-   Nombre de jours pendant lesquels les sauvegardes quotidiennes sont conservées: 5jours  
  
-   Nombre de semaines pendant lesquelles la dernière sauvegarde de la semaine est conservée: 4semaines  
  
-   Nombre de mois pendant lesquels la dernière sauvegarde du mois est conservée: 6mois  
  
#### <a name="to-reset-computer-backup-to-the-default-settings"></a>Pour réinitialiser la sauvegarde de l’ordinateur pour les paramètres par défaut  
  
1.  Ouvrez le **tableau de bord**, puis ouvrez le **périphériques** page.  
  
2.  Dans **tâches périphériques**, cliquez sur **les paramètres de l’historique des fichiers et de personnaliser la sauvegarde ordinateur**.  
  
    > [!NOTE]
    >  Dans WindowsServerEssentials, cliquez sur **les tâches de sauvegarde des ordinateurs clients**.  
  
3.  Sur le **sauvegarde de l’ordinateur** onglet de la **outils et paramètres de sauvegarde et d’ordinateur Client**, cliquez sur **rétablir les paramètres par défaut**.  
  
    > [!NOTE]
    >  Vous voudrez peut-être à noter les paramètres de stratégie de rétention et de planification personnalisés, car ils seront perdues lorsque vous réinitialisez les paramètres par défaut.  
  
4.  Cliquez sur **appliquer**, puis cliquez sur **OK**.  
  
##  <a name="BKMK_7"></a>Utiliser les outils de réparation et de récupération  
 **Réparer des sauvegardes:** si la base de données des sauvegardes d’ordinateurs est endommagée ou inutilisable pour une raison quelconque, vous pouvez tenter de réparer la base de données à l’aide de l’Assistant Réparation de la sauvegarde de base de données. L’Assistant analyse les fichiers de sauvegarde pour déterminer s’il existe des problèmes qui peuvent être réparés. L’Assistant essaie ensuite de corriger les problèmes détectés.  
  
> [!WARNING]
>  Si un problème ne peut pas être résolu, l’Assistant peut supprimer définitivement un fichier de sauvegarde d’ordinateur.  
  
 **Récupération d’ordinateur:** vous pouvez créer un lecteur flash USB à utiliser pour restaurer un ordinateur à partir d’une sauvegarde existante. Vous devez utiliser un lecteur flash USB qui est de 1Go ou plus. Une fois le lecteur flash USB est créé, insérez-le dans l’ordinateur que vous souhaitez restaurer, puis démarrez sur le lecteur flash USB pour démarrer le processus de restauration complète du système.  
  
##  <a name="BKMK_8"></a>Réparer la base de données de sauvegarde  
 Si vous recevez une alerte indiquant que la base de données de sauvegarde d’ordinateur présente des problèmes, vous pouvez tenter de réparer.  
  
#### <a name="to-repair-the-backup-database"></a>Pour réparer la base de données de sauvegarde  
  
1.  Ouvrez le **tableau de bord**.  
  
2.  Cliquez sur **périphériques**.  
  
3.  Dans le **tâches périphériques** volet, cliquez sur **paramètres ordinateur personnaliser la sauvegarde et de l’historique des fichiers.**  
  
    > [!NOTE]
    >  Dans WindowsServerEssentials, cliquez sur **les tâches de sauvegarde des ordinateurs clients**.  
  
4.  Dans **paramètres sauvegarde de l’ordinateur Client et les outils**, cliquez sur le **outils** onglet.  
  
5.  Dans le **réparer des sauvegardes**, cliquez sur **réparer maintenant**. L’Assistant Réparation de la sauvegarde de base de données s’ouvre.  
  
6.  Suivez les instructions de l’Assistant Réparation de la sauvegarde de base de données.  
  
7.  Selon la taille la base de données de sauvegarde est, la réparation de la base de données peut prendre plusieurs heures. Cliquez sur **fermer**, puis cliquez sur **OK** pour fermer la **les paramètres de l’historique des fichiers et de personnaliser la sauvegarde ordinateur** page.  
  
    > [!NOTE]
    >  Dans WindowsServerEssentials, cliquez sur **paramètres sauvegarde de l’ordinateur Client et les outils**.  
  
#### <a name="to-review-the-results-of-the-backup-database-repair"></a>Pour passer en revue les résultats de la réparation de la base de données de sauvegarde  
  
1.  Ouvrez le **tableau de bord**.  
  
2.  Cliquez sur **périphériques**.  
  
3.  Dans le **tâches périphériques** volet, cliquez sur **paramètres ordinateur personnaliser la sauvegarde et de l’historique des fichiers.**  
  
    > [!NOTE]
    >  Dans WindowsServerEssentials, cliquez sur **les tâches de sauvegarde des ordinateurs clients**.  
  
4.  Dans **paramètres sauvegarde de l’ordinateur Client et les outils**, cliquez sur le **outils** onglet.  
  
5.  Les résultats sont affichés dans le **réparer des sauvegardes** section.  
  
##  <a name="BKMK_9"></a>Désactiver la sauvegarde d’un ordinateur  
 Utilisez le tableau de bord pour désactiver rapidement les sauvegardes d’ordinateurs sur votre réseau.  
  
#### <a name="to-disable-backup-for-a-computer"></a>Pour désactiver la sauvegarde pour un ordinateur  
  
1.  Ouvrez le **tableau de bord**.  
  
2.  Cliquez sur le **périphériques** onglet.  
  
3.  Cliquez sur le nom de l’ordinateur pour lequel vous souhaitez désactiver les sauvegardes.  
  
4.  Dans le volet tâches, cliquez sur **personnaliser la sauvegarde de l’ordinateur**. L’Assistant Personnalisation de la sauvegarde s’affiche.  
  
5.  Cliquez sur **désactiver la sauvegarde pour cet ordinateur**, puis indiquez si vous souhaitez conserver ou supprimer les fichiers de sauvegarde existants.  
  
6.  Cliquez sur **enregistrer les modifications**, puis cliquez sur **fermer**.  
  
 Pour plus d’informations sur la façon d’activer la sauvegarde pour un ordinateur une fois la sauvegarde a été désactivée, voir [configurer la sauvegarde pour un ordinateur client](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_3).  
  
##  <a name="BKMK_10"></a>Exécuter la tâche de nettoyage de sauvegarde  
 La tâche de nettoyage des sauvegardes d’ordinateur client est planifiée pour s’exécuter à 11 h 59tous les samedis lorsque toutes les sauvegardes sont terminées. La tâche de nettoyage supprime les sauvegardes de la base de sauvegarde ordinateur client en fonction de la stratégie de rétention de sauvegarde. Par défaut pour la stratégie de rétention de sauvegarde sont les suivantes:  
  
-   Nombre de jours pendant lesquels les sauvegardes quotidiennes sont conservées: 5jours  
  
-   Nombre de semaines pendant lesquelles la dernière sauvegarde de la semaine est conservée: 4semaines  
  
-   Nombre de mois pendant lesquels la dernière sauvegarde du mois est conservée: 6mois  
  
 Pour plus d’informations sur la modification de la stratégie de rétention de sauvegarde, consultez [modifier la stratégie de rétention de sauvegarde d’ordinateur](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_5).  
  
#### <a name="to-run-the-client-backup-database-cleanup-task"></a>Pour exécuter le client de tâche de nettoyage de base de données de sauvegarde  
  
1.  Ouvrez une session sur le serveur avec votre compte d’utilisateur administrateur et le mot de passe.  
  
2.  Sur le serveur, ouvrez **le Planificateur de tâches**. Vous pouvez accéder à du programme du Planificateur de tâches à partir du **outils d’administration** console.  
  
3.  Dans le Planificateur de tâches, développez **bibliothèque du Planificateur de tâches**, développez **Microsoft**, développez **Windows**, puis cliquez sur **WindowsServerEssentials**.  
  
4.  Cliquez sur le **nettoyage des sauvegardes** de tâches, puis cliquez sur **exécuter** dans les **Actions** volet. L’état passe à **en cours d’exécution** jusqu'à ce que la tâche est terminée.  
  
##  <a name="BKMK_11"></a>Afficher les alertes dans la barre des tâches sur un ordinateur client  
 Vous pouvez recevoir une notification de sauvegarde dans la barre des tâches sur votre ordinateur pour les raisons suivantes:  
  
-   Une sauvegarde a démarré.  
  
-   Une sauvegarde a pris fin.  
  
-   Il n’est pas une sauvegarde récente. Le nombre de jours écoulés depuis la dernière sauvegarde réussie est inclus dans l’avis.  
  
-    WindowsServerEssentials uniquement: un nouveau volume est ajouté à votre ordinateur. La personne qui administre votre réseau doit exécuter l’Assistant Personnalisation de sauvegarde pour inclure ou exclure le volume pour les futures sauvegardes.  
  
##  <a name="BKMK_12"></a>Afficher l’état de sauvegarde à partir du Launchpad  
 Utiliser Launchpad pour afficher l’état rapide de la sauvegarde de votre ordinateur.  
  
> [!TIP]
>  Pour obtenir un état rapide, déplacez le curseur sur l’icône de Launchpad dans la zone de notification du bureau. L’état de sauvegarde s’affiche dans l’info-bulle.  
  
#### <a name="to-view-status-of-a-backup-for-your-computer"></a>Pour afficher l’état d’une sauvegarde de votre ordinateur  
  
1.  Connectez-vous à la **Launchpad** avec votre nom d’utilisateur et un mot de passe.  
  
2.  Cliquez sur **sauvegarde**.  
  
3.  Dans le **propriétés de la sauvegarde** la boîte de dialogue dans le **état de la sauvegarde**, l’état de la sauvegarde s’affiche.  
  
    -   **Aucune sauvegarde antérieure**: la sauvegarde n’a pas encore exécuté, ou l’historique de sauvegarde a été supprimé.  
  
    -   **Sauvegarde en attente**: une sauvegarde est en attente sur le serveur pour effectuer un autre processus.  
  
    -   **Connexion**: sauvegarde se connecte au serveur.  
  
    -   **Impossible de se connecter**: sauvegarde ne peut pas se connecter au Service fournisseur de sauvegarde d’ordinateur WindowsServerClient.  
  
        > [!NOTE]
        >  Dans WindowsServerEssentials, ce service a été renommé Service de gestion d’ordinateur Client WindowsServerEssentials.  
  
    -   **Sauvegarde en cours**: affiche le pourcentage de sauvegarde effectuée.  
  
    -   **Sauvegarde a été annulée**: affiche la date et l’heure de début de la sauvegarde si vous ou l’administrateur réseau avez annulé la sauvegarde de.  
  
    -   **Sauvegarde réussie**: affiche la date et l’heure de début de la sauvegarde s’est terminée avec succès.  
  
    -   **Sauvegarde incomplète**: affiche la date et l’heure de début de la sauvegarde si la sauvegarde n’a pas abouti.  
  
    -   **La sauvegarde a échoué**: affiche la date et l’heure de début de la sauvegarde si la sauvegarde n’a pas abouti.  
  
4.  Cliquez sur **OK** pour fermer la **propriétés de la sauvegarde** boîte de dialogue.  
  
##  <a name="BKMK_13"></a>Arrêter une sauvegarde en cours d’exécution à partir du Launchpad  
 Vous pouvez facilement arrêter une sauvegarde est en cours d’exécution.  
  
#### <a name="to-stop-a-backup-in-progress"></a>Pour arrêter une sauvegarde en cours  
  
1.  Ouvrez le **Launchpad**.  
  
    > [!NOTE]
    >  Si vous êtes invité, ouvrez une session sur le **Launchpad** avec votre nom d’utilisateur et un mot de passe.  
  
2.  Cliquez sur **sauvegarde**.  
  
3.  Dans le **propriétés de la sauvegarde** la boîte de dialogue dans le **état de la sauvegarde** section, le **démarrer la sauvegarde** bouton devient **arrêter la sauvegarde** lorsque la sauvegarde est en cours d’exécution. Cliquez sur **arrêter la sauvegarde**, puis cliquez sur **Oui** pour confirmer. La sauvegarde continue de s’exécuter jusqu'à ce que vous cliquez sur **Oui**.  
  
4.  Lorsque vous arrêtez une sauvegarde en cours, l’état devient **sauvegarde a été annulée** avec la date et l’heure de début de la sauvegarde de. Si, au lieu de cela, vous voyez l’état **réussite**, **incomplet**, ou **échec**, la dernière sauvegarde est déjà terminée.  
  
5.  Cliquez sur **OK** pour fermer la **propriétés de la sauvegarde** boîte de dialogue.  
  
##  <a name="BKMK_14"></a>Démarrer une sauvegarde à partir du Launchpad  
 Il peut arriver lorsque vous souhaitez sauvegarder vos fichiers et dossiers avant l’heure de sauvegarde planifiée périodique configuré sur votre serveur. Launchpad vous permet de démarrer manuellement la sauvegarde de votre ordinateur.  
  
#### <a name="to-start-a-backup"></a>Pour démarrer une sauvegarde  
  
1.  Ouvrez le **Launchpad**.  
  
    > [!NOTE]
    >  Si vous êtes invité, ouvrez une session sur le **Launchpad** avec votre nom d’utilisateur et un mot de passe.  
  
2.  Cliquez sur **sauvegarde**.  
  
3.  Dans le **propriétés de la sauvegarde** la boîte de dialogue dans le **état de la sauvegarde**, cliquez sur **démarrer la sauvegarde**, puis cliquez sur **OK**.  
  
4.  Tapez une description pour la sauvegarde, puis cliquez sur **OK**. L’état passe à **démarrage de la sauvegarde**, puis **sauvegarde en cours** avec un pourcentage d’achèvement.  
  
5.  Une fois la sauvegarde démarre, le bouton devient **arrêter la sauvegarde**. Si la sauvegarde est en cours d’exécution et que vous souhaitez arrêter, cliquez sur **arrêter la sauvegarde**, puis cliquez sur **Oui**. Lorsque vous arrêtez une sauvegarde en cours, l’état devient **sauvegarde a été annulée** avec la date et l’heure à laquelle la sauvegarde a été démarrée.  
  
6.  Lorsque la sauvegarde se termine correctement, l’état passe à **sauvegarde réussie** avec la date et l’heure de début de la sauvegarde de.  
  
7.  Cliquez sur **OK** pour fermer la **propriétés de la sauvegarde** boîte de dialogue.  
  
##  <a name="BKMK_15"></a>Fonctionne de la sauvegarde de l’ordinateur  
 Les événements suivants se produisent lors de la sauvegarde chaque jour:  
  
-   Les ordinateurs du réseau sont sauvegardés jusqu'à un après l’autre.  
  
-   Une sauvegarde est en cours terminée, même si vous fermez la fenêtre de sauvegarde.  
  
-   Mises à jour Windows sont installés, et WindowsServerEssentials redémarre (si nécessaire).  
  
-   Le dimanche, nettoyage des sauvegardes s’exécute.  
  
> [!IMPORTANT]
>  Volume Shadow Copy Service (VSS) ne gère pas la création d’un cliché instantané d’un volume virtuel et le volume hôte dans le même jeu d’instantanés. VSS prend en charge à la création de clichés instantanés sur un disque dur virtuel (VHD), si la sauvegarde du volume virtuel est nécessaire. Pour plus d’informations, voir [maintenance et sauvegarde des disques durs virtuels](https://go.microsoft.com/fwlink/p/?LinkId=256577).  
  
##  <a name="BKMK_16"></a>Conseils pour éviter la perte de données altération de la base de données de sauvegarde de client  
 Si la base de données de sauvegarde de client est endommagé, vous pouvez perdre des données critiques.  
  
 Afin d’éviter une perte de données altération de la base de données de sauvegarde client, procédez comme suit:  
  
-   Activer une méthode supplémentaire de sauvegarde de la base de données de sauvegarde de client. Par exemple, selon la version de WindowsServerEssentials vous êtes en cours d’exécution, utilisez la sauvegarde du serveur, la sauvegarde en ligne ou MicrosoftAzure Backup pour sauvegarder la base de données.  
  
    > [!IMPORTANT]
    >  Assurez-vous que vous sélectionnez pour sauvegarder tous les fichiers dans le **sauvegarde des ordinateurs Client** dossier.  
  
-   Dans le cas où vous devez restaurer la base de données, veillez à restaurer tous les fichiers dans le **sauvegarde des ordinateurs Client** dossier. Si vous ne restaurez pas tous les fichiers, la base de données peut devenir incohérente.  
  
-   Avant de nettoyer ou restaurer la base de données de sauvegarde client, veillez à arrêter toutes les sauvegardes de clients en cours d’exécution.  
  
     Si vous exécutez WindowsServerEssentials, vous devez également arrêter le Service de sauvegarde de WindowsServerClient ordinateur et le Service du fournisseur de sauvegarde WindowsServerClient ordinateur.  
  
     Si vous exécutez WindowsServerEssentials, vous devez également arrêter le Service de sauvegarde de Windows Server ordinateur.  
  
     Après avoir effectué l’opération de restauration, redémarrez le service.  
  
##  <a name="BKMK_17"></a>Restaurer des fichiers ou dossiers à partir d’une sauvegarde de l’ordinateur client  
 Vous pouvez parcourir et restaurer des fichiers et dossiers individuels à partir d’une sauvegarde.  
  
> [!NOTE]
>  Vous ne pouvez pas restaurer des fichiers et dossiers sur un ordinateur client à l’aide du tableau de bord sur le serveur. Vous devez ouvrir le tableau de bord sur un ordinateur client pour terminer la tâche.  
  
> [!IMPORTANT]
>  Vous ne pouvez pas restaurer des fichiers et dossiers sur un disque dur qui a été configuré pour être un disque dynamique.  
  
#### <a name="to-restore-files-and-folders"></a>Pour restaurer des fichiers et dossiers  
  
1.  Ouvrez le **tableau de bord** sur un ordinateur client.  
  
2.  Cliquez sur le **périphériques** onglet.  
  
3.  Cliquez sur l’ordinateur pour lequel vous souhaitez restaurer des fichiers et dossiers, puis cliquez sur **restaurer des fichiers ou dossiers de l’ordinateur** dans les **tâches** volet. La restauration de fichiers ou dossiers Assistant s’ouvre.  
  
4.  Suivez les instructions de l’Assistant.  
  
##  <a name="BKMK_FileHistory"></a>Historique des fichiers de présentation  
 La fonctionnalité de l’historique des fichiers de WindowsServerEssentials sauvegarde automatiquement les fichiers qui se trouvent dans les dossiers bibliothèques, Contacts, bureau et Favoris des ordinateurs du réseau qui ont la capacité de l’historique des fichiers. Si les originaux sont perdus, endommagés ou supprimés, vous pouvez les restaurer. Vous trouverez également différentes versions de vos fichiers à partir d’un point spécifique dans le temps. Au fil du temps, vous aurez un historique complet de vos fichiers.  
  
 Dans WindowsServerEssentials, vous pouvez personnaliser les paramètres de l’historique des fichiers à partir de la **l’historique des fichiers** page de **paramètres sauvegarde de l’ordinateur Client et les outils**.  
  
 Dans WindowsServerEssentials, vous pouvez personnaliser les paramètres de l’historique des fichiers en accédant à la **utilisateurs** onglet, puis en sélectionnant le **modifier le paramétrage de l’historique des fichiers** tâche.  
  
 La page de l’historique des fichiers fournit les options suivantes:  
  
|Paramètre de sauvegarde|Par défaut|Description|  
|--------------------|-------------|-----------------|  
|Activer / désactiver|Sur|L’historique des fichiers sont activé par défaut lorsque vous installez WindowsServerEssentials.|  
|Données de sauvegarde|Documents et bureau|Il existe trois paramètres préconfigurés, ce qui vous permet de spécifier toute une série de solutions de sauvegarde. Vous pouvez choisir une des options suivantes:<br /><br /> -Des documents et bureau<br /><br /> -Toutes les bibliothèques, bureau, Contacts et Favoris<br /><br /> -Toutes les données dans les bibliothèques, bureau, Contacts et Favoris à l’exception des données de la musique, vidéos et images|  
|Fréquence de sauvegarde|Toutes les heures|Spécifie la fréquence à laquelle l’historique des fichiers crée une sauvegarde des données sélectionnées. Vous pouvez choisir parmi plusieurs options allant de toutes les 10minutes à tous les jours.|  
|Conserver les copies pendant|1an|Spécifie la durée pendant laquelle l’historique des fichiers conserve une copie d’une sauvegarde.|  
  
 Pour plus d’informations sur les problèmes de l’historique des fichiers, consultez [résoudre les problèmes de l’historique des fichiers](../support/Troubleshoot-File-History-in-Windows-Server-Essentials.md).  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Gérer la sauvegarde et restauration](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Gérer WindowsServerEssentials](Manage-Windows-Server-Essentials.md)  
  
-   [Utiliser Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
