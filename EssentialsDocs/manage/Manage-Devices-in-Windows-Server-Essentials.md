---
title: "Gérer les périphériques dans WindowsServerEssentials"
description: "Décrit comment utiliser WindowsServerEssentials"
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
ms.openlocfilehash: 288d62a3fe4d9073ba2c0e3fdff385d8317f20d4
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="manage-devices-in-windows-server-essentials"></a>Gérer les périphériques dans WindowsServerEssentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials
 
 Les sections suivantes abordent les fonctionnalités de gestion de périphérique d’un serveur et expliquent comment configurer et utiliser des périphériques sur votre réseau:  
  
-   [Gérer des périphériques à l’aide du tableau de bord](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Affecter des comptes d’utilisateur autorisé à ouvrir une session sur des ordinateurs réseau spécifiques](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Supprimer un ordinateur à partir du serveur](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Configurer les paramètres de stratégie de groupe pour la redirection de dossiers et la sécurité](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Se connecter à un ordinateur réseau à l’aide d’une session Bureau à distance](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_7)  
  
-   [Affichage des propriétés de l’ordinateur](Manage-Devices-in-Windows-Server-Essentials.md#BKMK_8)  
  
##  <a name="BKMK_1"></a>Gérer des périphériques à l’aide du tableau de bord  
 Windows Server Essentials permet d’effectuer des tâches d’administration courantes à l’aide du tableau de bord Windows Server Essentials. Le **périphériques** page du tableau de bord fournit les éléments suivants:  
  
-   Liste des ordinateurs du réseau, qui affiche:  
  
    -   Le nom de l’ordinateur  
  
    -   L’état de l’ordinateur, soit **en ligne** ou **hors connexion**  
  
    -   La description de l’ordinateur  
  
    -   L’état de sauvegarde de l’ordinateur  
  
    -   L’état de mise à jour de l’ordinateur  
  
    -   L’état de sécurité de l’ordinateur  
  
    -   L’état des alertes de l’ordinateur  
  
    -   Informations de stratégie de groupe pour l’ordinateur  
  
-   Un volet d’informations avec des informations supplémentaires sur un ordinateur sélectionné  
  
-   Un volet des tâches qui inclut un ensemble de tâches d’administration des périphériques tels que l’affichage des alertes et les propriétés de l’ordinateur, la définition de la sauvegarde de l’ordinateur et la restauration de fichiers et dossiers à partir d’une sauvegarde  
  
#### <a name="to-view-the-status-of-network-computers"></a>Pour afficher l’état des ordinateurs du réseau  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans la barre de navigation, cliquez sur **périphériques**.  
  
3.  Afficher l’état de tous les ordinateurs du réseau dans le volet liste.  
  
 Le tableau suivant décrit les différents ordinateur et les tâches de sauvegarde sont disponibles dans le tableau de bord WindowsServerEssentials. Certaines tâches sont spécifiques à l’ordinateur, et elles ne sont visibles que lorsque vous sélectionnez un ordinateur dans la liste.  
  
### <a name="computer-tasks-in-the-dashboard"></a>Tâches d’ordinateur dans le tableau de bord  
  
|Nom de la tâche|Description|  
|---------------|-----------------|  
|Afficher les propriétés de l’ordinateur|Affiche des informations générales sur l’ordinateur sélectionné et vous permet d’afficher des informations sur les sauvegardes de l’ordinateur.|  
|Configurer la sauvegarde pour cet ordinateur|Exécute l’Assistant sauvegarde configurer.|  
|Personnaliser la sauvegarde de l’ordinateur|Ouvre les propriétés de la sauvegarde, à partir de laquelle vous pouvez modifier les paramètres de sauvegarde de l’ordinateur sélectionné.|  
|Démarrer la sauvegarde de l’ordinateur|Démarre une sauvegarde d’un ordinateur sélectionné.|  
|Arrêter la sauvegarde de l’ordinateur|Arrête la sauvegarde d’un ordinateur sélectionné.|  
|Restaurer des fichiers ou dossiers de l’ordinateur|Exécute l’Assistant de dossiers, qui vous permet de restaurer des fichiers spécifiques, des dossiers ou des lecteurs et de restauration de fichiers.|  
|Afficher les alertes pour l’ordinateur|Affiche les alertes d’information critiques et d’autres et vous permet de prendre des mesures correctives lorsque cela est possible.|  
|Bureau à distance à l’ordinateur|Ouvre la connexion Bureau à distance à l’ordinateur sélectionné.|  
|Supprimer l’ordinateur|Exécute la supprimer un Assistant de l’ordinateur, qui détache l’ordinateur à partir du tableau de bord WindowsServerEssentials.|  
|Personnaliser les paramètres de l’historique des fichiers et de sauvegarde de l’ordinateur|Ouvre la page Paramètres de sauvegarde, à partir de laquelle vous pouvez apporter des modifications à la planification de sauvegarde et les paramètres de l’historique des fichiers pour les clients ordinateurs.|  
|Comment connecter des ordinateurs au serveur?|Ouvre une rubrique d’aide qui décrit les étapes à suivre pour joindre un ordinateur au réseau.|  
|Stratégie de groupe de mise en œuvre|Applique des paramètres de stratégie aux ordinateurs Windows8 et Windows7 qui sont joints au domaine.|  
  
##  <a name="BKMK_2"></a>Affecter des comptes d’utilisateur autorisé à ouvrir une session sur des ordinateurs réseau spécifiques  
 Vous pouvez affecter des autorisations aux comptes d’utilisateurs pour que les utilisateurs peuvent se connecter uniquement à des ordinateurs réseau spécifiques lors de l’accès au réseau WindowsServerEssentials à partir d’un emplacement distant.  
  
#### <a name="to-change-the-computer-access-for-a-user-account"></a>Pour modifier l’accès à l’ordinateur pour un compte d’utilisateur  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans la barre de navigation, cliquez sur **utilisateurs**.  
  
3.  Dans la liste des comptes d’utilisateur, sélectionnez le compte d’utilisateur que vous souhaitez modifier.  
  
4.  Dans le **< utilisateur\ > tâches** volet, cliquez sur **permet d’afficher les propriétés du compte**. Le **propriétés** page pour le compte d’utilisateur s’affiche.  
  
5.  Sur le **accès à l’ordinateur**, sélectionnez l’ordinateur que cet utilisateur peut accéder à distance, puis cliquez sur **OK**.  
  
##  <a name="BKMK_3"></a>Supprimer un ordinateur à partir du serveur  
 Lorsque vous supprimez un ordinateur d’un serveur qui exécute WindowsServerEssentials à l’aide du tableau de bord, il n’est plus géré par le serveur. Par conséquent, le serveur arrête de créer des sauvegardes de l’ordinateur ou analyser son intégrité après sa suppression à partir du réseau.  
  
> [!NOTE]
>  Suppression d’un ordinateur à partir du serveur ne déconnecte pas l’ordinateur à partir du réseau. L’ordinateur peut encore accéder aux ressources du réseau de la même manière qu’il le pouvait avant d’être connecté au serveur. Pour empêcher l’ordinateur d’accéder aux ressources serveur et le déconnecter du serveur, vous devez supprimer l’ordinateur du domaine. En outre, la suppression de l’ordinateur à partir du serveur ne désinstalle pas automatiquement le logiciel connecteur ou le Launchpad à partir de l’ordinateur est supprimé. Vous devez supprimer manuellement le logiciel Connecteur de l’ordinateur. Pour plus d’informations, voir la section désinstaller le logiciel connecteur dans [connexion](../use/Get-Connected-in-Windows-Server-Essentials.md).  
  
#### <a name="to-remove-a-computer-from-the-network-by-using-the-dashboard"></a>Pour supprimer un ordinateur à partir du réseau à l’aide du tableau de bord  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans la barre de navigation, cliquez sur le **périphériques** onglet.  
  
3.  Dans la liste des ordinateurs, cliquez sur l’ordinateur que vous souhaitez supprimer du réseau, puis cliquez sur **supprimer l’ordinateur**.  
  
##  <a name="BKMK_5"></a>Configurer les paramètres de stratégie de groupe pour la redirection de dossiers et la sécurité  
 Vous pouvez configurer la stratégie de groupe et le déployer sur les ordinateurs du réseau de WindowsServerEssentials à l’aide du tableau de bord WindowsServerEssentials. Stratégie de groupe dans WindowsServerEssentials inclut des paramètres pour la redirection de dossiers et de sécurité qui impactent Windows Update, Windows Defender et le pare-feu du réseau.  
  
#### <a name="to-configure-group-policy-in-windows-server-essentials"></a>Pour configurer la stratégie de groupe dans WindowsServerEssentials  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans la barre de navigation, cliquez sur **périphériques**.  
  
3.  Pour WindowsServerEssentials: Dans le global **tâches utilisateurs** volet, cliquez sur **implémenter la stratégie de groupe**.  
  
     Pour WindowsServerEssentials: Dans le global **tâches périphériques** volet, cliquez sur **implémenter la stratégie de groupe**.  
  
4.  Assistant Stratégie de groupe implémenter s’ouvre.  
  
5.  Sur le **activer la stratégie de groupe de Redirection de dossier** page de l’Assistant, vous pouvez choisir les dossiers utilisateur que vous voulez rediriger.  
  
6.  Sur le **activer les paramètres de stratégie de sécurité** page de l’Assistant, vous pouvez choisir activer les paramètres de stratégie de groupe pour **mise à jour Windows**, **Windows Defender**et le **pare-feu réseau**.  
  
7.  Cliquez sur **Terminer** pour implémenter les paramètres de stratégie de groupe.  
  
##  <a name="BKMK_7"></a>Se connecter à un ordinateur réseau à l’aide d’une session Bureau à distance  
 Pour accéder à distance à votre ordinateur de WindowsServerEssentials réseau lorsque vous êtes en déplacement, utilisez votre navigateur Web pour vous connecter à votre site Web accès Web à distance de s organisation et sur le **ordinateurs**, cliquez sur le nom de l’ordinateur.  
  
 Le **état** colonne vous indique si vous pouvez vous connecter à un ordinateur sur votre réseau et pouvez contenir les valeurs suivantes:  
  
-   **Disponible**  
  
     L’ordinateur est allumé et est disponible pour une connexion à distance. Même si vous voyez cet état, vous ne pouvez pas être en mesure de se connecter à cet ordinateur, si un pare-feu tiers bloque la connexion.  
  
-   **En mode hors connexion ou en veille**  
  
     L’ordinateur est éteint ou est en mode veille ou veille prolongée. Si un ordinateur est en veille ou en mode hors connexion, l’état est mis à jour en temps réel afin que vous puissiez savoir lorsque l’ordinateur devient disponible.  
  
-   **Système d’exploitation non pris en charge**  
  
     Le système d’exploitation sur l’ordinateur ne prend pas en charge les services Bureau à distance. Il peut prendre jusqu'à 6heures pour cet état de mise à jour sur le serveur s’il existe une modification.  
  
-   **La connexion est désactivée**  
  
     La connexion de l’ordinateur est bloquée par un pare-feu ou le Bureau à distance est désactivé sur l’ordinateur ou par la stratégie de groupe. Il peut prendre jusqu'à 6heures pour cet état de mise à jour sur le serveur s’il existe une modification.  
  
##  <a name="BKMK_8"></a>Affichage des propriétés de l’ordinateur  
 Le **périphériques** section du tableau de bord WindowsServerEssentials affiche une liste des ordinateurs du réseau. La liste fournit également des informations supplémentaires sur chaque ordinateur.  
  
#### <a name="to-view-a-list-of-computers"></a>Pour afficher la liste des ordinateurs  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans la barre de navigation, cliquez sur **périphériques**.  
  
3.  Le tableau de bord affiche la liste actuelle des ordinateurs.  
  
#### <a name="to-view-or-change-properties-for-a-computer"></a>Pour afficher ou modifier les propriétés d’un ordinateur  
  
1.  Dans la liste des ordinateurs, sélectionnez le compte pour lequel vous souhaitez afficher ou modifier les propriétés.  
  
2.  Dans le **< computername\ > tâches** volet, cliquez sur **afficher les propriétés de l’ordinateur**. Le **propriétés** pour les ordinateurs s’affiche.  
  
3.  Cliquez sur un onglet pour afficher les propriétés pour cet ordinateur.  
  
4.  Pour enregistrer les modifications que vous apportez aux propriétés d’ordinateur, cliquez sur **appliquer**.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Gérer l’accès Web à distance](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Utiliser l’accès Web à distance](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Gérer les comptes d’utilisateur à l’aide du tableau de bord](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage8)  
  
-   [Gérer WindowsServerEssentials](Manage-Windows-Server-Essentials.md)  
  
-   [Utiliser Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
