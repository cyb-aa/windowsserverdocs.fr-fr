---
title: "Gérer la sauvegarde de serveur dans Windows Server Essentials"
description: "Décrit comment utiliser WindowsServerEssentials"
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
ms.openlocfilehash: 2b0cd926b15d65e5cd4c784681c40df29b18a48f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="manage-server-backup-in-windows-server-essentials"></a>Gérer la sauvegarde de serveur dans Windows Server Essentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials
  
 Les rubriques suivantes contiennent des informations sur les tâches de sauvegarde courantes que vous pouvez accomplir à l’aide du tableau de bord Windows Server Essentials:  
  
-   [Quelle sauvegarde dois-je choisir?](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_WhichBackup)  
  
-   [Configurer ou personnaliser la sauvegarde du serveur](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Arrêter la sauvegarde du serveur en cours](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Gérer à distance vos sauvegardes](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Désactiver la sauvegarde du serveur](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [En savoir plus sur la définition de la sauvegarde du serveur](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Repartitionner un disque dur sur le serveur](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [Restaurer des fichiers et dossiers à partir d’une sauvegarde du serveur](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_7)  
  
##  <a name="BKMK_WhichBackup"></a>Quelle sauvegarde dois-je choisir?  
 Choix d’une sauvegarde peut être simple si vous disposez d’une sauvegarde très récente et que vous savez que la sauvegarde contient toutes vos données critiques. Si vous tentez de restaurer sur le serveur ou un ordinateur à partir d’une sauvegarde ancienne, le choix d’une sauvegarde à restaurer à peut nécessiter certaine réflexion et, éventuellement, certaines compromettre.  
  
#### <a name="to-choose-a-backup"></a>Pour choisir une sauvegarde  
  
1.  Vérifiez auprès du propriétaire des fichiers ou dossiers et notez les dates et heures lorsqu’ils ajoutés ou modifiés. Utilisez ces dates et heures comme point de départ.  
  
2.  Sur le **choisir une option de restauration** dans l’Assistant de dossiers et de restauration de fichiers, cliquez sur **restaurer à partir d’une sauvegarde sélectionnée (avancée)**.  
  
3.  Selon que vous souhaitez restaurer une version plus ancienne ou plus récente des fichiers ou dossiers, sélectionnez la sauvegarde qui répond le mieux aux dates et heures notées à l’étape 1.  
  
4.  Comme meilleure pratique, vous pouvez restaurer des fichiers et dossiers vers un autre emplacement, puis de laisser le propriétaire des fichiers et dossiers déplacer ceux dont ils ont besoin pour l’emplacement d’origine. Lorsqu’ils ont terminé, les fichiers et dossiers qui restent dans l’autre emplacement peuvent être supprimés.  
  
##  <a name="BKMK_1"></a>Configurer ou personnaliser la sauvegarde du serveur  
 Sauvegarde du serveur n’est pas configurée automatiquement pendant l’installation. Vous devez protéger automatiquement votre serveur et ses données en planifiant des sauvegardes quotidiennes. Il est recommandé de mettre à jour un plan de sauvegarde quotidien, car la plupart des organisations ne peut pas se permettre de perdre les données qui a été créées sur plusieurs jours. Pour plus d’informations, voir [définir configurer ou personnaliser la sauvegarde du serveur](Set-up-or-customize-server-backup.md).  
  
##  <a name="BKMK_2"></a>Arrêter la sauvegarde du serveur en cours  
 Si une sauvegarde du serveur démarre à une heure régulière planifiée ou commencer une sauvegarde du serveur manuellement, vous pouvez arrêter la sauvegarde en cours.  
  
#### <a name="to-stop-a-backup-in-progress"></a>Pour arrêter une sauvegarde en cours  
  
1.  Ouvrez le tableau de bord.  
  
2.  Dans la barre de navigation, cliquez sur **périphériques**.  
  
3.  Dans la liste des ordinateurs, cliquez sur le serveur, puis cliquez sur **arrêter la sauvegarde pour le serveur** dans les **tâches** volet.  
  
4.  Cliquez sur **Oui** pour confirmer votre action.  
  
##  <a name="BKMK_3"></a>Gérer à distance vos sauvegardes  
 Lorsque vous êtes en déplacement, vous pouvez utiliser accès Web à distance de Windows Server Essentials pour le tableau de bord Windows Server Essentials pour gérer votre serveur d’accès.  
  
#### <a name="to-use-remote-web-access-to-manage-your-server"></a>Pour utiliser l’accès Web à distance pour gérer votre serveur  
  
1.  Ouvrez un navigateur web.  
  
2.  Dans la zone Adresse, tapez le nom du domaine Windows Server Essentials.  
  
3.  Lorsque vous y êtes invité, entrez votre nom d’utilisateur et un mot de passe.  
  
4.  Lorsque vous cliquez sur le nom du serveur dans accès Web à distance, la page d’ouverture de session pour le tableau de bord s’affiche.  
  
5.  Ouvrez une session sur le tableau de bord en tant qu’administrateur, puis cliquez sur **périphériques**.  
  
 Pour plus d’informations sur l’accès Web à distance, voir [vue d’ensemble de l’accès Web à distance](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Overview).  
  
##  <a name="BKMK_4"></a>Désactiver la sauvegarde du serveur  
 Vous devez protéger automatiquement votre serveur et ses données en planifiant des sauvegardes quotidiennes. Il est recommandé de mettre à jour un plan de sauvegarde quotidien, car la plupart des organisations ne peut pas se permettre de perdre les données qui a été créées sur plusieurs jours.  
  
 Si vous avez déjà configuré la sauvegarde du serveur et souhaitez utiliser une application tierce pour sauvegarder le serveur plus tard, vous pouvez désactiver la sauvegarde du serveur Windows Server Essentials.  
  
#### <a name="to-disable-server-backup"></a>Pour désactiver la sauvegarde du serveur  
  
1.  Ouvrez une session sur le bord Windows Server Essentials à l’aide d’un compte d’administrateur et le mot de passe.  
  
2.  Cliquez sur le **périphériques** onglet, puis cliquez sur le nom du serveur.  
  
3.  Dans le volet tâches, cliquez sur **personnaliser la sauvegarde pour le serveur**.  
  
    > [!NOTE]
    >  Le **personnaliser la sauvegarde pour le serveur** tâche s’affiche une fois que vous avez configuré la sauvegarde du serveur à l’aide de l’Assistant Configuration du serveur sauvegarde. Pour plus d’informations sur la configuration de la sauvegarde du serveur, consultez [définir configurer ou personnaliser la sauvegarde du serveur](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_1).  
  
4.  L’Assistant Personnalisation de sauvegarde de serveur s’affiche.  
  
5.  Sur le **Options de Configuration** , cliquez sur **désactiver la sauvegarde du serveur**. Suivez les instructions de l’Assistant.  
  
##  <a name="BKMK_5"></a>En savoir plus sur la définition de la sauvegarde du serveur  
 Sauvegarde du serveur n’est pas activée pendant l’installation du serveur.  
  
> [!NOTE]
>  Lorsque vous configurez la sauvegarde du serveur, vous devez vous connecter au moins un disque dur externe au serveur à utiliser en tant que le disque dur de destination de sauvegarde.  
  
###  <a name="BKMK_Target"></a>Lecteur de destination de sauvegarde  
 Vous pouvez utiliser plusieurs disques de stockage externe pour les sauvegardes, et vous pouvez faire pivoter les disques entre les emplacements de stockage sur site et hors site. Cela peut améliorer votre planification en vous aidant à récupérer vos données si les dommages physiques se produit pour le matériel local subit d’urgence.  
  
 Lorsque vous choisissez un lecteur de stockage pour sauvegarder votre serveur, procédez comme suit:  
  
-   Choisissez un lecteur qui contient suffisamment d’espace pour stocker vos données. Les lecteurs de stockage doivent contenir au moins 2,5 fois la capacité de stockage des données que vous souhaitez sauvegarder. Les lecteurs doit aussi être suffisamment grands pour contenir la croissance future des données de votre serveur.  
  
-   Si le lecteur de destination de sauvegarde contient des lecteurs hors connexion, la configuration de la sauvegarde échoue. Pour terminer la configuration, lorsque vous sélectionnez la destination de sauvegarde, décochez la case à cocher pour exclure les lecteurs qui sont en mode hors connexion.  
  
-   Si vous choisissez un lecteur qui contient des sauvegardes antérieures comme destination de sauvegarde, l’Assistant vous permet de que choisir si vous souhaitez conserver les sauvegardes précédentes. Si vous conservez les sauvegardes, l’Assistant ne formate pas le lecteur.  
  
-   Quand vous réutilisez un lecteur de stockage externe, assurez-vous que le lecteur est vide ou qu’il contient uniquement les données que vous n’avez pas besoin.  
  
-   Visitez le site Web de votre fabricant du lecteur de stockage externe pour vous assurer que votre lecteur de sauvegarde est pris en charge sur les ordinateurs exécutant Windows Server Essentials.  
  
> [!CAUTION]
>  L’Assistant Configuration du serveur sauvegarde formate les lecteurs de stockage quand il les configure pour la sauvegarde.  
  
### <a name="server-backup-schedule"></a>Planification de sauvegarde de serveur  
 Vous devez protéger automatiquement votre serveur et ses données en planifiant des sauvegardes quotidiennes. Il est recommandé de mettre à jour un plan de sauvegarde quotidien, car la plupart des organisations ne peut pas se permettre de perdre les données qui a été créées sur plusieurs jours.  
  
 Lorsque vous utilisez le Windows Server Essentials définir des Assistant sauvegarde du serveur, vous pouvez choisir de sauvegarder les données du serveur à plusieurs reprises pendant la journée. Dans la mesure où l’Assistant permet de planifier des sauvegardes différentielles, sauvegarde s’exécute rapidement, et les performances du serveur n’est pas largement affectée. Par défaut, **définir la sauvegarde du serveur** planifie une sauvegarde pour s’exécuter tous les jours à 12 h 00 et 23 h 00. Toutefois, vous pouvez ajuster la planification de sauvegarde en fonction des besoins de votre organisation. Vous devez parfois évaluer l’efficacité de votre plan de sauvegarde et modifier en conséquence.  
  
> [!NOTE]
>  Dans l’installation par défaut de Windows Server Essentials, le serveur est configuré pour exécuter automatiquement une défragmentation une fois par semaine. Cela peut être supérieure que les sauvegardes normales si vous utilisez des logiciels non Microsoft d’acquisition d’images. Si elle n’est pas nécessaire de défragmenter le serveur de façon régulière, vous pouvez suivre ces étapes pour désactiver la planification de la défragmentation:  
>   
>  1.  Appuyez sur la touche Windows + W pour ouvrir **recherche**.  
> 2.  Dans la zone de texte Rechercher, tapez **défragmenter**.  
> 3.  Dans la section de résultats, cliquez sur **défragmenter et optimiser les lecteurs**.  
> 4.  Dans le **optimiser les lecteurs** page, sélectionnez un lecteur, puis cliquez sur **modifier les paramètres**.  
> 5.  Dans le **planification de l’optimisation** fenêtre, désactivez le **exécuter selon un calendrier (recommandé)** case à cocher, puis cliquez sur **OK** pour enregistrer la modification.  
  
### <a name="items-to-be-backed-up"></a>Éléments à sauvegarder  
 Par défaut, tous les fichiers du système d’exploitation et les dossiers sont sélectionnés pour la sauvegarde. Vous pouvez choisir de sauvegarder tous les disques durs, fichiers et dossiers sur le serveur, ou sélectionner uniquement certains disques durs, fichiers ou dossiers à sauvegarder. Pour ajouter ou supprimer des éléments de la sauvegarde, effectuez l’une des opérations suivantes:  
  
-   Pour inclure un lecteur de données dans la sauvegarde du serveur, sélectionnez la case à cocher située en regard.  
  
-   Pour exclure un lecteur de données à partir de la sauvegarde du serveur, désactivez la case à cocher située en regard.  
  
> [!NOTE]
>  Si vous souhaitez exclure le **système d’exploitation** élément à partir de la sauvegarde, vous devez tout d’abord désactiver la **sauvegarde système (recommandé)** case à cocher.  
  
 Pour réduire la quantité d’espace disque qui utilisent des sauvegardes de votre serveur, vous voudrez peut-être exclure les dossiers qui contiennent des fichiers qui vous yeux particulièrement important.  
  
 Par exemple, peut avoir un dossier qui contient les programmes de télévision enregistrés qui utilise beaucoup d’espace disque dur. Vous pouvez choisir de ne pas de sauvegarder ces fichiers, car vous les supprimez après les avoir regardés. Vous pouvez également avoir un dossier qui contient les fichiers temporaires que vous ne souhaitez pas conserver.  
  
##  <a name="BKMK_6"></a>Repartitionner un disque dur sur le serveur  
 Lorsqu’un lecteur de disque dur interne non formaté est détecté sur le serveur Windows Server Essentials, qui contient un lien vers l’Assistant Ajouter un nouveau disque dur lecteur est déclenchée par une alerte d’intégrité. L’Assistant Ajouter un nouveau disque dur lecteur vous guide dans les différentes options de formatage du disque dur. Lorsque l’Assistant est terminé, un ou plusieurs, selon la taille du lecteur de disques durs logiques formatés seront créés sur le disque dur et au format NTFS.  
  
 S’il devient nécessaire de repartitionner un lecteur de disque dur, suivez ces instructions:  
  
#### <a name="to-repartition-a-hard-disk-drive"></a>Pour repartitionner un lecteur de disque dur  
  
1.  Sur le **Démarrer** , cliquez sur **outils d’administration**, puis double-cliquez sur **gestion de l’ordinateur**.  
  
2.  Dans Gestion de l’ordinateur, cliquez sur **stockage**, puis double-cliquez sur **gestion des disques**.  
  
3.  Cliquez sur le lecteur à repartitionner, cliquez sur **supprimer le Volume**, puis cliquez sur **Oui**.  
  
    > [!NOTE]
    >  Répétez cette étape pour chaque partition sur le lecteur de disque dur.  
  
4.  Cliquez sur le **non alloué** lecteur de disque dur, puis cliquez sur **nouveau Volume Simple**.  
  
5.  Dans l’Assistant Création de Volume Simple, créer et formater un volume qui est de 16 To (16,000,000 Mo) ou moins.  
  
    > [!NOTE]
    >  Répétez cette étape jusqu'à ce que tout l’espace non alloué sur le lecteur de disque dur est utilisé.  
  
##  <a name="BKMK_7"></a>Restaurer des fichiers et dossiers à partir d’une sauvegarde du serveur  
 Vous pouvez parcourir et restaurer des fichiers et dossiers individuels à partir d’une sauvegarde du serveur.  
  
#### <a name="to-restore-files-and-folders-from-a-server-backup"></a>Pour restaurer des fichiers et dossiers à partir d’une sauvegarde du serveur  
  
1.  Ouvrez le tableau de bord, puis cliquez sur le **périphériques** onglet.  
  
2.  Cliquez sur le nom du serveur, puis cliquez sur **restaurer des fichiers ou dossiers du serveur** dans les **tâches** volet.  
  
3.  La restauration de fichiers ou dossiers Assistant s’ouvre. Suivez les instructions de l’Assistant pour restaurer les fichiers ou dossiers.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Gérer la sauvegarde et restauration](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Gérer WindowsServerEssentials](Manage-Windows-Server-Essentials.md)  
  
-   [Utiliser Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
