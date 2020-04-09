---
title: Configurer des fonctionnalités à la demande dans Windows Server
description: Gestionnaire de serveur
ms.prod: windows-server
ms.technology: manage-server-manager
ms.topic: article
ms.assetid: e663bbea-d025-41fa-b16c-c2bff00a88e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 090b87810dc519728bf915bdb2cd79668c7f01f4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851552"
---
# <a name="configure-features-on-demand-in-windows-server"></a>Configurer des fonctionnalités à la demande dans Windows Server

>S’applique à Windows Server 2016

Cette rubrique décrit comment supprimer des fichiers de fonctionnalités dans une configuration de type Fonctionnalités à la demande en utilisant l’applet de commande Uninstall-WindowsFeature.

Fonctionnalités à la demande est une fonctionnalité, introduite dans Windows 8 et Windows Server 2012, qui vous permet de supprimer des fichiers de rôles et de fonctionnalités (parfois appelés *charge utile*de fonctionnalité) du système d’exploitation afin de conserver de l’espace disque, et d’installer des rôles et des fonctionnalités à partir d’emplacements distants ou de supports d’installation, et non à partir d’ordinateurs locaux. Vous pouvez supprimer des fichiers de fonctionnalités d’ordinateurs virtuels ou physiques en cours d’exécution. Vous pouvez également ajouter ou supprimer des fichiers de fonctionnalités dans les fichiers WIM (Windows Image) ou les disques durs virtuels hors connexion pour créer une copie reproductible des configurations de type Fonctionnalités à la demande.

Dans une configuration à la demande, lorsque des fichiers de fonctionnalités ne sont pas disponibles sur un ordinateur, si une installation requiert ces fichiers de fonctionnalités, Windows Server 2012 R2 ou Windows Server 2012 peut être dirigé pour obtenir les fichiers à partir d’un magasin de fonctionnalités côte à côte (un dossier partagé qui contient des fichiers de fonctionnalités et est disponible pour l’ordinateur sur le réseau), à partir de Windows Update ou à partir du support d’installation. Par défaut, lorsque les fichiers de fonctionnalités ne sont pas disponibles sur le serveur cible, Fonctionnalités à la demande recherche les fichiers de fonctionnalités manquants en effectuant les tâches suivantes, dans l’ordre indiqué.

1.  Recherche dans un emplacement spécifié par les utilisateurs de l’Assistant Ajout de rôles et de fonctionnalités ou commandes d’installation DISM

2.  Évaluation de la configuration du paramètre stratégie de groupe, configuration **ordinateur \ paramètres administration pour l’installation de composants facultatifs et la réparation de composants**

3.  Recherche sur Windows Update

Vous pouvez remplacer le comportement par défaut de Fonctionnalités à la demande en effectuant l’une des opérations suivantes.

-   Spécification d’un autre chemin d’accès source dans le cadre de l’applet de commande `Install-WindowsFeature` , en ajoutant le paramètre `Source`

-   Spécification d’un autre chemin d’accès source dans la page **confirmer les options d’installation** pendant que vous installez des fonctionnalités à l’aide de l’Assistant Ajout de rôles et de fonctionnalités

-   Configuration du paramètre de stratégie de groupe, **Spécifier des paramètres pour l’installation des composants facultatifs et la réparation de composants**

Cette rubrique contient les sections suivantes.

-   [Créer un fichier de fonctionnalités ou un magasin côte à côte](#BKMK_store)

-   [Méthodes de suppression des fichiers de fonctionnalités](#BKMK_methods)

-   [Supprimer des fichiers de fonctionnalités à l’aide de Uninstall-WindowsFeature](#BKMK_remove)

## <a name="create-a-feature-file-or-side-by-side-store"></a><a name=BKMK_store></a>Créer un fichier de fonctionnalités ou un magasin côte à côte
Cette section décrit comment configurer un dossier partagé de fichiers de fonctionnalités distant (également appelé magasin côte à côte) qui stocke les fichiers requis pour installer des rôles, des services de rôle et des fonctionnalités sur des serveurs qui exécutent Windows Server 2012 R2 ou Windows Server 2012. Une fois que vous avez configuré un magasin de fonctionnalités, vous pouvez installer des rôles, des services de rôle et des fonctionnalités sur des serveurs qui exécutent ces systèmes d’exploitation, et spécifier le magasin de fonctionnalités comme emplacement des fichiers sources d’installation.

#### <a name="to-create-a-feature-file-store"></a>Pour créer un magasin de fichiers de fonctionnalités

1.  Créez un dossier partagé sur un serveur de votre réseau. Par exemple, *\\\network\share\sxs*.

2.  Vérifiez que les autorisations appropriées sont affectées au magasin de fonctionnalités. Le chemin d’accès source ou partage de fichiers doit accorder des autorisations de **lecture** au groupe **tout le monde** (non recommandé pour des raisons de sécurité) ou aux comptes d’ordinateur (*domaine*\\*nom_serveur*$) des serveurs sur lesquels vous envisagez d’installer des fonctionnalités à l’aide de ce magasin de fonctionnalités. l’octroi de l’accès au compte d’utilisateur n’est pas suffisant.

    Vous pouvez accéder aux paramètres du partage de fichiers et des autorisations en effectuant l’une des opérations suivantes sur le Bureau Windows.

    -   Cliquez avec le bouton droit sur le dossier partagé, cliquez sur **Propriétés**, puis modifiez les utilisateurs autorisés et leurs droits d’accès au dossier sous l’onglet **Sécurité**.

    -   Cliquez avec le bouton droit sur le dossier partagé, pointez sur **Partager avec**, puis cliquez sur **Des personnes spécifiques**.

    > [!NOTE]
    > Les serveurs qui se trouvent dans des groupes de travail ne peuvent pas accéder aux partages de fichiers externes, même si le compte d’ordinateur pour le serveur de groupe de travail a des autorisations de **Lecture** sur le partage externe. D’autres emplacements sources qui fonctionnent pour les serveurs de groupes de travail sont notamment les supports d’installation, Windows Update, et les disques durs virtuels ou les fichiers WIM qui sont stockés sur le serveur de groupe de travail local.

3.  Copiez le dossier **Sources\SxS** de votre support d’installation Windows Server dans le dossier partagé que vous avez créé à l’étape 1.

## <a name="methods-of-removing-feature-files"></a><a name=BKMK_methods></a>Méthodes de suppression des fichiers de fonctionnalités
Deux méthodes sont disponibles pour supprimer des fichiers de fonctionnalités de Windows Server dans une configuration de type Fonctionnalités à la demande.

-   Le paramètre `remove` de l’applet de commande `Uninstall-WindowsFeature` vous permet de supprimer des fichiers de fonctionnalités d’un serveur ou d’un disque dur virtuel hors connexion (VHD) qui exécute Windows Server 2012 R2 ou Windows Server 2012. Les valeurs valides pour le paramètre `remove` sont les noms des rôles, des services de rôle et des fonctionnalités.

-   Les commandes de gestion et de maintenance des images de déploiement (DISM) vous permettent de créer des fichiers WIM personnalisés qui économisent l’espace disque en omettant les fichiers de fonctionnalités qui sont inutiles ou qui peuvent être obtenus auprès d’autres sources distantes. Pour plus d’informations sur la préparation d’images personnalisées à l’aide de l’outil DISM, consultez [Comment activer ou désactiver des fonctionnalités de Windows](https://technet.microsoft.com/library/hh824822.aspx).

## <a name="remove-feature-files-by-using-uninstall-windowsfeature"></a><a name=BKMK_remove></a>Supprimer des fichiers de fonctionnalités à l’aide de Uninstall-WindowsFeature
Vous pouvez utiliser l’applet de commande Uninstall-WindowsFeature à la fois pour désinstaller des rôles, des services de rôle et des fonctionnalités des serveurs et des disques durs virtuels hors connexion qui exécutent Windows Server 2012 R2 ou Windows Server 2012, et pour supprimer des fichiers de fonctionnalités. Vous pouvez désinstaller et supprimer les mêmes rôles, services de rôle et fonctionnalités dans la même commande, si vous le souhaitez.

> [!IMPORTANT]
> Lorsque vous supprimez des fichiers de fonctionnalités pour un rôle, un service de rôle ou une fonctionnalité, des rôles, des services de rôle et des fonctionnalités qui dépendent des fichiers que vous supprimez, sont également supprimés. Si vous supprimez des fichiers de fonctionnalités pour un service de rôle ou un sous-composant et qu’aucun autre service de rôle ni sous-composant pour le rôle ou le composant parent ne demeure installé, les fichiers pour tout le rôle ou le composant parent sont supprimés. Pour afficher tous les fichiers de fonctionnalités qui seraient supprimés par la commande `Uninstall-WindowsFeature -remove`, ajoutez le paramètre `whatif` à la commande pour l’exécuter et afficher les résultats sans réellement supprimer les fichiers de fonctionnalités.

#### <a name="to-remove-role-and-feature-files-by-using-uninstall-windowsfeature"></a>Pour supprimer des fichiers de rôles et de fonctionnalités en utilisant Uninstall-WindowsFeature

1.  Effectuez une des opérations suivantes pour ouvrir une session Windows PowerShell avec des droits utilisateur élevés.

    > [!NOTE]
    > Si vous désinstallez des rôles et des fonctionnalités à partir d’un serveur distant, vous n’avez pas besoin d’exécuter Windows PowerShell avec des droits d’utilisateur élevés.

    -   Sur le Bureau Windows, cliquez avec le bouton droit dans la barre des tâches sur **Windows PowerShell** , puis cliquez sur **Exécuter en tant qu’administrateur**.

    -   Sur l’écran d' **Accueil** de Windows, cliquez avec le bouton droit sur la vignette Windows PowerShell, puis dans la barre des applications, cliquez sur **exécuter en tant qu’administrateur**.

    -   Sur un serveur qui exécute l’option d’installation minimale, tapez **PowerShell** dans une invite de commandes, puis appuyez sur **entrée**.

2.  Tapez ce qui suit, puis appuyez sur **Entrée**.

    ```
    Uninstall-WindowsFeature -Name <feature_name> -computerName <computer_name> -remove
    ```

    **Exemple :** Gestionnaire de licences des services Bureau à distance est le dernier service de rôle restant des services Bureau à distance qui sont installés. La commande désinstalle Gestionnaire de licences des services Bureau à distance, puis supprime les fichiers de fonctionnalités pour l’intégralité du rôle Services Bureau à distance du serveur spécifié, *contoso_1*.

    ```
    Uninstall-WindowsFeature -Name rdS-Licensing -computerName contoso_1 -remove
    ```

    **Exemple :** Dans l’exemple suivant, la commande supprime les services de domaine Active Directory et la gestion des stratégie de groupe à partir d’un disque dur virtuel hors connexion. La fonctionnalité et le rôle sont d’abord désinstallés, puis leurs fichiers de fonctionnalités sont entièrement supprimés du disque dur virtuel hors connexion, *Contoso.vhd*.

    > [!NOTE]
    > Vous devez ajouter le paramètre `computerName` si vous exécutez l’applet de commande à partir d’un ordinateur exécutant Windows 8.1 ou Windows 8.
    > 
    > Si vous entrez le nom d’un fichier VHD à partir d’un partage réseau, ce partage doit accorder des autorisations **en lecture et en** **écriture** au compte d’ordinateur du serveur que vous avez sélectionné pour monter le disque dur virtuel. L’accès au compte d’utilisateur uniquement ne suffit pas. Le partage peut accorder les autorisations **Lire** et **Écrire** au groupe **Tout le monde** pour autoriser l’accès au disque dur virtuel, mais pour des raisons de sécurité, cela n’est pas recommandé.

    ```
    Uninstall-WindowsFeature -Name AD-Domain-Services,GPMC -VHD C:\WS2012VHDs\Contoso.vhd -computerName ContosoDC1
    ```

## <a name="see-also"></a>Voir aussi
[Installer ou désinstaller des rôles, des services de rôle ou des fonctionnalités](install-or-uninstall-roles-role-services-or-features.md)
les [options d’installation de Windows Server](https://technet.microsoft.com/library/hh831786.aspx)
[Comment activer ou désactiver les fonctionnalités Windows
la](https://technet.microsoft.com/library/hh824822.aspx) [gestion et la maintenance des images de déploiement (DISM)](https://technet.microsoft.com/library/hh825236.aspx)


