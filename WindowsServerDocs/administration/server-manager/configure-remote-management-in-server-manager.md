---
title: Configurer la gestion à distance dans Gestionnaire de serveur
description: Gestionnaire de serveur
ms.prod: windows-server
ms.technology: manage-server-manager
ms.topic: article
ms.assetid: 509182ed-c37d-4b81-84bc-aee43d006873
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 01bc2d2d262882c08d1213bae6149896a8b284ab
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851562"
---
# <a name="configure-remote-management-in-server-manager"></a>Configurer la gestion à distance dans Gestionnaire de serveur

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dans Windows Server, vous pouvez utiliser Gestionnaire de serveur pour effectuer des tâches de gestion sur des serveurs distants. la gestion à distance est activée par défaut sur les serveurs qui exécutent Windows Server 2016. Pour gérer un serveur à distance à l’aide de Gestionnaire de serveur, vous ajoutez le serveur au pool de serveurs Gestionnaire de serveur.

Vous pouvez utiliser Gestionnaire de serveur pour gérer les serveurs distants qui exécutent des versions antérieures de Windows Server, mais les mises à jour suivantes sont requises pour gérer entièrement ces anciens systèmes d’exploitation.

Pour gérer les serveurs qui exécutent des versions de Windows Server antérieures à Windows Server 2016, installez les logiciels et mises à jour suivants afin de rendre les versions plus anciennes de Windows Server gérables à l’aide de Gestionnaire de serveur dans Windows Server 2016.

|Système d'exploitation|Logiciels requis|Facilité de gestion|
|----------|-----------|---------|
| Windows Server 2012 R2 ou Windows Server 2012 |-   [.NET Framework 4,6](https://www.microsoft.com/download/details.aspx?id=45497)<br />-   [Windows Management Framework 5,0](https://go.microsoft.com/fwlink/?LinkID=395058). Windows Management Framework 5,0 Télécharger les fournisseurs de mises à jour du package Windows Management Instrumentation (WMI) sur Windows Server 2012 R2, Windows Server 2012 et Windows Server 2008 R2. Les fournisseurs WMI mis à jour permettent Gestionnaire de serveur collecter des informations sur les rôles et les fonctionnalités qui sont installés sur les serveurs gérés. Tant que la mise à jour n’est pas appliquée, les serveurs qui exécutent Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2008 R2 ont l’état de facilité de gestion **non accessible**.<br />-La mise à jour des performances associée à [l’article 2682011](https://go.microsoft.com/fwlink/p/?LinkID=245487) de la base de connaissances n’est plus nécessaire sur les serveurs qui exécutent windows server 2012 R2 ou windows server 2012.||
| Windows Server 2008 R2 |-   [.NET Framework 4,5](https://www.microsoft.com/download/details.aspx?id=30653)<br />-   [Windows Management Framework 4,0](https://go.microsoft.com/fwlink/?LinkId=293881). Windows Management Framework 4,0 Télécharger les fournisseurs de mises à jour du package Windows Management Instrumentation (WMI) sur Windows Server 2008 R2. Les fournisseurs WMI mis à jour permettent Gestionnaire de serveur collecter des informations sur les rôles et les fonctionnalités qui sont installés sur les serveurs gérés. Tant que la mise à jour n’est pas appliquée, les serveurs qui exécutent Windows Server 2008 R2 ont l’état de facilité de gestion **non accessible**.<br />-La mise à jour des performances associée à [l’article 2682011](https://go.microsoft.com/fwlink/p/?LinkID=245487) de la base de connaissances permet gestionnaire de serveur collecter les données de performances à partir de Windows Server 2008 R2.||
| Windows Server 2008 |-   [.NET Framework 4](https://www.microsoft.com/download/en/details.aspx?id=17718)<br />-   [Windows Management framework 3,0](https://go.microsoft.com/fwlink/p/?LinkID=229019) , Windows management Framework 3,0 Télécharger les fournisseurs de mises à jour du package Windows Management Instrumentation (WMI) sur windows Server 2008. Les fournisseurs WMI mis à jour permettent Gestionnaire de serveur collecter des informations sur les rôles et les fonctionnalités qui sont installés sur les serveurs gérés. Tant que la mise à jour n’est pas appliquée, les serveurs qui exécutent Windows Server 2008 ont l’état de facilité de gestion **non accessible-Vérifiez que les versions antérieures exécutent Windows Management Framework 3,0**.<br />-La mise à jour des performances associée à [l’article 2682011](https://go.microsoft.com/fwlink/p/?LinkID=245487) de la base de connaissances permet gestionnaire de serveur collecter des données de performances à partir de Windows Server 2008.||

Pour plus d’informations sur la façon d’ajouter des serveurs qui se trouvent dans des groupes de travail à gérer ou de gérer des serveurs distants à partir d’un ordinateur de groupe de travail qui exécute Gestionnaire de serveur, voir [Ajouter des serveurs à des gestionnaire de serveur](add-servers-to-server-manager.md).

## <a name="enabling-or-disabling-remote-management"></a>Activation ou désactivation de l’administration à distance
Dans Windows Server 2016, la gestion à distance est activée par défaut. Avant de pouvoir vous connecter à un ordinateur qui exécute Windows Server 2016 à distance à l’aide de Gestionnaire de serveur, Gestionnaire de serveur administration à distance doit être activée sur l’ordinateur de destination si elle a été désactivée. Les procédures fournies dans cette section décrivent la manière de désactiver l’administration à distance et la manière de la réactiver si elle a été désactivée. Dans la console Gestionnaire de serveur, l’état de la gestion à distance pour le serveur local est affiché dans la zone **Propriétés** de la page **serveur local** .

Les comptes d’administrateur local autres que le compte Administrateur intégré n’ont peut-être pas les droits requis pour gérer un serveur à distance, même si l’administration à distance est activée. Le paramètre de Registre **LocalAccountTokenFilterPolicy** du contrôle de compte d’utilisateur distant doit être configuré pour autoriser les comptes locaux du groupe administrateurs autres que le compte administrateur intégré à gérer à distance le serveur.

Dans Windows Server 2016, Gestionnaire de serveur s’appuie sur la gestion à distance de Windows (WinRM) et le modèle DCOM (Distributed Component Object Model) pour les communications à distance. Les paramètres qui sont contrôlés par la boîte de dialogue **configurer l’administration à distance** affectent uniquement les parties de gestionnaire de serveur et Windows PowerShell qui utilisent WinRM pour les communications à distance. Ils n’affectent pas les parties de Gestionnaire de serveur qui utilisent DCOM pour les communications à distance. Par exemple, Gestionnaire de serveur utilise WinRM pour communiquer avec les serveurs distants qui exécutent Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012, mais utilise DCOM pour communiquer avec les serveurs qui exécutent Windows Server 2008 et Windows Server 2008 R2, mais pour lesquels les mises à jour de Windows [Management framework 4,0](https://go.microsoft.com/fwlink/?LinkId=293881) ou de [windows Management Framework 3,0](https://go.microsoft.com/fwlink/p/?LinkID=229019) ne sont pas appliquées. La console MMC (Microsoft Management Console) et d’autres outils de gestion hérités utilisent DCOM. Pour plus d’informations sur la modification de ces paramètres, consultez [pour configurer MMC ou un autre outil de gestion à distance via DCOM](#to-configure-mmc-or-other-tool-remote-management-over-dcom) dans cette rubrique.

> [!NOTE]
> Les procédures de cette section ne peuvent être réalisées que sur des ordinateurs qui exécutent Windows Server. Vous ne pouvez pas activer ou désactiver la gestion à distance sur un ordinateur qui exécute Windows 10 à l’aide de ces procédures, car le système d’exploitation client ne peut pas être géré à l’aide de Gestionnaire de serveur.

-   Pour activer l’administration à distance via WinRM, sélectionnez l’une des procédures suivantes :

    -   [Pour activer la gestion à distance Gestionnaire de serveur à l’aide de l’interface Windows](#to-enable-server-manager-remote-management-by-using-the-windows-interface)

    -   [Pour activer la gestion à distance Gestionnaire de serveur à l’aide de Windows PowerShell](#to-enable-server-manager-remote-management-by-using-windows-powershell)

    -   [Pour activer la gestion à distance Gestionnaire de serveur à l’aide de la ligne de commande](#to-enable-server-manager-remote-management-by-using-the-command-line)

    -   [Pour activer la Gestionnaire de serveur et la gestion à distance de Windows PowerShell sur les versions antérieures de Windows Server](#to-enable-server-manager-and-windows-powershell-remote-management-on-earlier-releases-of-windows-server)

-   Pour désactiver WinRM et Gestionnaire de serveur administration à distance, sélectionnez l’une des procédures suivantes.

    -   [Pour désactiver la gestion à distance à l’aide de stratégie de groupe](#to-disable-remote-management-by-using-group-policy)

    -   [Pour désactiver la gestion à distance à l’aide d’un fichier de réponses au cours d’une installation sans assistance](#to-disable-remote-management-by-using-an-answer-file-during-unattended-installation)

-   Pour configurer la gestion à distance via DCOM, voir [Pour configurer la gestion à distance via DCOM](#to-configure-mmc-or-other-tool-remote-management-over-dcom).

### <a name="to-enable-server-manager-remote-management-by-using-the-windows-interface"></a>Activer l’administration à distance via le Gestionnaire de serveur à l’aide de l’interface Windows

1.  > [!NOTE]
    > Les paramètres contrôlés par la boîte de dialogue **configurer l’administration à distance** n’affectent pas les parties de gestionnaire de serveur qui utilisent DCOM pour les communications à distance.

    Sur l’ordinateur que vous souhaitez gérer à distance, ouvrez Gestionnaire de serveur, s’il n’est pas déjà ouvert. Dans la barre des tâches Windows, cliquez sur **Gestionnaire de serveur**. Dans l’écran d' **Accueil** , cliquez sur la vignette **Gestionnaire de serveur** .

2.  Dans la zone **Propriétés** de la page **serveurs locaux** , cliquez sur la valeur de lien hypertexte pour la propriété **gestion à distance** .

3.  Effectuez l’une des opérations ci-dessous, puis cliquez sur **OK**.

    -   Pour empêcher la gestion à distance de cet ordinateur à l’aide de Gestionnaire de serveur (ou Windows PowerShell s’il est installé), désactivez la case à cocher **activer la gestion à distance de ce serveur à partir des autres ordinateurs** .

    -   Pour permettre la gestion à distance de cet ordinateur à l’aide de Gestionnaire de serveur ou de Windows PowerShell, sélectionnez **activer la gestion à distance de ce serveur à partir d’autres ordinateurs**.

### <a name="to-enable-server-manager-remote-management-by-using-windows-powershell"></a>Activer l’administration à distance via le Gestionnaire de serveur à l’aide de Windows PowerShell

1.  Sur l’ordinateur que vous souhaitez gérer à distance, effectuez l’une des opérations suivantes pour ouvrir une session Windows PowerShell avec des droits d’utilisateur élevés.

    -   Sur le Bureau Windows, cliquez avec le bouton droit dans la barre des tâches sur **Windows PowerShell** , puis cliquez sur **Exécuter en tant qu’administrateur**.

    -   Sur l’écran d' **Accueil** de Windows, cliquez avec le bouton droit sur **Windows PowerShell**, puis dans la barre des applications, cliquez sur **exécuter en tant qu’administrateur**.

2.  tapez ce qui suit, puis appuyez sur **entrée** pour activer toutes les exceptions de règle de pare-feu requises.

    **Configure-SMRemoting. exe-Enable**

### <a name="to-enable-server-manager-remote-management-by-using-the-command-line"></a>Activer l’administration à distance via le Gestionnaire de serveur dans la ligne de commande

1.  Sur l’ordinateur que vous voulez gérer à distance, ouvrez une session d’invite de commandes avec des droits d’utilisateur élevés. Pour ce faire, dans l’écran d' **Accueil** , tapez **cmd**, cliquez avec le bouton droit sur la vignette **invite de commandes** lorsqu’elle est affichée dans les résultats **applications** , puis cliquez sur **exécuter en tant qu’administrateur**dans la barre des applications.

2.  Exécutez le fichier exécutable suivant :

    **%windir%\system32\Configure-SMremoting.exe**

3.  Effectuez l'une des opérations suivantes :

    -   Pour désactiver la gestion à distance, tapez configure **-SMRemoting. exe-Disable**, puis appuyez sur **entrée**.

    -   Pour activer la gestion à distance, tapez configure **-SMRemoting. exe-Enable**, puis appuyez sur **entrée**.

    -   Pour afficher le paramètre de gestion à distance actuel, tapez configure **-SMRemoting. exe-obtenir**, puis appuyez sur entrée.

### <a name="to-enable-server-manager-and-windows-powershell-remote-management-on-earlier-releases-of-windows-server"></a>Activer l’administration à distance via le Gestionnaire de serveur et Windows PowerShell dans des versions antérieures de Windows Server

-   Effectuez l'une des opérations suivantes :

    -   Pour activer la gestion à distance sur les serveurs qui exécutent Windows Server 2012, consultez [pour activer la gestion à distance gestionnaire de serveur à l’aide de l’interface Windows](#to-enable-server-manager-remote-management-by-using-the-windows-interface) dans cette rubrique.

    -   Pour activer la gestion à distance sur les serveurs qui exécutent Windows Server 2008 R2, consultez [gestion à distance avec Gestionnaire de serveur](https://go.microsoft.com/fwlink/?LinkID=137378) dans l’aide de windows Server 2008 R2.

    -   Pour activer la gestion à distance sur les serveurs qui exécutent Windows Server 2008, consultez [activer et utiliser des commandes distantes dans Windows PowerShell](https://go.microsoft.com/fwlink/p/?LinkId=242565).

### <a name="to-configure-mmc-or-other-tool-remote-management-over-dcom"></a>Pour configurer MMC ou un autre outil de gestion à distance via DCOM

1.  Procédez de l’une des manières suivantes pour ouvrir le composant logiciel enfichable Pare-feu Windows avec fonctions avancées de sécurité.

    -   Dans la zone **Propriétés** de la page **serveur local** de gestionnaire de serveur, cliquez sur la valeur hypertexte de la propriété **pare-feu Windows** , puis cliquez sur **Paramètres avancés**.

    -   Dans l’écran d' **Accueil** , tapez **WF. msc**, puis cliquez sur la vignette du composant logiciel enfichable lorsqu’elle est affichée dans les résultats **applications** .

2.  Dans le volet de l’arborescence, sélectionnez **Règles de trafic entrant**.

3.  Vérifiez que les exceptions aux règles de pare-feu suivantes sont activées et qu’elles n’ont pas été désactivées par les paramètres de stratégie de groupe. Si certaines sont désactivées, passez à l’étape suivante.

    -   Accès réseau COM+ (DCOM-In)

    -   Gestion à distance des journaux des événements (NP-in)

    -   Gestion à distance des journaux des événements (RPC)

    -   Gestion à distance des journaux des événements (RPC-EPMAP)

4.  Cliquez avec le bouton droit sur les règles qui ne sont pas activées, puis cliquez sur **Activer la règle** dans le menu contextuel.

5.  Fermez le composant logiciel enfichable Pare-feu Windows avec fonctions avancées de sécurité.

### <a name="to-disable-remote-management-by-using-group-policy"></a>Pour désactiver la gestion à distance à l’aide d’une stratégie de groupe

1.  Effectuez l’une des opérations suivantes pour ouvrir l’éditeur de stratégie de groupe local.

    -   Sur un serveur qui exécute Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012, dans l’écran d' **Accueil** , tapez **gpedit. msc**, puis cliquez sur la vignette **gpedit** lorsqu’elle est affichée.

    -   Sur un serveur qui exécute Windows Server 2008 R2 ou Windows Server 2008, dans la boîte de dialogue **exécuter** , tapez **gpedit. msc**, puis appuyez sur **entrée**.

2.  Ouvrez **Configuration ordinateur \ modèles d’administration\Composants Windows\Windows Remote Management (WinRM) \Service WinRM service**.

3.  Dans le volet de contenu, double-cliquez sur **Autoriser la gestion des services à distance via WinRM**.

4.  Dans la boîte de dialogue pour le paramètre de stratégie **Autoriser la gestion des services à distance via WinRM** , sélectionnez **Désactivé** pour désactiver la gestion à distance. Cliquez sur **OK** pour enregistrer les modifications et fermer la boîte de dialogue du paramètre de stratégie.

### <a name="to-disable-remote-management-by-using-an-answer-file-during-unattended-installation"></a>Pour désactiver la gestion à distance à l’aide d’un fichier de réponses au cours d’une installation sans assistance

1.  Créez un fichier de réponses d’installation sans assistance pour les installations de Windows Server 2016 à l’aide de Windows System Image Manager (Windows SIM). Pour plus d’informations sur la création d’un fichier de réponses et l’utilisation de l’Assistant Gestion d’installation, voir [Qu’est-ce que l’Assistant Gestion d’installation ?](https://technet.microsoft.com/library/cc766347.aspx) et [Procédure pas à pas : déploiement basique de Windows pour les professionnels de l’informatique](https://technet.microsoft.com/library/dd349348.aspx).

2.  Dans votre fichier de réponses, recherchez le paramètre **Microsoft-Windows-Web-Services-for-Management-Core\EnableServerremoteManagement**.

3.  Pour désactiver la gestion à distance Gestionnaire de serveur par défaut sur tous les serveurs auxquels vous voulez appliquer le fichier de réponses, affectez la valeur **false**à **Microsoft-Windows-Web-Services-for-Management-Core \EnableServerremoteManagement** .

    > [!NOTE]
    > Ce paramètre désactive la gestion à distance dans le cadre du processus de configuration du système d’exploitation. La configuration de ce paramètre n’empêche pas un administrateur d’activer la gestion à distance Gestionnaire de serveur sur un serveur une fois l’installation du système d’exploitation terminée. Les administrateurs peuvent réactiver Gestionnaire de serveur administration à distance en suivant les étapes de [la section pour configurer gestionnaire de serveur administration à distance à l’aide de l’interface Windows](#to-enable-server-manager-remote-management-by-using-the-windows-interface) ou [pour activer la gestion à distance gestionnaire de serveur à l’aide de Windows PowerShell](#to-enable-server-manager-remote-management-by-using-windows-powershell) dans cette rubrique.
    > 
    > Si vous désactivez la gestion à distance par défaut dans le cadre d’une installation sans assistance et que vous n’activez pas la gestion à distance sur le serveur après l’installation, les serveurs auxquels ce fichier de réponses est appliqué ne peuvent pas être entièrement gérés à l’aide de Gestionnaire de serveur. Les serveurs qui exécutent Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 (et dont la gestion à distance est désactivée par défaut) génèrent des erreurs d’état de la gestion dans la console Gestionnaire de serveur une fois qu’elles ont été ajoutées au pool de serveurs Gestionnaire de serveur.

## <a name="windows-remote-management-winrm-listener-settings"></a>Paramètres de l’écouteur de gestion à distance de Windows (WinRM)
Gestionnaire de serveur s’appuie sur les paramètres de l’écouteur WinRM par défaut sur les serveurs distants que vous souhaitez gérer. Si le mécanisme d’authentification par défaut ou le numéro de port de l’écouteur WinRM sur un serveur distant a été modifié par rapport aux paramètres par défaut, Gestionnaire de serveur ne peut pas communiquer avec le serveur distant.

La liste suivante affiche les paramètres par défaut de l’écouteur WinRM pour la gestion à l’aide de Gestionnaire de serveur.

-   Le service WinRM est en cours d’exécution.

-   Un écouteur WinRM est créé pour accepter les demandes HTTP via le numéro de port 5985.

-   Le numéro de port 5985 est activé dans les paramètres du Pare-feu Windows pour autoriser les demandes via WinRM.

-   Les deux types d’authentification **Kerberos** et **Négocier** sont activés.

Le numéro de port par défaut pour les communications de WinRM avec un ordinateur distant est 5985.

Pour plus d’informations sur la configuration des paramètres de l’écouteur WinRM, à l’invite de commandes, tapez **WinRM Help config**, puis appuyez sur entrée.

## <a name="see-also"></a>Voir aussi
[Ajouter des serveurs à Gestionnaire de serveur](add-servers-to-server-manager.md)
[windows PowerShell : About_remote_Troubleshooting sur le TechCenter Windows Server](https://technet.microsoft.com/library/dd347642.aspx)
[Description du contrôle de compte d’utilisateur](https://support.microsoft.com/kb/951016)



