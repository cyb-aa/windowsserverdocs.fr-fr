---
title: Installer ou désinstaller des rôles, des services de rôle ou des fonctionnalités
description: Gestionnaire de serveur
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 04f16d84-45c2-4771-84c1-1cc973d0ee02
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 32d356b3ae70b7b15f23a40247e73b4b8f61c3db
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822370"
---
# <a name="install-or-uninstall-roles-role-services-or-features"></a>Installer ou désinstaller des rôles, des services de rôle ou des fonctionnalités

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dans Windows Server, la console Gestionnaire de serveur et les applets de commande Windows PowerShell pour le Gestionnaire de serveur autoriser l’installation des rôles et fonctionnalités sur des serveurs locaux ou distants ou disques durs virtuels (VHD) hors connexion. Vous pouvez installer plusieurs rôles et fonctionnalités sur un seul serveur distant, ou ajouter de disque dur virtuel hors connexion dans un seul Assistant rôles et fonctionnalités ou session Windows PowerShell.  
  
> [!IMPORTANT]  
> Le Gestionnaire de serveur ne peut pas être utilisé pour gérer une version plus récente du système d’exploitation Windows Server. Gestionnaire de serveur s’exécutant sur Windows Server 2012 R2 ou Windows 8.1 ne peut pas être utilisé pour installer des rôles, services de rôle et fonctionnalités sur les serveurs qui exécutent Windows Server 2016.  
  
Vous devez être connecté à un serveur en tant qu’administrateur pour installer ou désinstaller des rôles, services de rôle et fonctionnalités. Si vous êtes connecté à l’ordinateur local avec un compte sans droits d’administrateur sur votre serveur cible, cliquez avec le bouton droit sur le serveur cible dans la vignette **Serveurs**, puis cliquez sur **Gérer en tant que** pour préciser un compte doté de droits d’administrateur. Le serveur sur lequel vous voulez monter un disque dur virtuel hors connexion doit être ajouté au Gestionnaire de serveur et vous devez avoir des droits d’administrateur sur le serveur.  
  
Pour plus d’informations sur ce que sont les rôles, services de rôle et fonctionnalités, consultez [rôles, Services de rôle et fonctionnalités](https://go.microsoft.com/fwlink/p/?LinkId=239558).  
  
Cette rubrique contient les sections suivantes.  
  
-   [Installer des rôles, services de rôle et fonctionnalités à l’aide de l’ajouter Assistant rôles et fonctionnalités](#BKMK_installarfw)  
  
-   [Installer des rôles, des services de rôle et des fonctionnalités à l’aide des applets de commande Windows PowerShell](#BKMK_installwps)  
  
-   [Supprimer des rôles, services de rôle et fonctionnalités à l’aide de la supprimer Assistant rôles et fonctionnalités](#BKMK_removerrfw)  
  
-   [Supprimer des rôles, des services de rôle et des fonctionnalités à l’aide des applets de commande Windows PowerShell](#BKMK_removewps)  
  
-   [Installer des rôles et des fonctionnalités sur plusieurs serveurs à l’aide d’un script Windows PowerShell](#BKMK_batch)  
  
-   [Installer le .NET Framework 3.5 et d’autres fonctionnalités à la demande](#BKMK_FoD)  
  
## <a name="BKMK_installarfw"></a>Installer des rôles, services de rôle et fonctionnalités à l’aide de l’ajouter Assistant rôles et fonctionnalités  
Dans une session de l’Assistant de fonctionnalités et d’ajouter des rôles, vous pouvez installer des rôles, services de rôle et fonctionnalités sur le serveur local, un serveur distant qui a été ajouté au Gestionnaire de serveur ou un disque dur virtuel hors connexion. Pour plus d’informations sur l’ajout d’un serveur au Gestionnaire de serveur à gérer, consultez [ajouter des serveurs au Gestionnaire de serveur](add-servers-to-server-manager.md).  
  
> [!NOTE]  
> Si vous exécutez le Gestionnaire de serveur sur Windows Server 2016 ou Windows 10, vous pouvez utiliser l’Assistant de fonctionnalités et d’ajouter des rôles pour installer des rôles et fonctionnalités uniquement sur les serveurs et disques durs virtuels hors connexion qui exécutent Windows Server 2016.  
  
#### <a name="to-install-roles-and-features-by-using-the-add-roles-and-features-wizard"></a>Pour installer des rôles et fonctionnalités à l’aide de l’ajouter Assistant rôles et fonctionnalités  
  
1.  Si le Gestionnaire de serveur est déjà ouvert, passez à l’étape suivante. S’il n’est pas déjà ouvert, ouvrez-le en effectuant l’une des opérations suivantes.  
  
    -   Sur le Bureau Windows, démarrez le Gestionnaire de serveur en cliquant sur **Gestionnaire de serveur** dans la barre des tâches Windows.  
  
    -   Sur le Windows **Démarrer** , cliquez sur le **le Gestionnaire de serveur** vignette.  
  
2.  Sur le **gérer** menu, cliquez sur **ajouter des rôles et fonctionnalités**.  
  
3.  Dans la page **Avant de commencer** , vérifiez que votre serveur de destination et environnement réseau sont préparés pour le rôle et la fonctionnalité que vous voulez installer. Cliquez sur **Suivant**.  
  
4.  Dans la page **Sélectionner le type d’installation** , sélectionnez **Installation basée sur un rôle ou une fonctionnalité** pour installer toutes les parties de rôles et fonctionnalités sur un même serveur ou **Installation des services Bureau à distance** pour installer une infrastructure de bureaux basée sur des ordinateurs virtuels ou sur une session pour les services Bureau à distance. L’option **Installation des services Bureau à distance** distribue les parties logiques du rôle des services Bureau à distance entre différents serveurs, comme en ont besoin les administrateurs. Cliquez sur **Suivant**.  
  
5.  Dans la page **Sélectionner le serveur de destination**, sélectionnez un serveur dans le pool de serveurs ou sélectionnez un disque dur virtuel hors connexion. Pour sélectionner un disque dur virtuel hors connexion en guise de serveur de destination, choisissez d’abord le serveur sur lequel monter le disque dur virtuel, puis sélectionnez le fichier VHD. Pour plus d’informations sur l’ajout de serveurs à votre pool de serveurs, consultez [ajouter des serveurs au Gestionnaire de serveur](add-servers-to-server-manager.md). Une fois que vous avez sélectionné le serveur de destination, cliquez sur **Suivant**.  
  
    > [!NOTE]  
    > Pour installer des rôles et des fonctionnalités sur des disques durs virtuels hors connexion, les disques durs virtuels hors connexion cibles doivent répondre aux exigences suivantes.  
    >   
    > -   Ils doivent être équipés de la version de Windows Server qui correspond à la version du Gestionnaire de serveur vous sont en cours d’exécution. Consultez la note au début de [installer des rôles, services de rôle et fonctionnalités à l’aide de l’ajouter des rôles et fonctionnalités Assistant](#BKMK_installarfw).  
    > -   Ils ne peuvent pas posséder plus d’un volume ou d’une partition système.  
    > -   Le dossier réseau partagé dans lequel le disque dur virtuel hors connexion est stocké doit accorder les droits d’accès suivants au compte d’ordinateur (ou système local) du serveur que vous avez sélectionné pour le montage du disque dur virtuel. L’accès au compte d’utilisateur uniquement ne suffit pas. Le partage peut accorder les autorisations **Lire** et **Écrire** au groupe **Tout le monde** pour autoriser l’accès au disque dur virtuel, mais pour des raisons de sécurité, cela n’est pas recommandé.  
    >   
    >     -   Accès **Lecture/écriture** dans la boîte de dialogue **Partage de fichiers**.  
    >     -   **Contrôle total** accéder sur le **sécurité** tab, fichier ou dossier **propriétés** boîte de dialogue.  
  
6.  Sélectionnez des rôles, sélectionnez des services de rôle pour le rôle, le cas échéant, puis cliquez sur **Suivant** pour sélectionner des fonctionnalités.  
  
    Avant de continuer, l’ajouter des rôles et fonctionnalités Assistant vous avertit automatiquement si les conflits ont été détectés sur le serveur de destination qui peut empêcher les rôles sélectionnés ou des fonctionnalités de l’installation ou de fonctionnement normal. Le système vous demande également d’ajouter tous les rôles, services de rôle ou fonctionnalités indispensables aux rôles ou fonctionnalités que vous avez sélectionnés.  
  
    En outre, si vous prévoyez de gérer le rôle à distance, d'un autre serveur ou d'un ordinateur client Windows qui exécute les outils d'administration de serveur distant, vous pouvez choisir de ne pas installer les outils de gestion et les composants logiciels enfichables pour les rôles sur le serveur de destination. Par défaut, dans l’Assistant de fonctionnalités et ajouter des rôles, les outils de gestion sont sélectionnés pour installation.  
  
7.  Dans la page **Confirmer les sélections d’installation**, passez en revue vos sélections de rôle, fonctionnalité et serveur. Si vous êtes prêt à effectuer l’installation, cliquez sur **Installer**.  
  
    Vous pouvez également exporter vos sélections dans un fichier de configuration basé sur XML que vous pouvez utiliser pour les installations sans assistance avec Windows PowerShell. Pour exporter la configuration que vous avez spécifié dans ce ajouter des rôles et de session de l’Assistant de fonctionnalités, cliquez sur **exporter les paramètres de configuration**, puis enregistrez le fichier XML dans un emplacement approprié.  
  
    La commande **Spécifier un autre chemin d’accès source** dans la page **Confirmer les sélections d’installation** vous permet de spécifier un autre chemin d’accès source pour les fichiers qui sont requis pour installer les rôles et les fonctionnalités sur le serveur sélectionné. Dans Windows Server 2012 et versions ultérieures de Windows Server, [fonctionnalités à la demande](https://go.microsoft.com/fwlink/p/?LinkID=241573) vous permet de réduire la quantité d’espace disque utilisée par le système d’exploitation, en supprimant les fichiers de rôles et fonctionnalités des serveurs exclusivement gérés à distance. Si vous avez supprimé les fichiers des rôles et des fonctionnalités d’un serveur à l’aide de l’applet de commande `Uninstall-WindowsFeature -remove` , vous pouvez installer des rôles et des fonctionnalités sur le serveur dans le futur en spécifiant un autre chemin d’accès source ou un partage sur lequel les fichiers des rôles et des fonctionnalités requis sont stockés. Le partage de fichier ou chemin d’accès source doit accorder **en lecture** autorisations à le **tout le monde** groupe (non recommandé pour des raisons de sécurité), ou pour le compte d’ordinateur (*domaine* \\ *Nom_serveur*$) du serveur de destination ; lui accordant l’accès de compte d’utilisateur n’est pas suffisant. Pour plus d’informations sur les fonctionnalités à la demande, voir [Options d’installation de Windows Server](https://go.microsoft.com/fwlink/p/?LinkId=241573).  
  
    Lorsque vous installez des rôles, services de rôle et fonctionnalités sur un serveur physique en cours d’exécution, vous pouvez spécifier un fichier WIM en tant que fonctionnalité autre fichier source. Le chemin d’accès source d’un fichier WIM doit adopter le format suivant, avec **WIM** comme préfixe et l’index dans lequel les fichiers de fonctionnalités sont situés en guise de suffixe : **WIM:e:\sources\install.wim:4**. Toutefois, vous ne pouvez pas utiliser un fichier WIM directement en tant que source pour l’installation des rôles, services de rôle et fonctionnalités sur un disque dur virtuel hors connexion ; Vous devez soit monter le disque dur virtuel hors connexion et pointez sur son chemin d’accès de montage pour les fichiers sources, ou vous devez pointer vers un dossier qui contient une copie du contenu du fichier WIM.  
  
8.  Après avoir cliqué sur **installer**, le **progression de l’Installation** page affiche la progression de l’installation, les résultats et les messages tels que des avertissements, échecs ou étapes de configuration post-installation obligatoire pour les rôles ou fonctionnalités que vous avez installé. Dans Windows Server 2012 et versions ultérieures de Windows Server, vous pouvez fermer l’ajouter des rôles et l’Assistant fonctionnalités pendant l’installation est toujours en cours et afficher les résultats installation ou d’autres messages dans la **Notifications** zone en haut de la console Gestionnaire de serveur. Cliquez sur le **Notifications** icône d’indicateur pour afficher plus de détails sur les installations ou autres tâches que vous effectuez dans le Gestionnaire de serveur.  
  
## <a name="BKMK_installwps"></a>Installer des rôles, services de rôle et fonctionnalités à l’aide des applets de commande Windows PowerShell  
Les applets de commande de déploiement de gestionnaire de serveur pour Windows PowerShell fonctionnent de la même façon à l’interface graphique utilisateur, Assistant Ajout de rôles et fonctionnalités et supprimer des rôles et fonctionnalités Assistant, avec toutefois une différence importante. Dans Windows PowerShell, contrairement à dans Ajouter des rôles et de l’Assistant de fonctionnalités, outils de gestion et des composants logiciels enfichables pour un rôle ne sont pas inclus par défaut. Pour inclure les outils de gestion dans le cadre de l’installation d’un rôle, ajoutez le paramètre `IncludeManagementTools` à l’applet de commande. Si vous installez des rôles et fonctionnalités sur un serveur qui exécute l’option d’installation Server Core de Windows Server 2012 ou versions ultérieures, vous pouvez ajouter les outils de gestion d’un rôle à une installation, mais enfichables et outils de gestion basée sur une interface graphique utilisateur ne peut pas être installés. sur les serveurs qui exécutent l’option d’installation Server Core de Windows Server. Uniquement de la ligne de commande et les outils de gestion de Windows PowerShell peuvent être installés sur l’option d’installation Server Core.  
  
#### <a name="to-install-roles-and-features-by-using-the-install-windowsfeature-cmdlet"></a>Pour installer des rôles et des fonctionnalités à l’aide de l’applet de commande Install-WindowsFeature  
  
1.  Effectuez une des opérations suivantes pour ouvrir une session Windows PowerShell avec des droits utilisateur élevés.  
  
    > [!NOTE]  
    > Si vous installez des rôles et fonctionnalités sur un serveur distant, il est inutile d’exécuter Windows PowerShell avec des droits utilisateur élevés.  
  
    -   Sur le Bureau Windows, cliquez avec le bouton droit dans la barre des tâches sur **Windows PowerShell** , puis cliquez sur **Exécuter en tant qu’administrateur**.  
  
    -   Sur le Windows **Démarrer** écran, cliquez sur la vignette pour Windows PowerShell et cliquez sur la barre des applications **exécuter en tant qu’administrateur**.  
  
2.  Tapez **Get-WindowsFeature**, puis appuyez sur **Entrée** pour afficher une liste de rôles et fonctionnalités installés et disponibles sur le serveur local. Si l’ordinateur local n’est pas un serveur, ou si vous souhaitez des informations sur un serveur distant, exécutez **Get-WindowsFeature - computerName <***Nom_Ordinateur***>**, dans lequel  *Nom_Ordinateur* représente le nom d’un ordinateur distant qui exécute Windows Server 2016. Les résultats de l’applet de commande contiennent les noms de commande des rôles et fonctionnalités que vous ajoutez à votre applet de commande à l’étape 4.  
  
    > [!NOTE]  
    > Dans Windows PowerShell 3.0 et versions ultérieures de Windows PowerShell, il n’est pas nécessaire d’importer le module d’applet de commande Gestionnaire de serveur dans la session Windows PowerShell avant d’exécuter les applets de commande qui font partie du module. Un module est automatiquement importé la première fois que vous exécutez une applet de commande qui fait partie du module. En outre, les applets de commande Windows PowerShell, ni les noms de fonctionnalités utilisés avec les applets de commande respectent la casse.  
  
3.  type **Get-help Install-WindowsFeature**, puis appuyez sur **entrée** pour afficher la syntaxe et les paramètres acceptés pour le `Install-WindowsFeature` applet de commande.  
  
4.  Tapez la commande suivante, puis appuyez sur **entrée**, où *feature_name* représente le nom de la commande d’un rôle ou une fonctionnalité que vous souhaitez installer (obtenu à l’étape 2), et *Nom_Ordinateur* représente un ordinateur distant sur lequel vous souhaitez installer des rôles et fonctionnalités. Séparez plusieurs valeurs pour *nom_fonctionnalité* à l’aide de virgules. Le paramètre `Restart` permet de redémarrer automatiquement le serveur de destination si l’installation des rôles ou des fonctionnalités le requiert.  
  
    ```  
    Install-WindowsFeature -Name <feature_name> -computerName <computer_name> -Restart  
    ```  
  
    Pour installer des rôles et fonctionnalités sur un disque dur virtuel hors connexion, ajoutez les paramètres `computerName` et `VHD` . Si vous n’ajoutez pas le paramètre `computerName` , l’applet de commande suppose que l’ordinateur local est monté pour accéder au disque dur virtuel. Le paramètre `computerName` contient le nom du serveur sur lequel monter le disque dur virtuel tandis que le paramètre `VHD` contient le chemin d’accès au fichier VHD sur le serveur spécifié.  
  
    > [!NOTE]  
    > Vous devez ajouter le `computerName` paramètre si vous exécutez l’applet de commande à partir d’un ordinateur qui exécute un système d’exploitation du client Windows.  
    >   
    > Pour installer des rôles et des fonctionnalités sur des disques durs virtuels hors connexion, les disques durs virtuels hors connexion cibles doivent répondre aux exigences suivantes.  
    >   
    > -   Ils doivent être équipés de la version de Windows Server qui correspond à la version du Gestionnaire de serveur vous sont en cours d’exécution. Consultez la note au début de [installer des rôles, services de rôle et fonctionnalités à l’aide de l’ajouter des rôles et fonctionnalités Assistant](#BKMK_installarfw).  
    > -   Ils ne peuvent pas posséder plus d’un volume ou d’une partition système.  
    > -   Le dossier réseau partagé dans lequel le disque dur virtuel hors connexion est stocké doit accorder les droits d’accès suivants au compte d’ordinateur (ou système local) du serveur que vous avez sélectionné pour le montage du disque dur virtuel. L’accès au compte d’utilisateur uniquement ne suffit pas. Le partage peut accorder les autorisations **Lire** et **Écrire** au groupe **Tout le monde** pour autoriser l’accès au disque dur virtuel, mais pour des raisons de sécurité, cela n’est pas recommandé.  
    >   
    >     -   Accès **Lecture/écriture** dans la boîte de dialogue **Partage de fichiers**.  
    >     -   **Contrôle total** accéder sur le **sécurité** tab, fichier ou dossier **propriétés** boîte de dialogue.  
  
    ```  
    Install-WindowsFeature -Name <feature_name> -VHD <path> -computerName <computer_name> -Restart  
    ```  
  
    **Exemple :** L’applet de commande suivante installe le rôle Services de domaine active directory et la fonctionnalité Gestion de stratégie de groupe sur un serveur distant nommé ContosoDC1. Les outils de gestion et les composants logiciels enfichables sont ajoutés à l’aide du paramètre `IncludeManagementTools` et le serveur de destination doit être redémarré automatiquement si l’installation exige un redémarrage des serveurs.  
  
    ```  
    Install-WindowsFeature -Name AD-Domain-Services,GPMC -computerName ContosoDC1 -IncludeManagementTools -Restart  
    ```  
  
5.  Lors de l’installation terminée, vérifiez-la en ouvrant le **tous les serveurs** page Gestionnaire de serveur, en sélectionnant un serveur sur lequel vous avez installé des rôles et fonctionnalités, puis en affichant le **des rôles et fonctionnalités** vignette sur la page pour le serveur sélectionné. Vous pouvez également exécuter la `Get-WindowsFeature` applet de commande ciblée sur le serveur sélectionné (Get-WindowsFeature - computerName <*Nom_Ordinateur*>) pour afficher la liste des rôles et fonctionnalités installés sur le serveur.  
  
## <a name="BKMK_removerrfw"></a>supprimer des rôles, services de rôle et fonctionnalités à l’aide de la supprimer Assistant rôles et fonctionnalités  
Vous devez être connecté à un serveur en tant qu’administrateur pour désinstaller des rôles, services de rôle et fonctionnalités. Si vous êtes connecté à l’ordinateur local avec un compte sans droits d’administrateur sur votre serveur cible de désinstallation, cliquez avec le bouton droit sur le serveur cible dans la vignette **Serveurs**, puis cliquez sur **Gérer en tant que** pour préciser un compte doté de droits d’administrateur. Le serveur sur lequel vous voulez monter un disque dur virtuel hors connexion doit être ajouté au Gestionnaire de serveur et vous devez avoir des droits d’administrateur sur le serveur.  
  
#### <a name="to-remove-roles-and-features-by-using-the-remove-roles-and-features-wizard"></a>Pour supprimer des rôles et fonctionnalités à l’aide de la supprimer Assistant rôles et fonctionnalités  
  
1.  Si le Gestionnaire de serveur est déjà ouvert, passez à l’étape suivante. S’il n’est pas déjà ouvert, ouvrez-le en effectuant l’une des opérations suivantes.  
  
    -   Sur le Bureau Windows, démarrez le Gestionnaire de serveur en cliquant sur **Gestionnaire de serveur** dans la barre des tâches Windows.  
  
    -   Dans l’**écran d’accueil** de Windows, cliquez sur la vignette **Gestionnaire de serveur**.  
  
2.  Dans le menu **Gérer**, cliquez sur **Supprimer des rôles et fonctionnalités**.  
  
3.  Dans la page **Avant de commencer**, vérifiez que vous avez préparé la suppression de rôles ou de fonctionnalités sur un serveur. Cliquez sur **Suivant**.  
  
4.  Sur le **sélectionner un serveur de Destination** page, sélectionnez un serveur du pool de serveurs ou sélectionnez un disque dur virtuel hors connexion. Pour sélectionner un disque dur virtuel hors connexion, choisissez d’abord le serveur sur lequel monter le disque dur virtuel, puis sélectionnez le fichier VHD.  
  
    > [!NOTE]  
    > Le dossier réseau partagé dans lequel le disque dur virtuel hors connexion est stocké doit accorder les droits d’accès suivants au compte d’ordinateur (ou système local) du serveur que vous avez sélectionné pour le montage du disque dur virtuel. L’accès au compte d’utilisateur uniquement ne suffit pas. Le partage peut accorder les autorisations **Lire** et **Écrire** au groupe **Tout le monde** pour autoriser l’accès au disque dur virtuel, mais pour des raisons de sécurité, cela n’est pas recommandé.  
    >   
    > -   Accès **Lecture/écriture** dans la boîte de dialogue **Partage de fichiers**.  
    > -   Accès **Contrôle total** dans l’onglet **Sécurité** de la boîte de dialogue **Propriétés** d’un fichier ou d’un dossier.  
  
    Pour plus d’informations sur l’ajout de serveurs à votre pool de serveurs, consultez [ajouter des serveurs au Gestionnaire de serveur](add-servers-to-server-manager.md). Une fois que vous avez sélectionné le serveur de destination, cliquez sur **Suivant**.  
  
    > [!NOTE]  
    > Vous pouvez utiliser la suppression de rôles et Assistant de fonctionnalités à supprimer des rôles et fonctionnalités à partir des serveurs qui exécutent la même version de Windows Server qui prend en charge la version de Server Manager que vous utilisez. Vous ne pouvez pas supprimer des rôles, services de rôle ou fonctionnalités à partir des serveurs qui exécutent Windows Server 2016, si vous exécutez le Gestionnaire de serveur sur Windows Server 2012 R2, Windows Server 2012 ou Windows 8. Vous ne pouvez pas utiliser la supprimer Assistant rôles et fonctionnalités à supprimer des rôles et fonctionnalités à partir des serveurs qui exécutent Windows Server 2008 ou Windows Server 2008 R2.  
  
5.  Sélectionnez des rôles, sélectionnez des services de rôle pour le rôle, le cas échéant, puis cliquez sur **Suivant** pour sélectionner des fonctionnalités.  
  
    Lorsque vous continuez, la suppression de rôles et fonctionnalités Assistant vous invite automatiquement à supprimer tous les rôles, services de rôle ou fonctionnalités qui ne peut pas s’exécuter sans les rôles ou fonctionnalités que vous supprimez.  
  
    en outre, vous pouvez choisir de supprimer des outils de gestion et des composants logiciels enfichables pour les rôles sur le serveur de destination. Par défaut, dans la suppression de rôles et fonctionnalités Assistant, les outils de gestion sont sélectionnés pour suppression. Vous pouvez ignorer les outils de gestion et les composants logiciels enfichables si vous comptez utiliser le serveur sélectionné pour gérer le rôle sur d’autres serveurs distants.  
  
6.  Dans la page **Confirmer les sélections pour la suppression**, passez en revue vos sélections de rôle, fonctionnalité et serveur. Si vous êtes prêt à supprimer les rôles ou fonctionnalités, cliquez sur **supprimer**.  
  
7.  Après avoir cliqué sur **supprimer**, le **progression de la suppression** page affiche la progression de la suppression, les résultats et les messages tels que des avertissements, échecs ou étapes de configuration de post-suppression qui sont requises, telles que redémarrer le serveur de destination. Dans Windows Server 2012 et versions ultérieures de Windows Server, vous pouvez fermer la supprimer Assistant rôles et fonctionnalités alors que la suppression est encore dans la progression et affichent les résultats de la suppression ou d’autres messages dans la **Notifications** zone en haut de la Console du Gestionnaire de serveur. Cliquez sur le **Notifications** indicateur pour afficher plus de détails sur les suppressions ou autres tâches que vous effectuez dans le Gestionnaire de serveur.  
  
## <a name="BKMK_removewps"></a>supprimer des rôles, services de rôle et fonctionnalités à l’aide des applets de commande Windows PowerShell  
Les applets de commande de déploiement de gestionnaire de serveur pour Windows PowerShell fonctionnent de la même façon à l’interface graphique utilisateur Supprimer Assistant rôles et fonctionnalités, avec une différence importante près. Dans Windows PowerShell, contrairement à dans la supprimer les rôles et fonctionnalités Assistant, outils de gestion et des composants logiciels enfichables pour un rôle ne sont pas supprimés par défaut. Pour supprimer les outils de gestion dans le cadre de la suppression d’un rôle, ajoutez le paramètre `IncludeManagementTools` à l’applet de commande. Si vous désinstallez des rôles et fonctionnalités à partir d’un serveur qui exécute l’option d’installation Server Core de Windows Server 2012 ou une version ultérieure de Windows Server, cette supprime du paramètre de ligne de commande et les outils de gestion de Windows PowerShell pour le texte spécifié rôles et fonctionnalités.  
  
#### <a name="to-remove-roles-and-features-by-using-the-uninstall-windowsfeature-cmdlet"></a>Pour supprimer des rôles et des fonctionnalités à l’aide de l’applet de commande Uninstall-WindowsFeature  
  
1.  Effectuez une des opérations suivantes pour ouvrir une session Windows PowerShell avec des droits utilisateur élevés.  
  
    > [!NOTE]  
    > Si vous désinstallez des rôles et fonctionnalités à partir d’un serveur distant, il est inutile d’exécuter Windows PowerShell avec des droits utilisateur élevés.  
  
    -   Sur le Bureau Windows, cliquez avec le bouton droit dans la barre des tâches sur **Windows PowerShell** , puis cliquez sur **Exécuter en tant qu’administrateur**.  
  
    -   Sur le Windows **Démarrer** écran, cliquez sur la vignette de Windows PowerShell et cliquez sur la barre des applications **exécuter en tant qu’administrateur**.  
  
2.  Tapez **Get-WindowsFeature**, puis appuyez sur **Entrée** pour afficher une liste de rôles et fonctionnalités installés et disponibles sur le serveur local. Si l’ordinateur local n’est pas un serveur, ou si vous souhaitez des informations sur un serveur distant, exécutez **Get-WindowsFeature - computerName <***Nom_Ordinateur***>**, dans lequel  *Nom_Ordinateur* représente le nom d’un ordinateur distant qui exécute Windows Server 2016. Les résultats de l’applet de commande contiennent les noms de commande des rôles et fonctionnalités que vous ajoutez à votre applet de commande à l’étape 4.  
  
    > [!NOTE]  
    > Dans Windows PowerShell 3.0 et versions ultérieures de Windows PowerShell, il n’est pas nécessaire d’importer le module d’applet de commande Gestionnaire de serveur dans la session Windows PowerShell avant d’exécuter les applets de commande qui font partie du module. Un module est automatiquement importé la première fois que vous exécutez une applet de commande qui fait partie du module. En outre, les applets de commande Windows PowerShell, ni les noms de fonctionnalités utilisés avec les applets de commande respectent la casse.  
  
3.  type **Get-help Uninstall-WindowsFeature**, puis appuyez sur **entrée** pour afficher la syntaxe et les paramètres acceptés pour le `Uninstall-WindowsFeature` applet de commande.  
  
4.  Tapez ce qui suit, puis appuyez sur **Entrée**, où *feature_name* représente le nom de commande d’un rôle ou d’une fonctionnalité que vous souhaitez supprimer (obtenu à l’étape 2) et *computer_name* représente un ordinateur distant duquel vous souhaitez supprimer des rôles et fonctionnalités. Séparez plusieurs valeurs pour *nom_fonctionnalité* à l’aide de virgules. Le paramètre `Restart` permet de redémarrer automatiquement les serveurs de destination si la suppression des rôles ou des fonctionnalités le requiert.  
  
    ```  
    Uninstall-WindowsFeature -Name <feature_name> -computerName <computer_name> -Restart  
    ```  
  
    Pour supprimer des rôles et fonctionnalités d’un disque dur virtuel hors connexion, ajoutez les paramètres `computerName` et `VHD` . Si vous n’ajoutez pas le paramètre `computerName` , l’applet de commande suppose que l’ordinateur local est monté pour accéder au disque dur virtuel. Le paramètre `computerName` contient le nom du serveur sur lequel monter le disque dur virtuel tandis que le paramètre `VHD` contient le chemin d’accès au fichier VHD sur le serveur spécifié.  
  
    > [!NOTE]  
    > Vous devez ajouter le `computerName` paramètre si vous exécutez l’applet de commande à partir d’un ordinateur qui exécute un système d’exploitation du client Windows.  
    >   
    > Le dossier réseau partagé dans lequel le disque dur virtuel hors connexion est stocké doit accorder les droits d’accès suivants au compte d’ordinateur (ou système local) du serveur que vous avez sélectionné pour le montage du disque dur virtuel. L’accès au compte d’utilisateur uniquement ne suffit pas. Le partage peut accorder les autorisations **Lire** et **Écrire** au groupe **Tout le monde** pour autoriser l’accès au disque dur virtuel, mais pour des raisons de sécurité, cela n’est pas recommandé.  
    >   
    > -   Accès **Lecture/écriture** dans la boîte de dialogue **Partage de fichiers**.  
    > -   **Contrôle total** accéder sur le **sécurité** tab, fichier ou dossier **propriétés** boîte de dialogue.  
  
    ```  
    Uninstall-WindowsFeature -Name <feature_name> -VHD <path> -computerName <computer_name> -Restart  
    ```  
  
    **Exemple :** L’applet de commande suivante supprime le rôle Services de domaine active directory et la fonctionnalité Gestion de stratégie de groupe à partir d’un serveur distant nommé ContosoDC1. Les outils de gestion et les composants logiciels enfichables sont également supprimés et le serveur de destination doit être redémarré automatiquement si la suppression exige un redémarrage des serveurs.  
  
    ```  
    Uninstall-WindowsFeature -Name AD-Domain-Services,GPMC -computerName ContosoDC1 -IncludeManagementTools -Restart  
    ```  
  
5.  Lors de la désinstallation terminée, vérifiez que les rôles et fonctionnalités sont supprimées en ouvrant le **tous les serveurs** page Gestionnaire de serveur, en sélectionnant le serveur à partir duquel vous avez supprimé des rôles et fonctionnalités, puis en affichant le **rôles et Fonctionnalités** vignette sur la page pour le serveur sélectionné. Vous pouvez également exécuter la `Get-WindowsFeature` applet de commande ciblée sur le serveur sélectionné (Get-WindowsFeature - computerName <*Nom_Ordinateur*>) pour afficher la liste des rôles et fonctionnalités installés sur le serveur.  
  
## <a name="BKMK_batch"></a>Installer des rôles et fonctionnalités sur plusieurs serveurs en exécutant un script Windows PowerShell  
Bien que vous ne pouvez pas utiliser les fonctionnalités Assistant Ajouter des rôles pour installer des rôles, services de rôle et fonctionnalités sur plusieurs serveurs cibles dans une session d’Assistant unique, vous pouvez utiliser un script Windows PowerShell pour installer des rôles, services de rôle et fonctionnalités sur plusieurs cibles serveurs que vous gérez à l’aide du Gestionnaire de serveur. Le script que vous utilisez pour effectuer le déploiement par lots, en tant que ce processus est appelé, pointe vers un fichier de configuration XML que vous pouvez aisément créer par l’Assistant de fonctionnalités et ajouter des rôles, puis en cliquant sur **exporter les paramètres de configuration** après progressé dans l’Assistant pour le **confirmer les sélections d’installation** page Ajouter des rôles et de l’Assistant les fonctionnalités.  
  
> [!IMPORTANT]  
> Tous les serveurs cibles qui sont spécifiés dans votre script doivent exécuter la version de Windows Server qui correspond à la version du Gestionnaire de serveur en cours d’exécution sur l’ordinateur local. Par exemple, si vous exécutez le Gestionnaire de serveur sur Windows 10, vous pouvez installer des rôles, services de rôle et fonctionnalités sur les serveurs qui exécutent Windows Server 2016. Si les outils de gestion basée sur une interface graphique utilisateur sont ajoutés à l’installation, le processus d’installation cible automatiquement les convertit les serveurs qui exécutent l’option d’installation Server Core de Windows Server à l’option d’installation complète (serveur avec interface GUI complète, également appelée comme en cours d’exécution interpréteur de commandes graphique).  
>   
> Le script fourni dans cette section est un exemple de comment le déploiement par lots peut être mené à l’aide de la `Install-WindowsFeature` applet de commande et un script Windows PowerShell. Il existe d’autres scripts et méthodes possibles pour le déploiement par lots sur plusieurs serveurs. Pour rechercher ou proposer d’autres scripts pour le déploiement des rôles et des fonctionnalités, effectuez une recherche dans le [référentiel du Centre de scripts](https://gallery.technet.microsoft.com/ScriptCenter).  
  
#### <a name="to-install-roles-and-features-on-multiple-servers"></a>Pour installer des rôles et des fonctionnalités sur plusieurs servers  
  
1.  Si vous ne le n'avez pas déjà fait, créez un fichier de configuration XML qui contient les rôles, services de rôle et fonctionnalités que vous voulez installer sur plusieurs serveurs. Vous pouvez créer ce fichier de configuration en exécutant l’ajouter des rôles et fonctionnalités Assistant, en sélectionnant rôles, services de rôle et fonctionnalités que vous souhaitez, puis en cliquant sur **exporter les paramètres de configuration** après avoir progressé dans l’Assistant pour le **Confirmer les sélections d’installation** page. Enregistrez le fichier de configuration à un endroit pratique. Vous n’avez pas besoin de cliquer sur **Installer** ni de mener à terme les étapes de l’assistant si vous l’exécutez dans le seul but de créer un fichier de configuration.  
  
2.  Effectuez une des opérations suivantes pour ouvrir une session Windows PowerShell avec des droits utilisateur élevés.  
  
    -   Sur le Bureau Windows, cliquez avec le bouton droit dans la barre des tâches sur **Windows PowerShell** , puis cliquez sur **Exécuter en tant qu’administrateur**.  
  
    -   Sur le Windows **Démarrer** écran, cliquez sur la vignette de Windows PowerShell et cliquez sur la barre des applications **exécuter en tant qu’administrateur**.  
  
3.  Copiez et collez le script suivant dans votre session Windows PowerShell.  
  
    ```  
    function Invoke-WindowsFeatureBatchDeployment {  
        param (  
            [parameter(mandatory)]  
            [string[]] $computerNames,  
            [parameter(mandatory)]  
            [string] $ConfigurationFilepath  
        )  
  
        # Deploy the features on multiple computers simultaneously.  
        $jobs = @()  
        foreach($computerName in $computerNames) {  
            $jobs += start-Job -Command {  
                Install-WindowsFeature -ConfigurationFilepath $using:ConfigurationFilepath -computerName $using:computerName -Restart  
            }   
        }  
  
        Receive-Job -Job $jobs -Wait | select-Object Success, RestartNeeded, exitCode, FeatureResult  
    }  
    ```  
  
    Si nécessaire, les serveurs cibles sont automatiquement redémarrés par les rôles et les fonctionnalités que vous choisissez.  
  
4.  Exécutez la fonction en effectuant ce qui suit.  
  
    1.  Créez une variable pour y stocker les noms de vos ordinateurs cibles, séparés par des virgules. Dans l’exemple qui suit, la variable `$ServerNames` stocke les noms des serveurs cibles *Contoso_01* et *Contoso_02*. Appuyez sur **Entrée**.  
  
        ```  
        # Sample Invocation  
        $ServerNames = 'Contoso_01','Contoso_02'  
        Invoke-WindowsFeatureBatchDeployment -computerNames $ServerNames -ConfigurationFilepath C:\Users\sampleuser\Desktop\DeploymentConfigTemplate.xml  
        ```  
  
    2.  Pour exécuter la fonction, tapez ce qui suit et appuyez sur **Entrée**. `$ServerNames` est un exemple de la variable que vous avez créée lors de l’étape précédente et *C:\Users\Sampleuser\Desktop\DeploymentConfigTemplate.xml* est un exemple de chemin d’accès au fichier de configuration créé à l’étape 1.  
  
        **Invoke-WindowsFeatureBatchDeployment -computerNames $ServerNames -ConfigurationFilepath C:\Users\Sampleuser\Desktop\DeploymentConfigTemplate.xml**  
  
5.  Lors de l’installation terminée, vérifiez-la en ouvrant le **tous les serveurs** page Gestionnaire de serveur, en sélectionnant un serveur sur lequel vous avez installé des rôles et fonctionnalités, puis en affichant le **des rôles et fonctionnalités** vignette sur la page pour le serveur sélectionné. Vous pouvez également exécuter la `Get-WindowsFeature` applet de commande ciblée sur un serveur spécifique (`Get-WindowsFeature -computerName` <*Nom_Ordinateur*>) pour afficher la liste des rôles et fonctionnalités installés sur le serveur.  
  
## <a name="BKMK_FoD"></a>Installer .NET Framework 3.5 et autres fonctionnalités à la demande  
à partir de Windows Server 2012 et Windows 8, les fichiers de fonctionnalités pour .NET Framework 3.5 (qui inclut le .NET Framework 2.0 et .NET Framework 3.0) ne sont pas disponibles sur l’ordinateur local par défaut. Les fichiers ont été supprimés. Les fichiers de fonctionnalités qui ont été supprimés dans une configuration Fonctionnalités à la demande, ainsi que les fichiers de fonctionnalités pour le .NET Framework 3.5, sont disponibles par le biais de Windows Update. Par défaut, si les fichiers de fonctionnalités ne sont pas disponibles sur le serveur de destination qui exécute Windows Server 2012 ou versions ultérieures, le processus d’installation recherche les fichiers manquants en se connectant à Windows Update. Vous pouvez remplacer le comportement par défaut en configurant un paramètre de stratégie de groupe ou en spécifiant un chemin d’accès de l’autre source pendant l’installation, que vous effectuiez une installation à l’aide de l’ajouter des rôles et fonctionnalités Assistant GUI ou une ligne de commande.  
  
Vous pouvez installer le .NET Framework 3.5 en effectuant l’une des opérations suivantes :  
  
-   Utilisez [Pour installer le .NET Framework 3.5 à l’aide de l’applet de commande Install-WindowsFeature](#BKMK_dotnetcmdlet) pour ajouter le paramètre `Source` et spécifiez une source à partir de laquelle obtenir les fichiers de fonctionnalités .NET Framework 3.5. Si vous n’ajoutez pas le paramètre `Source`, le processus d’installation détermine d’abord si un chemin d’accès aux fichiers de fonctionnalités a été spécifié par des paramètres de stratégie de groupe et, si aucun chemin d’accès n’est trouvé, il recherche les fonctionnalités manquantes à l’aide de Windows Update.  
  
-   Utilisez [pour installer .NET Framework 3.5 à l’aide de l’ajouter des rôles et fonctionnalités Assistant](#BKMK_arfw) pour spécifier un autre fichier emplacement source sur le **confirmer les options d’installation** page Ajouter des rôles et de l’Assistant les fonctionnalités.  
  
-   Appliquez la procédure [Pour installer le .NET Framework 3.5 à l’aide de DISM](#BKMK_dism) pour obtenir des fichiers à partir de Windows Update par défaut ou en spécifiant un chemin d’accès source vers le support d’installation.  
  
[Configurez d’autres sources pour les fichiers de fonctionnalités dans la stratégie de groupe](#BKMK_configgp) pour le .NET Framework 3.5 ou d’autres fonctionnalités, si les fichiers de fonctionnalités sont introuvables sur l’ordinateur local.  
  
> [!IMPORTANT]  
> Lors de l’installation des fichiers de fonctionnalités à partir d’une source distante, le chemin d’accès source ou partage de fichiers doit accorder des autorisations de **Lecture** au groupe **Tout le monde** (non recommandé pour des raisons de sécurité) ou au compte d’ordinateur (système local) du serveur de destination ; l’accord d’un accès au compte d’utilisateur n’est pas suffisant.  
>   
> Les serveurs qui se trouvent dans des groupes de travail ne peuvent pas accéder aux partages de fichiers externes, même si le compte d’ordinateur pour le serveur de groupe de travail a des autorisations de **Lecture** sur le partage externe. D’autres emplacements sources qui fonctionnent pour les serveurs de groupes de travail sont notamment les supports d’installation, Windows Update, et les disques durs virtuels ou les fichiers WIM qui sont stockés sur le serveur de groupe de travail local.  
>   
> Lorsque vous installez des rôles, services de rôle et fonctionnalités sur un serveur physique en cours d’exécution, vous pouvez spécifier un fichier WIM en tant que fonctionnalité autre fichier source. Le chemin d’accès source d’un fichier WIM doit adopter le format suivant, avec **WIM** comme préfixe et l’index dans lequel les fichiers de fonctionnalités sont situés en guise de suffixe : **WIM:e:\sources\install.wim:4**. Toutefois, vous ne pouvez pas utiliser un fichier WIM directement en tant que source pour l’installation des rôles, services de rôle et fonctionnalités sur un disque dur virtuel hors connexion ; Vous devez soit monter le disque dur virtuel hors connexion et pointez sur son chemin d’accès de montage pour les fichiers sources, ou vous devez pointer vers un dossier qui contient une copie du contenu du fichier WIM.  
  
### <a name="BKMK_dotnetcmdlet"></a>Pour installer .NET Framework 3.5 en exécutant l’applet de commande Install-WindowsFeature  
  
1.  Effectuez une des opérations suivantes pour ouvrir une session Windows PowerShell avec des droits utilisateur élevés.  
  
    > [!NOTE]  
    > Si vous installez des rôles et fonctionnalités à partir d’un serveur distant, il est inutile d’exécuter Windows PowerShell avec des droits utilisateur élevés.  
  
    -   Sur le Bureau Windows, cliquez avec le bouton droit dans la barre des tâches sur **Windows PowerShell** , puis cliquez sur **Exécuter en tant qu’administrateur**.  
  
    -   Sur le Windows **Démarrer** écran, cliquez sur la vignette de Windows PowerShell et cliquez sur la barre des applications **exécuter en tant qu’administrateur**.  
  
    -   Sur un serveur qui exécute l’option d’installation Server Core de Windows Server 2012 R2 ou Windows Server 2012, tapez **PowerShell** à une invite de commandes, puis appuyez sur **entrée**.  
  
2.  Tapez la commande suivante, puis appuyez sur **entrée**. Dans l’exemple suivant, les fichiers sources se trouvent dans un magasin côte à côte (abrégé en **SxS**) sur un support d’installation sur le disque D.  
  
    ```  
    Install-WindowsFeature NET-Framework-Core -Source D:\Sources\SxS  
    ```  
  
    Si vous voulez que la commande utilise Windows Update comme source pour les fichiers de fonctionnalités manquants ou si une source par défaut a déjà été configurée par la stratégie de groupe, il est inutile d’ajouter le paramètre `Source` , à moins que vous ne souhaitiez spécifier une source différente.  
  
### <a name="BKMK_arfw"></a>Pour installer .NET Framework 3.5 à l’aide de l’ajouter Assistant rôles et fonctionnalités  
  
1.  Sur le **gérer** menu dans le Gestionnaire de serveur, cliquez sur **ajouter des rôles et fonctionnalités**.  
  
2.  Sélectionnez un serveur de destination qui exécute Windows Server 2016.  
  
3.  Sur le **sélectionner des fonctionnalités** page Ajouter des rôles et de l’Assistant de fonctionnalités, sélectionnez **.NET Framework 3.5**.  
  
4.  Si l’ordinateur local y est autorisé par les paramètres de stratégie de groupe, le processus d’installation tente d’obtenir les fichiers de fonctionnalités manquants à l’aide de Windows Update. Cliquez sur **Installer** ; inutile de passer à l’étape suivante.  
  
    Si les paramètres de stratégie de groupe n’autorisent pas cela, ou vous souhaitez utiliser une autre source pour les fichiers de fonctionnalités .NET Framework 3.5, sur le **confirmer les sélections d’installation** page de l’Assistant, cliquez sur **spécifier un chemin d’accès de l’autre source** .  
  
5.  Spécifiez un chemin d’accès à un magasin côte à côte (appelé **SxS**) sur un support d’installation ou un chemin d’accès à un fichier WIM. Dans l’exemple suivant, le support d’installation se trouve sur le disque D.  
  
    **D:\Sources\SxS\\**  
  
    Pour spécifier un fichier WIM, ajoutez un préfixe  **WIM:** et ajoutez l’index de l’image à utiliser dans le fichier WIM comme suffixe, comme indiqué dans l’exemple suivant.  
  
    **WIM :\\\\***nom_serveur***\share\install.wim:3**  
  
6.  Cliquez sur **OK**, puis sur **Installer**.  
  
### <a name="BKMK_dism"></a>Pour installer .NET Framework 3.5 à l’aide de DISM  
  
1.  Effectuez une des opérations suivantes pour ouvrir une session Windows PowerShell avec des droits utilisateur élevés.  
  
    > [!NOTE]  
    > Si vous installez des rôles et fonctionnalités à partir d’un serveur distant, il est inutile d’exécuter Windows PowerShell avec des droits utilisateur élevés.  
  
    -   Sur le Bureau Windows, cliquez avec le bouton droit dans la barre des tâches sur **Windows PowerShell** , puis cliquez sur **Exécuter en tant qu’administrateur**.  
  
    -   Sur le Windows **Démarrer** écran, cliquez sur la vignette de Windows PowerShell et cliquez sur la barre des applications **exécuter en tant qu’administrateur**.  
  
    -   Sur un serveur qui exécute l’option d’installation Server Core, tapez **PowerShell** à une invite de commandes, puis appuyez sur **entrée**.  
  
2.  Exécutez l’une des commandes DISM suivantes.  
  
    -   Si l’ordinateur a accès à la mise à jour de Windows ou un emplacement de fichier source par défaut a déjà été configuré dans la stratégie de groupe, exécutez la commande suivante.  
  
        ```  
        DISM /online /Enable-Feature /Featurename:NetFx3 /All  
        ```  
  
    -   Si l’ordinateur a accès au support d’installation, exécutez une commande semblable à ce qui suit. Dans l’exemple suivant, le support d’installation du système d’exploitation se trouve sur le disque D. Le paramètre `LimitAccess` empêche la commande d’essayer de contacter Windows Update ou un serveur qui exécute les services WSUS.  
  
        ```  
        DISM /online /Enable-Feature /Featurename:NetFx3 /All /LimitAccess /Source:d:\sources\sxs  
        ```  
  
    > [!NOTE]  
    > La DISM respecte la casse.  
  
### <a name="BKMK_configgp"></a>Configurer d’autres sources pour les fichiers de la fonctionnalité stratégie de groupe  
Le paramètre de stratégie de groupe décrit dans cette section spécifie les emplacements sources autorisés pour les fichiers du .NET Framework 3.5 et d’autres fichiers de fonctionnalités qui ont été supprimés dans le cadre d’une configuration Fonctionnalités à la demande. Le paramètre de stratégie **spécifier les paramètres pour l’installation des composants facultatifs et la réparation de composants** se trouve dans le **ordinateur Configuration ordinateur\Modèles d’administration\Système** dossier dans la stratégie de groupe Éditeur de stratégie de groupe locale ou de la Console de gestion.  
  
> [!NOTE]  
> Vous devez être membre du groupe Administrateurs pour modifier des paramètres de stratégie de groupe sur l’ordinateur local. Si les paramètres de stratégie de groupe de l’ordinateur que vous voulez gérer sont contrôlés au niveau du domaine, vous devez être membre du groupe Administrateurs du domaine pour modifier des paramètres de stratégie de groupe.  
  
##### <a name="to-configure-a-default-alternate-source-path-in-group-policy"></a>Pour configurer un autre chemin d’accès source par défaut dans la stratégie de groupe  
  
1.  Dans l’éditeur de stratégie de groupe locale ou la Console de gestion de stratégie de groupe, ouvrez le paramètre de stratégie suivant.  
  
    **configuration de l’ordinateur\Modèles d’administration\système\spécifier des paramètres de l’ordinateur pour l’installation des composants facultatifs et la réparation de composants**  
  
2. Sélectionnez **activé** pour activer le paramètre de stratégie, s’il n’est pas déjà activé.  
  
3.  Dans la zone de texte **Autre chemin d’accès au fichier source** dans la zone **Options**, spécifiez un chemin d’accès complet à un dossier partagé ou un fichier WIM. Pour spécifier un fichier WIM comme autre emplacement de fichier source, ajoutez le préfixe **WIM:** au chemin d’accès et ajoutez l’index de l’image à utiliser dans le fichier WMI comme suffixe. Voici quelques exemples de valeurs que vous pouvez spécifier.  
  
    -   chemin d’accès à un dossier partagé : **\\\\***nom_serveur***\share\\*** nom_dossier*  
  
    -   chemin d’accès à un fichier WIM, dans lequel **3** représente l’index de l’image dans lequel sont trouvent les fichiers de fonctionnalités :  **WIM :\\\\***nom_serveur***\share\install.wim:3**  
  
4.  Si vous ne souhaitez pas que les ordinateurs contrôlés par ce paramètre de stratégie pour rechercher les fichiers manquants dans la mise à jour de Windows, sélectionnez **jamais tente de télécharger la charge utile à partir de Windows Update**.  
  
5.  Si les ordinateurs contrôlés par ce paramètre de stratégie reçoivent généralement des mises à jour par le biais des services WSUS, mais que vous préférez passer par Windows Update plutôt que par les services WSUS pour rechercher les fichiers manquants, sélectionnez **Contacter Windows Update directement pour télécharger le contenu de réparation au lieu de Windows Server Update Services (WSUS)**.  
  
6.  Cliquez sur **OK** une fois que vous avez terminé de modifier ce paramètre de stratégie, puis fermez l’Éditeur de stratégie de groupe.  
  
## <a name="see-also"></a>Voir aussi  
[Options d’Installation de Windows Server](https://go.microsoft.com/fwlink/p/?LinkId=241573)  
[Considérations relatives au déploiement de Microsoft .NET Framework 3.5](https://go.microsoft.com/fwlink/p/?LinkId=248869)  
[Comment activer ou désactiver des fonctionnalités de Windows](https://go.microsoft.com/fwlink/p/?LinkId=246552)  
  


