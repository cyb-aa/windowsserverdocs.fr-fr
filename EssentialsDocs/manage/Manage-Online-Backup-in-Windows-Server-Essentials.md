---
title: "Gérer la sauvegarde en ligne dans WindowsServerEssentials"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 95a9f593-fad7-4335-bd4d-c7bb8c033efb
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 37d59d500a2de1e2b98c848e7484ae13639d09b7
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="manage-online-backup-in-windows-server-essentials"></a>Gérer la sauvegarde en ligne dans WindowsServerEssentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials
  
 Après avoir intégré avec MicrosoftAzure Backup, le **sauvegarde en ligne** page de gestion s’affiche dans le tableau de bord WindowsServerEssentials. Le **sauvegarde en ligne** page vous permet d’effectuer des tâches d’administration courantes. Les fonctionnalités dans la page de sauvegarde en ligne sont les suivantes:  
  
-   Les pages de sous-section suivantes:  
  
    -   **Sauvegarde en ligne** après avoir inscrit le serveur pour la sauvegarde en ligne, cette section affiche l’état actuel de la sauvegarde, l’état de stockage et les informations de compte.  
  
    -   **Dossiers protégés** cette section répertorie tous les dossiers partagés et tous les dossiers de l’historique des fichiers sur le serveur et d’autres dossiers que vous avez choisi de sauvegarder dans Azure. La liste inclut le nom du dossier, chemin d’accès du dossier et l’état pour chaque dossier partagé.  
  
    -   **Historique de sauvegarde** cette section affiche la liste des sauvegardes en ligne récentes. La liste comprend le type d’opération et l’heure et le statut de chaque sauvegarde.  
  
-   Un volet d’informations avec des informations supplémentaires sur une opération de sauvegarde ou de restauration.  
  
-   Un volet des tâches qui inclut un ensemble de tâches d’administration que vous pouvez effectuer.  
  
 Comme meilleure pratique, vous devez sauvegarder les données d’entreprise et des utilisateurs plus importantes. Par exemple, vous devez sauvegarder des dossiers du serveur qui contiennent des fichiers de données critiques. Vous devez également sauvegarder l’historique des fichiers pour les ordinateurs du réseau qui contiennent des informations critiques.  
  
 Lorsque vous décidez de données à sauvegarder en ligne, envisagez de la stratégie de rétention et la fréquence de sauvegarde que vous souhaitez implémenter. Ensuite, passez en revue votre budget et déterminer la quantité d’espace de stockage que vous pouvez vous permettre de. Équilibrer le coût et le volume de stockage par rapport à vos besoins, puis configurez la sauvegarde en ligne afin de protéger vos données importantes que possible. Pour plus d’informations de tarification, consultez [détails de la tarification de la sauvegarde Azure](https://azure.microsoft.com/pricing/details/backup/).  
  
 Les sections suivantes décrivent les différentes tâches de sauvegarde en ligne qui peuvent apparaître dans le tableau de bord WindowsServerEssentials.  
  
## <a name="online-backup-tasks-in-the-dashboard"></a>Tâches de sauvegarde en ligne dans le tableau de bord  
  
-   [Télécharger un certificat dans le coffre de sauvegarde Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Configurer la sauvegarde en ligne](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Démarrer une sauvegarde en ligne](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Restaurer des fichiers et dossiers à partir d’une sauvegarde en ligne](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Inscrire ce serveur pour la sauvegarde](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Désinscrire le serveur](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_6)  
  
###  <a name="BKMK_1"></a>Télécharger un certificat dans le coffre de sauvegarde Azure  
 Avant de pouvoir utiliser Azure Backup pour les sauvegardes en ligne dans WindowsServerEssentials, vous devez télécharger un certificat public pour vous inscrire au coffre de sauvegarde. Le certificat est utilisé pour authentifier le déploiement de la sauvegarde Azure (agent), agissant pour le compte du propriétaire de l’abonnement MicrosoftOnline Services pour gérer les ressources associées à l’abonnement.  
  
> [!NOTE]
>  Avant de télécharger le certificat, vous devez effectuer les procédures de [inscription au Service de sauvegarde Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_16).  
  
##### <a name="to-upload-a-certificate-to-use-with-the-azure-backup-service"></a>Pour télécharger un certificat à utiliser avec le Service de sauvegarde Azure  
  
1.  Ouvrez une session sur le tableau de bord WindowsServerEssentials avec un compte d’administrateur.  
  
2.  Sur le tableau de bord **accueil**, cliquez sur **sauvegarde en ligne**.  
  
3.  Dans le **sauvegarde en ligne** zone, cliquez sur **télécharger le certificat coffre de sauvegarde Azure**.  
  
     Cela ouvre **Services de récupération** dans le portail de gestion Azure. Si t ne vous déjà ouvert une session Azure, vous devez ouvrir une session en utilisant votre compte Microsoft.  
  
4.  Cliquez sur le nom du coffre de sauvegarde que vous utiliserez pour les sauvegardes en ligne pour ouvrir le **Quick Start** page du coffre de sauvegarde.  
  
5.  Cliquez sur **gérer des certificats**.  
  
6.  Dans le **gérer les certificats** boîte de dialogue, collez le chemin d’accès du certificat public généré par WindowsServerEssentials. Pour obtenir le chemin d’accès du certificat public, procédez comme suit:  
  
    1.  Dans le tableau de bord WindowsServerEssentials, cliquez sur le **sauvegarde en ligne** onglet.  
  
    2.  Sur le **sauvegarde en ligne** page, copiez le chemin d’accès du certificat généré.  
  
    3.  Basculez vers le portail de gestion Azure, puis dans le **gérer les certificats** boîte de dialogue zone, collez le chemin d’accès pour télécharger le certificat public généré.  
  
    > [!NOTE]
    >  Vous pouvez également utiliser votre propre certificat public. Pour savoir quel certificat est requis, dans le **Quick Start**, cliquez sur le **obtenir le certificat** lien.  
  
    > [!NOTE]
    >   Azure requiert un certificat x.509 v2 avec une clé publique. Pour plus d’informations, voir [gérer les certificats de coffre](https://msdn.microsoft.com/library/azure/dn169036.aspx).  
  
7.  Après avoir sélectionné le certificat, cliquez sur **OK** (coche).  
  
8.  Vous revenez à la **Quick Start** page. Cliquez sur **tableau de bord**et vérifiez que le certificat a été téléchargé correctement. Une fois que le certificat a été téléchargé correctement, le tableau de bord affiche la date d’empreinte numérique et d’expiration de certificat.  
  
 Après avoir effectué cette procédure, procédez comme suit:  
  
1.  Inscrivez le serveur avec le Service de sauvegarde Azure. Pour plus d’informations, voir [inscrire ce serveur pour la sauvegarde](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5).  
  
2.  Configurer la sauvegarde en ligne du serveur. Pour plus d’informations, voir [configurer la sauvegarde en ligne](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2).  
  
###  <a name="BKMK_2"></a>Configurer la sauvegarde en ligne  
 Après avoir inscrit le serveur avec Azure Backup, vous pouvez configurer les paramètres de sauvegarde en ligne dans WindowsServerEssentials.  
  
##### <a name="to-configure-online-backup"></a>Pour configurer la sauvegarde en ligne  
  
1.  À partir de l’Assistant Inscription de votre serveur ou de la **sauvegarde en ligne** page du tableau de bord WindowsServerEssentials, cliquez sur **configurer la sauvegarde en ligne**. L’Assistant Configurer la sauvegarde en ligne s’affiche.  
  
2.  Sur le **configurer la sauvegarde en ligne** page de l’Assistant, sélectionnez la case à cocher de chaque dossier de serveur que vous souhaitez sauvegarder sur Azure Backup. Désactivez la case à cocher de chaque dossier de serveur que vous ne souhaitez pas inclure dans la sauvegarde. Pour ajouter des dossiers qui ne sont pas affichés dans la liste, cliquez sur **ajouter des dossiers**. Lorsque vous avez terminé votre sélection, cliquez sur **suivant**.  
  
    > [!NOTE]
    >  Vous ne pouvez pas sélectionner un dossier qui se trouve pas sur le serveur local ou un dossier qui se trouve sur un lecteur au format ReFS.  
  
3.  Sur le **ajouter des sauvegardes de l’historique des fichiers** page de l’Assistant, vous pouvez choisir d’inclure des sauvegardes de l’historique des fichiers pour les ordinateurs du réseau qui prennent en charge la fonctionnalité de l’historique des fichiers. Puis cliquez sur **suivant** pour continuer.  
  
4.  Sur le **spécifier la planification de sauvegarde** page de l’Assistant, configurez les éléments suivants, puis cliquez sur **suivant** pour continuer:  
  
    -   Sélectionnez ou acceptez l’heure du jour lorsque vous souhaitez que l’exécution de la sauvegarde en ligne. La durée par défaut est 10 h 00. Vous pouvez également spécifier un temps d’exécution d’une deuxième sauvegarde.  
  
    -   Sélectionnez ou acceptez les jours lorsque vous souhaitez que l’exécution de la sauvegarde en ligne. Par défaut, la sauvegarde en ligne s’exécute sur lundi au vendredi.  
  
5.  Sur le **spécifier la stratégie de rétention de sauvegarde** page de l’Assistant, sélectionnez le nombre de jours pendant lesquels vous souhaitez conserver les sauvegardes en ligne, puis cliquez sur **suivant**. La valeur par défaut est de 7jours. Vous pouvez également choisir de conserver les sauvegardes en ligne pendant 15 ou 30jours.  
  
    > [!NOTE]
    >   La sauvegarde Azure conserve toujours votre sauvegarde la plus récente. Si la destination de sauvegarde ne dispose pas de suffisamment d’espace stocker la sauvegarde, le processus de sauvegarde échoue. Pour éviter cette situation, acheter de l’espace de stockage supplémentaire ou réduisez la période de rétention de données. Pour plus d’informations de tarification, consultez [tarification](https://azure.microsoft.com/pricing/details/backup/) pour la sauvegarde MicrosoftAzure.  
  
6.  Sur le **choisir votre bande passante** page de l’Assistant, si vous souhaitez limiter la quantité de bande passante Internet qui est allouée à la sauvegarde en ligne, sélectionnez-le **utilisation de la bande passante Internet activer**. Utilisez les options de la page pour spécifier la bande passante Internet pour la sauvegarde en ligne à utiliser pendant le travail heures et pendant les heures chômées. Définissez ensuite vos heures et les jours ouvrables.  
  
    > [!NOTE]
    >   La sauvegarde Azure vous permet de personnaliser la manière avec laquelle le logiciel d’intégration utilise la bande passante réseau lors de la sauvegarde ou la restauration des informations. À l’aide d’une technique communément appelé la limitation, vous pouvez contrôler la quantité de bande passante réseau disponible pour une utilisation par la sauvegarde et restauration du processus pendant les jours spécifiques et les intervalles de temps. Après avoir sélectionné le **utilisation de la bande passante Internet activer** case à cocher dans l’Assistant Configuration de la sauvegarde en ligne, vous pouvez spécifier les jours et les heures de travail auquel appliquer la limite de bande passante des heures de travail. La limite des heures de travail est utilisée à tous les autres cas. Plages de bande passante valides vont de 256Kbits/s à illimité dans les deux cas.  
  
7.  Fin de la configuration de la sauvegarde en ligne, cliquez sur **fermer**.  
  
    > [!TIP]
    >  Après une configuration réussie de la sauvegarde, la **sauvegarde en ligne** page indique l’état de la dernière sauvegarde en ligne et la quantité d’espace de stockage utilisé par le coffre de sauvegarde. Pour afficher l’état des sauvegardes antérieures, cliquez sur **historique de sauvegarde**.  
  
###  <a name="BKMK_3"></a>Démarrer une sauvegarde en ligne  
  
> [!NOTE]
>  Avant de commencer une sauvegarde en ligne, vous devez d’abord [inscrire ce serveur pour la sauvegarde](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5), puis [configurer la sauvegarde en ligne](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2).  
  
##### <a name="to-start-an-online-backup-immediately"></a>Pour démarrer une sauvegarde en ligne immédiatement  
  
1.  Connectez-vous au tableau de bord en tant qu’administrateur.  
  
2.  Dans la barre de navigation, cliquez sur **sauvegarde en ligne**.  
  
3.  Dans le **des tâches de sauvegarde en ligne** volet, cliquez sur **démarrer la sauvegarde maintenant**.  
  
###  <a name="BKMK_4"></a>Restaurer des fichiers et dossiers à partir d’une sauvegarde en ligne  
 La restauration de fichiers et dossiers Assistant vous guide tout au long du processus de localisation, la sélection et la restauration des fichiers et dossiers qui sont sauvegardés en ligne. Que vous progresserez dans l’Assistant, vous allez effectuer les tâches suivantes:  
  
1.  **Choisir une sauvegarde en ligne à partir de laquelle restaurer des fichiers et dossiers**  
  
     Lorsque vous exécutez l’Assistant de dossiers et de restauration de fichiers, la première chose que vous devez faire est de spécifier si vous voulez restaurer des fichiers à partir d’une sauvegarde en ligne du serveur que vous êtes actuellement connecté, ou à partir d’une sauvegarde d’un autre serveur.  
  
2.  **Choisissez un serveur à partir de laquelle restaurer des fichiers et dossiers**  
  
     Si vous avez choisi de restaurer des fichiers et dossiers à partir d’un autre serveur, sélectionnez le serveur que vous souhaitez restaurer à partir de la liste des serveurs disponibles, puis cliquez sur **suivant**.  
  
3.  **Choisissez un volume qui contient les fichiers et dossiers à restaurer**  
  
     Les sauvegardes en ligne contiennent des volumes qui correspondent aux volumes sur le serveur source de la sauvegarde. Après avoir sélectionné le volume, sélectionnez la date et l’heure de la sauvegarde à partir de laquelle restaurer, puis cliquez sur **suivant**.  
  
4.  **Sélectionnez les fichiers et dossiers à restaurer**  
  
     Sur le **sélectionner les éléments à restaurer** page de l’Assistant, vous pouvez ouvrir les différents emplacements de dossier et sélectionnez la case à cocher pour chaque fichier ou dossier que vous voulez restaurer. Lorsque vous avez terminé de sélectionner des fichiers, cliquez sur **suivant**.  
  
5.  **Spécifiez l’emplacement de destination pour les fichiers et dossiers restaurés**  
  
     Le **spécifier les Options de restauration** page de l’Assistant vous permet de spécifier où restaurer les fichiers et dossiers. Il existe deux options:  
  
    -   **Restaurer à l’emplacement d’origine**. Sélectionnez cette option pour restaurer des fichiers et dossiers à l’emplacement exact à l’origine de la sauvegarde.  
  
    -   **Restaurer vers un autre emplacement**.  Choisissez cette option pour spécifier un autre emplacement sur le serveur dans lequel vous souhaitez restaurer les fichiers.  
  
         Dans l’installation par défaut, si un fichier portant le même nom qu’un fichier de restauration existe déjà à l’emplacement de destination, l’Assistant crée une copie du fichier d’origine. En outre, la liste de contrôle d’accès (ACL) est mis à jour pour refléter les autorisations de fichier en cours. Vous pouvez cliquer sur **avancé** pour personnaliser les paramètres par défaut.  
  
6.  **Vérifiez les informations de restauration**  
  
     Le **confirmer vos informations de restauration** page fournit un résumé des instructions de restauration que vous avez spécifiées. Pour procéder à la restauration de fichiers, vous devez taper la phrase secrète appropriée pour votre compte de sauvegarde en ligne.  
  
###  <a name="BKMK_5"></a>Inscrire ce serveur pour la sauvegarde  
 Pour sauvegarder ou restaurer des fichiers, dossiers et l’historique des fichiers sur votre serveur WindowsServerEssentials à Azure Backup, vous devez d’abord inscrire votre serveur avec le Service de sauvegarde MicrosoftAzure.  
  
> [!NOTE]
>  Avant de vous inscrivez votre serveur, vous devez télécharger un certificat à utiliser avec le coffre de sauvegarde qui stocke les sauvegardes en ligne. Pour plus d’informations, voir [télécharger un certificat dans le coffre de sauvegarde Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1).  
  
##### <a name="to-register-your-server-with-azure-backup"></a>Pour inscrire votre serveur auprès de la sauvegarde Azure  
  
1.  Ouvrez une session sur le serveur en tant qu’administrateur et ouvrez le tableau de bord.  
  
2.  Dans la barre de navigation, cliquez sur **sauvegarde en ligne**.  
  
3.  Dans le **sauvegarde en ligne** sous-section, cliquez sur **inscrire**.  
  
4.  Choisissez le certificat que vous souhaitez utiliser avec le coffre de sauvegarde pour vos sauvegardes en ligne. Le certificat par défaut est sélectionné par défaut. Si vous souhaitez sélectionner un autre certificat, utilisez **Parcourir**. Puis cliquez sur **suivant**.  
  
5.  Suivez les instructions de l’Assistant pour créer une phrase secrète et procéder à l’inscription.  
  
###  <a name="BKMK_6"></a>Désinscrire le serveur  
  
> [!CAUTION]
>  Si vous annulez l’inscription de votre serveur WindowsServerEssentials à partir du Service de sauvegarde MicrosoftAzure, Azure Backup peut ne plus sauvegarder le serveur. En outre, les données du serveur précédemment téléchargées seront effacées. Pour reprendre les sauvegardes en ligne, vous devez inscrire le serveur à nouveau.  
  
##### <a name="to-unregister-your-server-with-azure-backup"></a>Pour annuler l’inscription de votre serveur à la sauvegarde Azure  
  
1.  Connectez-vous à la [portail de gestion Azure](https://manage.windowsazure.com).  
  
2.  Cliquez sur **Services de récupération**.  
  
3.  Dans **Services de récupération**, cliquez sur le nom du coffre de sauvegarde.  
  
4.  À partir de la **Quick Start** pour le coffre de sauvegarde, cliquez sur **serveurs**.  
  
5.  Sélectionnez le serveur, cliquez sur **supprimer**, puis cliquez sur **Oui** à l’invite de confirmation.  
  
## <a name="related-tasks"></a>Tâches connexes  
  
-   [Modifier la stratégie de sauvegarde en ligne](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_7)  
  
-   [Afficher l’utilisation de stockage de sauvegarde en ligne](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [Inclure un nouveau dossier de sauvegarde en ligne](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [Supprimer ou exclure des sauvegardes de l’historique des fichiers de la stratégie de sauvegarde en ligne](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_10)  
  
-   [Désactiver ou réactiver la sauvegarde du serveur en ligne](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_11)  
  
-   [Arrêter une sauvegarde de serveur en ligne en cours](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_12)  
  
-   [Affichage état de la sauvegarde en ligne](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_13)  
  
-   [Afficher et gérer les alertes de sauvegarde en ligne](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_14)  
  
-   [Rétablir les paramètres par défaut la sauvegarde en ligne](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_15)  
  
-   [Inscription au Service de sauvegarde Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_16)  
  
-   [Intégrer la sauvegarde Azure avec WindowsServerEssentials](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_17)  
  
-   [Protéger des dossiers pour la sauvegarde en ligne dans WindowsServerEssentials](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_18)  
  
-   [Historique de sauvegarde en ligne dans WindowsServerEssentials](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_19)  
  
###  <a name="BKMK_7"></a>Modifier la stratégie de sauvegarde en ligne  
 Il est facile d’apporter des modifications à la stratégie de sauvegarde en ligne à l’aide du tableau de bord WindowsServerEssentials.  
  
##### <a name="to-change-the-online-backup-policy"></a>Pour modifier la stratégie de sauvegarde en ligne  
  
1.  Connectez-vous au tableau de bord en tant qu’administrateur.  
  
2.  Dans la barre de navigation, cliquez sur **sauvegarde en ligne**.  
  
3.  Dans le **des tâches de sauvegarde en ligne** volet, cliquez sur **configurer la sauvegarde en ligne**.  
  
4.  Suivez les instructions de l’Assistant pour personnaliser la stratégie de sauvegarde en ligne.  
  
 Pour plus d’informations sur les paramètres que vous pouvez personnaliser, consultez [configurer la sauvegarde en ligne](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2).  
  
###  <a name="BKMK_8"></a>Afficher l’utilisation de stockage de sauvegarde en ligne  
  
##### <a name="to-view-the-amount-of-storage-space-that-online-backup-uses"></a>Pour afficher la quantité d’espace de stockage qui utilise la sauvegarde en ligne  
  
1.  Connectez-vous au tableau de bord en tant qu’administrateur.  
  
2.  Dans la barre de navigation, cliquez sur **sauvegarde en ligne**.  
  
3.  Sur le **sauvegarde en ligne** onglet, le **état du stockage** s’affiche dans le volet d’informations.  
  
###  <a name="BKMK_9"></a>Inclure un nouveau dossier de sauvegarde en ligne  
  
##### <a name="to-include-a-new-folder-in-the-online-backup-policy"></a>Pour inclure un nouveau dossier dans la stratégie de sauvegarde en ligne  
  
1.  Connectez-vous au tableau de bord en tant qu’administrateur.  
  
2.  Dans la barre de navigation, cliquez sur **sauvegarde en ligne**.  
  
3.  Dans le **des tâches de sauvegarde en ligne** volet, cliquez sur **configurer la sauvegarde en ligne**.  
  
4.  Sur le **modifier ou supprimer la stratégie de sauvegarde**, cliquez sur **personnaliser la stratégie de sauvegarde en ligne**.  
  
5.  Sur le **configurer la sauvegarde en ligne** page, si le dossier que vous souhaitez inclure n’apparaît pas dans la liste, cliquez sur **ajouter des dossiers**.  
  
6.  Dans le **ajouter des dossiers** fenêtre, recherchez et sélectionnez le dossier que vous souhaitez inclure dans la sauvegarde en ligne, puis cliquez sur **OK**.  
  
7.  Sur le **configurer la sauvegarde en ligne**, sélectionnez d’autres dossiers à inclure comme vous le souhaitez, puis cliquez sur **suivant**.  
  
8.  Suivez les instructions de l’Assistant pour terminer la personnalisation de la stratégie de sauvegarde en ligne.  
  
 Pour plus d’informations sur les autres paramètres que vous pouvez personnaliser, consultez [configurer la sauvegarde en ligne](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2).  
  
###  <a name="BKMK_10"></a>Supprimer ou exclure des sauvegardes de l’historique des fichiers de la stratégie de sauvegarde en ligne  
  
##### <a name="to-remove-or-exclude-a-folder-from-the-online-backup-policy"></a>Pour supprimer ou exclure un dossier de la stratégie de sauvegarde en ligne  
  
1.  Connectez-vous au tableau de bord en tant qu’administrateur.  
  
2.  Dans la barre de navigation, cliquez sur **sauvegarde en ligne**.  
  
3.  Cliquez sur le **dossiers protégés** onglet. Le volet détails, affiche une liste des dossiers avec leur état de la sauvegarde en ligne.  
  
4.  Sélectionnez le dossier que vous souhaitez exclure de la stratégie de sauvegarde en ligne, puis dans le volet des tâches, cliquez sur **supprimer le dossier de sauvegarde en ligne**.  
  
###  <a name="BKMK_11"></a>Désactiver ou réactiver la sauvegarde du serveur en ligne  
 Pour obtenir des instructions sur la façon d’utiliser Azure Backup pour sauvegarder ou restaurer des données du serveur, consultez [inscrire ce serveur pour la sauvegarde](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5).  
  
 Pour obtenir des instructions sur la façon d’arrêter d’utiliser Azure Backup pour sauvegarder ou restaurer des données du serveur, consultez [désinscrire le serveur](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_6).  
  
###  <a name="BKMK_12"></a>Arrêter une sauvegarde de serveur en ligne en cours  
  
##### <a name="to-stop-an-online-server-backup-in-progress"></a>Pour arrêter une sauvegarde de serveur en ligne en cours  
  
1.  Connectez-vous au tableau de bord en tant qu’administrateur.  
  
2.  Dans la barre de navigation, cliquez sur **sauvegarde en ligne**.  
  
3.  Dans le **des tâches de sauvegarde en ligne** volet, cliquez sur **arrêter la sauvegarde**.  
  
     Après avoir arrêté la sauvegarde, l’état **Canceled** s’affiche pour la sauvegarde dans le **historique de sauvegarde** liste.  
  
###  <a name="BKMK_13"></a>Affichage état de la sauvegarde en ligne  
  
##### <a name="to-view-the-backup-status"></a>Pour afficher l’état de sauvegarde  
  
1.  Connectez-vous au tableau de bord en tant qu’administrateur.  
  
2.  Dans la barre de navigation, cliquez sur **sauvegarde en ligne**.  
  
3.  Cliquez sur le **historique de sauvegarde** onglet. La liste affiche l’état de chaque tâche de sauvegarde. Sélectionnez une tâche de sauvegarde pour afficher des détails supplémentaires sur cette tâche.  
  
###  <a name="BKMK_14"></a>Afficher et gérer les alertes de sauvegarde en ligne  
 Comme de nombreuses autres alertes, les alertes de la sauvegarde Azure s’affichent dans l’Afficheur des alertes.  
  
##### <a name="to-view-online-backup-alerts-in-the-alert-viewer"></a>Pour afficher les alertes de sauvegarde en ligne dans l’Afficheur des alertes  
  
1.  Ouvrez le tableau de bord.  
  
2.  Effectuez l’une des opérations suivantes:  
  
      WindowsServerEssentials: Dans le volet de navigation, cliquez sur l’icône d’alertes \ (peut être Informational\, avertissement ou critique). L’Afficheur des alertes s’ouvre.  
  
      WindowsServerEssentials: Sur le **accueil**, cliquez sur le **contrôle d’intégrité** onglet.  
  
3.  Passez en revue la liste des alertes pour les problèmes liés à la sauvegarde Azure.  
  
 Pour plus d’informations sur l’utilisation de l’onglet Afficheur des alertes ou analyse du fonctionnement pour gérer les alertes, consultez [Manage System Health](Manage-System-Health-in-Windows-Server-Essentials.md).  
  
###  <a name="BKMK_15"></a>Rétablir les paramètres par défaut la sauvegarde en ligne  
 WindowsServerEssentials fournit un Assistant qui vous permet de configurer les paramètres de sauvegarde en ligne. Si vous souhaitez restaurer les paramètres par défaut, exécutez le **configurer la sauvegarde en ligne** de tâches, puis choisissez le **supprimer la stratégie de sauvegarde en ligne** option. Ensuite, exécutez le **configurer la sauvegarde en ligne** à nouveau la tâche. Vos données précédemment téléchargées restent inchangées.  
  
###  <a name="BKMK_16"></a>Inscription au Service de sauvegarde Azure  
 Pour préparer l’intégration de MicrosoftAzure Backup avec WindowsServerEssentials, vous allez ouvrez une session sur le portail de gestion Azure avec votre compte MicrosoftOnline Services et puis créer un coffre de sauvegarde pour stocker vos sauvegardes en ligne dans Azure. Vous allez ensuite télécharger le module d’intégration de sauvegarde Azure et utiliser le fichier téléchargé pour installer la sauvegarde Azure Add-In sur votre serveur WindowsServerEssentials. Si vous ne disposez pas d’un compte Microsoft, vous pouvez vous inscrire pour une version d’évaluation gratuite.  
  
 Pour effectuer cette configuration, effectuez les tâches suivantes:  
  
1.  S’inscrire à un compte MicrosoftOnline Services et l’aperçu de la sauvegarde.  
  
2.  Créez un coffre de sauvegarde pour stocker les sauvegardes en ligne.  
  
3.  Télécharger l’Agent de sauvegarde Azure.  
  
4.  Installer la sauvegarde Azure Add-In sur le serveur.  
  
####  <a name="BKMK_SignupforaMicrosoftOnlineServiceAccount"></a>S’inscrire à un compte MicrosoftOnline Services et l’aperçu de la sauvegarde  
  
1.  Ouvrez une session sur le tableau de bord WindowsServerEssentials.  
  
2.  Sur le tableau de bord **accueil**, cliquez sur le **compléments** catégorie, cliquez sur **intégrer à Azure Backup**, puis cliquez sur **cliquez pour s’inscrire pour la sauvegarde Azure**.  
  
3.  Sur la Azure **Services de récupération** page, dans le **sauvegarde (aperçu)** section, passez en revue les détails.  
  
4.  Si vous ne disposez pas d’un abonnement Azure, cliquez sur **version d’évaluation gratuite**, puis suivez les instructions pour obtenir un abonnement Azure.  
  
     Si vous disposez déjà d’un abonnement Azure, cliquez sur **Portal** dans le coin supérieur droit de la page web pour accéder au portail de gestion Azure.  
  
5.  Sur la page du portail de gestion Azure, vous verrez **Services de récupération** dans le volet gauche. S où vous tout gérer la sauvegarde coffres-forts ce magasin, vos sauvegardes en ligne à partir de WindowsServerEssentials.  
  
####  <a name="BKMK_Createabackupvaulttostoreonlinebackups"></a>Créez un coffre de sauvegarde pour stocker les sauvegardes en ligne  
  
1.  Connectez-vous à la [portail de gestion Azure](https://manage.windowsazure.com)à partir du navigateur web sur votre serveur WindowsServerEssentials.  
  
2.  Dans le portail de gestion Azure, cliquez sur **New**, cliquez sur **Data Services**, cliquez sur **Services de récupération**, cliquez sur **coffre de sauvegarde**, puis cliquez sur **création rapide**.  
  
3.  Entrez un nom pour votre coffre d’archivage, choisissez la région où vous voulez stocker vos sauvegardes, puis cliquez sur **création rapide**.  
  
     Le **Services de récupération** zone s’ouvre avec votre nouveau coffre d’archivage affiché.  
  
####  <a name="BKMK_DownloadtheWindowsAzureBackupAgent"></a>Télécharger l’Agent de sauvegarde Azure  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Sur le tableau de bord **accueil**, cliquez sur le **main**, cliquez sur le **compléments** catégorie, cliquez sur **intégrer à Azure Backup**, puis cliquez sur **cliquez pour télécharger le module d’intégration Azure Backup**.  
  
####  <a name="BKMK_InstalltheWindowsAzureBackupAddIn"></a>Installer la sauvegarde Azure Add-In sur le serveur  
  
1.  Connectez-vous à votre serveur à l’aide d’un compte d’administrateur, puis exécutez le **OnlineBackupAddin.wssx** fichier que vous avez téléchargé à l’étape précédente.  
  
2.  Le **contrat de licence logiciel** sont affichés. Si vous acceptez les termes et conditions, cliquez sur **accepter** pour poursuivre l’installation.  
  
3.  Sur le **installer le complément**, cliquez sur **installer le complément**.  
  
    > [!NOTE]
    >  Vous pouvez être amené à redémarrer le serveur pour installer tout logiciel requis.  
  
     Le **Installation** page s’affiche. Un indicateur de progression affiche lors de l’installation démarre et affiche la progression de l’installation. Une fois l’installation terminée, vous recevrez un message que la sauvegarde Azure Add-in a été installé correctement.  
  
4.  Cliquez sur **Terminer**.  
  
5.  Fermez et rouvrez le tableau de bord.  
  
     Un nouvel onglet, **sauvegarde en ligne**, est ajouté au tableau de bord. À partir de cet onglet, vous pouvez inscrire votre serveur, configurer les paramètres de sauvegarde et ouvrir **Services de récupération** dans le portail de gestion pour gérer les coffres de sauvegarde pour vos serveurs.  
  
 Après avoir effectué ces étapes, procédez comme suit:  
  
1.  Dans WindowsServerEssentials, téléchargez un certificat à utiliser pour les sauvegardes en ligne. Pour plus d’informations, voir [télécharger un certificat dans le coffre de sauvegarde Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1).  
  
2.  Inscrivez le serveur avec le coffre de sauvegarde Azure. Pour plus d’informations, voir [inscrire ce serveur pour la sauvegarde](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5).  
  
3.  Configurer la sauvegarde en ligne du serveur. Pour plus d’informations, voir [configurer la sauvegarde en ligne](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2).  
  
###  <a name="BKMK_17"></a>Intégrer la sauvegarde Azure avec WindowsServerEssentials  
 Le module d’intégration de sauvegarde MicrosoftAzure est une nouvelle fonctionnalité de WindowsServerEssentials qui vous permet de chiffrer et sauvegarder les fichiers et dossiers à partir de votre serveur à un système de stockage Azure hébergé fourni par Microsoft. À l’aide de la sauvegarde Azure pour chiffrer et sauvegarder les données sur le serveur, vous pouvez éviter la perte de données critiques pour cause d’incendie, d’inondation, de vol ou de tout autre catastrophe. Lorsque vous utilisez Azure Backup pour sauvegarder les données de serveur, les informations sont chiffrées à l’aide de votre phrase secrète avant d’être téléchargée sur un centre de données sécurisé sur Internet. Pour accéder aux données à partir d’une ligne sauvegarde, vous devez disposer d’un serveur qui est authentifié par un certificat et devez fournir la phrase secrète.  
  
 Après avoir intégré et inscrit votre serveur avec Azure Backup, vous pouvez configurer les paramètres de sauvegarde en ligne pour effectuer des sauvegardes régulières. Vous pouvez également lancer une sauvegarde en ligne à tout moment en cliquant sur le **démarrer la sauvegarde maintenant** tâches dans le tableau de bord de sauvegarde en ligne.  
  
 La configuration de la sauvegarde en ligne implique les étapes suivantes:  
  
1.  [Inscription au Service de sauvegarde Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_16) - après vous être inscrit le service de sauvegarde, vous créez un coffre de sauvegarde, télécharge et installe le module d’intégration Azure Backup.  
  
2.  [Télécharger un certificat dans le coffre de sauvegarde Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1)  
  
3.  [Inscrire ce serveur pour la sauvegarde](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)  
  
4.  [Configurer la sauvegarde en ligne](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)  
  
> [!NOTE]
>   La sauvegarde Azure utilise la phrase secrète pour chiffrer les fichiers et dossiers pour la sauvegarde en ligne. Modification de la phrase secrète de chiffrement remplace la phrase secrète que vous avez spécifié lors de l’inscription du serveur. La phrase secrète accepte uniquement les caractères ASCII.  
  
###  <a name="BKMK_18"></a>Protéger des dossiers pour la sauvegarde en ligne dans WindowsServerEssentials  
 Le **dossiers protégés** sous-section dans la section sauvegarde en ligne du tableau de bord affiche une liste de tous les dossiers partagés sur le serveur. Le tableau suivant décrit les informations qui sont incluses dans la liste.  
  
|Colonne|Description|  
|------------|-----------------|  
|**Nom du dossier:**|Le nom du dossier qui est inclus dans la sauvegarde en ligne.<br /><br /> Pour ajouter ou exclure un dossier, exécutez la **configurer la sauvegarde en ligne** tâche.|  
|**Chemin d’accès du dossier:**|L’emplacement du dossier.|  
|**État:**|Il existe trois types d’état **protégé**, **non protégés**, et **inconnu**.|  
  
###  <a name="BKMK_19"></a>Historique de sauvegarde en ligne dans WindowsServerEssentials  
 Le **historique de sauvegarde** sous-section dans la section sauvegarde en ligne du tableau de bord affiche une liste des sauvegardes en ligne récentes. Vous pouvez utiliser les sauvegardes réussies pour restaurer des fichiers et dossiers. Le tableau suivant décrit les informations qui sont incluses dans la liste.  
  
|Colonne|Description|  
|------------|-----------------|  
|**Opération:**|Il existe deux types d’opérations - **sauvegarde** et **restaurer**.|  
|**Heure:**|Il s’agit de l’heure enregistrée de l’état le plus récent.|  
|**État:**|Il existe cinq types d’état **réussite**, **en cours**, **Canceled**, **avertissement**, et **échec**.|  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Gérer la sauvegarde et restauration](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Gérer les Services en ligne Microsoft](Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)  
  
-   [Gérer WindowsServerEssentials](Manage-Windows-Server-Essentials.md)  
  
-   [Utiliser Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
