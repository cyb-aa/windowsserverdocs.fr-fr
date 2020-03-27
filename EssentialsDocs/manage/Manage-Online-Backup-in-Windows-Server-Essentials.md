---
title: Gérer la sauvegarde en ligne dans Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 95a9f593-fad7-4335-bd4d-c7bb8c033efb
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 0879f0300892e26e1afb9b33e857ce0e255308ee
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80311204"
---
# <a name="manage-online-backup-in-windows-server-essentials"></a>Gérer la sauvegarde en ligne dans Windows Server Essentials

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
 Après l’intégration de à Sauvegarde Microsoft Azure, la page gestion de la **sauvegarde en ligne** s’affiche dans le tableau de bord Windows Server Essentials. La page **Sauvegarde en ligne** vous permet d'effectuer des tâches d'administration courantes. Les fonctionnalités de la page de sauvegarde en ligne incluent :  
  
- Les pages de sous-section suivantes :  
  
  -   **Sauvegarde en ligne** Une fois le serveur inscrit pour la sauvegarde en ligne, cette section affiche l'état actuel de la sauvegarde, du stockage et des informations de compte.  
  
  -   **Dossiers protégés** Cette section répertorie tous les dossiers partagés et tous les dossiers de l’historique des fichiers sur le serveur, ainsi que tous les autres dossiers que vous avez choisis de sauvegarder dans Azure. La liste inclut le nom de dossier, le chemin d'accès du dossier et le statut de chaque dossier partagé.  
  
  -   **Historique de sauvegarde** Cette section affiche la liste des sauvegardes en ligne récentes. La liste comprend le type de l'opération ainsi que l'heure et le statut de chaque sauvegarde.  
  
- Un volet d'informations contenant des informations supplémentaires sur une opération de sauvegarde ou de restauration sélectionnée.  
  
- Le volet des tâches qui inclut un ensemble de tâches d'administration que vous pouvez effectuer.  
  
  Au titre de meilleure pratique, vous devez sauvegarder les données d'entreprise et d'utilisateur les plus importantes. Par exemple, vous sauvegardez les dossiers de serveur qui contiennent des fichiers de données critiques. Vous devez également sauvegarder l'historique des fichiers pour les ordinateurs du réseau qui contiennent des informations critiques.  
  
  Lorsque vous choisissez des données à sauvegarder en ligne, songez également à la fréquence des sauvegardes et à la stratégie de rétention que vous souhaitez mettre en œuvre. Pensez ensuite en revue votre budget et déterminez la quantité de stockage dont vous pouvez disposer. Trouvez le juste équilibre entre le coût, le volume de stockage et vos besoins, puis configurez la sauvegarde en ligne pour protéger vos données importantes autant que possible. Pour des informations de tarification, consultez les [détails de la tarification d’Azure Backup](https://azure.microsoft.com/pricing/details/backup/).  
  
  Les sections suivantes décrivent différentes tâches de sauvegarde en ligne qui peuvent apparaître dans le tableau de bord Windows Server Essentials.  
  
## <a name="online-backup-tasks-in-the-dashboard"></a>Tâches de sauvegarde en ligne dans le tableau de bord  
  
-   [Télécharger un certificat dans le coffre de sauvegarde Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Configurer la sauvegarde en ligne](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Démarrer une sauvegarde en ligne](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Restaurer des fichiers et des dossiers à partir d’une sauvegarde en ligne](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Inscrire ce serveur pour la sauvegarde](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Annuler l’inscription du serveur](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_6)  
  
###  <a name="upload-a-certificate-to-the-azure-backup-vault"></a><a name="BKMK_1"></a>Télécharger un certificat dans le coffre de sauvegarde Azure  
 Avant de pouvoir utiliser la sauvegarde Azure pour les sauvegardes en ligne dans Windows Server Essentials, vous devez télécharger un certificat public à inscrire auprès du coffre de sauvegarde. Le certificat est utilisé pour authentifier le déploiement de la sauvegarde Azure (l’agent), agissant pour le compte du propriétaire de l’abonnement Microsoft Online Services pour gérer les ressources associées à l’abonnement.  
  
> [!NOTE]
>  Avant de télécharger le certificat, vous devez effectuer les procédures d'[inscription au service de Sauvegarde Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_16).  
  
##### <a name="to-upload-a-certificate-to-use-with-the-azure-backup-service"></a>Pour télécharger un certificat à utiliser avec le service de Sauvegarde Azure  
  
1. Ouvrez une session sur le tableau de bord Windows Server Essentials avec un compte d'administrateur.  
  
2. Dans la page **Accueil** du Tableau de bord, cliquez sur **SAUVEGARDE EN LIGNE**.  
  
3. Dans la zone **SAUVEGARDE EN LIGNE**, cliquez sur **Télécharger le certificat dans le coffre de Sauvegarde Azure**.  
  
    Cela ouvre **Recovery Services** dans le portail de gestion Azure. Si vous n’êtes pas déjà connecté à Azure, vous devez vous connecter à l’aide de votre compte Microsoft.  
  
4. Cliquez sur le nom du coffre de sauvegarde que vous utiliserez pour les sauvegardes en ligne pour ouvrir la page de **démarrage rapide** du coffre de sauvegarde.  
  
5. Cliquez sur **Gérer le certificat**.  
  
6. Dans la boîte de dialogue **Gérer le certificat**, collez le chemin d'accès du certificat public généré par Windows Server Essentials. Pour obtenir le chemin d'accès du certificat public, procédez comme suit :  
  
   1.  Dans le tableau de bord Windows Server Essentials, cliquez sur l'onglet **Sauvegarde en ligne**.  
  
   2.  Dans la page **Sauvegarde en ligne**, copiez le chemin d'accès du certificat généré.  
  
   3.  Basculez vers le Portail de gestion Azure, puis, dans la boîte de dialogue **gérer le certificat** , collez le chemin d’accès pour télécharger le certificat public généré.  
  
   > [!NOTE]
   >  Vous pouvez également utiliser votre propre certificat public. Pour savoir quel certificat est requis, dans la page **Démarrage rapide**, cliquez sur le lien **Obtenir le certificat**.  
  
   > [!NOTE]
   >   Azure requiert un certificat x. 509 v2 avec une clé publique. Pour plus d’informations, consultez [Gérer les certificats de coffre](https://msdn.microsoft.com/library/azure/dn169036.aspx).  
  
7. Après avoir sélectionné le certificat, cliquez sur **OK** (coche).  
  
8. Vous êtes renvoyé à la page **Démarrage rapide**. Cliquez sur **Tableau de bord**, et vérifiez que le certificat a été téléchargé correctement. Une fois que le certificat a été téléchargé correctement, le tableau de bord affiche l'empreinte numérique et la date d'expiration du certificat.  
  
   Après avoir effectué cette procédure, procédez comme suit :  
  
9. Inscrivez le serveur auprès du service Azure Backup. Pour plus d'informations, voir [Inscrire ce serveur pour la sauvegarde](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5).  
  
10. Configurez la sauvegarde en ligne du serveur. Pour plus d'informations, voir [Configurer la sauvegarde en ligne](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2).  
  
###  <a name="configure-online-backup"></a><a name="BKMK_2"></a>Configurer la sauvegarde en ligne  
 Après avoir inscrit le serveur auprès de la sauvegarde Azure, vous pouvez configurer les paramètres de sauvegarde en ligne dans Windows Server Essentials.  
  
##### <a name="to-configure-online-backup"></a>Pour configurer la sauvegarde en ligne  
  
1.  À partir de l'Assistant Inscription de votre serveur ou de la page **Sauvegarde en ligne** du tableau de bord Windows Server Essentials, cliquez sur **Configurer la sauvegarde en ligne**. L'Assistant Configurer la sauvegarde en ligne s'affiche.  
  
2.  Dans la page **configurer la sauvegarde en ligne** de l’Assistant, activez la case à cocher de chaque dossier de serveur que vous souhaitez sauvegarder dans sauvegarde Azure. Décochez la case de chaque dossier de serveur que vous ne souhaitez pas inclure dans la sauvegarde. Pour ajouter des dossiers qui ne figurent pas dans la liste, cliquez sur **Ajouter des dossiers**. Lorsque vous avez terminé votre sélection, cliquez sur **Suivant**.  
  
    > [!NOTE]
    >  Vous ne pouvez pas sélectionner un dossier qui ne se trouve pas sur le serveur local, ou qui se trouve sur un lecteur au format ReFS.  
  
3.  Dans la page **Ajouter des sauvegardes de l'historique des fichiers** de l'Assistant, vous pouvez choisir d'inclure des sauvegardes de l'historique des fichiers pour les ordinateurs du réseau qui prennent en charge la fonctionnalité d'historique des fichiers. Cliquez ensuite sur **Suivant** pour continuer.  
  
4.  Dans la page **Spécifier la planification de sauvegarde** de l'Assistant, configurez les paramètres suivants puis cliquez sur **Suivant** pour continuer :  
  
    -   Sélectionnez ou acceptez l'heure du jour à laquelle vous souhaitez que l'exécution de la sauvegarde en ligne se produise. L'heure par défaut est 22:00. Vous pouvez également spécifier l'heure d'exécution d'une deuxième sauvegarde.  
  
    -   Sélectionnez ou acceptez les jours auxquels vous souhaitez que l'exécution de la sauvegarde en ligne se produise. Par défaut, la sauvegarde en ligne est exécutée lundi au vendredi.  
  
5.  Dans la page **Spécifier la stratégie de rétention des sauvegardes** de l'Assistant, sélectionnez le nombre de jours pendant lesquels vous souhaitez conserver les sauvegardes en ligne, puis cliquez sur **Suivant**. La durée par défaut est de 7 jours. Vous pouvez également choisir de conserver les sauvegardes en ligne pendant 15 ou 30 jours.  
  
    > [!NOTE]
    >   Sauvegarde Azure conserve toujours votre sauvegarde la plus récente. Si la destination de sauvegarde ne dispose pas de suffisamment d'espace pour stocker la sauvegarde, le processus de sauvegarde échoue. Pour éviter cette situation, achetez de l'espace de stockage supplémentaire ou réduisez la période de rétention de données. Pour plus d’informations sur la tarification, consultez les détails de la [tarification](https://azure.microsoft.com/pricing/details/backup/) de sauvegarde Microsoft Azure.  
  
6.  Dans la page **Choisir votre bande passante** de l'Assistant, si vous voulez limiter la quantité de bande passante Internet allouée à la sauvegarde en ligne, sélectionnez **Activer l'utilisation de la bande passante sur Internet**. Utilisez les options de la page pour spécifier la quantité de bande passante Internet que vous acceptez de consacrer à la sauvegarde en ligne pendant les heures de travail et les heures non travaillées. Définissez ensuite vos heures et vos jours de travail.  
  
    > [!NOTE]
    >   Sauvegarde Azure vous permet de personnaliser la façon dont le logiciel d’intégration utilise la bande passante réseau lors de la sauvegarde ou de la restauration des informations. À l’aide d’une technique communément connue sous le nom de limitation, vous pouvez contrôler la quantité de bande passante réseau pouvant être utilisée par les processus de sauvegarde et de restauration pendant des intervalles de jours et de temps spécifiques. Après avoir coché la case **Activer l'utilisation de la bande passante sur Internet** dans l'Assistant Configuration de la sauvegarde en ligne, vous pouvez spécifier les jours et les heures de travail auxquels appliquer la limite de bande passante des heures de travail. La limite des heures non travaillées est utilisée en dehors de ces heures de travail. Les plages de bande passante valides vont de 256 Kbits/s à Illimité dans les deux cas.  
  
7.  À la fin de la configuration de la sauvegarde en ligne, cliquez sur **Fermer**.  
  
    > [!TIP]
    >  Après une configuration de sauvegarde réussie, la page **Sauvegarde en ligne** indique l'état de la dernière sauvegarde en ligne et la quantité d'espace de stockage utilisée par le coffre de sauvegarde. Pour afficher l'état des sauvegardes antérieures, cliquez sur **Historique de sauvegarde**.  
  
###  <a name="start-an-online-backup"></a><a name="BKMK_3"></a>Démarrer une sauvegarde en ligne  
  
> [!NOTE]
>  Avant de commencer une sauvegarde en ligne, vous devez d'abord [enregistrer ce serveur pour la sauvegarde](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5), puis [configurer la sauvegarde en ligne](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2).  
  
##### <a name="to-start-an-online-backup-immediately"></a>Pour démarrer une sauvegarde en ligne immédiatement  
  
1.  Ouvrez une session dans le tableau de bord en tant qu'administrateur.  
  
2.  Dans la barre de navigation, cliquez sur **SAUVEGARDE EN LIGNE**.  
  
3.  Dans le volet **Tâches de la sauvegarde en ligne**, cliquez sur **Démarrer la sauvegarde maintenant**.  
  
###  <a name="restore-files-and-folders-from-an-online-backup"></a><a name="BKMK_4"></a>Restaurer des fichiers et des dossiers à partir d’une sauvegarde en ligne  
 L'Assistant Restauration des fichiers et dossiers vous guide à travers le processus de recherche, de sélection et de restauration des fichiers et dossiers qui sont sauvegardés en ligne. À mesure que vous progresserez dans l'Assistant, vous allez effectuer les tâches suivantes :  
  
1.  **Choisir une sauvegarde en ligne à partir de laquelle restaurer des fichiers et des dossiers**  
  
     Lorsque vous exécutez l'Assistant Restauration des fichiers et dossiers, la première chose à faire est de spécifier si vous voulez restaurer des fichiers à partir d'une sauvegarde en ligne du serveur auquel vous êtes actuellement connecté, ou à partir d'une sauvegarde d'un autre serveur.  
  
2.  **Choisir un serveur à partir duquel restaurer des fichiers et des dossiers**  
  
     Si vous avez choisi de restaurer des fichiers et dossiers à partir d'un autre serveur, sélectionnez le serveur de restauration dans la liste des serveurs disponibles, puis cliquez sur **Suivant**.  
  
3.  **Choisir un volume qui contient les fichiers et dossiers à restaurer**  
  
     Les sauvegardes en ligne contiennent des volumes qui correspondent aux volumes sur le serveur source de la sauvegarde. Après avoir sélectionné le volume, sélectionnez la date et l'heure de la sauvegarde à restaurer, puis cliquez sur **Suivant**.  
  
4.  **Sélectionner les fichiers et dossiers à restaurer**  
  
     Dans la page **Sélectionner les éléments à restaurer** de l'Assistant, vous pouvez ouvrir les différents emplacements de dossiers et cocher la case en regard de chaque fichier ou dossier que vous voulez restaurer. Lorsque vous avez terminé de sélectionner les fichiers, cliquez sur **Suivant**.  
  
5.  **Spécifier l’emplacement de destination des fichiers et dossiers restaurés**  
  
     La page **Spécifier les options de restauration** de l'Assistant vous permet de spécifier où restaurer les fichiers et les dossiers. Deux options s’offrent à vous :  
  
    -   **Restaurer à l'emplacement d'origine**. Sélectionnez cette option pour restaurer les fichiers et dossiers à l'emplacement exact d'origine de la sauvegarde.  
  
    -   **Restaurer à un autre emplacement**.  Choisissez cette option pour spécifier un autre emplacement sur le serveur où restaurer les fichiers.  
  
         Dans l'installation par défaut, si un fichier portant le même nom qu'un fichier de restauration existe déjà à l'emplacement de destination, l'Assistant crée une copie du fichier d'origine. En outre, la liste de contrôle d'accès (ACL) est mise à jour pour refléter les autorisations de fichier actuelles. Vous pouvez cliquer sur **Avancé** pour personnaliser les paramètres par défaut.  
  
6.  **Confirmer les informations de restauration**  
  
     La page **Confirmer vos informations de restauration** fournit un résumé des instructions de restauration que vous avez spécifiées. Pour procéder à la restauration des fichiers, vous devez taper la phrase secrète appropriée pour votre compte de sauvegarde en ligne.  
  
###  <a name="register-this-server-for-backup"></a><a name="BKMK_5"></a>Inscrire ce serveur pour la sauvegarde  
 Pour sauvegarder ou restaurer des fichiers, des dossiers et l’historique des fichiers sur votre serveur Windows Server Essentials vers la sauvegarde Azure, vous devez d’abord inscrire votre serveur auprès du service Sauvegarde Microsoft Azure.  
  
> [!NOTE]
>  Avant d'inscrire votre serveur, vous devez télécharger un certificat à utiliser avec le coffre de sauvegarde dans lequel seront stockées les sauvegardes en ligne. Pour plus d'informations, voir [Upload a certificate to the Azure Backup vault](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1).  
  
##### <a name="to-register-your-server-with-azure-backup"></a>Pour inscrire votre serveur auprès d'Azure Backup  
  
1.  Connectez-vous au serveur en tant qu'administrateur, puis ouvrez le tableau de bord.  
  
2.  Dans la barre de navigation, cliquez sur **SAUVEGARDE EN LIGNE**.  
  
3.  Dans la sous-section **Sauvegarde en ligne**, cliquez sur **Inscrire**.  
  
4.  Choisissez le certificat que vous souhaitez utiliser avec le coffre de sauvegarde pour vos sauvegardes en ligne. Le certificat par défaut est sélectionné par défaut. Pour choisir un autre certificat, utilisez **Parcourir**. Ensuite, cliquez sur **Suivant**.  
  
5.  Suivez les instructions de l'Assistant pour créer une phrase secrète et procéder à l'inscription.  
  
###  <a name="unregister-server"></a><a name="BKMK_6"></a>Annuler l’inscription du serveur  
  
> [!CAUTION]
>  Si vous annulez l’inscription de votre serveur Windows Server Essentials à partir du service Sauvegarde Microsoft Azure, Azure Backup ne peut plus sauvegarder le serveur. En outre, les données du serveur précédemment téléchargées seront effacées. Pour reprendre les sauvegardes en ligne, vous devez vous réinscrire le serveur.  
  
##### <a name="to-unregister-your-server-with-azure-backup"></a>Pour désinscrire votre serveur d'Azure Backup  
  
1.  Connectez-vous au [portail de gestion Azure](https://manage.windowsazure.com).  
  
2.  Cliquez sur **Services de récupération**.  
  
3.  Dans **Services de récupération**, cliquez sur le nom du coffre de sauvegarde.  
  
4.  Dans la page **Démarrage rapide** du coffre, cliquez sur **Serveurs**.  
  
5.  Sélectionnez le serveur, cliquez sur **Supprimer**, puis sur **Oui** à l'invite de confirmation.  
  
## <a name="related-tasks"></a>Tâches connexes  
  
-   [Modifier la stratégie de sauvegarde en ligne](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_7)  
  
-   [Afficher l’utilisation du stockage de sauvegarde en ligne](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [Inclure un nouveau dossier dans la sauvegarde en ligne](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [Supprimer ou exclure des sauvegardes de l’historique des fichiers de la stratégie de sauvegarde en ligne](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_10)  
  
-   [Désactiver ou réactiver la sauvegarde de serveur en ligne](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_11)  
  
-   [Arrêter une sauvegarde de serveur en ligne en cours](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_12)  
  
-   [Afficher l’état de la sauvegarde en ligne](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_13)  
  
-   [Afficher et gérer les alertes de sauvegarde en ligne](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_14)  
  
-   [Rétablir les paramètres par défaut de la sauvegarde en ligne](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_15)  
  
-   [S’inscrire au service Azure Backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_16)  
  
-   [Intégrer la sauvegarde Azure à Windows Server Essentials](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_17)  
  
-   [Protéger des dossiers pour la sauvegarde en ligne dans Windows Server Essentials](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_18)  
  
-   [Historique de sauvegarde en ligne dans Windows Server Essentials](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_19)  
  
###  <a name="change-the-online-backup-policy"></a><a name="BKMK_7"></a>Modifier la stratégie de sauvegarde en ligne  
 Il est facile d'apporter des modifications à la stratégie de sauvegarde en ligne à l'aide du tableau de bord Windows Server Essentials.  
  
##### <a name="to-change-the-online-backup-policy"></a>Pour modifier la stratégie de sauvegarde en ligne  
  
1. Ouvrez une session dans le tableau de bord en tant qu'administrateur.  
  
2. Dans la barre de navigation, cliquez sur **Sauvegarde en ligne**.  
  
3. Dans le volet **Tâches de sauvegarde en ligne** , cliquez sur **Configurer la sauvegarde en ligne**.  
  
4. Suivez les instructions de l'Assistant pour personnaliser la stratégie de sauvegarde en ligne.  
  
   Pour plus d'informations sur les paramètres que vous pouvez personnaliser, voir [Configurer la sauvegarde en ligne](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2).  
  
###  <a name="view-online-backup-storage-usage"></a><a name="BKMK_8"></a>Afficher l’utilisation du stockage de sauvegarde en ligne  
  
##### <a name="to-view-the-amount-of-storage-space-that-online-backup-uses"></a>Pour afficher la quantité d'espace de stockage utilisée par la sauvegarde en ligne  
  
1.  Ouvrez une session dans le tableau de bord en tant qu'administrateur.  
  
2.  Dans la barre de navigation, cliquez sur **SAUVEGARDE EN LIGNE**.  
  
3.  Sous l'onglet **Sauvegarde en ligne** , le champ **État du stockage** apparaît dans le volet d'informations.  
  
###  <a name="include-a-new-folder-in-online-backup"></a><a name="BKMK_9"></a>Inclure un nouveau dossier dans la sauvegarde en ligne  
  
##### <a name="to-include-a-new-folder-in-the-online-backup-policy"></a>Pour inclure un nouveau dossier dans la stratégie de sauvegarde en ligne  
  
1. Ouvrez une session dans le tableau de bord en tant qu'administrateur.  
  
2. Dans la barre de navigation, cliquez sur **Sauvegarde en ligne**.  
  
3. Dans le volet **Tâches de sauvegarde en ligne** , cliquez sur **Configurer la sauvegarde en ligne**.  
  
4. Dans la page **Modifier ou supprimer la stratégie de sauvegarde**, cliquez sur **Personnaliser la stratégie de sauvegarde en ligne**.  
  
5. Dans la page **Configurer la sauvegarde en ligne** , si le dossier que vous souhaitez inclure n'apparaît pas dans la liste, cliquez sur **Ajouter des dossiers**.  
  
6. Dans la fenêtre **Ajouter des dossiers**, recherchez et sélectionnez le dossier que vous voulez inclure dans la sauvegarde en ligne, puis cliquez sur **OK**.  
  
7. Dans la page **Configurer la sauvegarde en ligne**, sélectionnez les autres dossiers à inclure en fonction des besoins, puis cliquez sur **Suivant**.  
  
8. Suivez les instructions de l'Assistant pour finaliser la personnalisation de la stratégie de sauvegarde en ligne.  
  
   Pour plus d'informations sur les autres paramètres que vous pouvez personnaliser, voir [Configurer la sauvegarde en ligne](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2).  
  
###  <a name="remove-or-exclude-file-history-backups-from-the-online-backup-policy"></a><a name="BKMK_10"></a>Supprimer ou exclure des sauvegardes de l’historique des fichiers de la stratégie de sauvegarde en ligne  
  
##### <a name="to-remove-or-exclude-a-folder-from-the-online-backup-policy"></a>Pour supprimer ou exclure un dossier de la stratégie de sauvegarde en ligne  
  
1.  Ouvrez une session dans le tableau de bord en tant qu'administrateur.  
  
2.  Dans la barre de navigation, cliquez sur **Sauvegarde en ligne**.  
  
3.  Cliquez sur l’onglet **dossiers protégés** . Le volet d’informations affiche la liste des dossiers avec leur état de sauvegarde en ligne.  
  
4.  Sélectionnez le dossier que vous souhaitez exclure de la stratégie de sauvegarde en ligne, puis dans le volet des tâches, cliquez sur **Supprimer le dossier de la sauvegarde en ligne**.  
  
###  <a name="disable-or-re-enable-online-server-backup"></a><a name="BKMK_11"></a>Désactiver ou réactiver la sauvegarde de serveur en ligne  
 Pour obtenir des instructions sur l’utilisation d’Azure Backup pour sauvegarder ou restaurer des données de serveur, consultez [inscrire ce serveur pour la sauvegarde](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5).  
  
 Pour obtenir des instructions sur la façon d’arrêter d’utiliser la sauvegarde Azure pour sauvegarder ou restaurer des données de serveur, consultez [Annuler l’inscription du serveur](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_6).  
  
###  <a name="stop-an-online-server-backup-in-progress"></a><a name="BKMK_12"></a>Arrêter une sauvegarde de serveur en ligne en cours  
  
##### <a name="to-stop-an-online-server-backup-in-progress"></a>Pour arrêter une sauvegarde de serveur en ligne en cours  
  
1.  Ouvrez une session dans le tableau de bord en tant qu'administrateur.  
  
2.  Dans la barre de navigation, cliquez sur **SAUVEGARDE EN LIGNE**.  
  
3.  Dans le volet **Tâches de sauvegarde en ligne**, cliquez sur **Arrêter la sauvegarde**.  
  
     Lorsque vous aurez arrêté la sauvegarde, celle-ci montrera l'état **Annulé** dans la liste **Historique de sauvegarde** .  
  
###  <a name="view-online-backup-status"></a><a name="BKMK_13"></a>Afficher l’état de la sauvegarde en ligne  
  
##### <a name="to-view-the-backup-status"></a>Pour afficher l'état de sauvegarde  
  
1.  Ouvrez une session dans le tableau de bord en tant qu'administrateur.  
  
2.  Dans la barre de navigation, cliquez sur **SAUVEGARDE EN LIGNE**.  
  
3.  Cliquez sur l’onglet **historique de sauvegarde** . Le mode liste indique l’état de chaque tâche de sauvegarde. Sélectionnez une tâche de sauvegarde pour afficher des détails supplémentaires sur cette tâche.  
  
###  <a name="view-and-manage-online-backup-alerts"></a><a name="BKMK_14"></a>Afficher et gérer les alertes de sauvegarde en ligne  
 À l’instar de nombreuses autres alertes, les alertes pour sauvegarde Azure s’affichent dans l’afficheur des alertes.  
  
##### <a name="to-view-online-backup-alerts-in-the-alert-viewer"></a>Pour afficher des alertes de sauvegarde en ligne dans l'Afficheur des alertes  
  
1. Ouvrez le Tableau de bord.  
  
2. Effectuez l'une des opérations suivantes :  
  
     Windows Server Essentials : dans le volet de navigation, cliquez sur l’icône alertes \(peut être critique, avertissement ou information\). L'Afficheur des alertes s'ouvre.  
  
     Windows Server Essentials : sur la page d' **hébergement** , cliquez sur l’onglet analyse de l' **intégrité** .  
  
3. Passez en revue la liste des alertes concernant les problèmes liés à la sauvegarde Azure.  
  
   Pour plus d’informations sur l’utilisation de l’onglet afficheur des alertes ou surveillance de l’intégrité pour gérer les alertes, consultez [gérer l’intégrité du système](Manage-System-Health-in-Windows-Server-Essentials.md).  
  
###  <a name="reset-online-backup-to-default-settings"></a><a name="BKMK_15"></a>Rétablir les paramètres par défaut de la sauvegarde en ligne  
 Windows Server Essentials fournit un Assistant qui vous permet de configurer les paramètres de sauvegarde en ligne. Si vous souhaitez restaurer les paramètres par défaut, exécutez la tâche **Configurer la sauvegarde en ligne**, puis choisissez l'option **Supprimer la stratégie de sauvegarde en ligne**. Ensuite, exécutez à nouveau la tâche **Configurer la sauvegarde en ligne**. Vos données précédemment téléchargées restent inchangées.  
  
###  <a name="sign-up-for-azure-backup-service"></a><a name="BKMK_16"></a>S’inscrire au service Azure Backup  
 Pour préparer l’intégration de Sauvegarde Microsoft Azure à Windows Server Essentials, vous devez vous connecter au Portail de gestion Azure avec votre compte Microsoft Online Services, puis créer un coffre de sauvegarde pour stocker vos sauvegardes en ligne dans Azure. Téléchargez ensuite le module d’intégration de sauvegarde Azure et utilisez le fichier téléchargé pour installer le complément Azure Backup sur votre serveur Windows Server Essentials. Si vous n'avez pas de compte Microsoft, vous pouvez vous inscrire à une évaluation gratuite.  
  
 Pour effectuer cette configuration, effectuez les tâches suivantes :  
  
1.  Inscrivez-vous à un compte Microsoft Online Services et la version d'évaluation de la sauvegarde.  
  
2.  Créez un coffre de sauvegarde pour stocker les sauvegardes en ligne.  
  
3.  Téléchargez l’agent de sauvegarde Azure.  
  
4.  Installez le complément Azure Backup sur le serveur.  
  
####  <a name="sign-up-for-a-microsoft-online-services-account-and-the-backup-preview"></a><a name="BKMK_SignupforaMicrosoftOnlineServiceAccount"></a>S’inscrire à un compte Microsoft Online Services et à la version préliminaire de la sauvegarde  
  
1.  Ouvrez une session sur le tableau de bord Windows Server Essentials.  
  
2.  Dans la page **Accueil** du tableau de bord, cliquez sur la catégorie **COMPLÉMENTS**, sur **Intégrer à la Sauvegarde Azure**, puis sur **Cliquer pour vous inscrire à la Sauvegarde Azure**.  
  
3.  Dans la page **Recovery Services** Azure, dans la section **sauvegarde (** préversion), passez en revue les détails.  
  
4.  Si vous n’avez pas d’abonnement Azure, cliquez sur **version d’évaluation gratuite**, puis suivez les instructions pour obtenir un abonnement Azure.  
  
     Si vous disposez déjà d’un abonnement Azure, cliquez sur **portail** dans le coin supérieur droit de la page Web pour accéder à la portail de gestion Azure.  
  
5.  Dans la page Portail de gestion Azure, vous verrez **Recovery Services** dans le volet gauche. C’est là que vous allez gérer les coffres de sauvegarde qui stockent vos sauvegardes en ligne à partir de Windows Server Essentials.  
  
####  <a name="create-a-backup-vault-to-store-online-backups"></a><a name="BKMK_Createabackupvaulttostoreonlinebackups"></a>Créer un coffre de sauvegarde pour stocker les sauvegardes en ligne  
  
1.  Connectez-vous au [portail de gestion Azure](https://manage.windowsazure.com)à partir du navigateur web sur votre serveur Windows Server Essentials.  
  
2.  Dans le Portail de gestion Azure, cliquez sur **nouveau**, sur **Data Services**, sur **Recovery Services**, sur **coffre de sauvegarde**, puis sur **création rapide**.  
  
3.  Entrez un nom pour votre coffre d'archivage, choisissez la région où vous voulez stocker vos sauvegardes, puis cliquez sur **Création rapide**.  
  
     La zone **Services de récupération** s'ouvre avec votre nouveau coffre d'archivage affiché.  
  
####  <a name="download-the-azure-backup-agent"></a><a name="BKMK_DownloadtheWindowsAzureBackupAgent"></a>Télécharger l’agent de sauvegarde Azure  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans la page **Accueil** du tableau de bord, cliquez sur l'onglet **Commencer** , sur la catégorie **COMPLÉMENTS** , sur **Intégrer à Azure Backup**, puis sur **Cliquer pour télécharger le module d'intégration Azure Backup**.  
  
####  <a name="install-the-azure-backup-add-in-on-the-server"></a><a name="BKMK_InstalltheWindowsAzureBackupAddIn"></a>Installer le complément Azure Backup sur le serveur  
  
1. Connectez-vous à votre serveur à l'aide d'un compte d'administrateur, puis exécutez le fichier **OnlineBackupAddin.wssx** que vous avez téléchargé à l'étape précédente.  
  
2. Les **termes du contrat de licence logiciel** sont affichés. Si vous acceptez les termes et conditions, cliquez sur **Accepter** pour continuer l'installation.  
  
3. Dans la page **Installer le complément**, cliquez sur **Installer le complément**.  
  
   > [!NOTE]
   >  Vous pouvez être invité à redémarrer le serveur pour installer tout logiciel requis.  
  
    La page **Installation** s’affiche. Un indicateur de progression s'affiche lorsque l'installation commence, pour indiquer la progression de l'installation. Une fois l’installation terminée, vous recevrez un message indiquant que le complément Azure Backup a été installé avec succès.  
  
4. Cliquez sur **Terminer**.  
  
5. Fermez et rouvrez le tableau de bord.  
  
    Un nouvel onglet **Sauvegarde en ligne**, est ajouté au tableau de bord. À partir de cet onglet, vous pouvez inscrire votre serveur, configurer les paramètres de sauvegarde et ouvrir **Recovery Services** dans le portail de gestion Azure pour gérer les coffres de sauvegarde pour vos serveurs.  
  
   Après avoir effectué cette procédure, procédez comme suit :  
  
6. Dans Windows Server Essentials, téléchargez un certificat à utiliser pour les sauvegardes en ligne. Pour plus d'informations, voir [Upload a certificate to the Azure Backup vault](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1).  
  
7. Inscrivez le serveur auprès du coffre de sauvegarde Azure. Pour plus d'informations, voir [Inscrire ce serveur pour la sauvegarde](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5).  
  
8. Configurez la sauvegarde en ligne du serveur. Pour plus d'informations, voir [Configurer la sauvegarde en ligne](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2).  
  
###  <a name="integrate-azure-backup-with-windows-server-essentials"></a><a name="BKMK_17"></a>Intégrer la sauvegarde Azure à Windows Server Essentials  
 Le module d’intégration Sauvegarde Microsoft Azure est une nouvelle fonctionnalité de Windows Server Essentials qui vous permet de chiffrer et de sauvegarder des fichiers et des dossiers à partir de votre serveur vers un système de stockage hébergé par Azure fourni par Microsoft. En utilisant la sauvegarde Azure pour chiffrer et sauvegarder des données sur le serveur, vous pouvez éviter une perte catastrophique de données métier critiques en raison d’un incendie, d’un inondation, d’un vol ou d’autres catastrophes. Lorsque vous utilisez la sauvegarde Azure pour sauvegarder les données du serveur, les informations sont chiffrées à l’aide de votre phrase secrète avant d’être téléchargées vers un centre de données sécurisé sur Internet. Pour accéder aux données à partir d'une sauvegarde en ligne, vous devez disposer d'un serveur authentifié par un certificat et fournir votre phrase secrète.  
  
 Après l’intégration et l’inscription du serveur auprès d’Azure Backup, vous pouvez configurer les paramètres de sauvegarde en ligne pour effectuer des sauvegardes planifiées régulièrement. Vous pouvez également démarrer une sauvegarde en ligne à tout moment en cliquant sur la tâche **Démarrer la sauvegarde maintenant** dans le tableau de bord de sauvegarde en ligne.  
  
 La configuration de la sauvegarde en ligne implique les étapes suivantes :  
  
1.  Inscrivez-vous au [service Azure Backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_16) -après vous être inscrit au service de sauvegarde, vous allez créer un coffre de sauvegarde, télécharger et installer le module d’intégration de sauvegarde Azure.  
  
2.  [Télécharger un certificat dans le coffre de sauvegarde Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1)  
  
3.  [Inscrire ce serveur pour la sauvegarde](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)  
  
4.  [Configurer la sauvegarde en ligne](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)  
  
> [!NOTE]
>   Azure Backup utilise la phrase secrète pour chiffrer les fichiers et les dossiers pour la sauvegarde en ligne. Changer la phrase secrète de chiffrement remplace la phrase secrète que vous avez spécifiée lors de l'inscription du serveur. La phrase secrète accepte uniquement les caractères ASCII.  
  
###  <a name="protect-folders-for-online-backup-in-windows-server-essentials"></a><a name="BKMK_18"></a>Protéger des dossiers pour la sauvegarde en ligne dans Windows Server Essentials  
 La sous-section **Dossiers protégés** de la section Sauvegarde en ligne du tableau de bord affiche une liste de dossiers partagés sur le serveur. Le tableau suivant décrit les informations qui figurent dans la liste.  
  
|Column|Description|  
|------------|-----------------|  
|**Nom du dossier :**|Le nom du dossier qui est inclus dans la sauvegarde en ligne.<br /><br /> Pour ajouter ou exclure un dossier, exécutez la tâche **Configurer la sauvegarde en ligne**.|  
|**Chemin d’accès au dossier :**|L'emplacement du dossier.|  
|**Statut :**|Il existe trois types d’État : **protégé**, **non protégé**et **inconnu**.|  
  
###  <a name="online-backup-history-in-windows-server-essentials"></a><a name="BKMK_19"></a>Historique de sauvegarde en ligne dans Windows Server Essentials  
 La sous-section **Historique de sauvegarde** de la section Sauvegarde en ligne du tableau de bord affiche une liste des sauvegardes en ligne récentes. Vous pouvez utiliser les sauvegardes réussies pour restaurer des fichiers et dossiers. Le tableau suivant décrit les informations qui figurent dans la liste.  
  
|Column|Description|  
|------------|-----------------|  
|**Opération**|Il existe deux types d'opérations - **Sauvegarde** et **Restauration**.|  
|**Simultanément**|Il s'agit de l'heure enregistrée de l'état le plus récent.|  
|**Statut :**|Il existe cinq types d’État : **succès**, **en cours**, **annulé**, **Avertissement**et **échec**.|  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Gérer la sauvegarde et la restauration](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Gérer Microsoft Online Services](Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)  
  
-   [Gérer Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Utiliser Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
