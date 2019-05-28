---
title: Outils d’administration de serveur distant
description: Rubrique de haut niveau pour les outils d’Administration de serveur distant
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-rsat
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: d54a1f5e-af68-497e-99be-97775769a7a7
author: coreyp-at-msft
ms.author: coreyp
manager: dansimp
ms.openlocfilehash: 748010e80cf2b54926ca226a7af8c49f1aa16800
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192642"
---
# <a name="remote-server-administration-tools"></a>Outils d’administration de serveur distant

>S'applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique prend en charge à distance Server Administration Tools pour Windows 10.

> [!IMPORTANT]
> À compter Windows 10 octobre 2018 mise à jour, le serveur distant est inclus comme un ensemble de **fonctionnalités à la demande** dans Windows 10 lui-même. Consultez **quand utiliser la version RSAT** ci-dessous pour obtenir des instructions d’installation.

RSAT permet aux administrateurs informatiques de gérer des rôles et fonctionnalités Windows Server à partir d’un PC Windows 10.

Outils d’Administration de serveur distant comprennent le Gestionnaire de serveur, des composants logiciels enfichables Microsoft Management Console (mmc), les consoles, les applets de commande Windows PowerShell et les fournisseurs, ainsi que certains outils de ligne de commande pour la gestion des rôles et des fonctionnalités qui s’exécutent sur Windows Server.

Outils d’Administration de serveur à distance inclut les modules d’applet de commande Windows PowerShell qui peuvent être utilisées pour gérer des rôles et fonctionnalités qui s’exécutent sur des serveurs distants. Bien que la gestion à distance de Windows PowerShell est activée par défaut sur Windows Server 2016, il n’est pas activé par défaut sur Windows 10. Pour exécuter les applets de commande qui font partie des outils d’Administration de serveur distant sur un serveur distant, exécutez `Enable-PSremoting` dans une session Windows PowerShell qui a été ouverte avec des droits utilisateur élevés (c'est-à-dire, exécuter en tant qu’administrateur) sur votre ordinateur client de Windows après installation des outils d’Administration de serveur distant.

## <a name="BKMK_Thresh"></a>Outils d’Administration de serveur distant pour Windows 10
Utilisez distant Server Administration Tools pour Windows 10 pour gérer des technologies spécifiques sur les ordinateurs qui exécutent Windows Server 2016, Windows Server 2012 R2, et dans certains cas limités, Windows Server 2012 ou Windows Server 2008 R2.

À distance Server Administration Tools pour Windows 10 inclut la prise en charge pour la gestion à distance des ordinateurs qui exécutent l’option d’installation Server Core ou la configuration de l’Interface serveur minimale de Windows Server 2016, Windows Server 2012 R2 et dans limité cas, les options d’installation Server Core de Windows Server 2012. Toutefois, à distance Server Administration Tools pour Windows 10 ne peut pas être installé sur toutes les versions du système d’exploitation Windows Server.

### <a name="tools-available-in-this-release"></a>Outils disponibles dans cette version
Pour obtenir la liste des outils disponibles dans à distance Server Administration Tools pour Windows 10, consultez le tableau dans [Server Administration outils distant (RSAT) pour les systèmes d’exploitation Windows](https://support.microsoft.com/help/2693643/remote-server-administration-tools-rsat-for-windows-operating-systems).

### <a name="system-requirements"></a>Configuration requise
À distance Server Administration Tools pour Windows 10 peut être installé uniquement sur les ordinateurs qui exécutent Windows 10. Outils d’Administration de serveur distant ne peut pas être installé sur les ordinateurs qui exécutent Windows RT 8.1 ou autres périphériques système sur puce.

À distance Server Administration Tools pour Windows 10 s’exécute sur les x86 et x64 64 des éditions de Windows 10.

> [!IMPORTANT]
> À distance Server Administration Tools pour Windows 10 ne doit pas être installé sur un ordinateur qui exécute le Pack des outils d’administration pour Windows 8.1, Windows 8, Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 ou Windows 2000 Server. Supprimez toutes les anciennes versions du Pack des outils d’Administration ou des outils d’Administration de serveur distant, y compris les versions préliminaires antérieures et les versions des outils pour différentes langues ou paramètres régionaux de l’ordinateur avant d’installer d’Administration de serveur distant Outils pour Windows 10.

Pour utiliser cette version du Gestionnaire de serveur pour accéder et gérer des serveurs distants qui exécutent Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2008 R2, vous devez installer plusieurs mises à jour pour rendre les anciens systèmes d’exploitation Windows Server gérables à l’aide de Se le Gestionnaire de serveur. Pour plus d’informations sur la façon de préparer Windows Server 2012 R2, Windows Server 2012 et Windows Server 2008 R2 pour la gestion à l’aide du Gestionnaire de serveur dans distant Server Administration Tools pour Windows 10, consultez [Manage Multiple, serveurs distants avec le Gestionnaire de serveur](https://technet.microsoft.com/library/hh831456.aspx).

Gestion à distance de Windows PowerShell et le Gestionnaire de serveur doit être activée sur des serveurs distants pour les gérer à l’aide des outils qui font partie de distant Server Administration Tools pour Windows 10. Gestion à distance est activée par défaut sur les serveurs qui exécutent Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012. Pour plus d’informations sur la manière d’activer la gestion à distance, si elle a été désactivée, voir [Gérer plusieurs serveurs distants avec le Gestionnaire de serveur](https://go.microsoft.com/fwlink/p/?LinkId=241358).

## <a name="install-uninstall-and-turn-offon-rsat-tools"></a>Installer, désinstaller et activer/désactiver les outils RSAT

### <a name="use-features-on-demand-fod-to-install-specific-rsat-tools-on-windows-10-october-2018-update-or-later"></a>Mettre à jour de l’utilisation des fonctionnalités à la demande (DOM) pour installer les outils RSAT spécifiques sur Windows 10 octobre 2018, ou une version ultérieure

À compter Windows 10 octobre 2018 mise à jour, le serveur distant est inclus comme un ensemble de **fonctionnalités à la demande** directement à partir de Windows 10. Maintenant, au lieu de télécharger un package RSAT vous pouvez simplement accéder à **gestion des fonctionnalités facultatives** dans **paramètres** et cliquez sur **ajouter une fonctionnalité** pour afficher la liste des outils RSAT disponibles. Sélectionnez et installez les outils RSAT spécifiques que vous avez besoin. Pour afficher la progression de l’installation, cliquez sur le **retour** bouton pour afficher l’état sur le **gestion des fonctionnalités facultatives** page.

Consultez le [disponible par le biais des outils de liste de RSAT **fonctionnalités à la demande**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-non-language-fod#remote-server-administration-tools-rsat). Outre l’installation via le graphique **paramètres** application, vous pouvez également installer les outils RSAT spécifiques via la ligne de commande ou à l’aide de l’automation [ **DISM /Add-Capability**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities#using-dism-add-capability-to-add-or-remove-fods).

Un des avantages des fonctionnalités à la demande sont que les fonctionnalités installées conserver entre les mises à niveau de Windows 10 version !

#### <a name="to-uninstall-specific-rsat-tools-on-windows-10-october-2018-update-or-later-after-installing-with-fod"></a>Pour désinstaller le serveur distant spécifique outils sur Windows 10 octobre 2018 mise à jour ou une version ultérieure (après avoir installé avec DOM)

Sur Windows 10, ouvrez le **paramètres** application, accédez à **gestion des fonctionnalités facultatives**, sélectionnez et désinstaller les outils RSAT spécifiques que vous souhaitez supprimer. Notez que dans certains cas, vous devez désinstaller manuellement les dépendances. Plus précisément, si l’outil de serveur distant A est requis par l’outil RSAT B, puis en choisissant désinstaller un outil RSAT échoue si RSAT outil B est toujours installé. Dans ce cas, désinstallez d’abord l’outil RSAT B et puis désinstallez RSAT, outil A. Notez également que dans certains cas, la désinstallation d’un outil RSAT peut sembler réussir même si l’outil est toujours installé. Dans ce cas, le redémarrage de l’ordinateur va terminer la suppression de l’outil.

Consultez le [la liste des outils RSAT, notamment les dépendances](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-non-language-fod#remote-server-administration-tools-rsat). En plus de la désinstallation via l’application paramètres du graphique, vous pouvez également désinstaller les outils RSAT spécifiques via la ligne de commande ou à l’aide de l’automation [ **DISM /Remove-Capability**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities#using-dism-add-capability-to-add-or-remove-fods).

### <a name="when-to-use-which-rsat-version"></a>Quand utiliser la version de serveur distant

Si vous avez une version de Windows 10 avant l’octobre 2018 (1809) de mettre à jour, vous ne serez pas en mesure d’utiliser **fonctionnalités à la demande**. Vous devez télécharger et installer le package RSAT.

- **Installez RSAT FODs directement à partir de Windows 10, comme indiqué ci-dessus**: Lors de l’installation sur Windows 10 octobre 2018 mettre à jour (1809) ou version ultérieure, pour la gestion de Windows Server 2019 ou des versions antérieures.

- **Téléchargez et installez le package RSAT de WS_1803, comme indiqué ci-dessous**: Lors de l’installation sur Windows 10 avril 2018 mettre à jour (1803) ou une version antérieure, pour la gestion de Windows Server, version 1803 ou Windows Server, version 1709.

- **Téléchargez et installez le package RSAT de WS2016, comme indiqué ci-dessous**: Lors de l’installation sur Windows 10 avril 2018 mettre à jour (1803) ou une version antérieure, pour la gestion de Windows Server 2016 ou des versions antérieures.

#### <a name="BKMK_installthresh"></a>Télécharger le package RSAT pour installer à distance Server Administration Tools pour Windows 10

1.  Télécharger le package à distance Server Administration Tools pour Windows 10 à partir de la [Microsoft Download Center](https://go.microsoft.com/fwlink/?LinkID=404281). Vous pouvez exécuter le programme d’installation depuis le site web du Centre de téléchargement ou enregistrer le package de téléchargement sur un ordinateur local ou un partage.

    > [!IMPORTANT]
    > Vous pouvez uniquement installer à distance Server Administration Tools pour Windows 10 sur les ordinateurs qui exécutent Windows 10. Outils d’Administration de serveur distant ne peut pas être installé sur les ordinateurs qui exécutent Windows RT 8.1 ou autres périphériques système sur puce.

2.  Si vous enregistrez le package de téléchargement sur un ordinateur local ou un partage, double-cliquez sur le programme d’installation, **WindowsTH-KB2693643-x64.msu** ou **WindowsTH-KB2693643-x86.msu**, selon l’architecture de l’ordinateur sur lequel vous voulez installer les outils.

3.  Quand vous êtes invité à installer la mise à jour par la boîte de dialogue **Programme d’installation de Windows Update en mode autonome**, cliquez sur **Oui**.

4.  Lisez et acceptez les termes du contrat de licence. Cliquez sur **J’accepte**.

5.  L’installation prend quelques minutes.

##### <a name="to-uninstall-remote-server-administration-tools-for-windows-10-after-rsat-package-install"></a>Pour désinstaller les outils d’Administration de serveur distant pour Windows 10 (après l’installation du package RSAT)

1.  Sur le Bureau, cliquez sur **Démarrer**, **Toutes les applications**, **Système Windows**, puis sur **Panneau de configuration**.

2.  Sous **Programmes**, cliquez sur **Désinstaller un programme**.

3.  Cliquez sur **Afficher les mises à jour installées**.

4.  Cliquez avec le bouton droit sur **Mise à jour pour Microsoft Windows (KB2693643)** , puis cliquez sur **Désinstaller**.

5.  Quand vous devez indiquer si vous voulez vraiment désinstaller la mise à jour, cliquez sur **Oui**.
S
##### <a name="to-turn-off-specific-tools-after-rsat-package-install"></a>Pour désactiver les outils spécifiques (après l’installation du package RSAT)

1.  Sur le Bureau, cliquez sur **Démarrer**, **Toutes les applications**, **Système Windows**, puis sur **Panneau de configuration**.

2.  Cliquez sur **Programmes**, puis dans **Programmes et fonctionnalités**, cliquez sur **Activer ou désactiver les fonctionnalités Windows**.

3.  Dans la boîte de dialogue **Fonctionnalités de Windows**, développez **Outils d’administration de serveur distant**, puis développez soit **Outils d’administration de rôles**, soit **Outils d’administration de fonctionnalités**.

4.  Désactivez les cases à cocher des outils que vous souhaitez désactiver.

    > [!NOTE]
    > Si vous désactiver le Gestionnaire de serveur, l’ordinateur doit être redémarré et outils qui étaient accessibles depuis le **outils** menu du Gestionnaire de serveur doit être ouvert à partir de la **outils d’administration** dossier.

5.  Quand vous avez terminé de désactiver les outils que vous ne voulez pas utilisez, cliquez sur **OK**.

### <a name="run-remote-server-administration-tools"></a>Exécuter les Outils d’administration de serveur distant

> [!NOTE]
> Après l’installation à distance Server Administration Tools pour Windows 10, le **outils d’administration** dossier s’affiche sur le **Démarrer** menu. Vous pouvez accéder aux outils à partir des emplacements suivants :
>
> -   Le **outils** menu dans la console du Gestionnaire de serveur.
> -   **Panneau de configuration/Système et sécurité/Outils d’administration**.
> -   Un raccourci enregistré sur le Bureau à partir du dossier **Outils d’administration** (pour ce faire, cliquez avec le bouton droit sur le lien **Panneau de configuration\Système et sécurité \Outils d’administration**, puis cliquez sur **Créer un raccourci**).

Les outils sont installés dans le cadre de distant Server Administration Tools pour Windows 10 ne peut pas être utilisés pour gérer l’ordinateur client local. Quel que soit l’outil que vous exécutez, vous devez spécifier un serveur distant, ou plusieurs serveurs distants, sur lequel exécuter l’outil. Étant donné que la plupart des outils sont intégrés au Gestionnaire de serveur, vous ajoutez des serveurs distants que vous souhaitez gérer au pool de serveurs de gestionnaire de serveur avant de gérer le serveur en utilisant les outils dans le **outils** menu. Pour plus d’informations sur la manière d’ajouter des serveurs à votre pool de serveurs et de créer des groupes personnalisés de serveurs, voir [Ajouter des serveurs au Gestionnaire de serveur](https://go.microsoft.com/fwlink/p/?LinkId=241353) et [Créer et gérer des groupes de serveurs](https://go.microsoft.com/fwlink/?LinkId=247328).

Dans Outils d’Administration de serveur distant pour Windows 10, tous les outils de gestion de serveur basé sur l’interface utilisateur graphique, tels que les composants logiciels enfichables mmc et de la boîte de dialogue cases, sont accessibles à partir de la **outils** menu de la console du Gestionnaire de serveur. Bien que l’ordinateur qui exécute à distance Server Administration Tools pour Windows 10 exécute un système d’exploitation client, après avoir installé les outils, Gestionnaire de serveur, inclus avec distant Server Administration Tools pour Windows 10, s’ouvre automatiquement par défaut sur l’ordinateur client. Notez qu’il existe aucune **serveur Local** page dans la console Gestionnaire de serveur qui s’exécute sur un ordinateur client.

##### <a name="to-start-server-manager-on-a-client-computer"></a>Pour démarrer le Gestionnaire de serveur sur un ordinateur client

1.  Dans le menu **Démarrer** , cliquez sur **Toutes les applications**, puis sur **Outils d’administration**.

2.  Dans le dossier **Outils d’administration**, cliquez sur **Gestionnaire de serveur**.

Bien qu’ils ne sont pas répertoriés dans la console du Gestionnaire de serveur **outils** menu, les applets de commande Windows PowerShell et les outils de gestion d’invite de commandes sont également installés pour les rôles et fonctionnalités dans le cadre des outils d’Administration de serveur distant. Par exemple, si vous ouvrez une session Windows PowerShell avec des droits utilisateur élevés (exécuter en tant qu’administrateur) et que vous exécutez l’applet de commande `Get-Command -Module RDManagement`, les résultats incluent une liste des applets de commande Services Bureau à distance qui sont désormais disponibles pour s’exécuter sur l’ordinateur local après installation des outils d’Administration de serveur distant, tant que les applets de commande sont ciblées sur un serveur distant qui exécute tout ou partie du rôle Services Bureau à distance.

##### <a name="to-start-windows-powershell-with-elevated-user-rights-run-as-administrator"></a>Pour démarrer Windows PowerShell avec des privilèges élevés (Exécuter en tant qu’administrateur)

1.  Dans le menu **Démarrer**, cliquez sur **Toutes les applications**, sur **Système Windows**, puis sur **Windows PowerShell**.

2.  Pour exécuter Windows PowerShell en tant qu’administrateur à partir du bureau, cliquez sur le **Windows PowerShell** raccourci, puis cliquez sur **exécuter en tant qu’administrateur**.

> [!NOTE]
> Vous pouvez également démarrer une session Windows PowerShell qui est ciblée sur un serveur spécifique en double-cliquant sur un serveur géré dans une page de rôle ou un groupe dans le Gestionnaire de serveur, puis en cliquant sur **Windows PowerShell**.


## <a name="known-issues"></a>Problèmes connus

### <a name="issue-rsat-fod-installation-fails-with-error-code-0x800f0954"></a>**Problème**: Installation de RSAT FOD échoue avec le code d’erreur 0x800f0954

> **Impact**: FODs RSAT sur Windows 10 1809 (mise à jour octobre 2018) dans des environnements WSUS/SCCM

> **Résolution**: Pour installer FODs sur un PC joint à un domaine qui reçoit les mises à jour via WSUS ou SCCM, vous devez modifier un paramètre de stratégie de groupe pour activer le téléchargement FODs directement à partir de Windows Update ou un partage local. Pour plus d’informations et pour obtenir des instructions sur la façon de modifier ce paramètre, consultez [comment rendre les fonctionnalités sur la demande et des modules linguistiques disponibles lorsque vous utilisez WSUS/SCCM](https://docs.microsoft.com/windows/deployment/update/fod-and-lang-packs).

---

### <a name="issue-rsat-fod-installation-via-settings-app-does-not-show-statusprogress"></a>**Problème**: Installation RSAT FOD via l’application paramètres n’affiche pas la progression de l’état /

> **Impact**: FODs RSAT sur Windows 10 1809 (mise à jour d’octobre 2018)

> **Résolution**: Pour afficher la progression de l’installation, cliquez sur le **retour** bouton pour afficher l’état sur le **gestion des fonctionnalités facultatives** page.

---

### <a name="issue-rsat-fod-uninstallation-via-settings-app-may-fail"></a>**Problème**: La désinstallation de RSAT FOD via l’application paramètres peut échouer.

> **Impact**: FODs RSAT sur Windows 10 1809 (mise à jour d’octobre 2018)

> **Résolution**: Dans certains cas, les échecs de désinstallation sont en raison de la nécessité de désinstaller manuellement les dépendances. Plus précisément, si l’outil de serveur distant A est requis par l’outil RSAT B, puis en choisissant désinstaller un outil RSAT échoue si RSAT outil B est toujours installé. Dans ce cas, désinstallez d’abord l’outil RSAT B et puis désinstallez RSAT, outil A. Afficher la liste des FODs RSAT, notamment les dépendances.

---

### <a name="issue-rsat-fod-uninstallation-appears-to-succeed-but-the-tool-is-still-installed"></a>**Problème**: La désinstallation de RSAT FOD semblera avoir réussi, mais l’outil est toujours installé

> **Impact**: FODs RSAT sur Windows 10 1809 (mise à jour d’octobre 2018)

> **Résolution**: Redémarrage de l’ordinateur va terminer la suppression de l’outil.

---

### <a name="issue-rsat-missing-after-windows-10-upgrade"></a>**Problème**: RSAT manquants après une mise à niveau Windows 10

> **Impact**: N’importe quel serveur distant. Installation du package MSU (avant RSAT FODs) pas automatiquement réinstallée

> **Résolution**: Une installation de serveur distant ne peut pas être persistante entre les mises à niveau du système d’exploitation en raison du serveur distant. MSU distribué sous la forme d’un package de mise à jour de Windows. Installez RSAT à nouveau après la mise à niveau Windows 10. Notez que cette limitation est une des raisons pourquoi nous sommes passés à FODs en commençant par Windows 10 1809. FODs RSAT qui sont installées persistera futures mises à niveau de version Windows 10.

## <a name="see-also"></a>Voir aussi
>- [Outils d’Administration de serveur distant pour Windows 10](https://go.microsoft.com/fwlink/?LinkID=404281)
>- [Outils d’Administration de serveur distant (RSAT) pour Windows Vista, Windows 7, Windows 8, Windows Server 2008, Windows Server 2008 R2, Windows Server 2012 et Windows Server 2012 R2](https://go.microsoft.com/fwlink/p/?LinkID=221055)
