---
title: Restaurer ou réparer votre serveur Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 27bf6f24-30c4-4935-9b24-069eb43e22f4
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 5618eb95fb8afcff2057575191699da05612a542
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433066"
---
# <a name="restore-or-repair-your-server-running-windows-server-essentials"></a>Restaurer ou réparer votre serveur Windows Server Essentials

>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 Cette rubrique fournit une vue d’ensemble et la prise en charge des procédures pour la restauration ou réparation d’un serveur exécutant Windows Server Essentials et comprend les sections suivantes :  
  
-   [Vue d’ensemble des restaurations de système de serveur](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Overview)  
  
-   [Restaurer ou réparer le lecteur système](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore)  
  
-   [Restaurer des fichiers et dossiers sur le serveur](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesAndFolders)  
  
##  <a name="BKMK_Overview"></a> Vue d’ensemble des restaurations de système de serveur  
 L'état du serveur quand vous effectuez une restauration a un impact sur la méthode de restauration disponible et sur l'étendue de la restauration que vous pouvez effectuer.  
  
 Les raisons les plus courantes pour la restauration d'un serveur sont les suivantes :  
  
- Un virus sur le serveur ne peut pas être inoculé ou supprimé.  
  
- Les paramètres de configuration du serveur sont incorrects et vous ne pouvez pas démarrer le serveur.  
  
- Vous avez remplacé le lecteur système.  
  
- Vous mettez le serveur hors service et vous souhaitez restaurer sur un nouveau serveur.  
  
  Vous pouvez restaurer le serveur à partir d'une sauvegarde ou rétablir les paramètres par défaut du serveur.  
  
- [Restauration du serveur à partir d’une sauvegarde](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFromBackup)  
  
- [Réinitialisation des paramètres d’usine par défaut le serveur](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_FactoryReset)  
  
###  <a name="BKMK_RestoreFromBackup"></a> Restauration du serveur à partir d’une sauvegarde  
 Cette section fournit des instructions sur le type de sauvegarde à choisir.  
  
 Si une sauvegarde est disponible, le meilleur choix pour la restauration de votre serveur consiste à utiliser le support d’installation s fabricant pour restaurer à partir d’une sauvegarde externe. La restauration récupérera les paramètres et les dossiers de serveur à partir de la sauvegarde que vous choisissez. Vous devez uniquement configurer les paramètres et restaurer les données créées après la sauvegarde.  
  
 Quand vous choisissez de récupérer votre serveur en effectuant une restauration à partir d'une sauvegarde antérieure, vous devez choisir la sauvegarde spécifique que vous souhaitez restaurer et vous devez posséder un fichier de sauvegarde valide sur un disque dur externe connecté directement au serveur :  
  
- **Si vous disposez d'une sauvegarde très récente du serveur** et que vous savez que la sauvegarde contient toutes vos données critiques, votre choix est assez simple. Vous devrez uniquement recréer les données créées après votre dernière sauvegarde et reconfigurer les paramètres en fonction des modifications apportées après cette sauvegarde.  
  
- **Si vous restaurez votre serveur à cause d'un virus**, sélectionnez une sauvegarde qui a été effectuée avant la contamination par le virus. Vous devrez peut-être revenir plusieurs jours en arrière pour sélectionner une sauvegarde correcte.  
  
- **Si vous restaurez votre serveur à cause de paramètres de configuration incorrects**, sélectionnez une sauvegarde qui a été effectuée avant la modification du paramètre de configuration qui provoque le problème sur le serveur.  
  
  Quand vous restaurez une sauvegarde, le processus exact et le suivi nécessaire dépendent du nombre de disques durs sur le serveur et du remplacement éventuel du lecteur système :  
  
- **Si le serveur dispose d'un seul disque dur et que le lecteur n'est pas remplacé**, les informations de partition du lecteur sont laissées intactes quand vous restaurez le serveur. Le volume système est restauré et les données sur le volume restant sont conservées.  
  
- **Si le serveur dispose d'un seul disque dur et que le lecteur est remplacé**, le volume système est restauré et vous devez restaurer manuellement les dossiers sur le volume de données. Tous les dossiers partagés autres que les dossiers par défaut doivent être créés, car ils ne sont pas créés lors de la recréation du stockage de serveur.  
  
- **Si le serveur dispose de plusieurs disques durs et que lecteur 0 (contenant le volume système) n'est pas remplacé**, les informations de partition du lecteur sont laissées intactes quand vous restaurez le serveur. Le volume système est restauré et les données sur tous les volumes restants sont conservées.  
  
- **Si le serveur dispose de plusieurs disques durs et que lecteur 0 (contenant le volume système) est remplacé**, le volume système est restauré et vous devez restaurer manuellement les dossiers partagés qui étaient auparavant stockés sur le disque 0.  
  
###  <a name="BKMK_FactoryReset"></a> Réinitialisation des paramètres d’usine par défaut le serveur  
 Si vous n'avez pas de sauvegarde à partir de laquelle vous pouvez restaurer ou si, pour une raison quelconque, vous souhaitez ou devez effectuer une restauration complète du système sans restaurer la configuration de serveur précédente, vous pouvez effectuer une restauration qui rétablit les paramètres par défaut du serveur à l'aide du support d'installation ou de récupération fourni par le fabricant de matériel de serveur.  
  
 Quand vous restaurez votre serveur en rétablissant les paramètres par défaut, tous les paramètres existants et les applications installées sur votre serveur sont supprimés et vous devez reconfigurer votre serveur. Après la réinitialisation, le serveur redémarre.  
  
 Quand vous rétablissez les paramètres d'origine, vous pouvez choisir de garder vos données ou de les supprimer, avec les conséquences suivantes :  
  
-   Si vous choisissez de conserver toutes vos données, toutes les données sur le volume système sont supprimées, mais les données sur les autres volumes sont conservées.  
  
    > [!CAUTION]
    >  Si les paramètres d'un disque ne correspondent pas aux paramètres par défaut, toutes les données sur un disque sont supprimées. Si vous avez remplacé le disque du système, le nouveau disque doit être supérieur au volume de système de s disque d’origine.  
    >   
    >  Si les informations de partition d'un lecteur système sont illisibles, ou si vous remplacez le lecteur système, toutes les données sur le lecteur système sont supprimées, même si vous choisissez de conserver toutes vos données.  
  
-   Si vous envisagez de mettre le serveur hors service ou de le réaffecter, choisissez de supprimer toutes les données. En plus de la configuration du serveur, des autres paramètres et des données sur le volume système, toutes les autres données sont supprimées et tous les disques durs sur le serveur sont reformatés.  
  
> [!NOTE]
>  Si Espaces de stockage est configuré sur le serveur, avant de rétablir les paramètres d'usine, vous devez utiliser la section **Avancé** de la console **Gérer les espaces de stockage** pour supprimer manuellement tous les espaces de stockage.  
  
 Après une réinitialisation, vous devez effectuer les tâches suivantes :  
  
-   **Reconfigurer le serveur.** Sur le serveur, utilisez l'Assistant Configuration du serveur pour réentrer les paramètres de configuration. Pour configurer un serveur géré à distance de Windows Server Essentials à partir d’un ordinateur client, ouvrez un navigateur web, puis tapez **http://***<YourServerName\>*  dans la barre d’adresses.  
  
-   **Reconnecter les ordinateurs clients au serveur.** Si un ordinateur était auparavant connecté au serveur, vous devez désinstaller le logiciel Connecteur Windows Server Essentials à partir de l’ordinateur avant de reconnecter l’ordinateur au serveur. Pour plus d'informations, consultez [Uninstall the Connector software](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13) et [Connect computers to the server](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  
  
##  <a name="BKMK_Restore"></a> Restaurer ou réparer le lecteur système  
 La première étape de restauration du serveur consiste à restaurer ou à réparer le lecteur système du serveur. Après avoir restauré le lecteur système, vous ferez tout ce qui est nécessaire pour restaurer les lecteurs de données sur le serveur et pour restaurer tous les partages qui ont été perdus durant la restauration.  
  
 Trois méthodes sont disponibles pour l'exécution de la restauration :  
  
-   [Restaurer ou réparer votre serveur à l'aide du support d'installation](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_1). Utilisez le support d'installation fourni par le fabricant du serveur pour effectuer une restauration à partir d'une sauvegarde.  
  
-   **Utiliser le support d'installation pour rétablir les paramètres d'origine du serveur**. Pour savoir comment procéder sur votre serveur, consultez la documentation du fabricant du serveur.  
  
-   [Restaurer ou réinitialiser votre serveur à partir d'un ordinateur client à l'aide du DVD de récupération](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_2). Si vous avez besoin restaurer un serveur administré à distance qui exécute Windows Server Essentials, vous devez effectuer la restauration à partir d’un ordinateur client à l’aide du DVD de restauration du fabricant du serveur.  
  
###  <a name="BKMK_Restore_1"></a> Restaurer ou réparer votre serveur à l’aide du support d’installation  
 La procédure suivante décrit comment restaurer le lecteur de système de votre serveur à partir d’une sauvegarde à l’aide du support d’installation de Windows Server Essentials. (Pour savoir comment utiliser le support d'installation pour rétablir les paramètres d'origine, consultez la documentation du fabricant du serveur).  
  
> [!NOTE]
>  Si le serveur utilise des espaces de stockage, et que vous restaurez les données vers un nouveau serveur, vous devez récupérer le lecteur système tout d’abord, puis ouvrez une session le tableau de bord Windows Server Essentials, configurer des espaces de stockage dans la même façon que sur l’ancien serveur et puis récupérer le dat des volumes.  
  
##### <a name="to-restore-the-server-system-drive-from-a-backup-using-installation-media"></a>Pour restaurer le lecteur système du serveur à partir d'une sauvegarde à l'aide du support d'installation  
  
1.  Insérez le DVD d’installation de Windows Server Essentials dans le lecteur de DVD de serveur, redémarrez le serveur, puis appuyez sur n’importe quelle touche pour démarrer à partir du DVD.  
  
    > [!NOTE]
    >  Si le processus de restauration ne démarre pas automatiquement, vérifiez les paramètres du BIOS de votre serveur pour vous assurer que le lecteur de DVD apparaît en premier dans le menu de démarrage.  
  
     \- Ou -  
  
     Si le fabricant a préchargé le support d'installation sur le serveur, appuyez sur F8 au démarrage pour démarrer à partir du support d'installation.  
  
2.  Une fois les fichiers Windows Server chargés, choisissez votre langue et d'autres préférences, puis cliquez sur **Suivant**.  
  
3.  Dans la page suivante de l'Assistant, cliquez sur **Réparer votre ordinateur**.  
  
    > [!CAUTION]
    >  Ne choisissez pas l'option **Installer maintenant**. Cette option permet d'effectuer une installation complète du système, qui supprime tous les paramètres de configuration et toutes les données sur le lecteur système.  
  
4.  Dans la page **Choisir une option**, cliquez sur **Dépannage**.  
  
5.  Sur le **récupération de l’Image système** , sélectionnez le système actuel ? soit **Windows Server Essentials** ou **Windows Server Essentials**.  
  
     L'Assistant Réimager l'ordinateur s'ouvre.  
  
6.  Dans la page **Sélectionner une sauvegarde d'image système**, vous pouvez choisir d'utiliser la dernière sauvegarde ou sélectionner une sauvegarde antérieure. Le système sera restauré à l'état où il se trouvait au moment de la sauvegarde que vous choisissez pour la restauration ou la réparation de votre serveur. Les données qui ont été ajoutées ou les modifications qui ont été apportées aux paramètres après l'enregistrement de la sauvegarde doivent être recréées.  
  
     Sélectionnez l'une des options suivantes, puis cliquez sur **Suivant**:  
  
    -   **Utiliser la dernière image système (recommandé)**  
  
    -   **Sélectionner une image système**  
  
    > [!NOTE]
    >  Si vous disposez d'une sauvegarde très récente du serveur et que vous savez que la sauvegarde contient toutes vos données critiques, votre choix est assez simple. Vous devrez uniquement recréer les données créées après votre dernière sauvegarde et reconfigurer les paramètres en fonction des modifications apportées après cette sauvegarde.  
    >   
    >  Si vous restaurez votre serveur à cause d'un virus, sélectionnez une sauvegarde qui a été effectuée avant la contamination par le virus. Vous devrez peut-être revenir plusieurs jours en arrière pour sélectionner une sauvegarde correcte.  
    >   
    >  Si vous restaurez votre serveur à cause de paramètres de configuration incorrects, sélectionnez une sauvegarde qui a été effectuée avant la modification du paramètre de configuration qui provoque le problème sur le serveur.  
  
7.  Suivez les instructions de l'Assistant pour terminer la restauration du système.  
  
8.  Une fois le serveur restauré, retirez le DVD d'installation si vous en avez utilisé un et redémarrez le serveur.  
  
> [!NOTE]
>  Pour restaurer et partager des dossiers sur le serveur, vous devrez peut-être effectuer des étapes supplémentaires. Pour plus d'informations, consultez [Restaurer des fichiers et des dossiers sur le serveur](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesAndFolders).  
  
###  <a name="BKMK_Restore_2"></a> Restaurer ou réinitialiser votre serveur à partir d’un ordinateur client à l’aide du DVD de récupération  
 Dans Windows Server Essentials, vous pouvez démarrer le serveur à partir d’un lecteur flash USB que vous créez, puis restaurer le serveur à partir d’un ordinateur client à l’aide du DVD que vous avez reçu du fabricant du serveur de récupération. L'ordinateur client doit se trouver sur le même réseau que le serveur. Cette méthode n’est pas disponible dans Windows Server Essentials.  
  
 La procédure suivante fournit les étapes générales pour effectuer une restauration du serveur. Ces étapes s'appliquent également à la restauration à partir d'une sauvegarde ou au rétablissement des paramètres d'origine. Pour obtenir des instructions plus spécifiques, consultez la documentation du fabricant de votre serveur.  
  
##### <a name="to-restore-or-reset-the-server-from-a-client-computer-using-the-recovery-dvd"></a>Pour restaurer ou réinitialiser votre serveur à partir d'un ordinateur client à l'aide du DVD de récupération  
  
1.  Insérez le support de récupération de serveur Windows Server Essentials que vous avez reçu du fabricant du serveur dans un ordinateur client.  
  
     L'Assistant Récupération de votre serveur s'ouvre.  
  
2.  Suivez les instructions de l'Assistant pour créer un lecteur flash USB que vous utiliserez pour démarrer le serveur en mode de récupération.  
  
3.  Une fois que l'Assistant Récupération de votre serveur a préparé le lecteur flash USB, insérez-le dans le serveur, puis démarrez celui-ci en mode de récupération. Pour savoir comment démarrer votre serveur en mode de récupération, consultez la documentation du fabricant de votre matériel de serveur.  
  
     Une fois le serveur démarré en mode de récupération, l'Assistant Récupération de votre serveur recherche le serveur, puis établit une connexion.  
  
4.  Suivez les instructions de l'Assistant pour terminer la restauration du serveur.  
  
> [!NOTE]
>  Cette méthode de récupération du serveur ignore les dispositifs de stockage externes qui sont connectés au serveur pendant la récupération. Si vous voulez effacer les données stockées sur un dispositif de stockage externe, vous devez le faire manuellement.  
  
> [!NOTE]
>  Si vous avez créé des dossiers partagés supplémentaires sur le serveur, ces dossiers risquent de ne pas être reconnus par le serveur après la restauration des données à partir de la sauvegarde. Vous devez repartager ces dossiers. Pour plus d'informations, consultez [Restaurer des fichiers et des dossiers sur le serveur](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesAndFolders).  
  
##  <a name="BKMK_RestoreFilesAndFolders"></a> Restaurer des fichiers et dossiers sur le serveur  
 En fonction de la méthode que vous avez employée pour restaurer ou réparer votre serveur et du type de stockage utilisé par le serveur, vous devrez peut-être récupérer les volumes de données après avoir restauré le lecteur système. Dans certains cas, vous devrez peut-être repartager des dossiers existants pour qu'ils soient reconnus par le serveur.  
  
 Voici quelques exemples de scénarios où il peut être nécessaire de restaurer des fichiers et des dossiers :  
  
-   [Restaurer des fichiers et dossiers à partir d'une sauvegarde du serveur](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesFromBackup). Si vous avez remplacé le disque système, ou si les informations de partition du disque système sont illisibles, vous pouvez restaurer le système, mais vous ne pouvez pas restaurer des données d'autres volumes sur ce disque. Pour restaurer des fichiers et des dossiers à partir d'autres volumes de données, vous devez utiliser l'Assistant Restauration de fichiers et de dossiers.  
  
-   [Restaurer des dossiers partagés sur le serveur](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_ConfigreSharedFolders). Si vous avez créé des dossiers partagés supplémentaires sur le serveur, à l'issue de la restauration du lecteur système à partir de la sauvegarde, les dossiers partagés sont toujours sur la partition de données ou ont été restaurés sur la partition de données, mais ils peuvent ne pas être reconnus par le serveur. Vous devez repartager ces dossiers.  
  
###  <a name="BKMK_RestoreFilesFromBackup"></a> Restaurer des fichiers et dossiers à partir d’une sauvegarde du serveur  
 L'Assistant Restauration de fichiers et de dossiers vous permet de protéger vos données si votre disque dur cesse de fonctionner ou en cas de suppression accidentelle de vos fichiers. Sauvegarde de Windows Server Essentials, vous pouvez créer une copie de toutes les données sur votre disque dur et stocker les données sur un périphérique de stockage externe. Si les données d'origine sur votre disque dur sont accidentellement effacées, remplacées ou deviennent inaccessibles en raison d'un dysfonctionnement, vous pouvez les restaurer à partir de la sauvegarde. L'Assistant Restauration de fichiers et de dossiers vous permet de restaurer un fichier ou dossier unique, plusieurs fichiers ou dossiers, ou un disque dur entier à partir d'une sauvegarde existante.  
  
 Après une restauration du système, vous devrez peut-être utiliser l'Assistant Restauration de fichiers et de dossiers pour restaurer des fichiers et dossiers qui n'ont pas été conservés lors de la restauration. Par exemple, si vous avez remplacé le disque système, ou si les informations de partition du disque système sont illisibles, vous ne pouvez pas restaurer les données d'autres volumes sur le disque système.  
  
> [!NOTE]
>  Vous ne pouvez pas utiliser l'Assistant Restauration de fichiers et de dossiers pour restaurer tout le lecteur système. Pour plus d’informations sur la restauration complète du système, consultez [restaurer ou réparer votre serveur à l’aide du support d’installation](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_1) ou [restaurer ou réinitialiser votre serveur à partir d’un ordinateur client à l’aide du DVD de récupération](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_2).  
  
##### <a name="to-restore-files-and-folders-from-a-server-backup"></a>Pour restaurer des fichiers et des dossiers à partir d'une sauvegarde du serveur  
  
1.  Ouvrez le tableau de bord Windows Server Essentials, puis cliquez sur le **appareils** onglet.  
  
2.  Cliquez sur le nom du serveur, puis sur **Restaurer des fichiers ou des dossiers du serveur** dans le volet **Tâches** .  
  
     L'Assistant Restauration de fichiers et de dossiers s'ouvre.  
  
3.  Suivez les instructions de l'Assistant pour restaurer les fichiers ou dossiers.  
  
> [!WARNING]
>  Pour plus d’informations sur la sauvegarde et restauration des fichiers et dossiers, consultez [Manage Backup and Restore](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md).  
  
###  <a name="BKMK_ConfigreSharedFolders"></a> Restaurer des dossiers partagés sur le serveur  
 Après avoir restauré le lecteur système du serveur s, si les dossiers partagés sont toujours sur la partition de données ou ont été restaurés sur la partition de données, vous devrez peut-être configurer à nouveau les dossiers partagés afin que le serveur pour déterminer les dossiers. La procédure suivante décrit comment ajouter des dossiers partagés qui ont été partagés précédemment.  
  
##### <a name="to-add-an-existing-folder-to-the-server-shared-folders"></a>Pour ajouter un dossier existant aux dossiers partagés du serveur  
  
1.  Dans l'Explorateur de fichiers, recherchez le dossier partagé sur le disque dur.  
  
2.  Cliquez avec le bouton droit sur le dossier partagé, cliquez sur **Propriétés**, sur l'onglet **Partage** , puis notez les autorisations du dossier.  
  
3.  Ouvrez une session sur le tableau de bord Windows Server Essentials, cliquez sur le **stockage** onglet, puis cliquez sur **ajouter un dossier** dans le **tâches de dossiers de serveur** volet.  
  
     L'Assistant Ajouter un dossier s'ouvre.  
  
4.  Tapez un nom pour le partage dans la zone **Nom** .  
  
5.  Cliquez sur **Parcourir**, accédez à *< lecteur\>\\< nom_serveur\>* \ServerFolders (par exemple *d:\Contoso\ServerFolders*), sélectionnez le dossier que vous souhaitez partager, puis cliquez sur **OK**.  
  
6.  Cliquez sur **Suivant**.  
  
7.  Spécifiez les autorisations que vous avez notées à l'étape 2, puis cliquez sur **Ajouter un dossier**.  
  
    > [!IMPORTANT]
    >  Ces autorisations remplaceront les autorisations existantes qui n’ont pas été ajoutées au dossier à l’aide du tableau de bord Windows Server Essentials.  
  
> [!IMPORTANT]
>  Une fois que vous avez fini d'ajouter des dossiers à la liste des dossiers partagés, assurez-vous que les dossiers sont inclus dans la sauvegarde du serveur. Pour plus d'informations sur l'ajout de dossiers à la sauvegarde du serveur, consultez [Configurer ou personnaliser la sauvegarde du serveur](Set-up-or-customize-server-backup.md).  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Gérer la sauvegarde et restauration](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Gérer Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Utiliser Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
