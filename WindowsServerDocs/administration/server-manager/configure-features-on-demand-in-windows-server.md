---
title: Configurer des fonctionnalités à la demande dans Windows Server
description: Gestionnaire de serveur
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e663bbea-d025-41fa-b16c-c2bff00a88e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c77bd088a02d405b17a4f7decd6093785296d5e8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818930"
---
# <a name="configure-features-on-demand-in-windows-server"></a>Configurer des fonctionnalités à la demande dans Windows Server

>S'applique à : Windows Server 2016

Cette rubrique décrit comment supprimer des fichiers de fonctionnalités dans une configuration de type Fonctionnalités à la demande en utilisant l’applet de commande Uninstall-WindowsFeature.

Fonctionnalités à la demande est une fonctionnalité, introduite dans Windows 8 et Windows Server 2012, qui vous permet de supprimer les fichiers de rôles et de fonctionnalités (parfois désignés sous le nom *charge utile*) à partir du système d’exploitation pour économiser l’espace disque et installer des rôles fonctionnalités et à partir d’emplacements distants ou de support d’installation au lieu d’à partir d’ordinateurs locaux. Vous pouvez supprimer des fichiers de fonctionnalités d’ordinateurs virtuels ou physiques en cours d’exécution. Vous pouvez également ajouter ou supprimer des fichiers de fonctionnalités dans les fichiers WIM (Windows Image) ou les disques durs virtuels hors connexion pour créer une copie reproductible des configurations de type Fonctionnalités à la demande.

Dans une fonctionnalités de configuration à la demande, lorsque les fichiers de fonctionnalités ne sont pas disponibles sur un ordinateur, si une installation requiert ces fichiers de fonctionnalités, Windows Server 2012 R2 ou Windows Server 2012 peut être dirigés vers obtenir les fichiers à partir d’un magasin de fonctionnalités de côte à côte (un dossier partagé qui contient les fichiers de fonctionnalités et est disponible à l’ordinateur sur le réseau), à partir de Windows Update ou à partir du support d’installation. Par défaut, lorsque les fichiers de fonctionnalités ne sont pas disponibles sur le serveur cible, Fonctionnalités à la demande recherche les fichiers de fonctionnalités manquants en effectuant les tâches suivantes, dans l’ordre indiqué.

1.  Recherche dans un emplacement qui a été spécifié par les utilisateurs d’ajouter des rôles et de fonctionnalités Assistant ou de commandes d’installation DISM

2.  L’évaluation de la configuration du paramètre de stratégie de groupe, **ordinateur Configuration ordinateur\Modèles d’administration\système\spécifier des paramètres pour l’installation des composants facultatifs et la réparation de composants**

3.  Recherche sur Windows Update

Vous pouvez remplacer le comportement par défaut de Fonctionnalités à la demande en effectuant l’une des opérations suivantes.

-   Spécification d’un autre chemin d’accès source dans le cadre de l’applet de commande `Install-WindowsFeature` , en ajoutant le paramètre `Source`

-   Spécification d’un chemin de source de remplacement sur la **confirmer les options d’installation** page pendant que vous installez des fonctionnalités à l’aide de l’ajouter Assistant rôles et fonctionnalités

-   Configuration du paramètre de stratégie de groupe, **Spécifier des paramètres pour l’installation des composants facultatifs et la réparation de composants**

Cette rubrique contient les sections suivantes.

-   [Créer un fichier de fonctionnalité ou d’un magasin de côte à côte](#BKMK_store)

-   [Méthodes de suppression des fichiers de fonctionnalités](#BKMK_methods)

-   [Supprimer les fichiers de fonctionnalités à l’aide de Uninstall-WindowsFeature](#BKMK_remove)

## <a name="BKMK_store"></a>Créer un fichier de fonctionnalité ou d’un magasin de côte à côte
Cette section décrit comment configurer un dossier de fichier partagé fonctionnalité à distance (également appelé magasin côte à côte) qui stocke les fichiers nécessaires pour installer des rôles, services de rôle et fonctionnalités sur des serveurs qui exécutent Windows Server 2012 R2 ou Windows Server 2012. Une fois que vous avez configuré un magasin de fonctionnalités, vous pouvez installer des rôles, services de rôle et fonctionnalités sur les serveurs qui exécutent ces systèmes d’exploitation et spécifier le magasin de fonctionnalités comme l’emplacement des fichiers sources d’installation.

#### <a name="to-create-a-feature-file-store"></a>Pour créer un magasin de fichiers de fonctionnalités

1.  Créez un dossier partagé sur un serveur de votre réseau. Par exemple,  *\\\network\share\sxs*.

2.  Vérifiez que les autorisations appropriées sont affectées au magasin de fonctionnalités. Le partage de fichier ou chemin d’accès source doit accorder **en lecture** autorisations à le **tout le monde** groupe (non recommandé pour des raisons de sécurité), ou aux comptes d’ordinateurs (*domaine* \\ *Nom_serveur*$) des serveurs sur lesquels vous prévoyez d’installer des fonctionnalités à l’aide de ce magasin de fonctionnalités ; l’octroi de l’accès de compte d’utilisateur n’est pas suffisant.

    Vous pouvez accéder aux paramètres du partage de fichiers et des autorisations en effectuant l’une des opérations suivantes sur le Bureau Windows.

    -   Cliquez avec le bouton droit sur le dossier partagé, cliquez sur **Propriétés**, puis modifiez les utilisateurs autorisés et leurs droits d’accès au dossier sous l’onglet **Sécurité**.

    -   Cliquez avec le bouton droit sur le dossier partagé, pointez sur **Partager avec**, puis cliquez sur **Des personnes spécifiques**.

    > [!NOTE]
    > Les serveurs qui se trouvent dans des groupes de travail ne peuvent pas accéder aux partages de fichiers externes, même si le compte d’ordinateur pour le serveur de groupe de travail a des autorisations de **Lecture** sur le partage externe. D’autres emplacements sources qui fonctionnent pour les serveurs de groupes de travail sont notamment les supports d’installation, Windows Update, et les disques durs virtuels ou les fichiers WIM qui sont stockés sur le serveur de groupe de travail local.

3.  Copiez le dossier **Sources\SxS** de votre support d’installation Windows Server dans le dossier partagé que vous avez créé à l’étape 1.

## <a name="BKMK_methods"></a>Méthodes de suppression des fichiers de fonctionnalités
Deux méthodes sont disponibles pour supprimer des fichiers de fonctionnalités de Windows Server dans une configuration de type Fonctionnalités à la demande.

-   Le `remove` paramètre de la `Uninstall-WindowsFeature` applet de commande vous permet de supprimer des fichiers de fonctionnalités à partir d’un serveur ou hors connexion disque dur virtuel (VHD) qui exécute Windows Server 2012 R2 ou Windows Server 2012. Les valeurs valides pour le `remove` paramètre sont les noms des rôles, services de rôle et fonctionnalités.

-   Les commandes de gestion et de maintenance des images de déploiement (DISM) vous permettent de créer des fichiers WIM personnalisés qui économisent l’espace disque en omettant les fichiers de fonctionnalités qui sont inutiles ou qui peuvent être obtenus auprès d’autres sources distantes. Pour plus d’informations sur la préparation d’images personnalisées à l’aide de l’outil DISM, consultez [Comment activer ou désactiver des fonctionnalités de Windows](https://technet.microsoft.com/library/hh824822.aspx).

## <a name="BKMK_remove"></a>Supprimer les fichiers de fonctionnalités à l’aide de Uninstall-WindowsFeature
Vous pouvez utiliser l’applet de commande Uninstall-WindowsFeature à la fois pour désinstaller des rôles, services de rôle et fonctionnalités des serveurs et des disques durs virtuels hors connexion qui exécutent Windows Server 2012 R2 ou Windows Server 2012 et pour supprimer les fichiers de fonctionnalités. Vous pouvez désinstaller et supprimer les rôles, services de rôle et fonctionnalités dans la même commande même si vous le souhaitez.

> [!IMPORTANT]
> Lorsque vous supprimez des fichiers de fonctionnalités pour un rôle, le service de rôle ou fonctionnalité, rôles, services de rôle et fonctionnalités qui dépendent de fichiers que vous supprimez sont également supprimées. Si vous supprimez des fichiers de fonctionnalités pour un service de rôle ou un sous-composant et qu’aucun autre service de rôle ni sous-composant pour le rôle ou le composant parent ne demeure installé, les fichiers pour tout le rôle ou le composant parent sont supprimés. Pour afficher tous les fichiers de fonctionnalités qui seraient supprimés par la commande `Uninstall-WindowsFeature -remove`, ajoutez le paramètre `whatif` à la commande pour l’exécuter et afficher les résultats sans réellement supprimer les fichiers de fonctionnalités.

#### <a name="to-remove-role-and-feature-files-by-using-uninstall-windowsfeature"></a>Pour supprimer des fichiers de rôles et de fonctionnalités en utilisant Uninstall-WindowsFeature

1.  Effectuez une des opérations suivantes pour ouvrir une session Windows PowerShell avec des droits utilisateur élevés.

    > [!NOTE]
    > Si vous désinstallez des rôles et fonctionnalités à partir d’un serveur distant, il est inutile d’exécuter Windows PowerShell avec des droits utilisateur élevés.

    -   Sur le Bureau Windows, cliquez avec le bouton droit dans la barre des tâches sur **Windows PowerShell** , puis cliquez sur **Exécuter en tant qu’administrateur**.

    -   Sur le Windows **Démarrer** écran, cliquez sur la vignette de Windows PowerShell et cliquez sur la barre des applications **exécuter en tant qu’administrateur**.

    -   Sur un serveur qui exécute l’option d’installation Server Core, tapez **PowerShell** à une invite de commandes, puis appuyez sur **entrée**.

2.  Tapez ce qui suit, puis appuyez sur **Entrée**.

    ```
    Uninstall-WindowsFeature -Name <feature_name> -computerName <computer_name> -remove
    ```

    **Exemple :** Gestionnaire de licences des services Bureau à distance est le dernier service de rôle restant des services Bureau à distance, qui est installé. La commande désinstalle Gestionnaire de licences des services Bureau à distance, puis supprime les fichiers de fonctionnalités pour l’intégralité du rôle Services Bureau à distance du serveur spécifié, *contoso_1*.

    ```
    Uninstall-WindowsFeature -Name rdS-Licensing -computerName contoso_1 -remove
    ```

    **Exemple :** Dans l’exemple suivant, la commande supprime Services de domaine active directory et gestion des stratégies de groupe à partir d’un disque dur virtuel hors connexion. La fonctionnalité et le rôle sont d’abord désinstallés, puis leurs fichiers de fonctionnalités sont entièrement supprimés du disque dur virtuel hors connexion, *Contoso.vhd*.

    > [!NOTE]
    > Vous devez ajouter le `computerName` paramètre si vous exécutez l’applet de commande à partir d’un ordinateur qui exécute Windows 8.1 ou Windows 8.
    > 
    > Si vous entrez le nom d’un fichier de disque dur virtuel à partir d’un partage réseau, ce partage doit accorder **en lecture** et **écrire** autorisations au compte d’ordinateur du serveur que vous avez sélectionné pour monter le disque dur virtuel. L’accès au compte d’utilisateur uniquement ne suffit pas. Le partage peut accorder les autorisations **Lire** et **Écrire** au groupe **Tout le monde** pour autoriser l’accès au disque dur virtuel, mais pour des raisons de sécurité, cela n’est pas recommandé.

    ```
    Uninstall-WindowsFeature -Name AD-Domain-Services,GPMC -VHD C:\WS2012VHDs\Contoso.vhd -computerName ContosoDC1
    ```

## <a name="see-also"></a>Voir aussi
[Installer ou désinstaller des rôles, Services de rôle ou fonctionnalités](install-or-uninstall-roles-role-services-or-features.md)
[Options d’Installation de Windows Server](https://technet.microsoft.com/library/hh831786.aspx)
[comment activer ou désactiver des fonctionnalités de Windows](https://technet.microsoft.com/library/hh824822.aspx) 
 [Déploiement Image Servicing and Management (DISM) Overview](https://technet.microsoft.com/library/hh825236.aspx)


