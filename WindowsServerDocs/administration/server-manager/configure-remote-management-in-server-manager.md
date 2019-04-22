---
title: Configurer la gestion à distance dans le Gestionnaire de serveur
description: Gestionnaire de serveur
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 509182ed-c37d-4b81-84bc-aee43d006873
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4a66fe7a274756de9bed9f6b14f5b9e491e5b623
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819580"
---
# <a name="configure-remote-management-in-server-manager"></a>Configurer la gestion à distance dans le Gestionnaire de serveur

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dans Windows Server, vous pouvez utiliser le Gestionnaire de serveur pour effectuer des tâches de gestion sur des serveurs distants. gestion à distance est activée par défaut sur les serveurs qui exécutent Windows Server 2016. Pour gérer un serveur à distance à l’aide du Gestionnaire de serveur, vous ajoutez le serveur pour le pool de serveurs du Gestionnaire de serveur.

Vous pouvez utiliser le Gestionnaire de serveur pour gérer les serveurs distants qui exécutent des versions plus anciennes de Windows Server, mais les mises à jour suivantes sont requises pour gérer entièrement ces anciens systèmes d’exploitation.

Pour gérer les serveurs qui exécutent des versions de Windows Server antérieure à Windows Server 2016, installez les logiciels et les mises à jour pour rendre les versions plus anciennes de Windows Server gérables à l’aide du Gestionnaire de serveur dans Windows Server 2016 suivante.

|Système d’exploitation|Logiciel requis|Facilité de gestion|
|----------|-----------|---------|
| Windows Server 2012 R2 ou Windows Server 2012 |-   [.NET framework 4.6](https://www.microsoft.com/download/details.aspx?id=45497)<br />-   [Windows Management Framework 5.0](https://go.microsoft.com/fwlink/?LinkID=395058). Le package de téléchargement Windows Management Framework 5.0 met à jour les fournisseurs Windows Management Instrumentation (WMI) sur Windows Server 2012 R2, Windows Server 2012 et Windows Server 2008 R2. Les fournisseurs WMI mis à jour le Gestionnaire de serveur permettent de collecter des informations sur les rôles et fonctionnalités installés sur les serveurs gérés. Jusqu'à ce que la mise à jour est appliquée, les serveurs qui exécutent Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2008 R2 présentent l’état de gérabilité **non accessible**.<br />-La mise à jour des performances associée [article 2682011 de la Base de connaissances](https://go.microsoft.com/fwlink/p/?LinkID=245487) n’est plus nécessaire sur les serveurs qui exécutent Windows Server 2012 R2 ou Windows Server 2012.||
| Windows Server 2008 R2 |-   [.NET framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)<br />-   [Windows Management Framework 4.0](https://go.microsoft.com/fwlink/?LinkId=293881). Le package de téléchargement Windows Management Framework 4.0 met à jour les fournisseurs Windows Management Instrumentation (WMI) sur Windows Server 2008 R2. Les fournisseurs WMI mis à jour le Gestionnaire de serveur permettent de collecter des informations sur les rôles et fonctionnalités installés sur les serveurs gérés. Jusqu'à ce que la mise à jour est appliquée, les serveurs qui exécutent Windows Server 2008 R2 présentent l’état de gérabilité **non accessible**.<br />-La mise à jour des performances associée [article 2682011 de la Base de connaissances](https://go.microsoft.com/fwlink/p/?LinkID=245487) Gestionnaire de serveur vous permet de collecter des données de performances à partir de Windows Server 2008 R2.||
| Windows Server 2008 |-   [.NET framework 4](https://www.microsoft.com/download/en/details.aspx?id=17718)<br />-   [Windows Management Framework 3.0](https://go.microsoft.com/fwlink/p/?LinkID=229019) package de téléchargement de The Windows Management Framework 3.0 met à jour les fournisseurs Windows Management Instrumentation (WMI) sur Windows Server 2008. Les fournisseurs WMI mis à jour le Gestionnaire de serveur permettent de collecter des informations sur les rôles et fonctionnalités installés sur les serveurs gérés. Jusqu'à ce que la mise à jour est appliquée, les serveurs qui exécutent Windows Server 2008 ont un état de facilité de **non accessible - vérifier les versions antérieures exécutent Windows Management Framework 3.0**.<br />-La mise à jour des performances associée [article 2682011 de la Base de connaissances](https://go.microsoft.com/fwlink/p/?LinkID=245487) Gestionnaire de serveur vous permet de collecter des données de performances à partir de Windows Server 2008.||

Pour plus d’informations sur l’ajout de serveurs qui se trouvent dans des groupes de travail pour gérer ou gérer des serveurs distants à partir d’un ordinateur de groupe de travail qui exécute le Gestionnaire de serveur, consultez [ajouter des serveurs au Gestionnaire de serveur](add-servers-to-server-manager.md).

## <a name="BKMK_remote"></a>L’activation ou désactivation de la gestion à distance
Dans Windows Server 2016, la gestion à distance est activée par défaut. Avant de vous connecter à un ordinateur qui exécute Windows Server 2016 à distance à l’aide du Gestionnaire de serveur, gestion à distance du Gestionnaire de serveur doit être activée sur l’ordinateur de destination si elle a été désactivée. Les procédures fournies dans cette section décrivent la manière de désactiver l’administration à distance et la manière de la réactiver si elle a été désactivée. Dans la console Gestionnaire de serveur, l’état de gestion à distance pour le serveur local est affiché dans le **propriétés** zone de la **serveur Local** page.

Les comptes d’administrateur local autres que le compte Administrateur intégré n’ont peut-être pas les droits requis pour gérer un serveur à distance, même si l’administration à distance est activée. Le contrôle de compte utilisateur (UAC) distant **LocalAccountTokenFilterPolicy** paramètre de Registre doit être configuré pour autoriser les comptes locaux du groupe administrateurs autres que le compte administrateur intégré pour gérer à distance le serveur.

Dans Windows Server 2016, le Gestionnaire de serveur s’appuie sur la gestion à distance de Windows (WinRM) et Distributed component Object model (DCOM) pour les communications distantes. Les paramètres qui sont contrôlés par le **configurer la gestion à distance** boîte de dialogue affectent uniquement les parties du Gestionnaire de serveur et de Windows PowerShell qui utilisent WinRM pour les communications à distance. Elles n’affectent pas les parties du Gestionnaire de serveur qui utilisent le modèle DCOM pour les communications distantes. Par exemple, le Gestionnaire de serveur utilise WinRM pour communiquer avec des serveurs distants qui exécutent Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012, mais utilise DCOM pour communiquer avec les serveurs qui exécutent Windows Server 2008 et Windows Server 2008 R2 mais n’ont pas la [Windows Management Framework 4.0](https://go.microsoft.com/fwlink/?LinkId=293881) ou [Windows Management Framework 3.0](https://go.microsoft.com/fwlink/p/?LinkID=229019) mises à jour appliquées. Microsoft Management Console (mmc) et autres outils de gestion hérités utilisent le modèle DCOM. Pour plus d’informations sur la façon de modifier ces paramètres, consultez [pour configurer mmc ou autres tâches de gestion à distance d’outil via DCOM](#BKMK_dcom) dans cette rubrique.

> [!NOTE]
> Les procédures de cette section ne peuvent être réalisées que sur des ordinateurs qui exécutent Windows Server. Impossible d’activer ou de désactiver la gestion à distance sur un ordinateur qui exécute Windows 10 à l’aide de ces procédures, car le système d’exploitation client ne peuvent pas être géré à l’aide du Gestionnaire de serveur.

-   Pour activer l’administration à distance via WinRM, sélectionnez l’une des procédures suivantes :

    -   [Pour activer la gestion à distance du Gestionnaire de serveur à l’aide de l’interface Windows](#BKMK_windows)

    -   [Pour activer la gestion à distance du Gestionnaire de serveur à l’aide de Windows PowerShell](#BKMK_ps)

    -   [Pour activer la gestion à distance du Gestionnaire de serveur à l’aide de la ligne de commande](#BKMK_cmdline)

    -   [Pour activer la gestion à distance Server Manager et Windows PowerShell sur des versions antérieures de Windows Server](#BKMK_old)

-   Pour désactiver la gestion à distance WinRM et le Gestionnaire de serveur, sélectionnez une des procédures suivantes.

    -   [Pour désactiver la gestion à distance à l’aide de stratégie de groupe](#BKMK_disableGP)

    -   [Pour désactiver la gestion à distance à l’aide d’un fichier de réponses lors de l’installation sans assistance](#BKMK_unattend)

-   Pour configurer l’administration à distance via le modèle DCOM, voir [Configurer l’administration à distance via le modèle DCOM](#BKMK_dcom).

### <a name="BKMK_windows"></a>Pour activer la gestion à distance du Gestionnaire de serveur à l’aide de l’interface Windows

1.  > [!NOTE]
    > Les paramètres qui sont contrôlés par le **configurer la gestion à distance** boîte de dialogue n’affectent pas les parties du Gestionnaire de serveur qui utilisent le modèle DCOM pour les communications distantes.

    Sur l’ordinateur que vous souhaitez gérer à distance, ouvrez le Gestionnaire de serveur, si elle n’est pas déjà ouverte. Dans la barre des tâches Windows, cliquez sur **Gestionnaire de serveur**. Sur le **Démarrer** , cliquez sur le **le Gestionnaire de serveur** vignette.

2.  Dans le **propriétés** zone de la **serveurs locaux** , cliquez sur la valeur de lien hypertexte pour le **gestion à distance** propriété.

3.  Effectuez l’une des opérations ci-dessous, puis cliquez sur **OK**.

    -   Pour empêcher cet ordinateur d’être gérés à distance en utilisant le Gestionnaire de serveur (ou Windows PowerShell s’il est installé), désactivez le **activer la gestion à distance de ce serveur à partir d’autres ordinateurs** case à cocher.

    -   Pour permettre à cet ordinateur de gestion à distance en utilisant le Gestionnaire de serveur ou de Windows PowerShell, sélectionnez **activer la gestion à distance de ce serveur à partir d’autres ordinateurs**.

### <a name="BKMK_ps"></a>Pour activer la gestion à distance du Gestionnaire de serveur à l’aide de Windows PowerShell

1.  Sur l’ordinateur que vous souhaitez gérer à distance, effectuez l’une des opérations suivantes pour ouvrir une session Windows PowerShell avec des droits utilisateur élevés.

    -   Sur le Bureau Windows, cliquez avec le bouton droit dans la barre des tâches sur **Windows PowerShell** , puis cliquez sur **Exécuter en tant qu’administrateur**.

    -   Sur le Windows **Démarrer** écran, cliquez sur **Windows PowerShell**, puis cliquez sur la barre des applications **exécuter en tant qu’administrateur**.

2.  Tapez la commande suivante, puis appuyez sur **entrée** pour activer toutes les exceptions des règles de pare-feu nécessaires.

    **Configure-SMremoting.exe -enable**

### <a name="BKMK_cmdline"></a>Pour activer la gestion à distance du Gestionnaire de serveur à l’aide de la ligne de commande

1.  Sur l’ordinateur que vous voulez gérer à distance, ouvrez une session d’invite de commandes avec des droits d’utilisateur élevés. Pour ce faire, sur le **Démarrer** , tapez **cmd**, avec le bouton droit le **invite de commandes** vignette lorsqu’elle est affichée dans le **applications** résultats, et Cliquez sur la barre des applications **exécuter en tant qu’administrateur**.

2.  Exécutez le fichier exécutable suivant :

    **%windir%\system32\Configure-SMremoting.exe**

3.  Faites une des actions suivantes :

    -   Pour désactiver la gestion à distance, tapez **SMremoting.exe-configurer-désactiver**, puis appuyez sur **entrée**.

    -   Pour activer la gestion à distance, tapez **SMremoting.exe-configurer-activer**, puis appuyez sur **entrée**.

    -   Pour afficher le paramètre actuel de la gestion à distance, tapez **SMremoting.exe-configurer-obtenir**, puis appuyez sur ENTRÉE.

### <a name="BKMK_old"></a>Pour activer la gestion à distance Server Manager et Windows PowerShell sur des versions antérieures de Windows Server

-   Faites une des actions suivantes :

    -   Pour activer la gestion à distance sur des serveurs qui exécutent Windows Server 2012, consultez [pour activer la gestion à distance du Gestionnaire de serveur à l’aide de l’interface Windows](#BKMK_windows) dans cette rubrique.

    -   Pour activer la gestion à distance sur les serveurs qui exécutent Windows Server 2008 R2, consultez [gestion à distance avec le Gestionnaire de serveur](https://go.microsoft.com/fwlink/?LinkID=137378) dans l’aide de Windows Server 2008 R2.

    -   Pour activer la gestion à distance sur des serveurs qui exécutent Windows Server 2008, consultez [activer et utiliser des commandes à distance dans Windows PowerShell](https://go.microsoft.com/fwlink/p/?LinkId=242565).

### <a name="BKMK_dcom"></a>Pour configurer mmc ou autres tâches de gestion à distance d’outil via DCOM

1.  Procédez de l’une des manières suivantes pour ouvrir le composant logiciel enfichable Pare-feu Windows avec fonctions avancées de sécurité.

    -   Dans le **propriétés** zone de la **serveur Local** dans le Gestionnaire de serveur, cliquez sur la valeur hypertexte pour le **Windows Firewall** propriété, puis cliquez sur  **Paramètres avancés**.

    -   Sur le **Démarrer** , tapez **WF.msc**, puis cliquez sur la vignette du composant logiciel enfichable lorsqu’elle est affichée dans le **applications** résultats.

2.  Dans le volet de l’arborescence, sélectionnez **Règles de trafic entrant**.

3.  Vérifiez que les exceptions aux règles de pare-feu suivantes sont activées et n’ont pas été désactivées par les paramètres de stratégie de groupe. Si certaines sont désactivées, passez à l’étape suivante.

    -   Accès réseau COM+ (DCOM-In)

    -   Gestion à distance de journal des événements (NP-Entrée)

    -   Gestion du journal des événements à distance (RPC)

    -   Gestion des journaux des événements à distance (RPC-EPMAP)

4.  Cliquez avec le bouton droit sur les règles qui ne sont pas activées, puis cliquez sur **Activer la règle** dans le menu contextuel.

5.  Fermez le composant logiciel enfichable Pare-feu Windows avec fonctions avancées de sécurité.

### <a name="BKMK_disableGP"></a>Pour désactiver la gestion à distance à l’aide de stratégie de groupe

1.  Effectuez l’une des opérations suivantes pour ouvrir l’éditeur de stratégie de groupe locale.

    -   Sur un serveur qui s’exécute sur Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012, le **Démarrer** , tapez **gpedit.msc**, puis cliquez sur le **gpedit** vignette Lorsqu’il est affiché.

    -   Sur un serveur qui exécute Windows Server 2008 R2 ou Windows Server 2008, dans le **exécuter** boîte de dialogue, tapez **gpedit.msc**, puis appuyez sur **entrée**.

2.  Ouvrez **ordinateur configuration de l’ordinateur\Modèles administratifs\Composants Windows\Windows distant Management (WinRM) \WinRM Service**.

3.  Dans le volet de contenu, double-cliquez sur **Autoriser la gestion des services à distance via WinRM**.

4.  Dans la boîte de dialogue pour le paramètre de stratégie **Autoriser la gestion des services à distance via WinRM** , sélectionnez **Désactivé** pour désactiver la gestion à distance. Cliquez sur **OK** pour enregistrer les modifications et fermer la boîte de dialogue du paramètre de stratégie.

### <a name="BKMK_unattend"></a>Pour désactiver la gestion à distance à l’aide d’un fichier de réponses lors de l’installation sans assistance

1.  créer un fichier de réponses d’installation sans assistance pour les installations de Windows Server 2016 à l’aide de Windows System Image Manager (Windows SIM). Pour plus d’informations sur la façon de créer un fichier de réponses et d’utiliser Windows SIM, consultez [What ' s Windows System Image Manager ?](https://technet.microsoft.com/library/cc766347.aspx) et [pas à pas : Déploiement de Windows de base pour les professionnels de l’informatique](https://technet.microsoft.com/library/dd349348.aspx).

2.  Dans votre fichier de réponses, recherchez le paramètre **Microsoft-Windows-Web-Services-for-Management-Core\EnableServerremoteManagement**.

3.  Pour désactiver la gestion à distance du Gestionnaire de serveur par défaut sur tous les serveurs auxquels vous souhaitez appliquer le fichier de réponses, affectez **Microsoft-Windows-Web-Services-for-Management-Core \EnableServerremoteManagement** à **False** .

    > [!NOTE]
    > Ce paramètre désactive la gestion à distance dans le cadre du processus de configuration du système d’exploitation. Configuration de ce paramètre n’empêche pas un administrateur d’activer la gestion à distance du Gestionnaire de serveur sur un serveur après l’installation du système d’exploitation. Les administrateurs peuvent activer la gestion à distance à nouveau à l’aide des étapes dans le Gestionnaire de serveur [pour configurer la gestion à distance du Gestionnaire de serveur à l’aide de l’interface Windows](#BKMK_windows) ou [pour activer la gestion à distance du Gestionnaire de serveur à l’aide de Windows PowerShell](#BKMK_ps) dans cette rubrique.
    > 
    > Si vous désactivez la gestion à distance par défaut dans le cadre d’une installation sans assistance et que vous n’activez pas la gestion à distance sur le serveur après l’installation, les serveurs auxquels ce fichier de réponses est appliqué ne peut pas être entièrement gérés à l’aide du Gestionnaire de serveur. Les serveurs qui exécutent Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 (et pour lesquels la gestion à distance désactivée par défaut) génèrent des erreurs d’état de la facilité de gestion dans la console du Gestionnaire de serveur après leur ajout au serveur du Gestionnaire de serveur pool.

## <a name="windows-remote-management-winrm-listener-settings"></a>Paramètres de l’écouteur Windows à distance Management (WinRM)
Le Gestionnaire de serveur s’appuie sur les paramètres de l’écouteur WinRM par défaut sur les serveurs distants que vous souhaitez gérer. Si le mécanisme d’authentification par défaut ou le numéro de port d’écouteur WinRM sur un serveur distant a été modifié à partir des paramètres par défaut, le Gestionnaire de serveur ne peut pas communiquer avec le serveur distant.

La liste suivante indique des paramètres de l’écouteur WinRM par défaut pour la gestion à l’aide du Gestionnaire de serveur.

-   Le service WinRM est en cours d’exécution.

-   Un écouteur WinRM est créé pour accepter les demandes HTTP via le numéro de port 5985.

-   Le numéro de port 5985 est activé dans les paramètres du Pare-feu Windows pour autoriser les demandes via WinRM.

-   Les deux types d’authentification **Kerberos** et **Négocier** sont activés.

Le numéro de port par défaut pour les communications de WinRM avec un ordinateur distant est 5985.

Pour plus d’informations sur la configuration des paramètres de l’écouteur WinRM, à l’invite de commandes, tapez **winrm help config**, puis appuyez sur ENTRÉE.

## <a name="see-also"></a>Voir aussi
[ajouter des serveurs au Gestionnaire de serveur](add-servers-to-server-manager.md)
[Windows PowerShell : about_remote_Troubleshooting sur le TechCenter Windows Server](https://technet.microsoft.com/library/dd347642.aspx)
[Description du contrôle de compte utilisateur](https://support.microsoft.com/kb/951016)



