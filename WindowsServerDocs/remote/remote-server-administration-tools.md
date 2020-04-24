---
title: Outils d’administration de serveur distant
description: Rubrique de niveau supérieur pour les Outils d’administration de serveur distant
ms.prod: windows-server
ms.technology: manage-rsat
ms.topic: get-started-article
ms.assetid: d54a1f5e-af68-497e-99be-97775769a7a7
author: coreyp-at-msft
ms.author: coreyp
manager: dansimp
ms.openlocfilehash: 510ad2cb1449f161658684eeceec4dbbb7ce6699
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80857092"
---
# <a name="remote-server-administration-tools"></a>Outils d’administration de serveur distant

>S'applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique prend en charge les Outils d’administration de serveur distant pour Windows 10.

> [!IMPORTANT]
> À compter de la mise à jour d’octobre 2018 de Windows 10, les Outils d’administration de serveur distant (RSAT) sont fournis sous la forme d’un ensemble de **Fonctionnalités à la demande** directement dans Windows 10. Pour obtenir des instructions d’installation, consultez **Quand utiliser les différentes versions des outils RSAT** ci-dessous.

Les outils RSAT permettent aux administrateurs informatiques de gérer les rôles et les fonctionnalités de Windows Server à partir d’un PC Windows 10.

Les Outils d’administration de serveur distant comprennent le Gestionnaire de serveur, des composants logiciels enfichables MMC (Microsoft Management Console), des consoles, des fournisseurs et des applets de commande Windows PowerShell, ainsi que des outils en ligne de commande pour la gestion des rôles et des fonctionnalités exécutés sur Windows Server.

Les Outils d’administration de serveur distant incluent des modules d’applet de commande Windows PowerShell qui peuvent être utilisés pour gérer des rôles et des fonctionnalités s’exécutant sur des serveurs distants. La gestion à distance avec Windows PowerShell est activée par défaut sur Windows Server 2016, alors que ce n’est pas le cas sur Windows 10. Pour exécuter des applets de commande qui font partie des Outils d’administration de serveur distant sur un serveur distant, exécutez `Enable-PSremoting` dans une session Windows PowerShell qui a été ouverte avec des droits d’utilisateur avec élévation de privilèges (c’est-à-dire, Exécuter en tant qu’administrateur) sur l’ordinateur client Windows après l’installation des Outils d’administration de serveur distant.

## <a name="remote-server-administration-tools-for-windows-10"></a><a name="BKMK_Thresh"></a>Outils d’administration de serveur distant pour Windows 10
Utilisez les Outils d’administration de serveur distant pour Windows 10 pour gérer des technologies spécifiques sur des ordinateurs Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 et, dans certains cas, Windows Server 2012 ou Windows Server 2008 R2.

Les Outils d’administration de serveur distant pour Windows 10 incluent la prise en charge de la gestion à distance des ordinateurs qui exécutent l’option d’installation minimale ou la configuration de l’interface serveur minimale de Windows Server 2016, Windows Server 2012 R2 et, dans certains cas, les options d’installation minimale de Windows Server 2012. En revanche, il n’est pas possible d’installer les Outils d’administration de serveur distant pour Windows 10 sur une version du système d’exploitation Windows Server.

### <a name="tools-available-in-this-release"></a>Outils disponibles dans cette version
Pour obtenir la liste des outils disponibles dans les Outils d’administration de serveur distant pour Windows 10, consultez le tableau dans [Outils d’administration de serveur distant (RSAT) pour systèmes d’exploitation Windows](https://support.microsoft.com/help/2693643/remote-server-administration-tools-rsat-for-windows-operating-systems).

### <a name="system-requirements"></a>Configuration système requise
Les Outils d’administration de serveur distant pour Windows 10 peuvent uniquement être installés sur des ordinateurs Windows 10. Vous ne pouvez pas installer les Outils d’administration de serveur distant sur des ordinateurs Windows RT 8.1 ou d’autres appareils de type « système sur puce ».

Les Outils d’administration de serveur distant pour Windows 10 s’exécutent à la fois sur les éditions x86 et x64 de Windows 10.

> [!IMPORTANT]
> Les Outils d’administration de serveur distant pour Windows 10 ne doivent pas être installés sur un ordinateur qui exécute des packs d’outils d’administration pour Windows 8.1, Windows 8, Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 ou Windows 2000 Server. Supprimez de l’ordinateur toutes les anciennes versions du Pack des outils d’administration ou des Outils d’administration de serveur distant, y compris les préversions antérieures et les versions des outils pour d’autres langues ou paramètres régionaux, avant d’installer les Outils d’administration de serveur distant pour Windows 10.

Pour utiliser cette version du Gestionnaire de serveur pour accéder aux serveurs distants qui exécutent Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2008 R2 et les gérer, vous devez installer plusieurs mises à jour qui permettront aux anciens systèmes d’exploitation Windows Server d’être gérés par le Gestionnaire de serveur. Pour plus d’informations sur la préparation de Windows Server 2012 R2, Windows Server 2012 et Windows Server 2008 R2 pour la gestion à l’aide du Gestionnaire de serveur dans les Outils d’administration de serveur distant pour Windows 10, consultez [Gérer plusieurs serveurs distants à l’aide du Gestionnaire de serveur](https://technet.microsoft.com/library/hh831456.aspx).
        
Vous devez activer la gestion à distance à l’aide de Windows PowerShell et du Gestionnaire de serveur sur les serveurs distants pour pouvoir les gérer à l’aide des outils intégrés dans les Outils d’administration de serveur distant pour Windows 10. La gestion à distance est activée par défaut sur les serveurs qui exécutent Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012. Pour plus d’informations sur la manière d’activer la gestion à distance, si elle a été désactivée, voir [Gérer plusieurs serveurs distants avec le Gestionnaire de serveur](https://go.microsoft.com/fwlink/p/?LinkId=241358).
        
## <a name="install-uninstall-and-turn-offon-rsat-tools"></a>Installer, désinstaller et désactiver/activer les outils RSAT        

### <a name="use-features-on-demand-fod-to-install-specific-rsat-tools-on-windows-10-october-2018-update-----r-l----ter"></a>Utiliser Fonctionnalités à la demande pour installer des outils RSAT spécifiques sur la mise à jour d’octobre 2018 de Windows 10 ou ultérieure

À compter de la mise à jour d’octobre 2018 de Windows 10, les outils RSAT sont fournis sous la forme d’un ensemble de **Fonctionnalités à la demande** directement dans Windows 10. Désormais, au lieu de télécharger un package RSAT, vous pouvez simplement accéder à **Gérer les fonctionnalités facultatives** dans **Paramètres** et cliquer sur **Ajouter une fonctionnalité** pour voir la liste des outils RSAT disponibles. Sélectionnez et installez les outils RSAT spécifiques dont vous avez besoin. Pour voir la progression de l’installation, cliquez sur le bouton **Retour**. Vous revenez à la page **Gérer les fonctionnalités facultatives**.
        
Consultez la [liste des outils RSAT disponibles dans **Fonctionnalités à la demande**](https://docs.microsoft.co    /wi    dows-hardware/manufacture/desktop/features-on-demand-non-language-fod#remote-server-administration-tools-rsat). En plus de l’application graphique **Paramètres**, vous pouvez utiliser la ligne de commande ou l’automatisation (avec [**DISM /Add-Capability**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities#using-dism-add-capability-to-add-or-remove-fods)) pour installer des outils RSAT spécifiques.

L’un des avantages de Fonctionnalités à la demande est que les fonctionnalités installées sont conservées après chaque mise à niveau de version de Windows 10.        
        
#### <a name="to-uninstall-specific-rsat-tools-on-windows-10-october-2018-update-or-later-after-installing-with-fod"></a>Désinstaller des outils RSAT spécifiques sur la mise à jour d’octobre 2018 de Windows 10 ou ultérieure (après une installation effectuée à l’aide de Fonctionnalités à la demande)        

Sur Windows 10, ouvrez l’application **Paramètres**, accédez à **Gérer les fonctionnalités facultatives**, puis sélectionnez et désinstallez les outils RSAT spécifiques à supprimer. Notez que, dans certains cas, vous devrez désinstaller manuellement les dépendances. En particulier, si l’outil RSAT A est demandé par l’outil RSAT B, toute tentative de désinstallation de l’outil RSAT A échoue si l’outil RSAT B est encore installé. Dans ce cas, désinstallez d’abord l’outil RSAT B, puis désinstallez l’outil RSAT A. Notez également que, dans certains cas, un outil RSAT peut encore être installé après une procédure de désinstallation apparemment réussie. Dans ce cas, redémarrez le PC pour terminer le processus de suppression de l’outil.

Consultez la [liste des outils RSAT, notamment les dépendances](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-non-language-fod#remote-server-administration-tools-rsat). En plus de l’application graphique Paramètres, vous pouvez utiliser la ligne de commande ou l’automatisation (avec [**DISM /Remove-Capability**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities#using-dism-add-capability-to-add-or-remove-fods)) pour désinstaller des outils RSAT spécifiques.

### <a name="when-to-use-which-rsat-version"></a>Quand utiliser les différentes versions des outils RSAT

Si vous disposez d’une version de Windows 10 antérieure à la mise à jour d’octobre 2018 (1809), vous ne pouvez pas utiliser **Fonctionnalités à la demande**. Vous devez télécharger et installer le package RSAT.

- **Installer les Fonctionnalités à la demande des outils RSAT directement à partir de Windows 10, comme indiqué ci-dessus** : installation sur la mise à jour d’octobre 2018 de Windows 10 (1809) ou ultérieure pour gérer Windows Server 2019 ou antérieur.

- **Télécharger et installer le package RSAT WS_1803, comme indiqué ci-dessous** : installation sur la mise à jour d’avril 2018 de Windows 10 (1803) ou antérieure pour gérer Windows Server version 1803 ou 1709.

- **Télécharger et installer le package RSAT WS2016, comme indiqué ci-dessous** : installation sur la mise à jour d’avril 2018 de Windows 10 (1803) ou antérieure pour gérer Windows Server 2016 ou antérieur.

#### <a name="download-the-rsat-package-to-install-remote-server-administration-tools-for-windows-10"></a><a name="BKMK_installthresh"></a>Télécharger le package RSAT pour installer les Outils d’administration de serveur distant pour Windows 10

1.  Téléchargez le package Outils d’administration de serveur distant pour Windows 10 à partir du [Centre de téléchargement Microsoft](https://go.microsoft.com/fwlink/?LinkID=404281). Vous pouvez exécuter le programme d’installation depuis le site web du Centre de téléchargement ou enregistrer le package de téléchargement sur un ordinateur local ou un partage.

    > [!IMPORTANT]
    > Vous pouvez uniquement installer les Outils d’administration de serveur distant pour Windows 10 sur des ordinateurs Windows 10. Vous ne pouvez pas installer les Outils d’administration de serveur distant sur des ordinateurs Windows RT 8.1 ou d’autres appareils de type « système sur puce ».

2.  Si vous enregistrez le package de téléchargement sur un ordinateur local ou un partage, double-cliquez sur le programme d’installation, **WindowsTH-KB2693643-x64.msu** ou **WindowsTH-KB2693643-x86.msu**, selon l’architecture de l’ordinateur sur lequel vous voulez installer les outils.

3.  Quand vous êtes invité à installer la mise à jour par la boîte de dialogue **Programme d’installation de Windows Update en mode autonome** , cliquez sur **Oui**.

4.  Lisez et acceptez les termes du contrat de licence. Cliquez sur **J’accepte**.

5.  L’installation prend quelques minutes.    
        
##### <a name="to-uninstall-remote-server-administration-tools-for-windows-10-----aft----r-rsat-package-install"></a>Pour désinstaller les Outils d’administration de serveur distant pour Windows 10 (après installation du package RSAT)
        
1. Sur le Bureau, cliquez sur **Démarrer**, **Toutes les applications**, **Système Windows**, puis sur **Panneau de configuration**.

2. Sous **Programmes**, cliquez sur **Désinstaller un programme**.

3. Cliquez sur **Afficher les mises à jour installées**.

4. Cliquez avec le bouton droit sur **Mise à jour pour Microsoft Windows (KB2693643)** , puis cliquez sur **Désinstaller**.

5. Quand vous devez indiquer si vous voulez vraiment désinstaller la mise à jour, cliquez sur **Oui**.
   S
   ##### <a name="to-turn----off-specific-tools-after-rsat-package-in----tall"></a>Pour désactiver des outils spécifiques (après installation du package RSAT)
        
6. Sur le Bureau, cliquez sur **Démarrer**, **Toutes les applications**, **Système Windows**, puis sur **Panneau de configuration**.

7. Cliquez sur **Programmes**, puis dans **Programmes et fonctionnalités** , cliquez sur **Activer ou désactiver les fonctionnalités Windows**.

8. Dans la boîte de dialogue **Fonctionnalités de Windows** , développez **Outils d’administration de serveur distant**, puis développez soit **Outils d’administration de rôles** , soit **Outils d’administration de fonctionnalités**.

9. Désactivez les cases à cocher des outils que vous souhaitez désactiver.

   > [!NOTE]
   > Si vous désactivez le Gestionnaire de serveur, vous devrez redémarrer l’ordinateur et ouvrir les outils qui étaient disponibles dans le menu **Outils** du Gestionnaire de serveur à partir du dossier **Outils d’administration**.
        
10. Quand vous avez terminé de désactiver les outils que vous ne voulez pas utilisez, cliquez sur **OK**.

### <a name="run-remote-server-administration-tools"></a>Exécuter les Outils d’administration de serveur distant

> [!NOTE]
> Au terme de l’installation des Outils d’administration de serveur distant pour Windows 10, le dossier **Outils d’administration** apparaît dans le menu **Démarrer**. Vous pouvez accéder aux outils à partir des emplacements suivants :
>
> -   Le menu **Outils** dans la console Gestionnaire de serveur.
> -   **Panneau de configuration/Système et sécurité/Outils d’administration**.
> -   Un raccourci enregistré sur le Bureau à partir du dossier **Outils d’administration** (pour ce faire, cliquez avec le bouton droit sur le lien **Panneau de configuration\Système et sécurité \Outils d’administration** , puis cliquez sur **Créer un raccourci**).

Vous ne pouvez pas utiliser les outils installés dans le cadre des Outils d’administration de serveur distant pour Windows 10 pour gérer l’ordinateur client local. Quel que soit l’outil utilisé, vous devez spécifier un ou plusieurs serveurs distants sur lesquels exécuter l’outil. Étant donné que la plupart des outils sont intégrés au Gestionnaire de serveur, ajoutez les serveurs distants que vous voulez gérer au pool de serveurs du Gestionnaire de serveur avant de gérer le serveur à l’aide des outils du menu **Outils**. Pour plus d’informations sur la manière d’ajouter des serveurs à votre pool de serveurs et de créer des groupes personnalisés de serveurs, voir [Ajouter des serveurs au Gestionnaire de serveur](https://go.microsoft.com/fwlink/p/?LinkId=241353) et [Créer et gérer des groupes de serveurs](https://go.microsoft.com/fwlink/?LinkId=247328).

Dans les Outils d’administration de serveur distant pour Windows 10, tous les outils de gestion de serveur basés sur l’interface utilisateur graphique, comme les composants logiciels enfichables MMC et les boîtes de dialogue, sont disponibles dans le menu **Outil** de la console du Gestionnaire de serveur. Même si l’ordinateur qui exécute les Outils d’administration de serveur distant pour Windows 10 exécute un système d’exploitation client, à l’issue de l’installation des outils, le Gestionnaire de serveur qui est inclus dans les Outils d’administration de serveur distant pour Windows 10 s’ouvre automatiquement par défaut sur l’ordinateur client. Notez qu’il n’existe pas de page **Serveur local** dans la console du Gestionnaire de serveur qui s’exécute sur un ordinateur client.

##### <a name="to-start-server-manager-on-a-clien-----co----puter"></a>Pour démarrer le Gestionnaire de serveur sur un ordinateur client

1.  Dans le menu **Démarrer** , cliquez sur **Toutes les applications**, puis sur **Outils d’administration**.

2.  Dans le dossier **Outils d’administration** , cliquez sur **Gestionnaire de serveur**.

Bien qu’elles ne soient pas listées dans le menu **Outils** de la console du Gestionnaire de serveur, les applets de commande Windows PowerShell et les outils de gestion de l’invite de commandes sont également installés pour les rôles et les fonctionnalités dans le cadre des Outils d’administration de serveur distant. Par exemple, si vous ouvrez une session Windows PowerShell avec des droits d’utilisateur élevés (Exécuter en tant qu’administrateur), puis exécutez l’applet de commande `Get-Command -Module RDManagement`, les résultats incluent la liste des applets de commande des services Bureau à distance qui sont maintenant disponibles pour une exécution sur l’ordinateur local à l’issue de l’installation des Outils d’administration de serveur distant, et ce tant que les applets de commande ciblent un serveur distant qui exécute tout ou partie du rôle des services Bureau à distance.

##### <a name="to-start-windows-powershell-with-elevated-user-rights-run-as-administrator"></a>Pour démarrer Windows PowerShell avec des privilèges élevés (Exécuter en tant qu’administrateur)

1.  Dans le menu **Démarrer** , cliquez sur **Toutes les applications**, sur **Système Windows**, puis sur **Windows PowerShell**.

2.  Pour exécuter Windows PowerShell en tant qu’administrateur à partir du Bureau, cliquez avec le bouton droit sur le raccourci de **Windows PowerShell**, puis cliquez sur **Exécuter en tant qu’administrateur**.

> [!NOTE]
> Vous pouvez également démarrer une session Windows PowerShell ciblant un serveur spécifique. Pour cela, cliquez avec le bouton droit sur un serveur managé dans une page de rôle ou de groupe dans le Gestionnaire de serveur, puis cliquez sur **Windows PowerShell**.
        

## <a name="known-issues"></a>Problèmes connus

### <a name="issue-rsat-fod-installation-fails-with-error-code-0x800f0954"></a>**Problème** : Échec de l’installation des Fonctionnalités à la demande des outils RSAT avec le code d’erreur 0x800f0954

> **Impact** : Fonctionnalités à la demande des outils RSAT sur Windows 10 1809 (mise à jour d’octobre 2018) dans les environnements WSUS/Configuration Manager
> 
> **Résolution** : Pour installer des Fonctionnalités à la demande sur un PC joint à un domaine qui reçoit des mises à jour par le biais de WSUS ou de Configuration Manager, vous devez modifier un paramètre de stratégie de groupe pour autoriser le téléchargement de Fonctionnalités à la demande directement à partir de Windows Update ou d’un partage local. Pour plus d’informations et d’instructions sur la façon de changer ce paramètre, consultez [Guide pratique pour rendre des fonctionnalités à la demande et des modules linguistiques disponibles avec WSUS/SCCM](https://docs.microsoft.com/windows/deployment/update/fod-and-lang-packs).

---

### <a name="issue-rsat-fod-installation-via-settings-app-does-not-show-statusprogress"></a>**Problème** : L’installation des Fonctionnalités à la demande des outils RSAT par le biais de l’application Paramètres n’indique pas l’état/la progression

> **Impact** : Fonctionnalités à la demande des outils RSAT sur Windows 10 1809 (mise à jour d’octobre 2018)
> 
> **Résolution** : Pour voir la progression de l’installation, cliquez sur le bouton **Retour**. Vous revenez à la page **Gérer les fonctionnalités facultatives**.

---

### <a name="issue-rsat-fod-uninstallation-via-settings-app-may-fail"></a>**Problème** : La désinstallation des Fonctionnalités à la demande des outils RSAT par le biais de l’application Paramètres peut échouer

> **Impact** : Fonctionnalités à la demande des outils RSAT sur Windows 10 1809 (mise à jour d’octobre 2018)
> 
> **Résolution** : Dans certains cas, les échecs de désinstallation sont dus à la présence de dépendances que vous devez désinstaller manuellement. En particulier, si l’outil RSAT A est demandé par l’outil RSAT B, toute tentative de désinstallation de l’outil RSAT A échoue si l’outil RSAT B est encore installé. Dans ce cas, désinstallez d’abord l’outil RSAT B, puis désinstallez l’outil RSAT A. Consultez la liste des Fonctionnalités à la demande des outils RSAT, notamment les dépendances.

---

### <a name="issue-rsat-fod-uninstallation-appears-to-succeed-but-the-tool-is-still-installed"></a>**Problème** : La désinstallation des Fonctionnalités à la demande des outils RSAT semble aboutir, mais l’outil est toujours installé

> **Impact** : Fonctionnalités à la demande des outils RSAT sur Windows 10 1809 (mise à jour d’octobre 2018)
> 
> **Résolution** : Redémarrez le PC pour terminer le processus de suppression de l’outil.

---

### <a name="issue-rsat-missing-after-windows-10-upgrade"></a>**Problème** : Outils RSAT manquant après la mise à niveau de Windows 10

> **Impact** : Les outils RSAT installés au moyen d’un package .MSU (avant les Fonctionnalités à la demande des outils RSAT) ne sont pas réinstallés automatiquement.
> 
> **Résolution** : Une installation des outils RSAT ne peut pas être conservée après la mise à niveau du système d’exploitation dans la mesure où le package .MSU des outils RSAT est fourni en tant que package Windows Update. Réinstallez les outils RSAT après la mise à niveau de Windows 10. Notez que cette limitation est l’une des raisons pour lesquelles nous sommes passés aux Fonctionnalités à la demande à partir de Windows 10 1809. Les Fonctionnalités à la demande des outils RSAT qui sont installées sont conservées à chaque mise à niveau de version de Windows 10.

## <a name="see-also"></a>Voir aussi
>- [Outils d’administration de serveur distant pour Windows 10](https://go.microsoft.com/fwlink/?LinkID=404281)
>- [Outils d’administration de serveur distant (RSAT) pour Windows Vista, Windows 7, Windows 8, Windows Server 2008, Windows Server 2008 R2, Windows Server 2012 et Windows Server 2012 R2](https://go.microsoft.com/fwlink/p/?LinkID=221055)                                                                                                                                                                                                                                                                                                                                                                                    