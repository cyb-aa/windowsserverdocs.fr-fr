---
title: "Restaurer ou réparer votre serveur exécutant WindowsServerEssentials"
description: "Décrit comment utiliser WindowsServerEssentials"
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
ms.openlocfilehash: e1ebc539928d13b0d34dfe5a0ee57ce6e98088e9
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="restore-or-repair-your-server-running-windows-server-essentials"></a>Restaurer ou réparer votre serveur exécutant WindowsServerEssentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials
 
 Cette rubrique fournit une vue d’ensemble et la prise en charge des procédures pour la restauration ou réparation d’un serveur exécutant WindowsServerEssentials et comprend les sections suivantes:  
  
-   [Vue d’ensemble des restaurations système du serveur](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Overview)  
  
-   [Restaurer ou réparer le lecteur système](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore)  
  
-   [Restaurer des fichiers et dossiers sur le serveur](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesAndFolders)  
  
##  <a name="BKMK_Overview"></a>Vue d’ensemble des restaurations système du serveur  
 L’état du serveur lorsque vous effectuez une restauration a une incidence sur la méthode de restauration disponible et une restauration de l’étendue que vous pouvez effectuer.  
  
 Les raisons les plus courantes pour la restauration d’un serveur sont:  
  
-   Un virus sur le serveur ne peut pas être inoculé ou supprimé.  
  
-   Les paramètres de configuration du serveur sont incorrects et vous ne pouvez pas démarrer le serveur.  
  
-   Vous avez remplacé le lecteur du système.  
  
-   Vous êtes retrait du serveur, et vous voulez restaurer vers un nouveau serveur.  
  
 Vous pouvez restaurer le serveur à partir d’une sauvegarde, ou vous pouvez restaurer le serveur de paramètres d’usine par défaut.  
  
-   [La restauration du serveur à partir d’une sauvegarde](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFromBackup)  
  
-   [Réinitialisation des paramètres d’usine par défaut le serveur](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_FactoryReset)  
  
###  <a name="BKMK_RestoreFromBackup"></a>La restauration du serveur à partir d’une sauvegarde  
 Cette section fournit des conseils sur le type de sauvegarde à choisir.  
  
 Si une sauvegarde est disponible, le meilleur choix pour la restauration de votre serveur consiste à utiliser le support d’installation s fabricant restaurer à partir d’une sauvegarde externe. La restauration récupérera les paramètres du serveur et des dossiers à partir de la sauvegarde que vous choisissez. Vous devez uniquement configurer les paramètres et restaurer les données créées après la sauvegarde.  
  
 Lorsque vous choisissez de récupérer votre serveur à la restauration à partir d’une sauvegarde précédente, vous devez choisir la sauvegarde spécifique que vous souhaitez restaurer, et vous devez disposer d’un fichier de sauvegarde valide sur un disque dur externe qui est connecté directement au serveur:  
  
-   **Si vous disposez d’une sauvegarde très récente du serveur**, et que vous savez que la sauvegarde contient toutes vos données critiques, votre choix est assez simple. Vous devrez uniquement recréer les données qui a été créées après que votre dernière bonne sauvegarde et reconfigurer les paramètres des modifications apportées après la sauvegarde.  
  
-   **Si vous restaurez votre serveur en raison d’un virus**, sélectionnez une sauvegarde qui s’est produite avant la contamination par le virus. Vous devrez peut-être revenir plusieurs jours pour sélectionner une sauvegarde correcte.  
  
-   **Si vous restaurez votre serveur en raison des paramètres de configuration incorrects**, sélectionnez une sauvegarde qui a été effectuée avant le paramètre modification qui provoque le problème sur le serveur de configuration.  
  
 Lorsque vous restaurez à partir d’une sauvegarde, le processus exact et le suivi nécessaire dépendent du nombre de disques durs sur le serveur et si le lecteur système est remplacé:  
  
-   **Si le serveur dispose d’un seul disque dur et que le lecteur n’est pas remplacé**, les informations de partition du lecteur sont laissées intactes quand vous restaurez le serveur. Le volume système est restauré et les données sur le volume restant sont conservées.  
  
-   **Si le serveur dispose d’un seul disque dur et le lecteur est remplacé**, le volume système est restauré et puis, vous devez restaurer manuellement les dossiers sur le volume de données. Les dossiers partagés par défaut doivent être créés, car ils ne sont pas créés lorsque le stockage de serveur est recréé.  
  
-   **Si le serveur dispose de plusieurs disques durs et que lecteur 0 (contenant le volume système) n’est pas remplacé**, les informations de partition du lecteur sont laissées intactes quand vous restaurez le serveur. Le volume système est restauré et les données sur tous les volumes restants sont conservées.  
  
-   **Si le serveur dispose de plusieurs disques durs et que lecteur 0 (contenant le volume système) est remplacé**, le volume système est restauré et puis vous devez restaurer manuellement les dossiers partagés qui étaient auparavant stockés sur le disque 0.  
  
###  <a name="BKMK_FactoryReset"></a>Réinitialisation des paramètres d’usine par défaut le serveur  
 Si vous ne disposez pas d’une sauvegarde que vous pouvez restaurer à partir de, ou pour une raison quelconque vous souhaitez ou devez effectuer une restauration complète du système sans restaurer la configuration de serveur précédente, vous pouvez effectuer une restauration qui rétablit le serveur de paramètres d’usine par défaut à l’aide d’installation ou support de récupération du fabricant du matériel de serveur.  
  
 Lorsque vous restaurez votre serveur en rétablissant les paramètres d’usine par défaut, tous les paramètres existants et les applications installées sur votre serveur sont supprimées, et vous devez configurer votre serveur à nouveau. Après la réinitialisation, le serveur redémarre.  
  
 Lorsque vous effectuez une réinitialisation d’usine, vous pouvez choisir de garder vos données ou de supprimer, avec les conséquences suivantes:  
  
-   Si vous choisissez de conserver toutes vos données, toutes les données sur le volume système est supprimé, mais les données sur les autres volumes sont conservées.  
  
    > [!CAUTION]
    >  Si les paramètres du disque ne correspondent pas les paramètres par défaut, toutes les données sur un disque sont supprimées. Si vous avez remplacé le disque système, le nouveau disque doit être supérieur au volume de système de s disque d’origine.  
    >   
    >  Si les informations de partition d’un lecteur système sont illisibles, ou si vous remplacez le lecteur du système, toutes les données sur le lecteur système sont supprimées, même si vous choisissez de conserver toutes vos données.  
  
-   Si vous prévoyez de retirer ou réaffecter le serveur, choisissez de supprimer toutes vos données. En plus de la configuration du serveur, les autres paramètres et les données sur le volume système, toutes les autres données est supprimé et tous les disques durs sur le serveur sont reformatés.  
  
> [!NOTE]
>  Si les espaces de stockage est configuré sur le serveur, avant d’effectuer une réinitialisation d’usine, vous devez utiliser le **avancé** section de la **gérer les espaces de stockage** console pour supprimer manuellement tous les espaces de stockage.  
  
 Après la réinitialisation, vous devez effectuer les tâches suivantes:  
  
-   **Reconfigurer le serveur.** Sur le serveur, utilisez l’Assistant Configuration de serveur pour réentrer les paramètres de configuration. Pour configurer un serveur géré à distance de WindowsServerEssentials à partir d’un ordinateur client, ouvrez un navigateur web et tapez **http://***< yourServerName\ >* dans la barre d’adresses.  
  
-   **Reconnecter les ordinateurs clients au serveur.** Si un ordinateur était auparavant connecté au serveur, vous devez désinstaller le logiciel Connecteur WindowsServerEssentials à partir de l’ordinateur avant de reconnecter l’ordinateur au serveur. Pour plus d’informations, voir [désinstaller le logiciel Connecteur](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13) et [connecter des ordinateurs au serveur](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  
  
##  <a name="BKMK_Restore"></a>Restaurer ou réparer le lecteur système  
 La première étape de restauration du serveur consiste à restaurer ou réparer le lecteur système du serveur. Après avoir restauré le lecteur système, vous ferez tout ce qui est nécessaire pour restaurer les lecteurs de données sur le serveur et la restauration de tous les partages qui ont été perdus durant la restauration.  
  
 Trois méthodes sont disponibles pour effectuer la restauration:  
  
-   [Restaurer ou réparer votre serveur à l’aide du support d’installation](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_1). Utiliser le support d’installation du fabricant du serveur pour restaurer à partir d’une sauvegarde.  
  
-   **Utiliser le support d’installation pour restaurer le serveur pour les paramètres d’usine par défaut**. Pour savoir comment procéder sur votre serveur, consultez la documentation du fabricant du serveur.  
  
-   [Restaurer ou réinitialiser votre serveur à partir d’un ordinateur client à l’aide du DVD de récupération](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_2). Si vous avez besoin restaurer un serveur administré à distance qui exécute WindowsServerEssentials, vous devez effectuer la restauration à partir d’un ordinateur client à l’aide du DVD de restauration du fabricant du serveur.  
  
###  <a name="BKMK_Restore_1"></a>Restaurer ou réparer votre serveur à l’aide du support d’installation  
 La procédure suivante décrit comment restaurer le lecteur de système de votre serveur à partir d’une sauvegarde à l’aide du support d’installation WindowsServerEssentials. (Pour savoir comment utiliser le support d’installation pour restaurer les paramètres d’usine par défaut, consultez la documentation du fabricant du serveur.)  
  
> [!NOTE]
>  Si le serveur utilise des espaces de stockage, et que vous restaurez les données vers un nouveau serveur, vous devez récupérer le lecteur système tout d’abord, puis ouvrez une session sur le tableau de bord WindowsServerEssentials, configurer les espaces de stockage de la même façon que sur l’ancien serveur et puis récupérer les volumes de données.  
  
##### <a name="to-restore-the-server-system-drive-from-a-backup-using-installation-media"></a>Pour restaurer le lecteur de système de serveur à partir d’une sauvegarde à l’aide du support d’installation  
  
1.  Insérez le DVD d’installation de WindowsServerEssentials dans le lecteur de DVD de serveur, redémarrez le serveur, puis appuyez sur une touche pour démarrer à partir du DVD.  
  
    > [!NOTE]
    >  Si le processus de restauration ne démarre pas automatiquement, vérifiez les paramètres du BIOS de votre serveur pour vous assurer que le lecteur de DVD apparaît en premier dans le menu de démarrage.  
  
     - Ou -  
  
     Si le fabricant a préchargé le support d’installation sur le serveur, appuyez sur F8 au démarrage pour démarrer à partir du support d’installation.  
  
2.  Une fois le serveur Windows fichiers charger, choisissez votre langue et autres préférences, puis cliquez sur **suivant**.  
  
3.  Sur la page suivante de l’Assistant, cliquez sur **réparer votre ordinateur**.  
  
    > [!CAUTION]
    >  Ne choisissez pas le **installer maintenant** option. Cette option vous guidera à travers une installation complète du système qui supprime tous les paramètres de configuration et toutes les données sur le lecteur système.  
  
4.  Sur le **choisir une option**, cliquez sur **dépanner**.  
  
5.  Sur le **récupération de l’Image système**, sélectionnez le système actuel? soit **WindowsServerEssentials** ou **WindowsServerEssentials**.  
  
     L’Assistant Réimager votre ordinateur s’ouvre.  
  
6.  Sur le **sélectionner une sauvegarde d’image système** page, vous pouvez choisir d’utiliser la dernière sauvegarde, ou vous pouvez sélectionner une sauvegarde antérieure. Le système sera restauré à l’état où il se trouvait au moment de la sauvegarde que vous choisissez pour la restauration ou la réparation de votre serveur. Les données qui ont été ajoutées ou les modifications apportées aux paramètres qui ont été effectuées après la sauvegarde a été enregistrée doivent être recréées.  
  
     Sélectionnez une des options suivantes, puis cliquez sur **suivant**:  
  
    -   **Utiliser la dernière image système (recommandée)**  
  
    -   **Sélectionnez une image du système**  
  
    > [!NOTE]
    >  Si vous disposez d’une sauvegarde très récente du serveur, et vous savez que la sauvegarde contient toutes vos données critiques, votre choix est assez simple. Vous devrez uniquement recréer les données qui a été créées après que votre dernière bonne sauvegarde et reconfigurer les paramètres des modifications apportées après la sauvegarde.  
    >   
    >  Si vous restaurez votre serveur en raison d’un virus, sélectionnez une sauvegarde qui a été effectuée avant la contamination par le virus. Vous devrez peut-être revenir plusieurs jours pour sélectionner une sauvegarde correcte.  
    >   
    >  Si vous restaurez votre serveur en raison des paramètres de configuration incorrects, sélectionnez une sauvegarde qui a été effectuée avant la modification du paramètre de configuration qui provoque le problème sur le serveur.  
  
7.  Suivez les instructions de l’Assistant pour effectuer la restauration du système.  
  
8.  Une fois que le serveur est correctement restauré, supprimez du DVD d’installation si vous avez utilisé un et puis redémarrez le serveur.  
  
> [!NOTE]
>  Pour restaurer et partager des dossiers sur le serveur, vous devrez peut-être effectuer des étapes supplémentaires. Pour plus d’informations, voir [restaurer des fichiers et dossiers sur le serveur](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesAndFolders).  
  
###  <a name="BKMK_Restore_2"></a>Restaurer ou réinitialiser votre serveur à partir d’un ordinateur client à l’aide du DVD de récupération  
 Dans WindowsServerEssentials, vous pouvez démarrer le serveur à partir d’un lecteur flash USB que vous créez, puis restaurer le serveur à partir d’un ordinateur client en utilisant le DVD de récupération que vous avez reçu du fabricant du serveur. L’ordinateur client doit être sur le même réseau que le serveur. Cette méthode n’est pas disponible dans WindowsServerEssentials.  
  
 La procédure suivante fournit les étapes générales pour effectuer une restauration du serveur. Les étapes sont également applicables pour la restauration à partir d’une sauvegarde ou restauration des paramètres d’usine par défaut. Pour obtenir des instructions plus spécifiques, consultez la documentation du fabricant de votre serveur.  
  
##### <a name="to-restore-or-reset-the-server-from-a-client-computer-using-the-recovery-dvd"></a>Pour restaurer ou réinitialiser le serveur à partir d’un ordinateur client à l’aide du DVD de récupération  
  
1.  Insérez le support de récupération de serveur WindowsServerEssentials que vous avez reçu du fabricant du serveur dans un ordinateur client.  
  
     L’Assistant Récupération de votre serveur s’ouvre.  
  
2.  Suivez les instructions de l’Assistant pour créer un lecteur flash USB que vous utiliserez pour démarrer le serveur en mode de récupération.  
  
3.  Une fois l’Assistant Récupération de votre serveur prépare le lecteur flash USB, insérez le disque mémoire flash sur le serveur et puis démarrer le serveur en mode de récupération. Pour savoir comment démarrer votre serveur en mode de récupération, reportez-vous à la documentation du fabricant de votre matériel de serveur.  
  
     Une fois que vous démarrez le serveur en mode de récupération, l’Assistant Récupération de votre serveur recherche le serveur et puis établit une connexion.  
  
4.  Suivez les instructions de l’Assistant pour terminer la restauration du serveur.  
  
> [!NOTE]
>  Cette méthode de récupération du serveur ignore les dispositifs de stockage externes qui sont connectés au serveur pendant la récupération. Si vous souhaitez effacer les données sur un périphérique de stockage externe, vous devez le faire manuellement.  
  
> [!NOTE]
>  Si vous avez créé des dossiers partagés supplémentaires sur le serveur, après avoir restauré les données à partir de la sauvegarde, les dossiers partagés supplémentaires peuvent ne pas être reconnus par le serveur. Vous devez repartager ces dossiers. Pour plus d’informations, voir [restaurer des fichiers et dossiers sur le serveur](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesAndFolders).  
  
##  <a name="BKMK_RestoreFilesAndFolders"></a>Restaurer des fichiers et dossiers sur le serveur  
 En fonction de la méthode que vous avez utilisé pour restaurer ou réparer votre serveur, et utilise le type de stockage du serveur, vous devrez peut-être récupérer les volumes de données après avoir restauré le lecteur du système. Dans certains cas, vous devrez peut-être repartager des dossiers existants afin qu’ils soient reconnus par le serveur.  
  
 Voici quelques exemples de lorsque vous devrez peut-être restaurer des fichiers et dossiers:  
  
-   [Restaurer des fichiers et dossiers à partir d’une sauvegarde du serveur](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesFromBackup). Si vous avez remplacé le disque système, ou si les informations de partition du disque système sont illisibles, vous pouvez restaurer le système, mais vous ne pouvez pas restaurer des données à partir d’autres volumes sur ce disque. Pour restaurer des fichiers et dossiers à partir d’autres volumes de données, vous devez utiliser l’Assistant de dossiers et de restauration de fichiers.  
  
-   [Restaurer des dossiers partagés sur le serveur](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_ConfigreSharedFolders). Si vous avez créé des dossiers partagés supplémentaires sur le serveur, après avoir restauré le lecteur du système à partir de la sauvegarde, les dossiers partagés sont toujours sur la partition de données ou ont été restaurés sur la partition de données, mais ne peuvent pas être reconnus par le serveur. Vous devez repartager ces dossiers.  
  
###  <a name="BKMK_RestoreFilesFromBackup"></a>Restaurer des fichiers et dossiers à partir d’une sauvegarde du serveur  
 La restauration de fichiers et dossiers Assistant vous aide à protéger vos données si votre disque dur cesse de fonctionner ou de suppression accidentelle de vos fichiers. Sauvegarde de WindowsServerEssentials, vous pouvez créer une copie de toutes les données sur votre disque dur et stocker les données sur un périphérique de stockage externe. Si les données d’origine sur votre disque dur sont accidentellement effacées, remplacées ou deviennent inaccessibles en raison d’un dysfonctionnement, vous pouvez restaurer les données à partir de la sauvegarde. La restauration de fichiers ou dossiers Assistant vous permet de restaurer un seul fichier ou dossier, plusieurs fichiers ou dossiers ou un disque dur entier à partir d’une sauvegarde existante.  
  
 Après une restauration du système, vous devrez peut-être utiliser l’Assistant de dossiers et de restauration de fichiers pour restaurer des fichiers et dossiers qui n’ont pas été conservés lors de la restauration. Par exemple, si vous avez remplacé le disque système, ou si les informations de partition du disque système sont illisibles, vous ne pouvez pas restaurer les données d’autres volumes sur le disque système.  
  
> [!NOTE]
>  Vous ne pouvez pas utiliser l’Assistant de dossiers et de restauration de fichiers pour restaurer le lecteur complète du système. Pour plus d’informations sur la restauration complète du système, consultez [restaurer ou réparer votre serveur à l’aide du support d’installation](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_1) ou [restaurer ou réinitialiser votre serveur à partir d’un ordinateur client à l’aide du DVD de récupération](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_2).  
  
##### <a name="to-restore-files-and-folders-from-a-server-backup"></a>Pour restaurer des fichiers et dossiers à partir d’une sauvegarde du serveur  
  
1.  Ouvrez le tableau de bord WindowsServerEssentials, puis cliquez sur le **périphériques** onglet.  
  
2.  Cliquez sur le nom du serveur, puis cliquez sur **restaurer des fichiers ou dossiers du serveur** dans les **tâches** volet.  
  
     L’Assistant Restauration de fichiers et dossiers s’ouvre.  
  
3.  Suivez les instructions de l’Assistant pour restaurer les fichiers ou dossiers.  
  
> [!WARNING]
>  Pour plus d’informations sur la sauvegarde et restauration des fichiers et dossiers, voir [gérer la sauvegarde et restauration](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md).  
  
###  <a name="BKMK_ConfigreSharedFolders"></a>Restaurer des dossiers partagés sur le serveur  
 Après avoir restauré le lecteur système du serveur s, si les dossiers partagés sont toujours sur la partition de données ou ont été restaurés sur la partition de données, vous devrez peut-être configurer les dossiers partagés à nouveau dans l’ordre pour le serveur les reconnaisse. La procédure suivante décrit comment ajouter des dossiers partagés qui ont été partagé précédemment.  
  
##### <a name="to-add-an-existing-folder-to-the-server-shared-folders"></a>Pour ajouter un dossier existant aux dossiers partagés du serveur  
  
1.  Dans l’Explorateur de fichiers, recherchez le dossier partagé sur le disque dur.  
  
2.  Cliquez sur le dossier partagé, cliquez sur **propriétés**, cliquez sur le **partage** onglet, puis notez les autorisations du dossier.  
  
3.  Ouvrez une session sur le tableau de bord WindowsServerEssentials, cliquez sur le **stockage** onglet, puis cliquez sur **ajouter un dossier** dans les **tâches de dossiers de serveur** volet.  
  
     L’ajout de qu'un dossier de l’Assistant s’ouvre.  
  
4.  Tapez un nom pour le partage dans le **nom** zone.  
  
5.  Cliquez sur **Parcourir**, accédez à *< lecteur partagé\ > \\ < ServerName\ >*\ServerFolders (par exemple *d:\Contoso\ServerFolders*), sélectionnez le dossier que vous souhaitez partager, puis cliquez sur **OK**.  
  
6.  Cliquez sur **suivant**.  
  
7.  Spécifiez les autorisations que vous avez notées à l’étape 2, puis cliquez sur **ajouter un dossier**.  
  
    > [!IMPORTANT]
    >  Ces autorisations remplaceront les autorisations existantes qui n’ont pas été ajoutées au dossier à l’aide du tableau de bord WindowsServerEssentials.  
  
> [!IMPORTANT]
>  Après avoir terminé l’ajout de dossiers à la liste des dossiers partagés, assurez-vous que les dossiers sont inclus dans la sauvegarde du serveur. Pour plus d’informations sur l’ajout de dossiers à la sauvegarde du serveur, consultez [définir configurer ou personnaliser la sauvegarde du serveur](Set-up-or-customize-server-backup.md).  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Gérer la sauvegarde et restauration](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Gérer WindowsServerEssentials](Manage-Windows-Server-Essentials.md)  
  
-   [Utiliser Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
