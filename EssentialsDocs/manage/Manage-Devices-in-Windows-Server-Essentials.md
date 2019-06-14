---
title: Gérer des appareils dans Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f5fe1088-ebe7-4799-a47d-075b0048dea1
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: a66f98b0896e706f520aa057b91cce2fe662d22d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433324"
---
# <a name="manage-devices-in-windows-server-essentials"></a>Gérer des appareils dans Windows Server Essentials

>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 Les sections suivantes expliquent les fonctionnalités de gestion des périphériques d'un serveur et expliquent comment configurer et utiliser des périphériques sur votre réseau :  
  
-   [Gérer des appareils à l’aide du tableau de bord](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Affecter des comptes d’utilisateur autorisé à vous connecter à des ordinateurs réseau spécifiques](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Supprimer un ordinateur à partir du serveur](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Configurer les paramètres de stratégie de groupe pour la redirection de dossiers et la sécurité](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Se connecter à un ordinateur réseau à l’aide d’une session Bureau à distance](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_7)  
  
-   [Afficher les propriétés ordinateur](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_8)  
  
##  <a name="BKMK_1"></a> Gérer des appareils à l’aide du tableau de bord  
 Windows Server Essentials vous permet d'effectuer des tâches d'administration courantes à l'aide du tableau de bord Windows Server Essentials. La page **Périphériques** du tableau de bord fournit :  
  
-   la liste des ordinateurs réseau, qui indique :  
  
    -   le nom de l'ordinateur ;  
  
    -   l'état de l'ordinateur, **En ligne** ou **Hors connexion** ;  
  
    -   la description de l'ordinateur ;  
  
    -   l'état de sauvegarde de l'ordinateur ;  
  
    -   l'état de mise à jour de l'ordinateur ;  
  
    -   l'état de sécurité de l'ordinateur ;  
  
    -   l'état des alertes de l'ordinateur ;  
  
    -   les informations de stratégie de groupe de l'ordinateur ;  
  
-   un volet d'informations avec des informations supplémentaires sur un ordinateur sélectionné ;  
  
-   un volet des tâches qui inclut un ensemble de tâches d'administration de périphériques, telles que l'affichage des alertes et des propriétés de l'ordinateur, la configuration de la sauvegarde de l'ordinateur et la restauration des fichiers et dossiers à partir d'une sauvegarde.  
  
#### <a name="to-view-the-status-of-network-computers"></a>Pour afficher l'état des ordinateurs réseau  
  
1. Ouvrez le tableau de bord Windows Server Essentials.  
  
2. Dans la barre de navigation, cliquez sur **Périphériques**.  
  
3. Examinez l'état de tous les ordinateurs du réseau dans le volet Liste.  
  
   Le tableau ci-dessous décrit les différentes tâches d'ordinateur et de sauvegarde disponibles dans le tableau de bord Windows Server Essentials. Certaines de ces tâches sont spécifiques aux ordinateurs et s'affichent uniquement quand vous sélectionnez un ordinateur dans la liste.  
  
### <a name="computer-tasks-in-the-dashboard"></a>Tâches d'ordinateur dans le tableau de bord  
  
|Nom de la tâche|Description|  
|---------------|-----------------|  
|Afficher les propriétés de l'ordinateur|Affiche des informations générales sur l'ordinateur sélectionné et vous permet d'afficher des informations sur les sauvegardes de l'ordinateur.|  
|Configurer la sauvegarde pour cet ordinateur|Exécute l'Assistant Configurer la sauvegarde.|  
|Personnaliser la sauvegarde pour l'ordinateur|Ouvre les propriétés de sauvegarde, à partir desquelles vous pouvez modifier les paramètres de sauvegarde de l'ordinateur sélectionné.|  
|Démarrer une sauvegarde pour l'ordinateur|Démarre une sauvegarde pour l'ordinateur sélectionné.|  
|Arrêter la sauvegarde pour l'ordinateur|Arrête la sauvegarde de l'ordinateur sélectionné.|  
|Restaurer des fichiers ou des dossiers de l'ordinateur|Exécute l'Assistant Restaurer les fichiers et dossiers, qui vous permet de restaurer des lecteurs, des dossiers ou des fichiers spécifiques.|  
|Afficher les alertes pour l'ordinateur|Affiche les alertes critiques et d'information, et vous permet de prendre des mesures correctives lorsque cela est possible.|  
|Connexion à l'ordinateur via Bureau à distance|Établit une connexion de type Bureau à distance avec l'ordinateur sélectionné.|  
|Supprimer l'ordinateur|Exécute l'Assistant Supprimer un ordinateur, qui détache l'ordinateur du tableau de bord Windows Server Essentials.|  
|Personnaliser les paramètres de la sauvegarde de l'ordinateur et de l'historique des fichiers|Ouvre la page des paramètres de sauvegarde, dans laquelle vous pouvez apporter des modifications aux paramètres de planification de sauvegarde et d'historique des fichiers pour les ordinateurs clients.|  
|Comment connecter des ordinateurs sur le serveur ?|Ouvre une rubrique d'aide qui décrit la procédure à suivre pour joindre un ordinateur au réseau.|  
|Implémenter la stratégie de groupe|Applique des paramètres de stratégie aux ordinateurs Windows 8 et Windows 7 qui sont joints au domaine.|  
  
##  <a name="BKMK_2"></a> Affecter des comptes d’utilisateur autorisé à vous connecter à des ordinateurs réseau spécifiques  
 Vous pouvez affecter des autorisations aux comptes d'utilisateur pour que les utilisateurs puissent se connecter uniquement à des ordinateurs réseau spécifiques quand ils accèdent au réseau Windows Server Essentials à partir d'un emplacement distant.  
  
#### <a name="to-change-the-computer-access-for-a-user-account"></a>Pour modifier l'accès d'un compte d'utilisateur aux ordinateurs  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans la barre de navigation, cliquez sur **Utilisateurs**.  
  
3.  Dans la liste des comptes d'utilisateur, sélectionnez le compte d'utilisateur à modifier.  
  
4.  Dans le **< compte d’utilisateur\> tâches** volet, cliquez sur **afficher les propriétés du compte**. La page **Propriétés** du compte d'utilisateur s'affiche.  
  
5.  Sous l'onglet **Accès à l'ordinateur** , sélectionnez l'ordinateur auquel cet utilisateur peut accéder à distance, puis cliquez sur **OK**.  
  
##  <a name="BKMK_3"></a> Supprimer un ordinateur à partir du serveur  
 Lorsque vous supprimez un ordinateur d'un serveur qui exécute Windows Server Essentials en ayant recours au tableau de bord, il n'est plus géré par le serveur. Par conséquent, le serveur arrête de créer des sauvegardes d'ordinateur ou d'analyser son intégrité après sa suppression du réseau.  
  
> [!NOTE]
>  La suppression d'un ordinateur du serveur ne déconnecte pas l'ordinateur du réseau. L'ordinateur peut encore accéder aux ressources du réseau de la même manière qu'il le pouvait avant d'être connecté au serveur. Pour empêcher l'ordinateur d'accéder aux ressources serveur et pour le déconnecter du serveur, vous devez supprimer l'ordinateur du domaine. En outre, la suppression de l'ordinateur du serveur ne désinstalle pas automatiquement le logiciel Connecteur ni le Launchpad de l'ordinateur en cours de suppression. Vous devez supprimer manuellement le logiciel Connecteur de l'ordinateur. Pour plus d’informations, consultez la section désinstaller le logiciel connecteur dans [connexion](../use/Get-Connected-in-Windows-Server-Essentials.md).  
  
#### <a name="to-remove-a-computer-from-the-network-by-using-the-dashboard"></a>Pour supprimer un ordinateur du réseau à l'aide du tableau de bord  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans la barre de navigation, cliquez sur l'onglet **Périphériques** .  
  
3.  Dans la liste des ordinateurs, cliquez avec le bouton droit sur l'ordinateur à supprimer du réseau, puis cliquez sur **Supprimer l'ordinateur**.  
  
##  <a name="BKMK_5"></a> Configurer les paramètres de stratégie de groupe pour la redirection de dossiers et la sécurité  
 Vous pouvez configurer la stratégie de groupe et la déployer sur les ordinateurs du réseau Windows Server Essentials à l'aide du tableau de bord Windows Server Essentials. La stratégie de groupe dans Windows Server Essentials inclut des paramètres pour la redirection des dossiers et la sécurité, qui impactent Windows Update, Windows Defender et le pare-feu du réseau.  
  
#### <a name="to-configure-group-policy-in-windows-server-essentials"></a>Pour configurer la stratégie de groupe dans Windows Server Essentials  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans la barre de navigation, cliquez sur **PÉRIPHÉRIQUES**.  
  
3.  Pour Windows Server Essentials : Dans le volet **Tâches Utilisateurs**, cliquez sur **Implémenter la stratégie de groupe**.  
  
     Pour Windows Server Essentials : Dans le volet global **Tâches Périphériques**, cliquez sur **Implémenter la stratégie de groupe**.  
  
4.  L'Assistant Implémenter la stratégie de groupe s'ouvre.  
  
5.  Dans la page **Activer la stratégie de groupe de redirection de dossiers** de l'Assistant, vous pouvez choisir les dossiers utilisateur à rediriger.  
  
6.  Dans la page **Activer les paramètres de stratégie de sécurité** de l'Assistant, vous pouvez activer les paramètres de stratégie de groupe pour **Windows Update**, **Windows Defender** et le **Pare-feu du réseau**.  
  
7.  Cliquez sur **Terminer** pour implémenter les paramètres de stratégie de groupe.  
  
##  <a name="BKMK_7"></a> Se connecter à un ordinateur réseau à l’aide d’une session Bureau à distance  
 Pour accéder à distance à votre ordinateur de réseau Windows Server Essentials quand vous êtes en déplacement, utilisez votre navigateur Web pour vous connecter à votre site Web accès Web à distance de s organisation et, sous la **ordinateurs** , cliquez sur le nom de la ordinateur.  
  
 La colonne **État** vous indique si vous pouvez vous connecter à un ordinateur sur votre réseau. Elle peut contenir les valeurs suivantes :  
  
-   **Disponible**  
  
     L'ordinateur est allumé et est disponible pour une connexion à distance. Même si vous voyez cet état, il se peut que vous ne soyez pas en mesure de vous connecter à cet ordinateur, si un pare-feu tiers bloque la connexion.  
  
-   **En mode hors connexion ou en veille**  
  
     L'ordinateur est éteint ou est en mode Veille ou Veille prolongée. Si un ordinateur est hors connexion ou en veille, l'état est mis à jour en temps réel afin que vous puissiez savoir quand l'ordinateur est disponible.  
  
-   **Système d’exploitation non pris en charge**  
  
     Le système d'exploitation installé sur l'ordinateur ne prend pas en charge les services Bureau à distance. La mise à jour de cet état sur le serveur peut prendre jusqu'à 6 heures en cas de modifications.  
  
-   **La connexion est désactivée**  
  
     La connexion de l'ordinateur est bloquée par un pare-feu ou le Bureau à distance est désactivé sur l'ordinateur ou par la stratégie de groupe. La mise à jour de cet état sur le serveur peut prendre jusqu'à 6 heures en cas de modifications.  
  
##  <a name="BKMK_8"></a> Afficher les propriétés ordinateur  
 La section **Périphériques** du tableau de bord Windows Server Essentials affiche la liste des ordinateurs réseau. Cette liste fournit des informations supplémentaires sur chaque ordinateur.  
  
#### <a name="to-view-a-list-of-computers"></a>Pour afficher la liste des ordinateurs  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans la barre de navigation, cliquez sur **Périphériques**.  
  
3.  Le tableau de bord affiche la liste actuelle des ordinateurs.  
  
#### <a name="to-view-or-change-properties-for-a-computer"></a>Pour afficher ou modifier les propriétés d'un ordinateur  
  
1.  Dans la liste des ordinateurs, sélectionnez le compte pour lequel vous voulez afficher ou modifier les propriétés.  
  
2.  Dans le **< nom_ordinateur\> tâches** volet, cliquez sur **afficher les propriétés de l’ordinateur**. La page **Propriétés** des ordinateurs s'affiche.  
  
3.  Cliquez sur un onglet pour afficher les propriétés pour cet ordinateur.  
  
4.  Pour enregistrer les modifications que vous apportez aux propriétés d'ordinateur, cliquez sur **Appliquer**.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Gérer l’accès Web à distance](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Utiliser l’accès Web à distance](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Gérer les comptes d’utilisateur à l’aide du tableau de bord](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage8)  
  
-   [Gérer Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Utiliser Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
