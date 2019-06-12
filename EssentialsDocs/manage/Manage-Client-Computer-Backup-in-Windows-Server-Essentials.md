---
title: Gérer la sauvegarde des ordinateurs clients dans Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
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
ms.openlocfilehash: 4249273c42e07fea980af3e97ae59b31db243e56
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433338"
---
# <a name="manage-client-computer-backup-in-windows-server-essentials"></a>Gérer la sauvegarde des ordinateurs clients dans Windows Server Essentials

>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 Cette rubrique contient des informations sur les tâches de sauvegarde courantes des ordinateurs clients, que vous pouvez effectuer à l'aide du tableau de bord Windows Server Essentials. Elle inclut les sections suivantes :  
  
-   [Fonctionne de l’Assistant Réparation de la sauvegarde de base de données](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Comprendre les paramètres de sauvegarde d’ordinateur](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Configurer la sauvegarde pour un ordinateur client](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Modifier l’heure de sauvegarde est planifiée pour s’exécuter](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Modifier la stratégie de rétention des sauvegardes d’ordinateur](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Rétablir les paramètres par défaut de sauvegarde](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [Utiliser les outils de réparation et de récupération](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_7)  
  
-   [Réparer la base de données de sauvegarde](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [Désactiver la sauvegarde pour un ordinateur](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [Exécuter la tâche de nettoyage de sauvegarde](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_10)  
  
-   [Afficher les alertes dans la barre des tâches sur un ordinateur client](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_11)  
  
-   [Afficher l’état de sauvegarde à partir du Launchpad](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_12)  
  
-   [Arrêter une sauvegarde en cours d’exécution à partir du Launchpad](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_13)  
  
-   [Démarrer une sauvegarde à partir du Launchpad](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_14)  
  
-   [Fonctionne de la sauvegarde de l’ordinateur](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_15)  
  
-   [Conseils pour éviter la perte de données en raison d’une altération de la base de données de sauvegarde de client](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_16)  
  
-   [Restaurer des fichiers ou dossiers à partir d’une sauvegarde de l’ordinateur client](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_17)  
  
-   [Comprendre l’historique des fichiers](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_FileHistory)  
  
##  <a name="BKMK_1"></a> Fonctionne de l’Assistant Réparation de la sauvegarde de base de données  
 Si Windows Server Essentials détecte des erreurs dans votre base de données de sauvegarde, il vous envoie une notification d'intégrité. Par ailleurs, l'état d'alerte devient rouge, ce qui indique une situation critique.  
  
 Dans le tableau de bord Windows Server Essentials, cliquez sur l'icône d'état d'alerte pour afficher la notification d'erreur de la base de données de sauvegarde. La notification inclut un bouton **Réparer** qui démarre l'Assistant Réparation de la base de données de sauvegarde. L'exécution de l'Assistant peut prendre plusieurs heures.  
  
 L'Assistant examine la base de données de sauvegarde de l'ordinateur sélectionné, puis tente de la réparer si des erreurs sont détectées. Dans certains cas, l'Assistant ne peut pas réparer l'intégralité de la base de données de sauvegarde. Avant de démarrer l'Assistant, vérifiez que tous les disques durs externes que vous utilisez sur votre serveur sont connectés à ce dernier, qu'ils sont sous tension et qu'ils fonctionnent correctement. L'erreur de la base de données de sauvegarde peut être résolue automatiquement via la reconnexion d'un disque dur manquant.  
  
> [!CAUTION]
>  Vous devez sauvegarder la base de données avant de tenter de la réparer. Selon le type des erreurs détectées dans la base de données de sauvegarde, l'Assistant n'est pas toujours capable de récupérer les sauvegardes. Il arrive qu'une partie ou que la totalité des sauvegardes soit définitivement perdue.  
  
##  <a name="BKMK_2"></a> Comprendre les paramètres de sauvegarde d’ordinateur  
 Une fois la sauvegarde configurée pour les ordinateurs clients, vous pouvez spécifier une autre fenêtre temporelle de sauvegarde. De même, vous pouvez spécifier une durée de rétention de sauvegarde plus longue ou plus courte que celle par défaut.  
  
 L'option **Restaurer les paramètres par défaut** vous permet de réinitialiser la fenêtre de sauvegarde et la stratégie de rétention en fonction des valeurs par défaut fournies durant la configuration initiale de la sauvegarde.  
  
 Les paramètres par défaut sont :  
  
|Paramètre de sauvegarde|Par défaut|Description|  
|--------------------|-------------|-----------------|  
|Heure de début|18h00|Spécifie l'heure de début de la sauvegarde quotidienne. Il est recommandé de définir cette valeur en fonction du moment où les utilisateurs ne se servent pas de leurs ordinateurs.|  
|Heure de fin|9 h 00|Spécifie l'heure de fin de la sauvegarde. Si la sauvegarde ne nécessite pas la durée spécifiée, elle utilise uniquement le temps dont elle a besoin pour sauvegarder correctement l'ordinateur.|  
|Conserver les sauvegardes quotidiennes|5 jours|Spécifie le nombre de jours pendant lesquels les sauvegardes quotidiennes sont conservées. Par exemple, si le paramètre par défaut est utilisé, 5 sauvegardes quotidiennes sont conservées. Le sixième jour, et chaque jour suivant, le fichier de sauvegarde quotidienne le plus ancien est supprimé.|  
|Conserver les sauvegardes hebdomadaires|4 semaines|Spécifie le nombre de semaines pendant lesquelles la dernière sauvegarde de la semaine est conservée. Par exemple, si le paramètre par défaut est utilisé, 4 sauvegardes hebdomadaires sont conservées. La cinquième semaine, et chaque semaine suivante, la sauvegarde hebdomadaire la plus ancienne est supprimée.|  
|Conserver les sauvegardes mensuelles|6 mois|Spécifie le nombre de mois pendant lesquels la dernière sauvegarde du mois est conservée. Par exemple, si le paramètre par défaut est utilisé, 6 sauvegardes mensuelles sont conservées. Le septième mois, et chaque mois suivant, la sauvegarde mensuelle la plus ancienne est supprimée.|  
  
##  <a name="BKMK_3"></a> Configurer la sauvegarde pour un ordinateur client  
 Si la sauvegarde est désactivée, vous pouvez définir la sauvegarde de l'ordinateur à partir du tableau de bord. Quand vous définissez une sauvegarde pour un ordinateur, vous pouvez choisir de tout sauvegarder sur l'ordinateur, ou de sélectionner les volumes et les dossiers que vous voulez sauvegarder.  
  
> [!NOTE]
>  L'ordinateur doit être en ligne pour vous permettre de configurer la sauvegarde.  
  
> [!IMPORTANT]
>   Windows Server Essentials ne prend pas en charge la sauvegarde et restauration des disques dynamiques sur les ordinateurs clients.  
  
#### <a name="to-set-up-backup-for-a-client-computer"></a>Pour configurer la sauvegarde pour un ordinateur client  
  
1.  Ouvrez le **Tableau de bord**, puis cliquez sur l'onglet **Périphériques**.  
  
2.  Cliquez sur le nom de l'ordinateur client dont vous souhaitez configurer la sauvegarde, puis dans le volet **Tâches** , cliquez sur **Configurer la sauvegarde pour cet ordinateur**.  
  
    > [!NOTE]
    >  Si la sauvegarde est déjà configurée pour l'ordinateur client, l'option **Personnaliser la sauvegarde pour l'ordinateur** est répertoriée dans le volet **Tâches** à la place de **Configurer la sauvegarde pour cet ordinateur**.  
  
3.  Dans l'Assistant Configuration de la sauvegarde du serveur, vous pouvez choisir de sauvegarder tous les dossiers ou certains d'entre eux. Suivez les instructions de l'Assistant.  
  
4.  Cliquez sur **Fermer** quand la sauvegarde est configurée pour l'ordinateur.  
  
### <a name="critical-system-files"></a>Fichiers système critiques  
 Quand vous installez le système d'exploitation Windows, le programme d'installation crée des dossiers sur votre lecteur système, où il place les fichiers nécessaires au démarrage et à l'exécution du système. Les fichiers système critiques incluent les fichiers ayant les extensions .dll, .exe, .ocx et .sys. Certains de ces fichiers sont des polices True Type. En outre, les fichiers d’état système, telles que le Registre du système, sont requis pour le système d’exploitation s’exécute correctement.  
  
### <a name="find-the-file-you-are-looking-for"></a>Rechercher le fichier souhaité  
 Vous pouvez restaurer tous les dossiers d'un ordinateur, plusieurs fichiers et dossiers, ou un seul fichier ou dossier à partir d'une sauvegarde existante.  
  
 Une fois que vous avez sélectionné la sauvegarde à utiliser pour la restauration, le fichier de sauvegarde est lu, et tous les fichiers ainsi que tous les dossiers sont affichés. Vous pouvez accéder au fichier ou dossier spécifique à restaurer en double-cliquant sur le dossier de premier niveau, puis faire défiler la hiérarchie des dossiers jusqu'à ce que vous trouviez le fichier recherché.  
  
### <a name="why-am-i-unable-to-select-some-items"></a>Pourquoi ne puis-je pas sélectionner certains éléments ?  
 La case à cocher du menu de sélection de la page **Sélectionner les éléments à sauvegarder** peut indiquer un état différent pour chaque dossier. Quand la case est :  
  
- **cochée**, le dossier associé et son contenu sont sélectionnés pour la sauvegarde.  
  
- **décochée**, le dossier associé et son contenu sont exclus de la sauvegarde.  
  
- **de couleur unie**, le dossier associé est sélectionné pour la sauvegarde, mais un ou plusieurs éléments du dossier sont exclus de la sauvegarde.  
  
  Si vous ne pouvez pas sélectionner un dossier spécifique :  
  
- Le volume est éventuellement configuré pour la sauvegarde, mais il est peut-être hors connexion. Cela est courant pour les lecteurs USB amovibles. Les volumes hors connexion sont affichés en gris.  
  
- Vous pouvez uniquement sauvegarder les données d'un lecteur local formaté selon le système de fichiers NTFS. Les lecteurs formatés selon le système de fichiers FAT (y compris FAT32) ou ReFS n'apparaissent pas dans la liste des lecteurs à sauvegarder.  
  
> [!IMPORTANT]
>  Le service de cliché instantané de volumes (VSS, Volume Shadow Copy Service) ne prend pas en charge la création d'un cliché instantané d'un volume virtuel et du volume hôte dans le même jeu d'instantanés. VSS prend en charge la création de clichés instantanés de volumes sur un disque dur virtuel (VHD), si la sauvegarde du volume virtuel est nécessaire. Pour plus d’informations, consultez [Maintenance et sauvegarde des disques durs virtuels](https://go.microsoft.com/fwlink/p/?LinkId=256577).  
  
##  <a name="BKMK_4"></a> Modifier l’heure de sauvegarde est planifiée pour s’exécuter  
 Le processus de sauvegarde doit être planifié pendant une période où le moins de personnes possible utilisent leurs ordinateurs réseau. Il s'agit généralement des périodes tardives en soirée ou au petit matin. La période par défaut de la sauvegarde va de 6 h 00 à 9 h 00. Le serveur tente de sauvegarder les ordinateurs clients uniquement durant la fenêtre de temps planifiée.  
  
 Toutefois, si votre entreprise est ouverte durant ces heures qui sont généralement des moments d'inactivité, changez ces paramètres par défaut.  
  
#### <a name="to-change-the-time-that-backup-is-scheduled-to-run"></a>Pour changer l'heure de sauvegarde planifiée  
  
1.  Ouvrez le **Tableau de bord**du serveur, puis cliquez sur **Périphériques**.  
  
2.  Dans **Tâches du périphérique**, cliquez sur **Personnaliser les paramètres de la sauvegarde de l'ordinateur et de l'historique des fichiers**.  
  
    > [!NOTE]
    >  Dans Windows Server Essentials, cette tâche a été renommée **les tâches de sauvegarde des ordinateurs clients**.  
  
3.  Dans **Paramètres et outils de sauvegarde d'ordinateurs clients**, sous l'onglet **Sauvegarde de l'ordinateur**, vous pouvez changer les heures de début et de fin en fonction de vos besoins.  
  
4.  Cliquez sur **Appliquer**, puis sur **OK**.  
  
##  <a name="BKMK_5"></a> Modifier la stratégie de rétention des sauvegardes d’ordinateur  
 Avec le temps, les sauvegardes quotidiennes de tous vos ordinateurs s'accumulent sur votre serveur. Pour vous aider à gérer ces sauvegardes, Windows Server Essentials vous permet d'accéder à la base de données des sauvegardes d'ordinateurs. Vous pouvez configurer le nombre de sauvegardes à conserver pour tous vos ordinateurs.  
  
 La stratégie de rétention de sauvegarde détermine la durée pendant laquelle une sauvegarde est conservée avant d'être supprimée durant le processus de nettoyage des sauvegardes. Le nettoyage des sauvegardes s'exécute à 11 h 59 tous les samedis. Il supprime toutes les sauvegardes exclues par la stratégie de rétention de sauvegarde. Les valeurs par défaut de la stratégie de rétention de sauvegarde sont les suivantes :  
  
-   **Conserver les sauvegardes quotidiennes pendant 5 jours.** La première sauvegarde de la journée est conservée sous forme de sauvegarde quotidienne. Après 5 jours, la sauvegarde quotidienne la plus ancienne est supprimée durant le processus de nettoyage.  
  
-   **Conserver les sauvegardes hebdomadaires pendant 4 semaines.** La première sauvegarde de la semaine est conservée sous forme de sauvegarde hebdomadaire. Après 4 semaines, la sauvegarde hebdomadaire la plus ancienne est supprimée durant le processus de nettoyage.  
  
-   **Conserver les sauvegardes mensuelles pendant 6 mois.** La première sauvegarde du mois est conservée sous forme de sauvegarde mensuelle. Après 6 mois, la sauvegarde mensuelle la plus ancienne est supprimée durant le processus de nettoyage.  
  
#### <a name="to-change-the-computer-backup-retention-policy"></a>Pour changer la stratégie de rétention des sauvegardes d'ordinateurs  
  
1.  Ouvrez le **Tableau de bord**.  
  
2.  Cliquez sur **Périphériques**.  
  
3.  Dans le volet **Tâches du périphérique** , cliquez sur **Personnaliser les paramètres de la sauvegarde de l'ordinateur et de l'historique des fichiers**.  
  
    > [!NOTE]
    >  Dans Windows Server Essentials, cette tâche a été renommée **tâches de sauvegarde d’ordinateur Client**.  
  
4.  Dans **Paramètres et outils de sauvegarde d'ordinateurs clients**, dans la section **Stratégie de conservation des sauvegardes d'ordinateurs client** , apportez les changements souhaités à la stratégie de rétention qui correspond à vos besoins.  
  
5.  Une fois que vous avez fini, cliquez sur **OK** pour appliquer les changements et fermer la boîte de dialogue. La stratégie de rétention mise à jour est appliquée la prochaine fois que le nettoyage de la sauvegarde s'exécute.  
  
    > [!NOTE]
    >  La stratégie de rétention mise à jour s'applique à tous les ordinateurs clients de votre réseau, qui sont configurés pour la sauvegarde.  
  
##  <a name="BKMK_6"></a> Rétablir les paramètres par défaut de sauvegarde  
 Une fois la sauvegarde configurée pour les ordinateurs clients, il est possible que l'administrateur réseau ait spécifié une autre fenêtre de temps. De même, l'administrateur a peut-être spécifié une durée de rétention de sauvegarde plus longue ou plus courte que la valeur par défaut. Le bouton **Rétablir les paramètres par défaut** vous permet de réinitialiser la fenêtre de sauvegarde et la stratégie de rétention en fonction des valeurs par défaut fournies durant la configuration initiale de la sauvegarde.  
  
 Les paramètres par défaut sont :  
  
-   Heure de début de la sauvegarde : 18h00  
  
-   Heure de fin : 9 h 00  
  
-   Nombre de jours pendant lesquels les sauvegardes quotidiennes sont conservées : 5 jours  
  
-   Nombre de semaines pendant lesquelles la dernière sauvegarde de la semaine est conservée : 4 semaines  
  
-   Nombre de mois pendant lesquels la dernière sauvegarde du mois est conservée : 6 mois  
  
#### <a name="to-reset-computer-backup-to-the-default-settings"></a>Pour réinitialiser la sauvegarde de l'ordinateur en fonction des paramètres par défaut  
  
1.  Ouvrez le **Tableau de bord**, puis la page **Périphériques** .  
  
2.  Dans **Tâches du périphérique**, cliquez sur **Personnaliser les paramètres de la sauvegarde de l'ordinateur et de l'historique des fichiers**.  
  
    > [!NOTE]
    >  Dans Windows Server Essentials, cliquez sur **les tâches de sauvegarde des ordinateurs clients**.  
  
3.  Sous l'onglet **Sauvegarde de l'ordinateur** de la page **Paramètres et outils de sauvegarde d'ordinateurs clients**, cliquez sur **Rétablir les paramètres par défaut**.  
  
    > [!NOTE]
    >  N'hésitez pas à noter les paramètres de stratégie de rétention et de planification personnalisés, car ils disparaîtront quand vous rétablirez les paramètres par défaut.  
  
4.  Cliquez sur **Appliquer**, puis sur **OK**.  
  
##  <a name="BKMK_7"></a> Utiliser les outils de réparation et de récupération  
 **Réparer des sauvegardes :** Si la base de données des sauvegardes d'ordinateurs est endommagée ou inutilisable pour une raison quelconque, vous pouvez essayer de la réparer à l'aide de l'Assistant Réparation de la base de données de sauvegarde. L'Assistant analyse les fichiers de sauvegarde pour déterminer s'il existe des problèmes qui peuvent être réparés. L'Assistant essaie ensuite de corriger les problèmes détectés.  
  
> [!WARNING]
>  Si un problème ne peut pas être résolu, l'Assistant peut supprimer définitivement un fichier de sauvegarde d'ordinateur.  
  
 **Récupération de l’ordinateur :** Vous pouvez créer un disque mémoire flash USB de démarrage à utiliser pour restaurer un ordinateur à partir d'une sauvegarde existante. Vous devez utiliser un disque mémoire flash USB d'au moins 1 Go d'espace disponible. Une fois le disque mémoire flash USB de démarrage créé, insérez-le dans l'ordinateur à restaurer, puis démarrez sur le disque mémoire flash USB pour lancer le processus de restauration complète du système.  
  
##  <a name="BKMK_8"></a> Réparer la base de données de sauvegarde  
 Si vous recevez une alerte indiquant que la base de données de sauvegarde d'ordinateurs présente des problèmes, vous pouvez essayer de la réparer.  
  
#### <a name="to-repair-the-backup-database"></a>Pour réparer la base de données de sauvegarde  
  
1.  Ouvrez le **Tableau de bord**.  
  
2.  Cliquez sur **Périphériques**.  
  
3.  Dans le volet **Tâches du périphérique** , cliquez sur **Personnaliser les paramètres de la sauvegarde de l'ordinateur et de l'historique des fichiers.**  
  
    > [!NOTE]
    >  Dans Windows Server Essentials, cliquez sur **les tâches de sauvegarde des ordinateurs clients**.  
  
4.  Dans **Paramètres et outils de sauvegarde d'ordinateurs clients**, cliquez sur l'onglet **Outils** .  
  
5.  Dans la section **Réparer des sauvegardes**, cliquez sur **Réparer maintenant**. L'Assistant Réparation de la base de données de sauvegarde s'ouvre.  
  
6.  Suivez les instructions de l'Assistant Réparation de la base de données de sauvegarde.  
  
7.  Selon la taille de la base de données de sauvegarde, sa réparation peut prendre plusieurs heures. Cliquez sur **Fermer**, puis sur **OK** pour fermer la page **Personnaliser les paramètres de la sauvegarde de l'ordinateur et de l'historique des fichiers** .  
  
    > [!NOTE]
    >  Dans Windows Server Essentials, cliquez sur **les paramètres de sauvegarde des ordinateurs clients et outils**.  
  
#### <a name="to-review-the-results-of-the-backup-database-repair"></a>Pour passer en revue les résultats de la réparation de la base de données de sauvegarde  
  
1.  Ouvrez le **Tableau de bord**.  
  
2.  Cliquez sur **Périphériques**.  
  
3.  Dans le volet **Tâches du périphérique** , cliquez sur **Personnaliser les paramètres de la sauvegarde de l'ordinateur et de l'historique des fichiers.**  
  
    > [!NOTE]
    >  Dans Windows Server Essentials, cliquez sur **les tâches de sauvegarde des ordinateurs clients**.  
  
4.  Dans **Paramètres et outils de sauvegarde d'ordinateurs clients**, cliquez sur l'onglet **Outils** .  
  
5.  Les résultats sont affichés dans la section **Réparer des sauvegardes** .  
  
##  <a name="BKMK_9"></a> Désactiver la sauvegarde pour un ordinateur  
 Utilisez le tableau de bord pour désactiver rapidement les sauvegardes d'ordinateurs sur votre réseau.  
  
#### <a name="to-disable-backup-for-a-computer"></a>Pour désactiver la sauvegarde d'un ordinateur  
  
1. Ouvrez le **Tableau de bord**.  
  
2. Cliquez sur l'onglet **Périphériques**.  
  
3. Cliquez sur le nom d'un ordinateur dont vous souhaitez désactiver les sauvegardes.  
  
4. Dans le volet Tâches, cliquez sur **Personnaliser la sauvegarde pour l'ordinateur**. L'Assistant Personnalisation de la sauvegarde s'affiche.  
  
5. Cliquez sur **Désactiver la sauvegarde pour cet ordinateur**, puis indiquez si vous souhaitez conserver ou supprimer les fichiers de sauvegarde existants.  
  
6. Cliquez sur **Enregistrer les modifications**, puis sur **Fermer**.  
  
   Pour plus d’informations sur la réactivation de la sauvegarde pour un ordinateur, consultez [Set up backup for a client computer](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_3).  
  
##  <a name="BKMK_10"></a> Exécuter la tâche de nettoyage de sauvegarde  
 La tâche de nettoyage des sauvegardes d'ordinateurs clients est planifiée pour s'exécuter à 11 h 59 tous les samedis, une fois que toutes les sauvegardes ont été effectuées. La tâche de nettoyage supprime les sauvegardes de la base de données de sauvegarde des ordinateurs clients en fonction de la stratégie de rétention de sauvegarde. Les paramètres par défaut de la stratégie de rétention de sauvegarde sont les suivants :  
  
- Nombre de jours pendant lesquels les sauvegardes quotidiennes sont conservées : 5 jours  
  
- Nombre de semaines pendant lesquelles la dernière sauvegarde de la semaine est conservée : 4 semaines  
  
- Nombre de mois pendant lesquels la dernière sauvegarde du mois est conservée : 6 mois  
  
  Pour plus d'informations sur le changement de la stratégie de rétention de sauvegarde, voir [Changer la stratégie de rétention des sauvegardes d'ordinateurs](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_5).  
  
#### <a name="to-run-the-client-backup-database-cleanup-task"></a>Pour exécuter la tâche de nettoyage de la base de données de sauvegarde des clients  
  
1.  Connectez-vous au serveur à l'aide de votre compte Administrateur et de votre mot de passe.  
  
2.  Sur le serveur, ouvrez le **Planificateur de tâches**. Vous pouvez accéder au programme du Planificateur de tâches à partir de la console **Outils d'administration** .  
  
3.  Dans le Planificateur de tâches, développez **Bibliothèque du Planificateur de tâches**, développez **Microsoft**, développez **Windows**, puis cliquez sur **Windows Server Essentials**.  
  
4.  Cliquez sur la tâche **Nettoyage des sauvegardes** , puis sur **Exécuter** dans le volet **Actions** . L'état devient **En cours d'exécution** jusqu'à ce que la tâche soit finie.  
  
##  <a name="BKMK_11"></a> Afficher les alertes dans la barre des tâches sur un ordinateur client  
 Vous pouvez éventuellement recevoir une notification de sauvegarde dans la barre des tâches de votre ordinateur pour les raisons suivantes :  
  
-   Une sauvegarde a démarré.  
  
-   Une sauvegarde s'est achevée.  
  
-   Il n'existe aucune sauvegarde récente réussie. Le nombre de jours écoulés depuis la dernière sauvegarde réussie est inclus dans la notification.  
  
-    Windows Server Essentials uniquement : Un nouveau volume est ajouté à votre ordinateur. La personne qui administre votre réseau doit exécuter l'Assistant Personnalisation de la sauvegarde afin d'inclure ou exclure le volume pour les futures sauvegardes.  
  
##  <a name="BKMK_12"></a> Afficher l’état de sauvegarde à partir du Launchpad  
 Utilisez Launchpad pour afficher l'état rapide de la sauvegarde de votre ordinateur.  
  
> [!TIP]
>  Pour obtenir un état rapide, déplacez le curseur sur l'icône de Launchpad dans la zone de notification du Bureau. L'état de la sauvegarde s'affiche dans l'info-bulle.  
  
#### <a name="to-view-status-of-a-backup-for-your-computer"></a>Pour afficher l'état d'une sauvegarde pour votre ordinateur  
  
1.  Connectez-vous à **Launchpad** à l'aide de votre nom d'utilisateur et votre mot de passe.  
  
2.  Cliquez sur **Sauvegarde**.  
  
3.  Dans la boîte de dialogue **Propriétés de la sauvegarde**, dans la section **État de la sauvegarde**, l'état de la sauvegarde s'affiche.  
  
    -   **Aucune sauvegarde antérieure** : soit la sauvegarde ne s'est pas encore exécutée, soit l'historique de sauvegarde a été supprimé.  
  
    -   **Sauvegarde en attente** : une sauvegarde attend qu'un autre processus s'achève sur le serveur.  
  
    -   **Connexion en cours** : la sauvegarde se connecte au serveur.  
  
    -   **Impossible de se connecter** : la sauvegarde ne peut pas se connecter au Service fournisseur de sauvegarde Windows Server pour ordinateurs client.  
  
        > [!NOTE]
        >  Dans Windows Server Essentials, ce service a été renommé Service de gestion d’ordinateur Client Windows Server Essentials.  
  
    -   **Sauvegarde en cours** : affiche le pourcentage de sauvegarde effectuée.  
  
    -   **La sauvegarde a été annulée** : affiche la date et l'heure de début de la sauvegarde si vous ou l'administrateur réseau avez annulé la sauvegarde.  
  
    -   **Sauvegarde réussie** : affiche la date et l'heure de début de la sauvegarde si cette dernière s'est correctement effectuée.  
  
    -   **La sauvegarde est incomplète** : Affiche la date et l'heure de début de la sauvegarde si la sauvegarde a échoué.  
  
    -   **Échec de la sauvegarde** : Affiche la date et l'heure de début de la sauvegarde si la sauvegarde a échoué.  
  
4.  Cliquez sur **OK** pour fermer la boîte de dialogue **Propriétés de la sauvegarde**.  
  
##  <a name="BKMK_13"></a> Arrêter une sauvegarde en cours d’exécution à partir du Launchpad  
 vous pouvez facilement arrêter une sauvegarde en cours d'exécution.  
  
#### <a name="to-stop-a-backup-in-progress"></a>Pour arrêter une sauvegarde en cours  
  
1.  Ouvrez **Launchpad**.  
  
    > [!NOTE]
    >  Si vous y êtes invité, connectez-vous à **Launchpad** avec votre nom d'utilisateur et votre mot de passe.  
  
2.  Cliquez sur **Sauvegarde**.  
  
3.  Dans la boîte de dialogue **Propriétés de la sauvegarde** , dans la section **État de la sauvegarde** , le bouton **Démarrer la sauvegarde** devient **Arrêter la sauvegarde** quand cette dernière est en cours d'exécution. Cliquez sur **Arrêter la sauvegarde**, puis sur **Oui** pour confirmer. La sauvegarde continue de s'exécuter jusqu'à ce que vous cliquiez sur **Oui**.  
  
4.  Quand vous arrêtez une sauvegarde en cours d'exécution, l'état passe à **La sauvegarde a été annulée** en indiquant la date et l'heure de début de la sauvegarde. Si, à la place, vous voyez l'état **Réussite**, **Incomplet**ou **Échec**, cela signifie que la dernière sauvegarde est déjà finie.  
  
5.  Cliquez sur **OK** pour fermer la boîte de dialogue **Propriétés de la sauvegarde**.  
  
##  <a name="BKMK_14"></a> Démarrer une sauvegarde à partir du Launchpad  
 Parfois, vous souhaitez sauvegarder vos fichiers et dossiers avant l'heure de la sauvegarde planifiée périodique configurée sur votre serveur. Launchpad vous permet de démarrer la sauvegarde de votre ordinateur manuellement.  
  
#### <a name="to-start-a-backup"></a>Pour démarrer une sauvegarde  
  
1.  Ouvrez **Launchpad**.  
  
    > [!NOTE]
    >  Si vous y êtes invité, connectez-vous à **Launchpad** avec votre nom d'utilisateur et votre mot de passe.  
  
2.  Cliquez sur **Sauvegarde**.  
  
3.  Dans la boîte de dialogue **Propriétés de la sauvegarde** , dans la section **État de la sauvegarde** , cliquez sur **Démarrer la sauvegarde**, puis sur **OK**.  
  
4.  Tapez une description de la sauvegarde, puis cliquez sur **OK**. L'état passe de **Démarrage de la sauvegarde** à **Sauvegarde en cours** avec un pourcentage d'achèvement.  
  
5.  Une fois la sauvegarde démarrée, le bouton indique **Arrêter la sauvegarde**. Si la sauvegarde est en cours d'exécution et si vous souhaitez l'arrêter, cliquez sur **Arrêter la sauvegarde**, puis sur **Oui**. Quand vous arrêtez une sauvegarde en cours d'exécution, l'état passe à **La sauvegarde a été annulée** en indiquant la date et l'heure de début de la sauvegarde.  
  
6.  Quand la sauvegarde réussit, son état devient **Sauvegarde réussie**. Par ailleurs, la date et l'heure de début de la sauvegarde effectuée sont indiquées.  
  
7.  Cliquez sur **OK** pour fermer la boîte de dialogue **Propriétés de la sauvegarde**.  
  
##  <a name="BKMK_15"></a> Fonctionne de la sauvegarde de l’ordinateur  
 Les événements suivants se produisent quotidiennement durant la sauvegarde :  
  
-   Les ordinateurs du réseau sont sauvegardés l'un après l'autre.  
  
-   La sauvegarde en cours d'exécution va à son terme, même si vous fermez la fenêtre de sauvegarde.  
  
-   Les mises à jour Windows sont installées, et Windows Server Essentials redémarre (si nécessaire).  
  
-   Le dimanche, le nettoyage des sauvegardes s'exécute.  
  
> [!IMPORTANT]
>  Le service de cliché instantané de volumes (VSS, Volume Shadow Copy Service) ne prend pas en charge la création d'un cliché instantané d'un volume virtuel et du volume hôte dans le même jeu d'instantanés. VSS prend en charge la création de clichés instantanés de volumes sur un disque dur virtuel (VHD), si la sauvegarde du volume virtuel est nécessaire. Pour plus d’informations, consultez [Maintenance et sauvegarde des disques durs virtuels](https://go.microsoft.com/fwlink/p/?LinkId=256577).  
  
##  <a name="BKMK_16"></a> Conseils pour éviter la perte de données en raison d’une altération de la base de données de sauvegarde de client  
 Si la base de données de sauvegarde des clients est endommagée, vous risquez de perdre des données critiques.  
  
 Pour éviter de perdre des données après l'altération de la base de données de sauvegarde des clients, tenez compte des éléments suivants :  
  
-   Activez une méthode supplémentaire de sauvegarde de la base de données de sauvegarde des clients. Par exemple, selon la version de Windows Server Essentials vous sont en cours d’exécution, utilisez la sauvegarde du serveur, la sauvegarde en ligne ou Microsoft Azure Backup pour sauvegarder la base de données.  
  
    > [!IMPORTANT]
    >  Veillez à sauvegarder tous les fichiers dans le dossier **Sauvegarde des ordinateurs client** .  
  
-   Au cas où vous devriez restaurer la base de données, veillez à restaurer tous les fichiers dans le dossier **Sauvegarde des ordinateurs client** . Si vous ne restaurez pas tous les fichiers, la base de données risque de devenir incohérente.  
  
-   Avant de nettoyer ou restaurer la base de données de sauvegarde des clients, veillez à arrêter toutes les sauvegardes de clients en cours d'exécution.  
  
     Si vous exécutez Windows Server Essentials, vous devez également arrêter le Service de sauvegarde ordinateur Client Windows Server et le Service du fournisseur de sauvegarde de Windows Server Client ordinateur.  
  
     Si vous exécutez Windows Server Essentials, vous devez également arrêter le Service de sauvegarde de Windows Server ordinateur.  
  
     Après avoir effectué l'opération de restauration, redémarrez le service.  
  
##  <a name="BKMK_17"></a> Restaurer des fichiers ou dossiers à partir d’une sauvegarde de l’ordinateur client  
 Vous pouvez parcourir et restaurer des fichiers et des dossiers individuels à partir d'une sauvegarde.  
  
> [!NOTE]
>  Vous ne pouvez pas restaurer des fichiers et des dossiers sur un ordinateur client à l'aide du tableau de bord du serveur. Vous devez ouvrir le tableau de bord sur un ordinateur client pour effectuer la tâche.  
  
> [!IMPORTANT]
>  Vous ne pouvez pas restaurer de fichiers et dossiers sur un disque dur configuré pour être un disque dynamique.  
  
#### <a name="to-restore-files-and-folders"></a>Pour restaurer des fichiers et des dossiers  
  
1.  Ouvrez le **Tableau de bord** sur un ordinateur client.  
  
2.  Cliquez sur l'onglet **Périphériques**.  
  
3.  Cliquez sur l'ordinateur dont vous souhaitez restaurer des fichiers et des dossiers, puis cliquez sur **Restaurer des fichiers ou des dossiers de l'ordinateur** dans le volet **Tâches** . L'Assistant Restauration de fichiers ou de dossiers s'ouvre.  
  
4.  Suivez les instructions de l'Assistant.  
  
##  <a name="BKMK_FileHistory"></a> Comprendre l’historique des fichiers  
 La fonctionnalité Historique des fichiers de Windows Server Essentials sauvegarde automatiquement les fichiers qui se trouvent dans les dossiers Bibliothèques, Contacts, Bureau et Favoris des ordinateurs réseau disposant de la fonctionnalité Historique des fichiers. Si les originaux sont perdus, endommagés ou supprimés, vous pouvez les restaurer. Vous trouverez également différentes versions de vos fichiers à partir d'une date et heure spécifiques. Au fil du temps, vous disposerez d'un historique complet de vos fichiers.  
  
 Dans Windows Server Essentials, vous pouvez personnaliser les paramètres de l’historique des fichiers à partir de la **l’historique des fichiers** page de **les paramètres de sauvegarde des ordinateurs clients et outils**.  
  
 Dans Windows Server Essentials, vous pouvez personnaliser les paramètres de l’historique des fichiers en accédant à la **utilisateurs** onglet, puis en sélectionnant le **modifier le paramètre de l’historique des fichiers** tâche.  
  
 La page Historique des fichiers fournit les options suivantes :  
  
|Paramètre de sauvegarde|Par défaut|Description|  
|--------------------|-------------|-----------------|  
|Activer/Désactiver|Activé|L'historique des fichiers est activé par défaut quand vous installez Windows Server Essentials.|  
|Données de sauvegarde|Documents et bureau|Il existe trois paramètres préconfigurés, ce qui vous permet de spécifier toute une série de solutions de sauvegarde. Vous pouvez choisir l'une des options suivantes :<br /><br /> -Les documents et de bureau<br /><br /> -Toutes les bibliothèques, bureau, Contacts et Favoris<br /><br /> -Toutes les données dans les bibliothèques, bureau, Contacts et Favoris à l’exclusion des données dans la musique, vidéos et images|  
|Fréquence de sauvegarde|Toutes les heures|Spécifie la fréquence à laquelle l'historique des fichiers crée une sauvegarde des données sélectionnées. Vous pouvez choisir parmi plusieurs options allant de toutes les 10 minutes à tous les jours.|  
|Conserver les copies pendant|1 an|Spécifie la durée pendant laquelle l'historique des fichiers conserve une copie d'une sauvegarde.|  
  
 Pour plus d'informations sur les problèmes liés à l'historique des fichiers, voir [Résoudre les problèmes de l'historique des fichiers](../support/Troubleshoot-File-History-in-Windows-Server-Essentials.md).  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Gérer la sauvegarde et restauration](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Gérer Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Utiliser Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
