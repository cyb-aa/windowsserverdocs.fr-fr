---
title: Configurer ou personnaliser la sauvegarde du serveur
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 441c2d6c-435a-42cb-90f2-6d680d279d34
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 727dd74e4bddc52f735969f216914b9d76d1f413
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="set-up-or-customize-server-backup"></a>Configurer ou personnaliser la sauvegarde du serveur

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials
  
 Sauvegarde du serveur n’est pas configurée automatiquement pendant l’installation. Vous devez protéger automatiquement votre serveur et ses données en planifiant des sauvegardes quotidiennes. Il est recommandé de mettre à jour un plan de sauvegarde quotidien, car la plupart des organisations ne peut pas se permettre de perdre les données qui a été créées sur plusieurs jours.  
  
 Consultez les sections suivantes pour configurer ou personnaliser la sauvegarde du serveur:  
  
-   [Définir ou modifier les paramètres de sauvegarde de serveur](Set-up-or-customize-server-backup.md#BKMK_1)  
  
-   [Planification de sauvegarde de serveur](Set-up-or-customize-server-backup.md#BKMK_2)  
  
-   [Lecteur cible de sauvegarde](Set-up-or-customize-server-backup.md#BKMK_Target)  
  
-   [Éléments à sauvegarder](Set-up-or-customize-server-backup.md#BKMK_4)  
  
##  <a name="BKMK_1"></a>Définir ou modifier les paramètres de sauvegarde de serveur  
  
#### <a name="to-set-up-or-change-server-backup-settings"></a>Pour configurer ou modifier les paramètres de sauvegarde de serveur  
  
1.  Ouvrez le **tableau de bord**, puis cliquez sur le **périphériques** onglet.  
  
2.  Dans la liste, cliquez sur votre serveur pour le sélectionner.  
  
3.  Dans le volet tâches, cliquez sur **configurer la sauvegarde pour le serveur**.  
  
    > [!NOTE]
    >  Si vous souhaitez modifier les paramètres de sauvegarde existants, cliquez sur **personnaliser la sauvegarde pour le serveur**.  
  
4.  Suivez les instructions de l’Assistant.  
  
    > [!NOTE]
    >  Si vous démarrez l’Assistant avant d’attacher le disque dur externe au serveur, cliquez sur **actualiser la liste** sur le **sélectionner la destination de sauvegarde** page après avoir attaché le disque dur.  
  
> [!NOTE]
>  Dans l’installation par défaut de WindowsServerEssentials, le serveur est configuré pour exécuter automatiquement une défragmentation une fois par semaine. Cela peut être supérieure que les sauvegardes normales si vous utilisez des logiciels non Microsoft d’acquisition d’images. Si elle n’est pas nécessaire de défragmenter le serveur de façon régulière, vous pouvez suivre ces étapes pour désactiver la planification de la défragmentation:  
>   
>  1.  Appuyez sur la touche Windows + W pour ouvrir **recherche**.  
> 2.  Dans la zone de texte Rechercher, tapez **défragmenter**.  
> 3.  Dans la section de résultats, cliquez sur **défragmenter et optimiser vos lecteurs**.  
> 4.  Dans le **optimiser les lecteurs** page, sélectionnez un lecteur, puis cliquez sur **modifier les paramètres**.  
> 5.  Dans le **planification de l’optimisation** fenêtre, désactivez le **exécuter selon un calendrier (recommandé)** case à cocher, puis cliquez sur **OK** pour enregistrer la modification.  
  
##  <a name="BKMK_2"></a>Planification de sauvegarde de serveur  
 Lorsque vous utilisez l’Assistant Configurer la sauvegarde serveur ou l’Assistant Personnalisation de la sauvegarde de serveur, vous pouvez choisir de sauvegarder les données du serveur à plusieurs reprises pendant la journée. Étant donné que les Assistants de planifient des sauvegardes incrémentielles, les sauvegardes s’exécutent rapidement et performances du serveur n’est pas largement affectée. Par défaut, les Assistants de planifient une sauvegarde pour s’exécuter tous les jours à 12 h et 23 h 00. Toutefois, vous pouvez ajuster la planification de sauvegarde en fonction des besoins de votre organisation. Vous devez parfois évaluer l’efficacité de votre plan de sauvegarde et modifier en conséquence.  
  
##  <a name="BKMK_Target"></a>Lecteur cible de sauvegarde  
 Vous pouvez utiliser plusieurs disques de stockage externe pour les sauvegardes, et vous pouvez faire pivoter les disques entre les emplacements de stockage sur site et hors site. Cela peut améliorer votre planification en vous aidant à récupérer vos données si les dommages physiques se produit pour le matériel local subit d’urgence.  
  
 Lorsque vous choisissez un lecteur de stockage pour sauvegarder votre serveur, procédez comme suit:  
  
-   Choisissez un lecteur qui contient suffisamment d’espace pour stocker vos données. Les lecteurs de stockage doivent contenir au moins 2,5 fois la capacité de stockage des données que vous souhaitez sauvegarder. Les lecteurs doit aussi être suffisamment grands pour contenir la croissance future des données de votre serveur.  
  
-   Lorsque vous utilisez un lecteur de stockage externe, assurez-vous que le lecteur est vide ou qu’il contient uniquement les données que vous n’avez pas besoin.  
  
    > [!CAUTION]
    >  L’Assistant Configuration du serveur sauvegarde formate les lecteurs de stockage quand il les configure pour la sauvegarde.  
  
-   Si le lecteur cible de sauvegarde contient des lecteurs hors connexion, la configuration de la sauvegarde échoue. Pour terminer la configuration, lorsque vous sélectionnez la cible de sauvegarde, décochez la case à cocher pour exclure les lecteurs qui sont en mode hors connexion.  
  
-   Si vous choisissez un lecteur qui contient des sauvegardes précédentes comme cible de sauvegarde, l’Assistant vous permet de que choisir si vous souhaitez conserver les sauvegardes précédentes. Si vous conservez les sauvegardes, l’Assistant ne formate pas le lecteur.  
  
-   Visitez le site Web de votre fabricant du lecteur de stockage externe pour vous assurer que votre lecteur de sauvegarde est pris en charge sur les ordinateurs exécutant WindowsServerEssentials.  
  
-   Le lecteur ne peut pas contenir une partition de système Extensible Firmware Interface (EFI). Si une partition EFI est présente sur un lecteur USB, il est supposé que le disque est un disque de démarrage. Si vous êtes certain que vous n’avez pas besoin les données sur le disque, vous pouvez reformater le disque et l’utiliser pour les sauvegardes.  
  
    > [!CAUTION]
    >  Toutes les données seront supprimées quand vous reformatez le disque.  
  
    #### <a name="to-remove-an-efi-system-partition-from-a-usb-disk"></a>Pour supprimer une partition système EFI à partir d’un disque USB  
  
    1.  Dans le panneau de configuration, ouvrez **systèmes et sécurité**.  
  
    2.  Sous **outils d’administration**, cliquez sur **créer et formater des partitions de disque dur**.  
  
    3.  Supprimez tous les volumes sur le disque USB ou simplement supprimer la partition EFI. L’Assistant Configurer la sauvegarde serveur supprimera tous les volumes.  
  
-   Le lecteur ne peut pas contenir les dossiers de serveur partagés. Avant de pouvoir utiliser le disque comme lecteur cible de sauvegarde, vous devez arrêter le partage sur tous les dossiers serveur partagé. Vous pouvez arrêter le partage à partir du tableau de bord ou dans l’Explorateur de fichiers.  
  
    #### <a name="to-stop-sharing-on-a-server-folder-from-the-dashboard"></a>Pour arrêter le partage sur un dossier de serveur à partir du tableau de bord  
  
    1.  Dans le tableau de bord, cliquez sur **stockage**, puis cliquez sur **dossiers du serveur**.  
  
    2.  Sélectionnez le dossier que vous souhaitez arrêter le partage, puis, dans le volet des tâches, cliquez sur **arrêter**.  
  
> [!NOTE]
>  Si une sauvegarde échoue parce que le lecteur de sauvegarde a pas assez d’espace, la lettre de lecteur pour le lecteur cible de sauvegarde est supprimée de la base de données de WindowsServerEssentials, et le tableau de bord n’affiche pas le lecteur. Si vous souhaitez utiliser le lecteur dans les futures sauvegardes, vous devez réaffecter la lettre de lecteur à l’aide d’un outil natif.  
>   
>  **Pour réassigner une lettre de lecteur pour un volume existant**  
>   
>  1.  Dans le panneau de configuration, ouvrez **systèmes et sécurité**.  
> 2.  Sous **outils d’administration**, cliquez sur **créer et formater des partitions de disque dur**.  
> 3.  Cliquez sur le lecteur, puis cliquez sur **modifier la lettre de lecteur et les chemins d’accès**.  
> 4.  Cliquez sur **ajouter**.  
> 5.  Dans la boîte de dialogue Ajouter une lettre de lecteur ou de chemin d’accès, sélectionnez une lettre de lecteur à attribuer. (Vous pouvez réaffecter la même lettre de lecteur). Puis cliquez sur **OK**.  
>   
>      Le lecteur apparaît immédiatement sur le tableau de bord.  
  
##  <a name="BKMK_4"></a>Éléments à sauvegarder  
 Vous pouvez choisir de sauvegarder tous les lecteurs, fichiers et dossiers sur le serveur, ou sélectionner uniquement des lecteurs, fichiers ou dossiers à sauvegarder.  
  
 Lorsque vous ajoutez ou supprimez un lecteur, ou ajoutez ou supprimez des fichiers et dossiers partagés, vous devez revoir la configuration de sauvegarde de serveur pour vous assurer que ces éléments sont ajoutés ou supprimés de la configuration de la sauvegarde. Pour ajouter ou supprimer des éléments de la sauvegarde, effectuez l’une des opérations suivantes:  
  
-   Pour inclure un lecteur de données dans la sauvegarde du serveur, activez la case à cocher située en regard  
  
-   Pour exclure un lecteur de données de la sauvegarde du serveur, décochez la case à cocher située en regard  
  
    > [!NOTE]
    >  Si vous souhaitez exclure le **système d’exploitation** élément à partir de la sauvegarde, vous devez tout d’abord désactiver la **sauvegarde système (recommandé)** case à cocher.  
  
 Pour réduire la quantité de stockage de serveur qui utilisent des sauvegardes de votre serveur, vous voudrez peut-être exclure les dossiers qui contiennent les fichiers que vous considérez importantes ou particulièrement important.  
  
 Par exemple, peut avoir un dossier qui contient les programmes de télévision enregistrés qui utilise beaucoup d’espace disque dur. Vous pouvez choisir de ne pas de sauvegarder ces fichiers, car vous les supprimez après les avoir regardés. Ou que vous avez un dossier qui contient les fichiers temporaires que vous ne souhaitez pas conserver.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Gérer la sauvegarde du serveur](Manage-Server-Backup-in-Windows-Server-Essentials.md)  
  
-   [Gérer la sauvegarde et restauration](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Gérer WindowsServerEssentials](Manage-Windows-Server-Essentials.md)  
  
-   [Utiliser Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
