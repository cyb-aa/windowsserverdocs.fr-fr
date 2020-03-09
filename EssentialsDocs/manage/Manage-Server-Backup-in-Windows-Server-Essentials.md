---
title: Gérer la sauvegarde du serveur dans Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0302d070-c58a-40f2-b56d-7e7842813d02
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 7e40a4675cf77d55a3047b41e0ab852fd7cd9de9
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78371202"
---
# <a name="manage-server-backup-in-windows-server-essentials"></a>Gérer la sauvegarde du serveur dans Windows Server Essentials

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
 Les rubriques suivantes contiennent des informations sur les tâches de sauvegarde courantes que vous pouvez accomplir à l'aide du tableau de bord Windows Server Essentials :  
  
-   [Quelle sauvegarde dois-je choisir ?](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_WhichBackup)  
  
-   [Configurer ou personnaliser la sauvegarde du serveur](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Arrêt de la sauvegarde du serveur en cours](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Gérer à distance vos sauvegardes](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Désactiver la sauvegarde du serveur](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [En savoir plus sur la configuration de la sauvegarde du serveur](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Repartitionner un disque dur sur le serveur](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [Restaurer des fichiers et des dossiers à partir d’une sauvegarde du serveur](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_7)  
  
##  <a name="BKMK_WhichBackup"></a>Quelle sauvegarde dois-je choisir ?  
 Si vous disposez d'une sauvegarde très récente et fiable contenant l'intégralité de vos données critiques, vous n'avez aucune hésitation à avoir. Si vous essayez de restaurer une sauvegarde antérieure sur le serveur ou un ordinateur, le choix de la sauvegarde à restaurer peut demander une certaine réflexion et, éventuellement, des compromis.  
  
#### <a name="to-choose-a-backup"></a>Pour choisir une sauvegarde  
  
1.  Contactez le ou les propriétaires des fichiers ou des dossiers, puis notez les dates et heures auxquelles ils les ont ajoutés ou modifiés. Utilisez ces dates et heures comme point de départ.  
  
2.  Dans la page **Choisir une option de restauration** de l'Assistant Restauration de fichiers ou de dossiers, cliquez sur **Restaurer à partir d'une sauvegarde sélectionnée (avancé)** .  
  
3.  Selon que vous souhaitez restaurer une version plus ancienne ou plus récente des fichiers ou des dossiers, sélectionnez la sauvegarde qui convient le mieux aux dates et heures notées à l'étape 1.  
  
4.  Il est recommandé de restaurer les fichiers et les dossiers vers un autre emplacement, puis de laisser le propriétaire des fichiers et des dossiers déplacer ceux dont il a besoin vers leur emplacement d'origine. À la fin de l'opération, les fichiers et dossiers qui restent dans l'autre emplacement peuvent être supprimés.  
  
##  <a name="BKMK_1"></a>Configurer ou personnaliser la sauvegarde du serveur  
 La sauvegarde du serveur n'est pas configurée automatiquement pendant l'installation. Nous vous conseillons de protéger automatiquement votre serveur et ses données en planifiant des sauvegardes quotidiennes. Il est recommandé de mettre en place un plan de sauvegarde quotidien, car la plupart des organisations ne peuvent pas se permettre de perdre plusieurs jours de données. Pour plus d'informations, voir [Configurer ou personnaliser la sauvegarde du serveur](Set-up-or-customize-server-backup.md).  
  
##  <a name="BKMK_2"></a>Arrêt de la sauvegarde du serveur en cours  
 Qu'une sauvegarde du serveur démarre à une heure régulière planifiée ou que vous démarriez une sauvegarde du serveur manuellement, vous pouvez arrêter la sauvegarde en cours d'exécution.  
  
#### <a name="to-stop-a-backup-in-progress"></a>Pour arrêter une sauvegarde en cours  
  
1.  Ouvrez le Tableau de bord.  
  
2.  Dans la barre de navigation, cliquez sur **Périphériques**.  
  
3.  Dans la liste des ordinateurs, cliquez sur le serveur, puis sur **Arrêter la sauvegarde pour le serveur** dans le volet **Tâches**.  
  
4.  Cliquez sur **Oui** pour confirmer votre action.  
  
##  <a name="BKMK_3"></a>Gérer à distance vos sauvegardes  
 Quand vous êtes en déplacement, vous pouvez utiliser l'accès web à distance Windows Server Essentials pour accéder au tableau de bord Windows Server Essentials et gérer votre serveur.  
  
#### <a name="to-use-remote-web-access-to-manage-your-server"></a>Pour utiliser l'accès web à distance pour gérer votre serveur  
  
1. Ouvrez un navigateur web.  
  
2. Dans la zone d'adresse, tapez le nom du domaine Windows Server Essentials.  
  
3. Quand vous y êtes invité, entrez votre nom d'utilisateur et votre mot de passe.  
  
4. Lorsque vous cliquez sur le nom du serveur dans la Accès web distante, la page d’ouverture de session du tableau de bord s’affiche.  
  
5. Connectez-vous au tableau de bord en tant qu'administrateur, puis cliquez sur **Périphériques**.  
  
   Pour plus d’informations sur les Accès web distants, consultez [vue d’ensemble des accès Web à distance](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Overview).  
  
##  <a name="BKMK_4"></a>Désactiver la sauvegarde du serveur  
 Nous vous conseillons de protéger automatiquement votre serveur et ses données en planifiant des sauvegardes quotidiennes. Il est recommandé de mettre en place un plan de sauvegarde quotidien, car la plupart des organisations ne peuvent pas se permettre de perdre plusieurs jours de données.  
  
 Si vous avez déjà configuré la sauvegarde du serveur et si vous souhaitez utiliser plus tard une application tierce pour sauvegarder le serveur, vous pouvez désactiver la sauvegarde du serveur Windows Server Essentials.  
  
#### <a name="to-disable-server-backup"></a>Pour désactiver la sauvegarde du serveur  
  
1.  Connectez-vous au tableau de bord Windows Server Essentials à l'aide d'un compte Administrateur et du mot de passe correspondant.  
  
2.  Cliquez sur l'onglet **Périphériques**, puis sur le nom du serveur.  
  
3.  Dans le volet Tâches, cliquez sur **Personnaliser la sauvegarde pour le serveur**.  
  
    > [!NOTE]
    >  La tâche **Personnaliser la sauvegarde pour le serveur** s'affiche une fois que vous avez configuré la sauvegarde du serveur à l'aide de l'Assistant Configuration de la sauvegarde du serveur. Pour plus d'informations sur la configuration de la sauvegarde du serveur, voir [Configurer ou personnaliser la sauvegarde du serveur](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_1).  
  
4.  L'Assistant Personnalisation de la sauvegarde du serveur s'affiche.  
  
5.  Dans la page **Options de Configuration** , cliquez sur **Désactiver la sauvegarde du serveur**. Suivez les instructions de l'Assistant.  
  
##  <a name="BKMK_5"></a>En savoir plus sur la configuration de la sauvegarde du serveur  
 La sauvegarde du serveur n'est pas activée durant l'installation du serveur.  
  
> [!NOTE]
>  Quand vous configurez la sauvegarde du serveur, vous devez connecter au moins un disque dur externe au serveur pour l'utiliser en tant que disque dur de destination de la sauvegarde.  
  
###  <a name="BKMK_Target"></a>Sauvegarder le lecteur de destination  
 Vous pouvez utiliser plusieurs lecteurs de stockage externe pour vos sauvegardes et les entreposer à tour de rôle dans des emplacements sur site et hors site. Cela peut vous aider à renforcer votre stratégie de planification d'urgence, car vous pourrez récupérer vos données si votre matériel local subit des dommages.  
  
 Au moment d'acquérir un lecteur de stockage pour sauvegarder votre serveur, tenez compte des points suivants :  
  
-   Choisissez un lecteur avec suffisamment d'espace pour stocker vos données. Les lecteurs de stockage doivent disposer d'une capacité de stockage au moins égale à 2,5 fois la quantité de données à sauvegarder. La capacité de stockage des lecteurs doit aussi être suffisamment grande pour prendre en charge la croissance future des données serveur.  
  
-   Si le disque de destination de sauvegarde contient des lecteurs hors connexion, la configuration de la sauvegarde échoue. Pour achever la configuration, quand vous sélectionnez la destination de sauvegarde, décochez la case pour exclure les lecteurs hors connexion.  
  
-   Si vous choisissez un lecteur qui contient des sauvegardes antérieures comme destination de sauvegarde, l'Assistant vous permet de choisir si vous souhaitez conserver ces sauvegardes antérieures. Si vous conservez les sauvegardes, l'Assistant ne formate pas le lecteur.  
  
-   Quand vous réutilisez un lecteur de stockage externe, assurez-vous que le lecteur est vide ou qu'il contient uniquement des données dont vous n'avez pas besoin.  
  
-   Visitez le site web du fabricant de votre lecteur de stockage externe pour vous assurer que ce dernier est pris en charge par les ordinateurs exécutant Windows Server Essentials.  
  
> [!CAUTION]
>  L'Assistant Configurer la sauvegarde du serveur formate les lecteurs de stockage quand il les configure pour la sauvegarde.  
  
### <a name="server-backup-schedule"></a>Planification de la sauvegarde du serveur  
 Nous vous conseillons de protéger automatiquement votre serveur et ses données en planifiant des sauvegardes quotidiennes. Il est recommandé de mettre en place un plan de sauvegarde quotidien, car la plupart des organisations ne peuvent pas se permettre de perdre plusieurs jours de données.  
  
 Quand vous utilisez l'Assistant Configuration de la sauvegarde du serveur Windows Server Essentials, vous pouvez choisir de sauvegarder les données du serveur plusieurs fois durant la journée. Dans la mesure où l'Assistant permet de planifier des sauvegardes différentielles, la sauvegarde s'exécute rapidement. En outre, les performances du serveur sont peu affectées. Par défaut, la fonctionnalité **Configurer la sauvegarde du serveur** planifie l'exécution quotidienne d'une sauvegarde à 12 h 00 et 23 h 00. Vous pouvez toutefois modifier la planification de la sauvegarde en fonction des besoins de votre organisation. Nous vous conseillons d'évaluer de temps en temps l'efficacité de votre plan de sauvegarde et de le modifier en conséquence.  
  
> [!NOTE]
>  Dans l'installation par défaut de Windows Server Essentials, le serveur est configuré pour effectuer automatiquement une défragmentation une fois par semaine. Si vous utilisez un logiciel de création d'images qui n'est pas fourni par Microsoft, la taille de vos sauvegardes peut être supérieure à la normale. S'il est inutile de défragmenter régulièrement le serveur, vous pouvez suivre ces étapes pour désactiver la planification de la défragmentation :  
> 
> 1. Appuyez sur la touche Windows + W pour ouvrir **Rechercher**.  
>    2. Dans la zone de texte Rechercher, tapez **Defragment**.  
>    3. Dans la section de résultats, cliquez sur **Défragmenter et optimiser les lecteurs**.  
>    4. Dans la page **Optimiser les lecteurs**, sélectionnez un lecteur, puis cliquez sur **Modifier les paramètres**.  
>    5. Dans la fenêtre **Planification de l'optimisation**, décochez la case **Exécution planifiée (recommandé)** , puis cliquez sur **OK** pour enregistrer les modifications.  
  
### <a name="items-to-be-backed-up"></a>Éléments à sauvegarder  
 Par défaut, tous les fichiers et dossiers du système d'exploitation sont sélectionnés pour la sauvegarde. Vous pouvez sauvegarder tous les disques durs, fichiers et dossiers situés sur le serveur, ou sélectionner uniquement certains disques durs, fichiers ou dossiers à sauvegarder. Pour ajouter ou supprimer des éléments à sauvegarder, effectuez l'une des opérations suivantes :  
  
-   Pour inclure un lecteur de données dans la sauvegarde du serveur, cochez la case adjacente.  
  
-   Pour exclure un lecteur de données de la sauvegarde du serveur, décochez la case adjacente.  
  
> [!NOTE]
>  Si vous souhaitez exclure l'élément **Système d'exploitation** de la sauvegarde, vous devez d'abord décocher la case **Sauvegarde système (recommandé)** .  
  
 Pour réduire la quantité d'espace disque utilisée par vos sauvegardes du serveur, vous pouvez exclure les dossiers qui contiennent des fichiers peu importants à vos yeux.  
  
 Par exemple, vous possédez peut-être un dossier contenant des programmes de télévision enregistrés qui prend beaucoup de place sur le disque. Vous pouvez choisir ne pas de sauvegarder ces fichiers, car vous les supprimez de toute façon après les avoir regardés. Vous pouvez également avoir un dossier qui contient les fichiers temporaires que vous ne souhaitez pas conserver.  
  
##  <a name="BKMK_6"></a>Repartitionner un disque dur sur le serveur  
 Quand un lecteur de disque dur interne non formaté est détecté sur le serveur Windows Server Essentials, cela entraîne le déclenchement d'une alerte d'intégrité qui contient un lien vers l'Assistant Ajout d'un nouveau disque dur. L'Assistant Ajout d'un nouveau disque dur vous guide parmi les différentes options de formatage du disque dur. À la fin de l'exécution de l'Assistant, un ou plusieurs (selon la taille du lecteur) disques durs logiques formatés sont créés sur le disque dur et formatés en NTFS.  
  
 S'il s'avère nécessaire de repartitionner un lecteur de disque dur, suivez les instructions ci-après :  
  
#### <a name="to-repartition-a-hard-disk-drive"></a>Pour repartitionner un lecteur de disque dur  
  
1.  Dans l'écran d'**accueil**, cliquez sur **Outils d'administration**, puis double-cliquez sur **Gestion de l'ordinateur**.  
  
2.  Dans Gestion de l'ordinateur, cliquez sur **Stockage**, puis double-cliquez sur **Gestion des disques**.  
  
3.  Cliquez avec le bouton droit sur le lecteur à repartitionner, cliquez sur **Supprimer le volume**, puis sur **Oui**.  
  
    > [!NOTE]
    >  Répétez cette étape pour chaque partition du lecteur de disque dur.  
  
4.  Cliquez avec le bouton droit sur le lecteur de disque dur **Non alloué**, puis cliquez sur **Nouveau volume simple**.  
  
5.  Dans l'Assistant Création d'un volume simple, créez et formatez un volume pouvant aller jusqu'à 16 To (16 000 000 Mo) au maximum.  
  
    > [!NOTE]
    >  Répétez cette étape jusqu'à ce que tout l'espace non alloué sur le lecteur de disque dur soit utilisé.  
  
##  <a name="BKMK_7"></a>Restaurer des fichiers et des dossiers à partir d’une sauvegarde du serveur  
 Vous pouvez parcourir et restaurer des fichiers et des dossiers individuels à partir d'une sauvegarde du serveur.  
  
#### <a name="to-restore-files-and-folders-from-a-server-backup"></a>Pour restaurer des fichiers et des dossiers à partir d'une sauvegarde du serveur  
  
1.  Ouvrez le tableau de bord, puis cliquez sur l'onglet **Périphériques**.  
  
2.  Cliquez sur le nom du serveur, puis sur **Restaurer des fichiers ou des dossiers du serveur** dans le volet **Tâches**.  
  
3.  L'Assistant Restauration de fichiers ou de dossiers s'ouvre. Suivez les instructions de l'Assistant pour restaurer les fichiers ou dossiers.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Gérer la sauvegarde et la restauration](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Gérer Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Utiliser Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
