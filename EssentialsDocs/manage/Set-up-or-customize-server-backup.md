---
title: Configurer ou personnaliser la sauvegarde du serveur
description: Décrit comment utiliser Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831930"
---
# <a name="set-up-or-customize-server-backup"></a>Configurer ou personnaliser la sauvegarde du serveur

>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
 La sauvegarde du serveur n'est pas configurée automatiquement pendant l'installation. Nous vous conseillons de protéger automatiquement votre serveur et ses données en planifiant des sauvegardes quotidiennes. Il est recommandé de mettre en place un plan de sauvegarde quotidien, car la plupart des organisations ne peuvent pas se permettre de perdre plusieurs jours de données.  
  
 Consultez les sections suivantes pour configurer ou personnaliser la sauvegarde du serveur :  
  
-   [Définir ou modifier les paramètres de sauvegarde de serveur](Set-up-or-customize-server-backup.md#BKMK_1)  
  
-   [Planification de sauvegarde de serveur](Set-up-or-customize-server-backup.md#BKMK_2)  
  
-   [Lecteur cible de sauvegarde](Set-up-or-customize-server-backup.md#BKMK_Target)  
  
-   [Éléments à sauvegarder](Set-up-or-customize-server-backup.md#BKMK_4)  
  
##  <a name="BKMK_1"></a> Définir ou modifier les paramètres de sauvegarde de serveur  
  
#### <a name="to-set-up-or-change-server-backup-settings"></a>Pour définir ou modifier les paramètres de sauvegarde de serveur  
  
1.  Ouvrez le **Tableau de bord**, puis cliquez sur l'onglet **Périphériques**.  
  
2.  Dans la liste, cliquez sur votre serveur pour le sélectionner.  
  
3.  Dans le volet Tâches, cliquez sur **Configurer la sauvegarde pour le serveur**.  
  
    > [!NOTE]
    >  Si vous souhaitez modifier les paramètres de sauvegarde existants, cliquez sur **Personnaliser la sauvegarde pour le serveur**.  
  
4.  Suivez les instructions de l'Assistant.  
  
    > [!NOTE]
    >  Si vous démarrez l'Assistant avant d'attacher le disque dur externe au serveur, cliquez sur **Actualiser la liste** dans la page **Sélectionner la destination de sauvegarde** après avoir attaché le disque dur.  
  
> [!NOTE]
>  Dans l’installation par défaut de Windows Server Essentials, le serveur est configuré pour exécuter automatiquement une défragmentation une fois par semaine. Si vous utilisez un logiciel de création d'images qui n'est pas fourni par Microsoft, la taille de vos sauvegardes peut être supérieure à la normale. S'il est inutile de défragmenter régulièrement le serveur, vous pouvez suivre ces étapes pour désactiver la planification de la défragmentation :  
>   
>  1.  Appuyez sur la touche Windows + W pour ouvrir **Rechercher**.  
> 2.  Dans la zone de texte Rechercher, tapez **Defragment**.  
> 3.  Dans la section de résultats, cliquez sur **Défragmenter et optimiser vos lecteurs**.  
> 4.  Dans la page **Optimiser les lecteurs**, sélectionnez un lecteur, puis cliquez sur **Modifier les paramètres**.  
> 5.  Dans la fenêtre **Planification de l'optimisation** , décochez la case **Exécution planifiée (recommandé)** , puis cliquez sur **OK** pour enregistrer les modifications.  
  
##  <a name="BKMK_2"></a> Planification de sauvegarde de serveur  
 Quand vous utilisez l'Assistant Configurer la sauvegarde du serveur ou l'Assistant Personnaliser la sauvegarde du serveur, vous pouvez choisir de sauvegarder les données du serveur à plusieurs reprises durant la journée. Étant donné que les Assistants planifient des sauvegardes incrémentielles, les sauvegardes s'exécutent rapidement et les performances du serveur ne sont que modérément affectées. Par défaut, les Assistants planifient l'exécution d'une sauvegarde tous les jours à 12h00 et 23h00. Vous pouvez toutefois modifier la planification de la sauvegarde en fonction des besoins de votre organisation. Nous vous conseillons d'évaluer de temps en temps l'efficacité de votre plan de sauvegarde et de le modifier en conséquence.  
  
##  <a name="BKMK_Target"></a> Lecteur cible de sauvegarde  
 Vous pouvez utiliser plusieurs lecteurs de stockage externe pour vos sauvegardes et les entreposer à tour de rôle dans des emplacements sur site et hors site. Cela peut vous aider à renforcer votre stratégie de planification d'urgence, car vous pourrez récupérer vos données si votre matériel local subit des dommages.  
  
 Au moment d'acquérir un lecteur de stockage pour sauvegarder votre serveur, tenez compte des points suivants :  
  
-   Choisissez un lecteur avec suffisamment d'espace pour stocker vos données. Les lecteurs de stockage doivent disposer d'une capacité de stockage au moins égale à 2,5 fois la quantité de données à sauvegarder. La capacité de stockage des lecteurs doit aussi être suffisamment grande pour prendre en charge la croissance future des données serveur.  
  
-   Quand vous utilisez un lecteur de stockage externe, assurez-vous que le lecteur est vide ou qu'il contient uniquement des données dont vous n'avez pas besoin.  
  
    > [!CAUTION]
    >  L'Assistant Configurer la sauvegarde du serveur formate les lecteurs de stockage quand il les configure pour la sauvegarde.  
  
-   Si le lecteur cible de sauvegarde contient des lecteurs en mode hors connexion, la configuration de la sauvegarde échoue. Pour terminer la configuration, quand vous sélectionnez la cible de sauvegarde, décochez la case pour exclure les lecteurs qui sont hors connexion.  
  
-   Si vous choisissez un lecteur qui contient des sauvegardes précédentes comme cible de sauvegarde, l'Assistant vous permet de choisir si vous souhaitez conserver les sauvegardes précédentes. Si vous conservez les sauvegardes, l'Assistant ne formate pas le lecteur.  
  
-   Visitez le site Web du fabricant de votre lecteur de stockage externe pour vous assurer que votre lecteur de sauvegarde est prise en charge sur les ordinateurs exécutant Windows Server Essentials.  
  
-   Le lecteur ne peut pas contenir de partition système EFI (Extensible Firmware Interface). Si une partition EFI est présente sur un lecteur USB, il est supposé que le disque est un disque de démarrage. Si vous êtes certain que vous n’avez pas besoin de t les données sur le disque, vous pouvez reformater le disque et l’utiliser pour les sauvegardes.  
  
    > [!CAUTION]
    >  Toutes les données sont supprimées quand vous reformatez le disque.  
  
    #### <a name="to-remove-an-efi-system-partition-from-a-usb-disk"></a>Pour supprimer une partition système EFI d'un disque USB  
  
    1.  Dans le Panneau de configuration, ouvrez **Systèmes et sécurité**.  
  
    2.  Sous **Outils d'administration**, cliquez sur **Créer et formater des partitions de disque dur**.  
  
    3.  Supprimez tous les volumes du disque USB ou simplement la partition EFI. L'Assistant Configurer la sauvegarde du serveur supprimera tous les volumes.  
  
-   Le lecteur ne peut pas contenir de dossiers de serveur partagés. Avant de pouvoir utiliser le disque comme lecteur cible de sauvegarde, vous devez arrêter le partage sur tous les dossiers de serveur partagés. Vous pouvez arrêter le partage à partir du Tableau de bord ou dans l'Explorateur de fichiers.  
  
    #### <a name="to-stop-sharing-on-a-server-folder-from-the-dashboard"></a>Pour arrêter le partage d'un dossier de serveur à partir du Tableau de bord  
  
    1.  Dans le Tableau de bord, cliquez sur **Stockage**, puis sur **Dossiers de serveur**.  
  
    2.  Sélectionnez le dossier dont vous souhaitez arrêter le partage puis, dans le volet des tâches, cliquez sur **Arrêter**.  
  
> [!NOTE]
>  Si une sauvegarde échoue, car le lecteur de sauvegarde n’était pas suffisamment d’espace, la lettre de lecteur pour le lecteur cible de sauvegarde est supprimée de la base de données Windows Server Essentials et le tableau de bord n’affiche pas le lecteur. Si vous souhaitez utiliser le lecteur lors des sauvegardes ultérieures, vous devez réassigner la lettre de lecteur à l'aide d'un outil natif.  
>   
>  **Pour réaffecter une lettre de lecteur pour un volume existant**  
>   
>  1.  Dans le Panneau de configuration, ouvrez **Systèmes et sécurité**.  
> 2.  Sous **Outils d'administration**, cliquez sur **Créer et formater des partitions de disque dur**.  
> 3.  Cliquez avec le bouton droit sur le lecteur, puis cliquez sur **Modifier la lettre de lecteur et les chemins d'accès**.  
> 4.  Cliquez sur **Ajouter**.  
> 5.  Dans la boîte de dialogue Ajouter une lettre de lecteur ou de chemin d'accès, sélectionnez une lettre de lecteur à assigner. (Vous pouvez réassigner la même lettre de lecteur). Cliquez sur **OK**.  
>   
>      Le lecteur apparaît immédiatement sur le Tableau de bord.  
  
##  <a name="BKMK_4"></a> Éléments à sauvegarder  
 Vous pouvez choisir de sauvegarder tous les lecteurs, fichiers et dossiers sur le serveur ou sélectionner uniquement des lecteurs, fichiers ou dossiers spécifiques à sauvegarder.  
  
 Quand vous ajoutez ou supprimez un lecteur ou des fichiers et dossiers partagés, vous devez modifier la configuration de sauvegarde du serveur pour vous assurer que ces éléments sont ajoutés ou supprimés de la configuration de sauvegarde. Pour ajouter ou supprimer des éléments à sauvegarder, effectuez l'une des opérations suivantes :  
  
-   Pour inclure un lecteur de données dans la sauvegarde du serveur, cochez la case correspondante  
  
-   Pour exclure un lecteur de données de la sauvegarde du serveur, décochez la case correspondante  
  
    > [!NOTE]
    >  Si vous souhaitez exclure l'élément **Système d'exploitation** de la sauvegarde, vous devez d'abord décocher la case **Sauvegarde système (recommandé)** .  
  
 Pour réduire la quantité de stockage de serveur utilisée par les sauvegardes de votre serveur, vous pouvez exclure les dossiers contenant des fichiers que vous jugez sans grande importance.  
  
 Par exemple, vous possédez peut-être un dossier contenant des programmes de télévision enregistrés qui prend beaucoup de place sur le disque. Vous pouvez choisir ne pas de sauvegarder ces fichiers, car vous les supprimez de toute façon après les avoir regardés. Vous possédez peut-être aussi un dossier qui contient des fichiers temporaires que vous ne souhaitez pas conserver.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Gérer la sauvegarde du serveur](Manage-Server-Backup-in-Windows-Server-Essentials.md)  
  
-   [Gérer la sauvegarde et restauration](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Gérer Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Utiliser Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
