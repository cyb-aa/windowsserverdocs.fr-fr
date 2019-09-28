---
title: Installer ou désinstaller des rôles, des services de rôle ou des fonctionnalités
description: Gestionnaire de serveur
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: cca2e4c7ba2658c4d85b14ef61ef5f79fbc96345
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383190"
---
# <a name="install-or-uninstall-roles-role-services-or-features"></a>Installer ou désinstaller des rôles, des services de rôle ou des fonctionnalités

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Dans Windows Server, la console Gestionnaire de serveur et les applets de commande Windows PowerShell pour Gestionnaire de serveur autoriser l’installation de rôles et de fonctionnalités sur des serveurs locaux ou distants, ou sur des disques durs virtuels hors connexion (VHD). Vous pouvez installer plusieurs rôles et fonctionnalités sur un seul serveur distant ou disque dur virtuel hors connexion dans un seul Assistant Ajout de rôles et de fonctionnalités ou à une session Windows PowerShell.  
  
> [!IMPORTANT]  
> Le Gestionnaire de serveur ne peut pas être utilisé pour gérer une version plus récente du système d’exploitation Windows Server. Gestionnaire de serveur s’exécutant sur Windows Server 2012 R2 ou Windows 8.1 ne peuvent pas être utilisés pour installer des rôles, des services de rôle et des fonctionnalités sur des serveurs qui exécutent Windows Server 2016.  
  
Vous devez être connecté à un serveur en tant qu’administrateur pour installer ou désinstaller des rôles, des services de rôle et des fonctionnalités. Si vous êtes connecté à l’ordinateur local avec un compte sans droits d’administrateur sur votre serveur cible, cliquez avec le bouton droit sur le serveur cible dans la vignette **Serveurs**, puis cliquez sur **Gérer en tant que** pour préciser un compte doté de droits d’administrateur. Le serveur sur lequel vous voulez monter un disque dur virtuel hors connexion doit être ajouté au Gestionnaire de serveur et vous devez avoir des droits d’administrateur sur le serveur.  
  
Pour plus d’informations sur les rôles, les services de rôle et les fonctionnalités, voir [rôles, services de rôle et fonctionnalités](https://go.microsoft.com/fwlink/p/?LinkId=239558).  
  
Cette rubrique contient les sections suivantes.  
  
-   [Installer des rôles, des services de rôle et des fonctionnalités à l’aide de l’Assistant Ajout de rôles et de fonctionnalités](#install-roles-role-services-and-features-by-using-the-add-roles-and-features-wizard)  
  
-   [Installer des rôles, des services de rôle et des fonctionnalités à l’aide des applets de commande Windows PowerShell](#install-roles-role-services-and-features-by-using-windows-powershell-cmdlets)  
  
-   [Supprimer des rôles, des services de rôle et des fonctionnalités à l’aide de l’Assistant Suppression de rôles et de fonctionnalités](#remove-roles-role-services-and-features-by-using-the-remove-roles-and-features-wizard)  
  
-   [Supprimer des rôles, des services de rôle et des fonctionnalités à l’aide des applets de commande Windows PowerShell](#remove-roles-role-services-and-features-by-using-windows-powershell-cmdlets)  
  
-   [Installer des rôles et des fonctionnalités sur plusieurs serveurs à l’aide d’un script Windows PowerShell](#install-roles-and-features-on-multiple-servers-by-running-a-windows-powershell-script)  
  
-   [Installer le .NET Framework 3.5 et d’autres fonctionnalités à la demande](#install-net-framework-35-and-other-features-on-demand)  
  
## <a name="install-roles-role-services-and-features-by-using-the-add-roles-and-features-wizard"></a>Installer des rôles, des services de rôle et des fonctionnalités à l’aide de l’Assistant Ajout de rôles et de fonctionnalités  
Dans une session unique de l’Assistant Ajout de rôles et de fonctionnalités, vous pouvez installer des rôles, des services de rôle et des fonctionnalités sur le serveur local, un serveur distant qui a été ajouté à Gestionnaire de serveur ou un disque dur virtuel hors connexion. Pour plus d’informations sur l’ajout d’un serveur à Gestionnaire de serveur à gérer, consultez [Ajouter des serveurs à des gestionnaire de serveur](add-servers-to-server-manager.md).  
  
> [!NOTE]  
> Si vous exécutez Gestionnaire de serveur sur Windows Server 2016 ou Windows 10, vous pouvez utiliser l’Assistant Ajout de rôles et de fonctionnalités pour installer des rôles et fonctionnalités uniquement sur les serveurs et les disques durs virtuels hors connexion qui exécutent Windows Server 2016.  
  
#### <a name="to-install-roles-and-features-by-using-the-add-roles-and-features-wizard"></a>Pour installer des rôles et des fonctionnalités à l’aide de l’Assistant Ajout de rôles et de fonctionnalités  
  
1.  Si le Gestionnaire de serveur est déjà ouvert, passez à l’étape suivante. S’il n’est pas déjà ouvert, ouvrez-le en effectuant l’une des opérations suivantes.  
  
    -   Sur le Bureau Windows, démarrez le Gestionnaire de serveur en cliquant sur **Gestionnaire de serveur** dans la barre des tâches Windows.  
  
    -   Sur l’écran d' **Accueil** de Windows, cliquez sur la vignette **Gestionnaire de serveur** .  
  
2.  Dans le menu **gérer** , cliquez sur **Ajouter des rôles et des fonctionnalités**.  
  
3.  Dans la page **Avant de commencer** , vérifiez que votre serveur de destination et environnement réseau sont préparés pour le rôle et la fonctionnalité que vous voulez installer. Cliquez sur **Suivant**.  
  
4.  Dans la page **Sélectionner le type d’installation** , sélectionnez **Installation basée sur un rôle ou une fonctionnalité** pour installer toutes les parties de rôles et fonctionnalités sur un même serveur ou **Installation des services Bureau à distance** pour installer une infrastructure de bureaux basée sur des ordinateurs virtuels ou sur une session pour les services Bureau à distance. L’option **Installation des services Bureau à distance** distribue les parties logiques du rôle des services Bureau à distance entre différents serveurs, comme en ont besoin les administrateurs. Cliquez sur **Suivant**.  
  
5.  Dans la page **Sélectionner le serveur de destination**, sélectionnez un serveur dans le pool de serveurs ou sélectionnez un disque dur virtuel hors connexion. Pour sélectionner un disque dur virtuel hors connexion en guise de serveur de destination, choisissez d’abord le serveur sur lequel monter le disque dur virtuel, puis sélectionnez le fichier VHD. Pour plus d’informations sur l’ajout de serveurs à votre pool de serveurs, voir [Ajouter des serveurs à des gestionnaire de serveur](add-servers-to-server-manager.md). Une fois que vous avez sélectionné le serveur de destination, cliquez sur **Suivant**.  
  
    > [!NOTE]  
    > Pour installer des rôles et des fonctionnalités sur des disques durs virtuels hors connexion, les disques durs virtuels hors connexion cibles doivent répondre aux exigences suivantes.  
    >   
    > -   Les disques durs virtuels doivent exécuter la version de Windows Server qui correspond à la version de Gestionnaire de serveur que vous exécutez. Consultez la note au début de la section [installer des rôles, des services de rôle et des fonctionnalités à l’aide de l’Assistant Ajout de rôles et de fonctionnalités](#install-roles-role-services-and-features-by-using-the-add-roles-and-features-wizard).  
    > -   Ils ne peuvent pas posséder plus d’un volume ou d’une partition système.  
    > -   Le dossier réseau partagé dans lequel le disque dur virtuel hors connexion est stocké doit accorder les droits d’accès suivants au compte d’ordinateur (ou système local) du serveur que vous avez sélectionné pour le montage du disque dur virtuel. L’accès au compte d’utilisateur uniquement ne suffit pas. Le partage peut accorder les autorisations **Lire** et **Écrire** au groupe **Tout le monde** pour autoriser l’accès au disque dur virtuel, mais pour des raisons de sécurité, cela n’est pas recommandé.  
    >   
    >     -   Accès **Lecture/écriture** dans la boîte de dialogue **Partage de fichiers**.  
    >     -   Accès **contrôle total** dans l’onglet **sécurité** de la boîte de dialogue **Propriétés** du fichier ou du dossier.  
  
6.  Sélectionnez des rôles, sélectionnez des services de rôle pour le rôle, le cas échéant, puis cliquez sur **Suivant** pour sélectionner des fonctionnalités.  
  
    Au fur et à mesure que vous continuez, l’Assistant Ajout de rôles et de fonctionnalités vous informe automatiquement si des conflits ont été détectés sur le serveur de destination et peuvent empêcher des rôles ou des fonctionnalités sélectionnés d’être installés ou de fonctionner normalement. Le système vous demande également d’ajouter tous les rôles, services de rôle ou fonctionnalités indispensables aux rôles ou fonctionnalités que vous avez sélectionnés.  
  
    En outre, si vous prévoyez de gérer le rôle à distance, d'un autre serveur ou d'un ordinateur client Windows qui exécute les outils d'administration de serveur distant, vous pouvez choisir de ne pas installer les outils de gestion et les composants logiciels enfichables pour les rôles sur le serveur de destination. Par défaut, dans l’Assistant Ajout de rôles et de fonctionnalités, les outils de gestion sont sélectionnés pour l’installation.  
  
7.  Dans la page **Confirmer les sélections d’installation**, passez en revue vos sélections de rôle, fonctionnalité et serveur. Si vous êtes prêt à effectuer l’installation, cliquez sur **Installer**.  
  
    Vous pouvez également exporter vos sélections dans un fichier de configuration XML que vous pouvez utiliser pour les installations sans assistance avec Windows PowerShell. Pour exporter la configuration que vous avez spécifiée dans cette session de l’Assistant Ajout de rôles et de fonctionnalités, cliquez sur **Exporter les paramètres de configuration**, puis enregistrez le fichier XML à un emplacement approprié.  
  
    La commande **Spécifier un autre chemin d’accès source** dans la page **Confirmer les sélections d’installation** vous permet de spécifier un autre chemin d’accès source pour les fichiers qui sont requis pour installer les rôles et les fonctionnalités sur le serveur sélectionné. Dans Windows Server 2012 et versions ultérieures de Windows Server, les [fonctionnalités à la demande](https://go.microsoft.com/fwlink/p/?LinkID=241573) vous permettent de réduire la quantité d’espace disque utilisée par le système d’exploitation, en supprimant les fichiers de rôles et de fonctionnalités des serveurs qui sont gérés exclusivement à distance. Si vous avez supprimé les fichiers des rôles et des fonctionnalités d’un serveur à l’aide de l’applet de commande `Uninstall-WindowsFeature -remove` , vous pouvez installer des rôles et des fonctionnalités sur le serveur dans le futur en spécifiant un autre chemin d’accès source ou un partage sur lequel les fichiers des rôles et des fonctionnalités requis sont stockés. Le chemin d’accès source ou le partage de fichiers doit accorder des autorisations de **lecture** au groupe **tout le monde** (non recommandé pour des raisons de sécurité) ou au compte d’ordinateur (*domaine*\\*nom_serveur*$) du serveur de destination ; l’octroi de l’accès au compte d’utilisateur n’est pas suffisant. Pour plus d’informations sur les fonctionnalités à la demande, voir [Options d’installation de Windows Server](https://go.microsoft.com/fwlink/p/?LinkId=241573).  
  
    Vous pouvez spécifier un fichier WIM comme autre source de fichier de fonctionnalité lorsque vous installez des rôles, des services de rôle et des fonctionnalités sur un serveur physique en cours d’exécution. Le chemin d’accès source d’un fichier WIM doit être au format suivant, avec **Wim** comme préfixe, et l’index dans lequel les fichiers de fonctionnalités se trouvent sous la forme d’un suffixe : **Wim : e : \sources\install.wim : 4**. Toutefois, vous ne pouvez pas utiliser un fichier WIM directement en tant que source pour l’installation de rôles, de services de rôle et de fonctionnalités sur un disque dur virtuel hors connexion. vous devez soit monter le disque dur virtuel hors connexion et pointer sur son chemin de montage pour les fichiers sources, soit pointer vers un dossier qui contient une copie du contenu du fichier WIM.  
  
8.  Une fois que vous avez cliqué sur **installer**, la page progression de l' **installation** affiche la progression de l’installation, les résultats et les messages tels que les avertissements, les échecs ou les étapes de configuration consécutives à l’installation requises pour les rôles ou fonctionnalités que vous ordinateur. Dans Windows Server 2012 et versions ultérieures de Windows Server, vous pouvez fermer l’Assistant Ajout de rôles et de fonctionnalités pendant que l’installation est toujours en cours et afficher les résultats de l’installation ou d’autres messages dans la zone **notifications** en haut de la gestionnaire de serveur Console. Cliquez sur l’icône de l’indicateur **notifications** pour afficher plus de détails sur les installations ou autres tâches que vous effectuez dans Gestionnaire de serveur.  
  
## <a name="install-roles-role-services-and-features-by-using-windows-powershell-cmdlets"></a>Installer des rôles, des services de rôle et des fonctionnalités à l’aide des applets de commande Windows PowerShell  
Les applets de commande de déploiement Gestionnaire de serveur pour Windows PowerShell fonctionnent de la même façon que l’Assistant Ajout de rôles et de fonctionnalités basé sur l’interface graphique utilisateur et supprime des rôles et des fonctionnalités, avec une différence importante. Dans Windows PowerShell, contrairement à l’Assistant Ajout de rôles et de fonctionnalités, les outils de gestion et les composants logiciels enfichables pour un rôle ne sont pas inclus par défaut. Pour inclure les outils de gestion dans le cadre de l’installation d’un rôle, ajoutez le paramètre `IncludeManagementTools` à l’applet de commande. Si vous installez des rôles et des fonctionnalités sur un serveur qui exécute l’option d’installation minimale de Windows Server 2012 ou des versions ultérieures, vous pouvez ajouter les outils de gestion d’un rôle à une installation, mais les composants logiciels enfichables et les outils de gestion basés sur l’interface graphique utilisateur ne peuvent pas être installés. sur les serveurs qui exécutent l’option d’installation minimale de Windows Server. Seuls les outils de gestion de la ligne de commande et de Windows PowerShell peuvent être installés sur l’option d’installation Server Core.  
  
#### <a name="to-install-roles-and-features-by-using-the-install-windowsfeature-cmdlet"></a>Pour installer des rôles et des fonctionnalités à l’aide de l’applet de commande Install-WindowsFeature  
  
1. Effectuez une des opérations suivantes pour ouvrir une session Windows PowerShell avec des droits utilisateur élevés.  
  
   > [!NOTE]  
   > Si vous installez des rôles et des fonctionnalités sur un serveur distant, vous n’avez pas besoin d’exécuter Windows PowerShell avec des droits d’utilisateur élevés.  
  
   -   Sur le Bureau Windows, cliquez avec le bouton droit dans la barre des tâches sur **Windows PowerShell** , puis cliquez sur **Exécuter en tant qu’administrateur**.  
  
   -   Sur l’écran d' **Accueil** de Windows, cliquez avec le bouton droit sur la vignette de Windows PowerShell, puis dans la barre des applications, cliquez sur **exécuter en tant qu’administrateur**.  
  
2. Tapez **Get-WindowsFeature**, puis appuyez sur **Entrée** pour afficher une liste de rôles et fonctionnalités installés et disponibles sur le serveur local. Si l’ordinateur local n’est pas un serveur ou si vous souhaitez des informations sur un serveur distant, exécutez la commande **obtenir-WindowsFeature-computerName <** <em>nom_ordinateur</em> **>** , où *nom_ordinateur* représente le nom d’un ordinateur distant qui exécute Windows Server 2016. Les résultats de l’applet de commande contiennent les noms de commande des rôles et des fonctionnalités que vous ajoutez à votre applet de commande à l’étape 4.  
  
   > [!NOTE]  
   > Dans Windows PowerShell 3,0 et les versions ultérieures de Windows PowerShell, il n’est pas nécessaire d’importer le module d’applet de commande Gestionnaire de serveur dans la session Windows PowerShell avant d’exécuter des applets de commande qui font partie du module. Un module est automatiquement importé la première fois que vous exécutez une applet de commande qui fait partie du module. De plus, ni les applets de commande Windows PowerShell ni les noms de fonctionnalités utilisés avec les applets de commande ne respectent la casse.  
  
3. tapez **obtenir-Help install-WindowsFeature**, puis appuyez sur **entrée** pour afficher la syntaxe et les paramètres acceptés pour l’applet de commande `Install-WindowsFeature`.  
  
4. tapez ce qui suit, puis appuyez sur **entrée**, où *feature_name* représente le nom de commande d’un rôle ou d’une fonctionnalité que vous souhaitez installer (obtenu à l’étape 2) et *nom_ordinateur* représente un ordinateur distant sur lequel vous voulez installer rôles et fonctionnalités. Séparez plusieurs valeurs pour *nom_fonctionnalité* à l’aide de virgules. Le paramètre `Restart` permet de redémarrer automatiquement le serveur de destination si l’installation des rôles ou des fonctionnalités le requiert.  
  
   ```  
   Install-WindowsFeature -Name <feature_name> -computerName <computer_name> -Restart  
   ```  
  
   Pour installer des rôles et fonctionnalités sur un disque dur virtuel hors connexion, ajoutez les paramètres `computerName` et `VHD` . Si vous n’ajoutez pas le paramètre `computerName` , l’applet de commande suppose que l’ordinateur local est monté pour accéder au disque dur virtuel. Le paramètre `computerName` contient le nom du serveur sur lequel monter le disque dur virtuel tandis que le paramètre `VHD` contient le chemin d’accès au fichier VHD sur le serveur spécifié.  
  
   > [!NOTE]  
   > Vous devez ajouter le paramètre `computerName` si vous exécutez l’applet de commande à partir d’un ordinateur qui exécute un système d’exploitation client Windows.  
   >   
   > Pour installer des rôles et des fonctionnalités sur des disques durs virtuels hors connexion, les disques durs virtuels hors connexion cibles doivent répondre aux exigences suivantes.  
   >   
   > -   Les disques durs virtuels doivent exécuter la version de Windows Server qui correspond à la version de Gestionnaire de serveur que vous exécutez. Consultez la note au début de la section [installer des rôles, des services de rôle et des fonctionnalités à l’aide de l’Assistant Ajout de rôles et de fonctionnalités](#install-roles-role-services-and-features-by-using-the-add-roles-and-features-wizard).  
   > -   Ils ne peuvent pas posséder plus d’un volume ou d’une partition système.  
   > -   Le dossier réseau partagé dans lequel le disque dur virtuel hors connexion est stocké doit accorder les droits d’accès suivants au compte d’ordinateur (ou système local) du serveur que vous avez sélectionné pour le montage du disque dur virtuel. L’accès au compte d’utilisateur uniquement ne suffit pas. Le partage peut accorder les autorisations **Lire** et **Écrire** au groupe **Tout le monde** pour autoriser l’accès au disque dur virtuel, mais pour des raisons de sécurité, cela n’est pas recommandé.  
   >   
   >     -   Accès **Lecture/écriture** dans la boîte de dialogue **Partage de fichiers**.  
   >     -   Accès **contrôle total** dans l’onglet **sécurité** de la boîte de dialogue **Propriétés** du fichier ou du dossier.  
  
   ```  
   Install-WindowsFeature -Name <feature_name> -VHD <path> -computerName <computer_name> -Restart  
   ```  
  
   **Tels** L’applet de commande suivante installe le rôle Services de domaine Active Directory et la fonctionnalité de gestion de stratégie de groupe sur un serveur distant, nommé contosodc1. Les outils de gestion et les composants logiciels enfichables sont ajoutés à l’aide du paramètre `IncludeManagementTools` et le serveur de destination doit être redémarré automatiquement si l’installation exige un redémarrage des serveurs.  
  
   ```  
   Install-WindowsFeature -Name AD-Domain-Services,GPMC -computerName ContosoDC1 -IncludeManagementTools -Restart  
   ```  
  
5. Une fois l’installation terminée, vérifiez l’installation en ouvrant la page **tous les serveurs** dans Gestionnaire de serveur, en sélectionnant un serveur sur lequel vous avez installé des rôles et des fonctionnalités, puis en affichant la vignette **rôles et fonctionnalités** sur la page du serveur sélectionné. Vous pouvez également exécuter l’applet de commande `Get-WindowsFeature` ciblée sur le serveur sélectionné (obtenir-WindowsFeature-computerName <*nom_ordinateur*>) pour afficher la liste des rôles et fonctionnalités installés sur le serveur.  
  
## <a name="remove-roles-role-services-and-features-by-using-the-remove-roles-and-features-wizard"></a>Supprimer des rôles, des services de rôle et des fonctionnalités à l’aide de l’Assistant Suppression de rôles et de fonctionnalités  
Vous devez être connecté à un serveur en tant qu’administrateur pour pouvoir désinstaller des rôles, des services de rôle et des fonctionnalités. Si vous êtes connecté à l’ordinateur local avec un compte sans droits d’administrateur sur votre serveur cible de désinstallation, cliquez avec le bouton droit sur le serveur cible dans la vignette **Serveurs**, puis cliquez sur **Gérer en tant que** pour préciser un compte doté de droits d’administrateur. Le serveur sur lequel vous voulez monter un disque dur virtuel hors connexion doit être ajouté au Gestionnaire de serveur et vous devez avoir des droits d’administrateur sur le serveur.  
  
#### <a name="to-remove-roles-and-features-by-using-the-remove-roles-and-features-wizard"></a>Pour supprimer des rôles et des fonctionnalités à l’aide de l’Assistant Suppression de rôles et de fonctionnalités  
  
1.  Si le Gestionnaire de serveur est déjà ouvert, passez à l’étape suivante. S’il n’est pas déjà ouvert, ouvrez-le en effectuant l’une des opérations suivantes.  
  
    -   Sur le Bureau Windows, démarrez le Gestionnaire de serveur en cliquant sur **Gestionnaire de serveur** dans la barre des tâches Windows.  
  
    -   Dans l’**écran d’accueil** de Windows, cliquez sur la vignette **Gestionnaire de serveur**.  
  
2.  Dans le menu **Gérer**, cliquez sur **Supprimer des rôles et fonctionnalités**.  
  
3.  Dans la page **Avant de commencer**, vérifiez que vous avez préparé la suppression de rôles ou de fonctionnalités sur un serveur. Cliquez sur **Suivant**.  
  
4.  Dans la page **Sélectionner le serveur de destination** , sélectionnez un serveur à partir du pool de serveurs ou sélectionnez un disque dur virtuel hors connexion. Pour sélectionner un disque dur virtuel hors connexion, choisissez d’abord le serveur sur lequel monter le disque dur virtuel, puis sélectionnez le fichier VHD.  
  
    > [!NOTE]  
    > Le dossier réseau partagé dans lequel le disque dur virtuel hors connexion est stocké doit accorder les droits d’accès suivants au compte d’ordinateur (ou système local) du serveur que vous avez sélectionné pour le montage du disque dur virtuel. L’accès au compte d’utilisateur uniquement ne suffit pas. Le partage peut accorder les autorisations **Lire** et **Écrire** au groupe **Tout le monde** pour autoriser l’accès au disque dur virtuel, mais pour des raisons de sécurité, cela n’est pas recommandé.  
    >   
    > -   Accès **Lecture/écriture** dans la boîte de dialogue **Partage de fichiers**.  
    > -   Accès **Contrôle total** dans l’onglet **Sécurité** de la boîte de dialogue **Propriétés** d’un fichier ou d’un dossier.  
  
    Pour plus d’informations sur l’ajout de serveurs à votre pool de serveurs, voir [Ajouter des serveurs à des gestionnaire de serveur](add-servers-to-server-manager.md). Une fois que vous avez sélectionné le serveur de destination, cliquez sur **Suivant**.  
  
    > [!NOTE]  
    > Vous pouvez utiliser l’Assistant Suppression de rôles et de fonctionnalités pour supprimer des rôles et des fonctionnalités des serveurs qui exécutent la même version de Windows Server qui prend en charge la version de Gestionnaire de serveur que vous utilisez. Vous ne pouvez pas supprimer des rôles, des services de rôle ou des fonctionnalités des serveurs qui exécutent Windows Server 2016, si vous exécutez Gestionnaire de serveur sur Windows Server 2012 R2, Windows Server 2012 ou Windows 8. Vous ne pouvez pas utiliser l’Assistant Suppression de rôles et de fonctionnalités pour supprimer des rôles et des fonctionnalités des serveurs qui exécutent Windows Server 2008 ou Windows Server 2008 R2.  
  
5.  Sélectionnez des rôles, sélectionnez des services de rôle pour le rôle, le cas échéant, puis cliquez sur **Suivant** pour sélectionner des fonctionnalités.  
  
    Lorsque vous continuez, l’Assistant Suppression de rôles et de fonctionnalités vous invite automatiquement à supprimer les rôles, services de rôle ou fonctionnalités qui ne peuvent pas s’exécuter sans les rôles ou fonctionnalités que vous supprimez.  
  
    en outre, vous pouvez choisir de supprimer les outils de gestion et les composants logiciels enfichables pour les rôles sur le serveur de destination. Par défaut, dans l’Assistant Suppression de rôles et de fonctionnalités, les outils de gestion sont sélectionnés pour être supprimés. Vous pouvez ignorer les outils de gestion et les composants logiciels enfichables si vous comptez utiliser le serveur sélectionné pour gérer le rôle sur d’autres serveurs distants.  
  
6.  Dans la page **Confirmer les sélections pour la suppression**, passez en revue vos sélections de rôle, fonctionnalité et serveur. Si vous êtes prêt à supprimer les rôles ou les fonctionnalités, cliquez sur **supprimer**.  
  
7.  Une fois que vous avez cliqué sur **supprimer**, la page progression de la **suppression** affiche la progression de la suppression, les résultats et les messages tels que les avertissements, les échecs ou les étapes de configuration postérieures à la suppression nécessaires, tels que le redémarrage du serveur de destination. Dans Windows Server 2012 et versions ultérieures de Windows Server, vous pouvez fermer l’Assistant Suppression de rôles et de fonctionnalités pendant que la suppression est toujours en cours et afficher les résultats de la suppression ou d’autres messages dans la zone **notifications** en haut de la console Gestionnaire de serveur. Cliquez sur l’indicateur **notifications** pour afficher plus de détails sur les suppressions ou d’autres tâches que vous effectuez dans Gestionnaire de serveur.  
  
## <a name="remove-roles-role-services-and-features-by-using-windows-powershell-cmdlets"></a>Supprimer des rôles, des services de rôle et des fonctionnalités à l’aide des applets de commande Windows PowerShell  
Les applets de commande de déploiement Gestionnaire de serveur pour Windows PowerShell fonctionnent de la même façon que l’Assistant Suppression de rôles et de fonctionnalités basé sur l’interface graphique utilisateur, avec une différence importante. Dans Windows PowerShell, contrairement à l’Assistant Suppression de rôles et de fonctionnalités, les outils de gestion et les composants logiciels enfichables pour un rôle ne sont pas supprimés par défaut. Pour supprimer les outils de gestion dans le cadre de la suppression d’un rôle, ajoutez le paramètre `IncludeManagementTools` à l’applet de commande. Si vous désinstallez des rôles et des fonctionnalités à partir d’un serveur qui exécute l’option d’installation minimale de Windows Server 2012 ou d’une version ultérieure de Windows Server, ce paramètre supprime les outils de gestion de ligne de commande et de Windows PowerShell pour le spécifié. rôles et fonctionnalités.  
  
#### <a name="to-remove-roles-and-features-by-using-the-uninstall-windowsfeature-cmdlet"></a>Pour supprimer des rôles et des fonctionnalités à l’aide de l’applet de commande Uninstall-WindowsFeature  
  
1. Effectuez une des opérations suivantes pour ouvrir une session Windows PowerShell avec des droits utilisateur élevés.  
  
   > [!NOTE]  
   > Si vous désinstallez des rôles et des fonctionnalités à partir d’un serveur distant, vous n’avez pas besoin d’exécuter Windows PowerShell avec des droits d’utilisateur élevés.  
  
   -   Sur le Bureau Windows, cliquez avec le bouton droit dans la barre des tâches sur **Windows PowerShell** , puis cliquez sur **Exécuter en tant qu’administrateur**.  
  
   -   Sur l’écran d' **Accueil** de Windows, cliquez avec le bouton droit sur la vignette Windows PowerShell, puis dans la barre des applications, cliquez sur **exécuter en tant qu’administrateur**.  
  
2. Tapez **Get-WindowsFeature**, puis appuyez sur **Entrée** pour afficher une liste de rôles et fonctionnalités installés et disponibles sur le serveur local. Si l’ordinateur local n’est pas un serveur ou si vous souhaitez des informations sur un serveur distant, exécutez la commande **obtenir-WindowsFeature-computerName <** <em>nom_ordinateur</em> **>** , où *nom_ordinateur* représente le nom d’un ordinateur distant qui exécute Windows Server 2016. Les résultats de l’applet de commande contiennent les noms de commande des rôles et des fonctionnalités que vous ajoutez à votre applet de commande à l’étape 4.  
  
   > [!NOTE]  
   > Dans Windows PowerShell 3,0 et les versions ultérieures de Windows PowerShell, il n’est pas nécessaire d’importer le module d’applet de commande Gestionnaire de serveur dans la session Windows PowerShell avant d’exécuter des applets de commande qui font partie du module. Un module est automatiquement importé la première fois que vous exécutez une applet de commande qui fait partie du module. De plus, ni les applets de commande Windows PowerShell ni les noms de fonctionnalités utilisés avec les applets de commande ne respectent la casse.  
  
3. tapez **obtenir-Help Uninstall-WindowsFeature**, puis appuyez sur **entrée** pour afficher la syntaxe et les paramètres acceptés pour l’applet de commande `Uninstall-WindowsFeature`.  
  
4. Tapez ce qui suit, puis appuyez sur **Entrée**, où *feature_name* représente le nom de commande d’un rôle ou d’une fonctionnalité que vous souhaitez supprimer (obtenu à l’étape 2) et *computer_name* représente un ordinateur distant duquel vous souhaitez supprimer des rôles et fonctionnalités. Séparez plusieurs valeurs pour *nom_fonctionnalité* à l’aide de virgules. Le paramètre `Restart` permet de redémarrer automatiquement les serveurs de destination si la suppression des rôles ou des fonctionnalités le requiert.  
  
   ```  
   Uninstall-WindowsFeature -Name <feature_name> -computerName <computer_name> -Restart  
   ```  
  
   Pour supprimer des rôles et fonctionnalités d’un disque dur virtuel hors connexion, ajoutez les paramètres `computerName` et `VHD` . Si vous n’ajoutez pas le paramètre `computerName` , l’applet de commande suppose que l’ordinateur local est monté pour accéder au disque dur virtuel. Le paramètre `computerName` contient le nom du serveur sur lequel monter le disque dur virtuel tandis que le paramètre `VHD` contient le chemin d’accès au fichier VHD sur le serveur spécifié.  
  
   > [!NOTE]  
   > Vous devez ajouter le paramètre `computerName` si vous exécutez l’applet de commande à partir d’un ordinateur qui exécute un système d’exploitation client Windows.  
   >   
   > Le dossier réseau partagé dans lequel le disque dur virtuel hors connexion est stocké doit accorder les droits d’accès suivants au compte d’ordinateur (ou système local) du serveur que vous avez sélectionné pour le montage du disque dur virtuel. L’accès au compte d’utilisateur uniquement ne suffit pas. Le partage peut accorder les autorisations **Lire** et **Écrire** au groupe **Tout le monde** pour autoriser l’accès au disque dur virtuel, mais pour des raisons de sécurité, cela n’est pas recommandé.  
   >   
   > -   Accès **Lecture/écriture** dans la boîte de dialogue **Partage de fichiers**.  
   > -   Accès **contrôle total** dans l’onglet **sécurité** de la boîte de dialogue **Propriétés** du fichier ou du dossier.  
  
   ```  
   Uninstall-WindowsFeature -Name <feature_name> -VHD <path> -computerName <computer_name> -Restart  
   ```  
  
   **Tels** L’applet de commande suivante supprime le rôle Services de domaine Active Directory et la fonctionnalité de gestion de stratégie de groupe d’un serveur distant, nommé contosodc1. Les outils de gestion et les composants logiciels enfichables sont également supprimés et le serveur de destination doit être redémarré automatiquement si la suppression exige un redémarrage des serveurs.  
  
   ```  
   Uninstall-WindowsFeature -Name AD-Domain-Services,GPMC -computerName ContosoDC1 -IncludeManagementTools -Restart  
   ```  
  
5. Une fois la suppression terminée, vérifiez que les rôles et les fonctionnalités sont supprimés en ouvrant la page **tous les serveurs** dans Gestionnaire de serveur, en sélectionnant le serveur à partir duquel vous avez supprimé des rôles et des fonctionnalités, puis en affichant la vignette **rôles et fonctionnalités** sur la page du serveur sélectionné. Vous pouvez également exécuter l’applet de commande `Get-WindowsFeature` ciblée sur le serveur sélectionné (obtenir-WindowsFeature-computerName <*nom_ordinateur*>) pour afficher la liste des rôles et fonctionnalités installés sur le serveur.  
  
## <a name="install-roles-and-features-on-multiple-servers-by-running-a-windows-powershell-script"></a>Installer des rôles et des fonctionnalités sur plusieurs serveurs à l’aide d’un script Windows PowerShell  
Bien que vous ne puissiez pas utiliser l’Assistant Ajout de rôles et de fonctionnalités pour installer des rôles, des services de rôle et des fonctionnalités sur plusieurs serveurs cibles dans une session d’Assistant unique, vous pouvez utiliser un script Windows PowerShell pour installer des rôles, des services de rôle et des fonctionnalités sur plusieurs cibles. serveurs que vous gérez à l’aide de Gestionnaire de serveur. Le script que vous utilisez pour effectuer un déploiement par lots, comme ce processus est appelé, pointe vers un fichier de configuration XML que vous pouvez créer facilement à l’aide de l’Assistant Ajout de rôles et de fonctionnalités, puis en cliquant sur **Exporter les paramètres de configuration** après avoir progressé dans la sur la page **confirmer les sélections d’installation** de l’Assistant Ajout de rôles et de fonctionnalités.  
  
> [!IMPORTANT]  
> Tous les serveurs cibles spécifiés dans votre script doivent exécuter la version de Windows Server qui correspond à la version de Gestionnaire de serveur que vous exécutez sur l’ordinateur local. Par exemple, si vous exécutez Gestionnaire de serveur sur Windows 10, vous pouvez installer des rôles, des services de rôle et des fonctionnalités sur des serveurs qui exécutent Windows Server 2016. Si des outils de gestion basés sur l’interface graphique utilisateur sont ajoutés à l’installation, le processus d’installation convertit automatiquement les serveurs cibles qui exécutent l’option d’installation Server Core de Windows Server vers l’option d’installation complète (serveur avec une interface utilisateur graphique complète, également connue sous le nom en tant qu’interpréteur de commandes graphique de serveur en cours d’exécution).  
>   
> Le script fourni dans cette section est un exemple de la façon dont le déploiement par lots peut être effectué à l’aide de l’applet de commande `Install-WindowsFeature` et d’un script Windows PowerShell. Il existe d’autres scripts et méthodes possibles pour le déploiement par lots sur plusieurs serveurs. Pour rechercher ou proposer d’autres scripts pour le déploiement des rôles et des fonctionnalités, effectuez une recherche dans le [référentiel du Centre de scripts](https://gallery.technet.microsoft.com/ScriptCenter).  
  
#### <a name="to-install-roles-and-features-on-multiple-servers"></a>Pour installer des rôles et des fonctionnalités sur plusieurs servers  
  
1.  Si vous ne l’avez pas encore fait, créez un fichier de configuration XML qui contient les rôles, les services de rôle et les fonctionnalités que vous souhaitez installer sur plusieurs serveurs. Vous pouvez créer ce fichier de configuration en exécutant l’Assistant Ajout de rôles et de fonctionnalités, en sélectionnant les rôles, les services de rôle et les fonctionnalités de votre choix, puis en cliquant sur **Exporter les paramètres de configuration** après avoir parcouru l’Assistant jusqu’à la **confirmation page sélections d’installation** . Enregistrez le fichier de configuration à un endroit pratique. Vous n’avez pas besoin de cliquer sur **Installer** ni de mener à terme les étapes de l’assistant si vous l’exécutez dans le seul but de créer un fichier de configuration.  
  
2.  Effectuez une des opérations suivantes pour ouvrir une session Windows PowerShell avec des droits utilisateur élevés.  
  
    -   Sur le Bureau Windows, cliquez avec le bouton droit dans la barre des tâches sur **Windows PowerShell** , puis cliquez sur **Exécuter en tant qu’administrateur**.  
  
    -   Sur l’écran d' **Accueil** de Windows, cliquez avec le bouton droit sur la vignette Windows PowerShell, puis dans la barre des applications, cliquez sur **exécuter en tant qu’administrateur**.  
  
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
  
        **Invoke-WindowsFeatureBatchDeployment-computerNames $ServerNames-ConfigurationFilepath C:\Users\Sampleuser\Desktop\DeploymentConfigTemplate.xml**  
  
5.  Une fois l’installation terminée, vérifiez l’installation en ouvrant la page **tous les serveurs** dans Gestionnaire de serveur, en sélectionnant un serveur sur lequel vous avez installé des rôles et des fonctionnalités, puis en affichant la vignette **rôles et fonctionnalités** sur la page du serveur sélectionné. Vous pouvez également exécuter l’applet de commande `Get-WindowsFeature` ciblant un serveur spécifique (`Get-WindowsFeature -computerName` @ no__t-2*nom_ordinateur*>) pour afficher la liste des rôles et fonctionnalités installés sur le serveur.  
  
## <a name="install-net-framework-35-and-other-features-on-demand"></a>Installer le .NET Framework 3.5 et d’autres fonctionnalités à la demande  
à compter de Windows Server 2012 et Windows 8, les fichiers de fonctionnalités de .NET Framework 3,5 (qui incluent .NET Framework 2,0 et .NET Framework 3,0) ne sont pas disponibles sur l’ordinateur local par défaut. Les fichiers ont été supprimés. Les fichiers de fonctionnalités qui ont été supprimés dans une configuration Fonctionnalités à la demande, ainsi que les fichiers de fonctionnalités pour le .NET Framework 3.5, sont disponibles par le biais de Windows Update. Par défaut, si les fichiers de fonctionnalités ne sont pas disponibles sur le serveur de destination qui exécute Windows Server 2012 ou versions ultérieures, le processus d’installation recherche les fichiers manquants en se connectant à Windows Update. Vous pouvez remplacer le comportement par défaut en configurant un paramètre stratégie de groupe ou en spécifiant un autre chemin d’accès source au cours de l’installation, que vous installiez à l’aide de l’interface utilisateur graphique de l’Assistant Ajout de rôles et de fonctionnalités ou d’une ligne de commande.  
  
Vous pouvez installer le .NET Framework 3.5 en effectuant l’une des opérations suivantes :  
  
-   Utilisez [Pour installer le .NET Framework 3.5 à l’aide de l’applet de commande Install-WindowsFeature](#to-install-net-framework-35-by-running-the-install-windowsfeature-cmdlet) pour ajouter le paramètre `Source` et spécifiez une source à partir de laquelle obtenir les fichiers de fonctionnalités .NET Framework 3.5. Si vous n’ajoutez pas le paramètre `Source`, le processus d’installation détermine d’abord si un chemin d’accès aux fichiers de fonctionnalités a été spécifié par des paramètres de stratégie de groupe et, si aucun chemin d’accès n’est trouvé, il recherche les fonctionnalités manquantes à l’aide de Windows Update.  
  
-   Utilisez [pour installer .NET Framework 3,5 à l’aide de l’Assistant Ajout de rôles et de fonctionnalités](#to-install-net-framework-35-by-using-the-add-roles-and-features-wizard) pour spécifier un autre emplacement de fichier source dans la page **confirmer les options d’installation** de l’Assistant Ajout de rôles et de fonctionnalités.  
  
-   Appliquez la procédure [Pour installer le .NET Framework 3.5 à l’aide de DISM](#to-install-net-framework-35-by-using-dism) pour obtenir des fichiers à partir de Windows Update par défaut ou en spécifiant un chemin d’accès source vers le support d’installation.  
  
[Configurez d’autres sources pour les fichiers de fonctionnalités dans la stratégie de groupe](#configure-alternate-sources-for-feature-files-in-group-policy) pour le .NET Framework 3.5 ou d’autres fonctionnalités, si les fichiers de fonctionnalités sont introuvables sur l’ordinateur local.  
  
> [!IMPORTANT]  
> Lors de l’installation des fichiers de fonctionnalités à partir d’une source distante, le chemin d’accès source ou partage de fichiers doit accorder des autorisations de **Lecture** au groupe **Tout le monde** (non recommandé pour des raisons de sécurité) ou au compte d’ordinateur (système local) du serveur de destination ; l’accord d’un accès au compte d’utilisateur n’est pas suffisant.  
>   
> Les serveurs qui se trouvent dans des groupes de travail ne peuvent pas accéder aux partages de fichiers externes, même si le compte d’ordinateur pour le serveur de groupe de travail a des autorisations de **Lecture** sur le partage externe. D’autres emplacements sources qui fonctionnent pour les serveurs de groupes de travail sont notamment les supports d’installation, Windows Update, et les disques durs virtuels ou les fichiers WIM qui sont stockés sur le serveur de groupe de travail local.  
>   
> Vous pouvez spécifier un fichier WIM comme autre source de fichier de fonctionnalité lorsque vous installez des rôles, des services de rôle et des fonctionnalités sur un serveur physique en cours d’exécution. Le chemin d’accès source d’un fichier WIM doit être au format suivant, avec **Wim** comme préfixe, et l’index dans lequel les fichiers de fonctionnalités se trouvent sous la forme d’un suffixe : **Wim : e : \sources\install.wim : 4**. Toutefois, vous ne pouvez pas utiliser un fichier WIM directement en tant que source pour l’installation de rôles, de services de rôle et de fonctionnalités sur un disque dur virtuel hors connexion. vous devez soit monter le disque dur virtuel hors connexion et pointer sur son chemin de montage pour les fichiers sources, soit pointer vers un dossier qui contient une copie du contenu du fichier WIM.  
  
### <a name="to-install-net-framework-35-by-running-the-install-windowsfeature-cmdlet"></a>Pour installer le .NET Framework 3.5 à l’aide de l’applet de commande Install-WindowsFeature  
  
1.  Effectuez une des opérations suivantes pour ouvrir une session Windows PowerShell avec des droits utilisateur élevés.  
  
    > [!NOTE]  
    > Si vous installez des rôles et des fonctionnalités à partir d’un serveur distant, vous n’avez pas besoin d’exécuter Windows PowerShell avec des droits d’utilisateur élevés.  
  
    -   Sur le Bureau Windows, cliquez avec le bouton droit dans la barre des tâches sur **Windows PowerShell** , puis cliquez sur **Exécuter en tant qu’administrateur**.  
  
    -   Sur l’écran d' **Accueil** de Windows, cliquez avec le bouton droit sur la vignette Windows PowerShell, puis dans la barre des applications, cliquez sur **exécuter en tant qu’administrateur**.  
  
    -   Sur un serveur qui exécute l’option d’installation minimale de Windows Server 2012 R2 ou Windows Server 2012, tapez **PowerShell** dans une invite de commandes, puis appuyez sur **entrée**.  
  
2.  tapez la commande suivante, puis appuyez sur **entrée**. Dans l’exemple suivant, les fichiers sources se trouvent dans un magasin côte à côte (abrégé en **SxS**) sur un support d’installation sur le disque D.  
  
    ```  
    Install-WindowsFeature NET-Framework-Core -Source D:\Sources\SxS  
    ```  
  
    Si vous voulez que la commande utilise Windows Update comme source pour les fichiers de fonctionnalités manquants ou si une source par défaut a déjà été configurée par la stratégie de groupe, il est inutile d’ajouter le paramètre `Source` , à moins que vous ne souhaitiez spécifier une source différente.  
  
### <a name="to-install-net-framework-35-by-using-the-add-roles-and-features-wizard"></a>Pour installer .NET Framework 3,5 à l’aide de l’Assistant Ajout de rôles et de fonctionnalités  
  
1. Dans le menu **gérer** de gestionnaire de serveur, cliquez sur **Ajouter des rôles et des fonctionnalités**.  
  
2. Sélectionnez un serveur de destination qui exécute Windows Server 2016.  
  
3. Dans la page **Sélectionner des fonctionnalités** de l’Assistant Ajout de rôles et de fonctionnalités, sélectionnez **.NET Framework 3,5**.  
  
4. Si l’ordinateur local y est autorisé par les paramètres de stratégie de groupe, le processus d’installation tente d’obtenir les fichiers de fonctionnalités manquants à l’aide de Windows Update. Cliquez sur **Installer** ; inutile de passer à l’étape suivante.  
  
   Si stratégie de groupe paramètres n’autorisent pas cette opération ou si vous souhaitez utiliser une autre source pour les fichiers de fonctionnalités .NET Framework 3,5, dans la page **confirmer les sélections d’installation** de l’Assistant, cliquez sur **spécifier un autre chemin d’accès source**.  
  
5. Spécifiez un chemin d’accès à un magasin côte à côte (appelé **SxS**) sur un support d’installation ou un chemin d’accès à un fichier WIM. Dans l’exemple suivant, le support d’installation se trouve sur le disque D.  
  
   **D:\Sources\SxS @ no__t-1**  
  
   Pour spécifier un fichier WIM, ajoutez un préfixe  **WIM:** et ajoutez l’index de l’image à utiliser dans le fichier WIM comme suffixe, comme indiqué dans l’exemple suivant.  
  
   **Wim : \\ @ no__t-2**<em>nom_serveur</em> **\share\install.wim : 3**  
  
6. Cliquez sur **OK**, puis sur **Installer**.  
  
### <a name="to-install-net-framework-35-by-using-dism"></a>Pour installer le .NET Framework 3.5 à l’aide de DISM  
  
1.  Effectuez une des opérations suivantes pour ouvrir une session Windows PowerShell avec des droits utilisateur élevés.  
  
    > [!NOTE]  
    > Si vous installez des rôles et des fonctionnalités à partir d’un serveur distant, vous n’avez pas besoin d’exécuter Windows PowerShell avec des droits d’utilisateur élevés.  
  
    -   Sur le Bureau Windows, cliquez avec le bouton droit dans la barre des tâches sur **Windows PowerShell** , puis cliquez sur **Exécuter en tant qu’administrateur**.  
  
    -   Sur l’écran d' **Accueil** de Windows, cliquez avec le bouton droit sur la vignette Windows PowerShell, puis dans la barre des applications, cliquez sur **exécuter en tant qu’administrateur**.  
  
    -   Sur un serveur qui exécute l’option d’installation minimale, tapez **PowerShell** dans une invite de commandes, puis appuyez sur **entrée**.  
  
2.  Exécutez l’une des commandes DISM suivantes.  
  
    -   Si l’ordinateur a accès à Windows Update ou si un emplacement de fichier source par défaut a déjà été configuré dans stratégie de groupe, exécutez la commande suivante.  
  
        ```  
        DISM /online /Enable-Feature /Featurename:NetFx3 /All  
        ```  
  
    -   Si l’ordinateur a accès au support d’installation, exécutez une commande semblable à la suivante. Dans l’exemple suivant, le support d’installation du système d’exploitation se trouve sur le disque D. Le paramètre `LimitAccess` empêche la commande d’essayer de contacter Windows Update ou un serveur qui exécute les services WSUS.  
  
        ```  
        DISM /online /Enable-Feature /Featurename:NetFx3 /All /LimitAccess /Source:d:\sources\sxs  
        ```  
  
    > [!NOTE]  
    > La DISM respecte la casse.  
  
### <a name="configure-alternate-sources-for-feature-files-in-group-policy"></a>Configurer d’autres sources pour les fichiers de fonctionnalités dans la stratégie de groupe  
Le paramètre de stratégie de groupe décrit dans cette section spécifie les emplacements sources autorisés pour les fichiers du .NET Framework 3.5 et d’autres fichiers de fonctionnalités qui ont été supprimés dans le cadre d’une configuration Fonctionnalités à la demande. Le paramètre **de stratégie spécifiez les paramètres pour l’installation de composants facultatifs et la réparation de composants** se trouve dans le dossier **Computer Configuration\Administrative modèles** dans le console de gestion des stratégies de groupe ou local stratégie de groupe éditeurs.  
  
> [!NOTE]  
> Vous devez être membre du groupe Administrateurs pour modifier des paramètres de stratégie de groupe sur l’ordinateur local. Si les paramètres de stratégie de groupe de l’ordinateur que vous voulez gérer sont contrôlés au niveau du domaine, vous devez être membre du groupe Administrateurs du domaine pour modifier des paramètres de stratégie de groupe.  
  
##### <a name="to-configure-a-default-alternate-source-path-in-group-policy"></a>Pour configurer un autre chemin d’accès source par défaut dans la stratégie de groupe  
  
1. Dans l’éditeur de stratégie de groupe local ou Console de gestion des stratégies de groupe, ouvrez le paramètre de stratégie suivant.  
  
   **configuration de l’ordinateur \ paramètres administration pour l’installation des composants facultatifs et la réparation des composants**  
  
2. Sselect **activé** pour activer le paramètre de stratégie, s’il n’est pas déjà activé.  
  
3. Dans la zone de texte **Autre chemin d’accès au fichier source** dans la zone **Options**, spécifiez un chemin d’accès complet à un dossier partagé ou un fichier WIM. Pour spécifier un fichier WIM comme autre emplacement de fichier source, ajoutez le préfixe **WIM:** au chemin d’accès et ajoutez l’index de l’image à utiliser dans le fichier WMI comme suffixe. Voici quelques exemples de valeurs que vous pouvez spécifier.  
  
   - chemin d’accès à un dossier partagé : **\\ @ no__t-2**<em>nom_serveur</em> **\Share @ no__t-5**<em>nom_dossier</em>  
  
   - chemin d’accès à un fichier WIM, dans lequel **3** représente l’index de l’image dans laquelle les fichiers de fonctionnalités sont trouvés :  **Wim : \\ @ no__t-2**<em>nom_serveur</em> **\share\install.wim : 3**  
  
4. Si vous ne souhaitez pas que les ordinateurs contrôlés par ce paramètre de stratégie recherchent les fichiers de fonctionnalités manquants dans Windows Update, sélectionnez **ne jamais essayer de télécharger la charge utile à partir de Windows Update**.  
  
5. Si les ordinateurs contrôlés par ce paramètre de stratégie reçoivent généralement des mises à jour par le biais des services WSUS, mais que vous préférez passer par Windows Update plutôt que par les services WSUS pour rechercher les fichiers manquants, sélectionnez **Contacter Windows Update directement pour télécharger le contenu de réparation au lieu de Windows Server Update Services (WSUS)** .  
  
6. Cliquez sur **OK** une fois que vous avez terminé de modifier ce paramètre de stratégie, puis fermez l’Éditeur de stratégie de groupe.  
  
## <a name="see-also"></a>Voir aussi  
[Options d’installation de Windows Server](https://go.microsoft.com/fwlink/p/?LinkId=241573)  
[Considérations relatives au déploiement de Microsoft .NET Framework 3,5](https://go.microsoft.com/fwlink/p/?LinkId=248869)  
[Comment activer ou désactiver les fonctionnalités Windows](https://go.microsoft.com/fwlink/p/?LinkId=246552)  
  


