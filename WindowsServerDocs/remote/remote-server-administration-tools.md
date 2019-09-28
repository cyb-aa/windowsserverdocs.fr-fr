---
title: Outils d’administration de serveur distant
description: Rubrique de niveau supérieur pour Outils d’administration de serveur distant
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-rsat
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: d54a1f5e-af68-497e-99be-97775769a7a7
author: coreyp-at-msft
ms.author: coreyp
manager: dansimp
ms.openlocfilehash: 121914f721cda7cbf0a117527b69568032d5541b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387091"
---
# <a name="remote-server-administration-tools"></a>Outils d’administration de serveur distant

>S’applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique prend en charge Outils d’administration de serveur distant pour Windows 10.

> [!IMPORTANT]
> À compter de la mise à jour 2018 de Windows 10 octobre, les outils d’aide à la demande sont inclus sous la forme d’un ensemble de **fonctionnalités à la demande** dans Windows 10. Pour obtenir des instructions d’installation, consultez **quand utiliser la version de RSAT** ci-dessous.

Les outils d’administration de serveur distant permettent aux administrateurs informatiques de gérer les rôles et les fonctionnalités Windows Server à partir d’un PC Windows 10.

Outils d’administration de serveur distant comprend Gestionnaire de serveur, les composants logiciels enfichables MMC (Microsoft Management Console), les consoles, les fournisseurs et les applets de commande Windows PowerShell, ainsi que certains outils en ligne de commande pour la gestion des rôles et des fonctionnalités qui s’exécutent sur Windows Server.

Outils d’administration de serveur distant comprend des modules d’applet de commande Windows PowerShell qui peuvent être utilisés pour gérer des rôles et des fonctionnalités qui s’exécutent sur des serveurs distants. Bien que la gestion à distance de Windows PowerShell soit activée par défaut sur Windows Server 2016, elle n’est pas activée par défaut sur Windows 10. Pour exécuter des applets de commande qui font partie de Outils d’administration de serveur distant sur un serveur distant, exécutez `Enable-PSremoting` dans une session Windows PowerShell qui a été ouverte avec des droits d’utilisateur élevés (c’est-à-dire, exécutez en tant qu’administrateur) sur votre ordinateur client Windows après l’installation de Outils d’administration de serveur distant.

## <a name="BKMK_Thresh"></a>Outils d’administration de serveur distant pour Windows 10
Utilisez Outils d’administration de serveur distant pour Windows 10 pour gérer des technologies spécifiques sur les ordinateurs qui exécutent Windows Server 2016, Windows Server 2012 R2 et, dans certains cas limités, Windows Server 2012 ou Windows Server 2008 R2.

Outils d’administration de serveur distant pour Windows 10 prend en charge la gestion à distance des ordinateurs qui exécutent l’option d’installation minimale ou la configuration de l’interface serveur minimale de Windows Server 2016, Windows Server 2012 R2 et, dans certains cas, cas, les options d’installation minimale de Windows Server 2012. Toutefois, Outils d’administration de serveur distant pour Windows 10 ne peut pas être installé sur une version du système d’exploitation Windows Server.

### <a name="tools-available-in-this-release"></a>Outils disponibles dans cette version
pour obtenir la liste des outils disponibles dans Outils d’administration de serveur distant pour Windows 10, consultez le tableau [Outils d’administration de serveur distant (RSAT) pour les systèmes d’exploitation Windows](https://support.microsoft.com/help/2693643/remote-server-administration-tools-rsat-for-windows-operating-systems).

### <a name="system-requirements"></a>Configuration requise
Outils d’administration de serveur distant pour Windows 10 ne peut être installé que sur les ordinateurs qui exécutent Windows 10. Outils d’administration de serveur distant ne peut pas être installé sur les ordinateurs qui exécutent Windows RT 8,1 ou d’autres périphériques système sur puce.

Outils d’administration de serveur distant pour Windows 10 s’exécute sur les versions x86 et x64 de Windows 10.

> [!IMPORTANT]
> Outils d’administration de serveur distant pour Windows 10 ne doit pas être installé sur un ordinateur qui exécute des packs d’outils d’administration pour Windows 8.1, Windows 8, Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 ou Windows 2000 Server. Supprimer toutes les anciennes versions du Pack ou Outils d’administration de serveur distant d’outils d’administration, y compris les versions préliminaires antérieures et les versions des outils pour différentes langues ou paramètres régionaux de l’ordinateur avant d’installer l’administration de serveur distant Outils pour Windows 10.

Pour utiliser cette version de Gestionnaire de serveur pour accéder aux serveurs distants qui exécutent Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2008 R2 et les gérer, vous devez installer plusieurs mises à jour pour rendre les anciens systèmes d’exploitation Windows Server gérables à l’aide de se Gestionnaire ORT. Pour plus d’informations sur la préparation de Windows Server 2012 R2, Windows Server 2012 et Windows Server 2008 R2 pour la gestion à l’aide de Gestionnaire de serveur dans Outils d’administration de serveur distant pour Windows 10, consultez [gérer plusieurs serveurs distants avec un serveur Gestionnaire](https://technet.microsoft.com/library/hh831456.aspx).

Windows PowerShell et Gestionnaire de serveur administration à distance doivent être activés sur les serveurs distants pour les gérer à l’aide d’outils qui font partie de Outils d’administration de serveur distant pour Windows 10. La gestion à distance est activée par défaut sur les serveurs qui exécutent Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012. Pour plus d’informations sur la manière d’activer la gestion à distance, si elle a été désactivée, voir [Gérer plusieurs serveurs distants avec le Gestionnaire de serveur](https://go.microsoft.com/fwlink/p/?LinkId=241358).

## <a name="install-uninstall-and-turn-offon-rsat-tools"></a>Installer, désinstaller et désactiver/sur les outils RSAT

### <a name="use-features-on-demand-fod-to-install-specific-rsat-tools-on-windows-10-october-2018-update-or-later"></a>Utiliser les fonctionnalités à la demande (DOM) pour installer des outils RSAT spécifiques sur Windows 10 octobre 2018 Update, ou version ultérieure

À compter de la mise à jour 2018 de Windows 10 octobre, les outils d’aide à la demande sont inclus sous la forme d’un ensemble de **fonctionnalités à la demande** à partir de Windows 10 Maintenant, au lieu de télécharger un package RSAT, vous pouvez simplement accéder à **gérer les fonctionnalités facultatives** dans **paramètres** et cliquer sur **Ajouter une fonctionnalité** pour afficher la liste des outils RSAT disponibles. Sélectionnez et installez les outils RSAT spécifiques dont vous avez besoin. Pour voir la progression de l’installation, cliquez sur le bouton **précédent** pour afficher l’État dans la page **gérer les fonctionnalités facultatives** .

Consultez la [liste des outils RSAT disponibles via les **fonctionnalités à la demande**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-non-language-fod#remote-server-administration-tools-rsat). Outre l’installation via l’application de **paramètres** graphiques, vous pouvez également installer des outils RSAT spécifiques via la ligne de commande ou l’automatisation à l’aide de [**DISM/Add-Capability**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities#using-dism-add-capability-to-add-or-remove-fods).

L’un des avantages offerts par les fonctionnalités à la demande est que les fonctionnalités installées sont conservées entre les mises à niveau de version de Windows 10.

#### <a name="to-uninstall-specific-rsat-tools-on-windows-10-october-2018-update-or-later-after-installing-with-fod"></a>Pour désinstaller des outils RSAT spécifiques sur Windows 10 octobre 2018 Update ou version ultérieure (après l’installation de avec les DOM)

Sur Windows 10, ouvrez l’application **paramètres** , accédez à **gérer les fonctionnalités facultatives**, sélectionnez et désinstallez les outils RSAT spécifiques que vous souhaitez supprimer. Notez que dans certains cas, vous devrez désinstaller manuellement les dépendances. En particulier, si l’outil RSAT a est requis par l’outil RSAT B, le choix de désinstaller l’outil RSAT échoue si l’outil RSAT B est encore installé. Dans ce cas, désinstallez d’abord l’outil RSAT B, puis désinstallez l’outil RSAT A. Notez également que, dans certains cas, la désinstallation d’un outil RSAT peut sembler réussie même si l’outil est toujours installé. Dans ce cas, le redémarrage du PC terminera la suppression de l’outil.

Consultez la [liste des outils RSAT, y compris les dépendances](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-non-language-fod#remote-server-administration-tools-rsat). Outre la désinstallation via l’application de paramètres graphiques, vous pouvez également désinstaller des outils RSAT spécifiques via la ligne de commande ou l’automatisation à l’aide de [**DISM/Remove-Capability**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities#using-dism-add-capability-to-add-or-remove-fods).

### <a name="when-to-use-which-rsat-version"></a>Quand utiliser quelle version de RSAT

Si vous disposez d’une version de Windows 10 antérieure à la mise à jour 2018 d’octobre (1809), vous ne pourrez pas utiliser les **fonctionnalités à la demande**. Vous devrez télécharger et installer le package RSAT.

- **Installez les FODS RSAT directement à partir de Windows 10, comme indiqué ci-dessus**: Lors de l’installation sur Windows 10 octobre 2018 Update (1809) ou version ultérieure, pour la gestion de Windows Server 2019 ou des versions antérieures.

- **Téléchargez et installez le package WS_1803 RSAT, comme indiqué ci-dessous**: Lors de l’installation de sur Windows 10 avril 2018 (1803) ou version antérieure, pour la gestion de Windows Server, version 1803 ou Windows Server, version 1709.

- **Téléchargez et installez le package WS2016 RSAT, comme indiqué ci-dessous**: Lors de l’installation de sur Windows 10 avril 2018 (1803) ou version antérieure, pour la gestion de Windows Server 2016 ou des versions antérieures.

#### <a name="BKMK_installthresh"></a>Télécharger le package RSAT pour installer Outils d’administration de serveur distant pour Windows 10

1.  Téléchargez le package Outils d’administration de serveur distant pour Windows 10 à partir du [Centre de téléchargement Microsoft](https://go.microsoft.com/fwlink/?LinkID=404281). Vous pouvez exécuter le programme d’installation depuis le site web du Centre de téléchargement ou enregistrer le package de téléchargement sur un ordinateur local ou un partage.

    > [!IMPORTANT]
    > Vous ne pouvez installer Outils d’administration de serveur distant pour Windows 10 que sur les ordinateurs qui exécutent Windows 10. Outils d’administration de serveur distant ne peut pas être installé sur les ordinateurs qui exécutent Windows RT 8,1 ou d’autres périphériques système sur puce.

2.  Si vous enregistrez le package de téléchargement sur un ordinateur local ou un partage, double-cliquez sur le programme d’installation, **WindowsTH-KB2693643-x64.msu** ou **WindowsTH-KB2693643-x86.msu**, selon l’architecture de l’ordinateur sur lequel vous voulez installer les outils.

3.  Quand vous êtes invité à installer la mise à jour par la boîte de dialogue **Programme d’installation de Windows Update en mode autonome**, cliquez sur **Oui**.

4.  Lisez et acceptez les termes du contrat de licence. Cliquez sur **J’accepte**.

5.  L’installation prend quelques minutes.

##### <a name="to-uninstall-remote-server-administration-tools-for-windows-10-after-rsat-package-install"></a>Pour désinstaller Outils d’administration de serveur distant pour Windows 10 (après l’installation du package RSAT)

1. Sur le Bureau, cliquez sur **Démarrer**, **Toutes les applications**, **Système Windows**, puis sur **Panneau de configuration**.

2. Sous **Programmes**, cliquez sur **Désinstaller un programme**.

3. Cliquez sur **Afficher les mises à jour installées**.

4. Cliquez avec le bouton droit sur **Mise à jour pour Microsoft Windows (KB2693643)** , puis cliquez sur **Désinstaller**.

5. Quand vous devez indiquer si vous voulez vraiment désinstaller la mise à jour, cliquez sur **Oui**.
   S
   ##### <a name="to-turn-off-specific-tools-after-rsat-package-install"></a>Pour désactiver des outils spécifiques (après l’installation du package RSAT)

6. Sur le Bureau, cliquez sur **Démarrer**, **Toutes les applications**, **Système Windows**, puis sur **Panneau de configuration**.

7. Cliquez sur **Programmes**, puis dans **Programmes et fonctionnalités**, cliquez sur **Activer ou désactiver les fonctionnalités Windows**.

8. Dans la boîte de dialogue **Fonctionnalités de Windows**, développez **Outils d’administration de serveur distant**, puis développez soit **Outils d’administration de rôles**, soit **Outils d’administration de fonctionnalités**.

9. Désactivez les cases à cocher des outils que vous souhaitez désactiver.

   > [!NOTE]
   > Si vous désactivez Gestionnaire de serveur, l’ordinateur doit être redémarré et les outils qui étaient accessibles à partir du menu **Outils** de gestionnaire de serveur doivent être ouverts à partir du dossier **Outils d’administration** .

10. Quand vous avez terminé de désactiver les outils que vous ne voulez pas utilisez, cliquez sur **OK**.

### <a name="run-remote-server-administration-tools"></a>Exécuter les Outils d’administration de serveur distant

> [!NOTE]
> Après l’installation de Outils d’administration de serveur distant pour Windows 10, le dossier **Outils d’administration** s’affiche dans le menu **Démarrer** . Vous pouvez accéder aux outils à partir des emplacements suivants :
>
> -   Le menu **Outils** de la console Gestionnaire de serveur.
> -   **Panneau de configuration/Système et sécurité/Outils d’administration**.
> -   Un raccourci enregistré sur le Bureau à partir du dossier **Outils d’administration** (pour ce faire, cliquez avec le bouton droit sur le lien **Panneau de configuration\Système et sécurité \Outils d’administration**, puis cliquez sur **Créer un raccourci**).

Les outils installés dans le cadre d’Outils d’administration de serveur distant pour Windows 10 ne peuvent pas être utilisés pour gérer l’ordinateur client local. Quel que soit l’outil que vous exécutez, vous devez spécifier un serveur distant ou plusieurs serveurs distants sur lesquels exécuter l’outil. Étant donné que la plupart des outils sont intégrés à Gestionnaire de serveur, vous ajoutez des serveurs distants que vous souhaitez gérer dans le pool de serveurs Gestionnaire de serveur avant de gérer le serveur à l’aide des outils du menu **Outils** . Pour plus d’informations sur la manière d’ajouter des serveurs à votre pool de serveurs et de créer des groupes personnalisés de serveurs, voir [Ajouter des serveurs au Gestionnaire de serveur](https://go.microsoft.com/fwlink/p/?LinkId=241353) et [Créer et gérer des groupes de serveurs](https://go.microsoft.com/fwlink/?LinkId=247328).

Dans Outils d’administration de serveur distant pour Windows 10, tous les outils de gestion de serveur basés sur l’interface graphique utilisateur, tels que les composants logiciels enfichables MMC et les boîtes de dialogue, sont accessibles à partir du menu **Outils** de la console Gestionnaire de serveur. Bien que l’ordinateur qui exécute Outils d’administration de serveur distant pour Windows 10 exécute un système d’exploitation basé sur le client, après avoir installé les outils, Gestionnaire de serveur, inclus avec Outils d’administration de serveur distant pour Windows 10, s’ouvre automatiquement par défaut. sur l’ordinateur client. Notez qu’il n’existe pas de page **serveur local** dans la console Gestionnaire de serveur qui s’exécute sur un ordinateur client.

##### <a name="to-start-server-manager-on-a-client-computer"></a>Pour démarrer le Gestionnaire de serveur sur un ordinateur client

1.  Dans le menu **Démarrer** , cliquez sur **Toutes les applications**, puis sur **Outils d’administration**.

2.  Dans le dossier **Outils d’administration**, cliquez sur **Gestionnaire de serveur**.

Bien qu’ils ne soient pas répertoriés dans le menu **Outils** de la console Gestionnaire de serveur, les applets de commande Windows PowerShell et les outils de gestion d’invite de commandes sont également installés pour les rôles et les fonctionnalités dans le cadre de outils d’administration de serveur distant. Par exemple, si vous ouvrez une session Windows PowerShell avec des droits d’utilisateur élevés (exécuter en tant qu’administrateur) et que vous exécutez l’applet de commande `Get-Command -Module RDManagement`, les résultats incluent une liste d’applets de commande des services Bureau à distance qui sont désormais disponibles pour s’exécuter sur l’ordinateur local après l’installation de Outils d’administration de serveur distant, tant que les applets de commande sont ciblées sur un serveur distant qui exécute l’ensemble ou une partie du rôle Services Bureau à distance.

##### <a name="to-start-windows-powershell-with-elevated-user-rights-run-as-administrator"></a>Pour démarrer Windows PowerShell avec des privilèges élevés (Exécuter en tant qu’administrateur)

1.  Dans le menu **Démarrer**, cliquez sur **Toutes les applications**, sur **Système Windows**, puis sur **Windows PowerShell**.

2.  Pour exécuter Windows PowerShell en tant qu’administrateur à partir du bureau, cliquez avec le bouton droit sur le raccourci **Windows PowerShell** , puis cliquez sur **exécuter en tant qu’administrateur**.

> [!NOTE]
> Vous pouvez également démarrer une session Windows PowerShell ciblée sur un serveur spécifique en cliquant avec le bouton droit sur un serveur géré dans une page de rôle ou de groupe dans Gestionnaire de serveur, puis en cliquant sur **Windows PowerShell**.


## <a name="known-issues"></a>Problèmes connus

### <a name="issue-rsat-fod-installation-fails-with-error-code-0x800f0954"></a>**Problème**: Échec de l’installation des DOM de RSAT avec le code d’erreur 0x800f0954

> **Impact**: RSAT FODs sur Windows 10 1809 (mise à jour d’octobre 2018) dans les environnements WSUS/SCCM
> 
> **Résolution**: Pour installer FODs sur un PC joint à un domaine qui reçoit des mises à jour via WSUS ou SCCM, vous devez modifier un paramètre de stratégie de groupe pour activer le téléchargement de FODs directement à partir de Windows Update ou d’un partage local. Pour plus d’informations et pour obtenir des instructions sur la façon de modifier ce paramètre, consultez [Comment rendre des fonctionnalités à la demande et modules linguistiques disponibles lorsque vous utilisez WSUS/SCCM](https://docs.microsoft.com/windows/deployment/update/fod-and-lang-packs).

---

### <a name="issue-rsat-fod-installation-via-settings-app-does-not-show-statusprogress"></a>**Problème**: L’installation des DOM RSAT via les paramètres de l’application n’affiche pas l’état/la progression

> **Impact**: RSAT FODs sur Windows 10 1809 (mise à jour d’octobre 2018)
> 
> **Résolution**: Pour voir la progression de l’installation, cliquez sur le bouton **précédent** pour afficher l’État dans la page **gérer les fonctionnalités facultatives** .

---

### <a name="issue-rsat-fod-uninstallation-via-settings-app-may-fail"></a>**Problème**: La désinstallation des DOM de RSAT via les paramètres de l’application peut échouer

> **Impact**: RSAT FODs sur Windows 10 1809 (mise à jour d’octobre 2018)
> 
> **Résolution**: Dans certains cas, les échecs de désinstallation sont dus à la nécessité de désinstaller manuellement les dépendances. En particulier, si l’outil RSAT a est requis par l’outil RSAT B, le choix de désinstaller l’outil RSAT échoue si l’outil RSAT B est encore installé. Dans ce cas, désinstallez d’abord l’outil RSAT B, puis désinstallez l’outil RSAT A. Consultez la liste des FODs RSAT, y compris les dépendances.

---

### <a name="issue-rsat-fod-uninstallation-appears-to-succeed-but-the-tool-is-still-installed"></a>**Problème**: La désinstallation des DOM de RSAT semble réussie, mais l’outil est toujours installé

> **Impact**: RSAT FODs sur Windows 10 1809 (mise à jour d’octobre 2018)
> 
> **Résolution**: Le redémarrage de l’ordinateur entraînera la suppression de l’outil.

---

### <a name="issue-rsat-missing-after-windows-10-upgrade"></a>**Problème**: RSAT manquant après la mise à niveau de Windows 10

> **Impact**: N’importe quel RSAT. L’installation du package MSU (avant les FODs RSAT) n’a pas été réinstallée automatiquement
> 
> **Résolution**: Une installation de RSAT ne peut pas être rendue persistante au-delà des mises à niveau du système d’exploitation en raison des RSAT. MSU remis en tant que package Windows Update. Veuillez réinstaller les outils d’installation à nouveau après la mise à niveau de Windows 10. Notez que cette limitation est l’une des raisons pour lesquelles nous avons déplacé vers FODs à partir de Windows 10 1809. Les FODs d’outils d’installation à redémarrage, qui sont installés, sont conservés dans les futures mises à niveau de version Windows 10.

## <a name="see-also"></a>Voir aussi
>- [Outils d’administration de serveur distant pour Windows 10](https://go.microsoft.com/fwlink/?LinkID=404281)
>- [Outils d’administration de serveur distant (RSAT) pour Windows Vista, Windows 7, Windows 8, Windows Server 2008, Windows Server 2008 R2, Windows Server 2012 et Windows Server 2012 R2](https://go.microsoft.com/fwlink/p/?LinkID=221055)
