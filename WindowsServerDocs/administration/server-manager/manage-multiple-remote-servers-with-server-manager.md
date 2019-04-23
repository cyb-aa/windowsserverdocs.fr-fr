---
title: Gérer plusieurs serveurs distants avec le Gestionnaire de serveur
description: Gestionnaire de serveur
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3a17e686-e7f2-47e2-b7af-733777c38b5f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e4db3e486cec5cb065bfd202b7da2547be41a01a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847610"
---
# <a name="manage-multiple-remote-servers-with-server-manager"></a>Gérer plusieurs serveurs distants avec le Gestionnaire de serveur

>S'applique à : Windows Server 2016

Le Gestionnaire de serveur est une console de gestion dans Windows Server 2012 R2 et Windows Server 2012 qui permet aux professionnels de l’informatique de configurer et gérer des serveurs locaux ou à distance basé sur Windows à partir de leurs bureaux, sans nécessiter un accès physique aux serveurs, ou nécessité d’activer les connexions Remote Desktop protocol (rdP) à chaque serveur. Bien que le Gestionnaire de serveur est disponible dans Windows Server 2008 R2 et Windows Server 2008, le Gestionnaire de serveur a été mis à jour dans Windows Server 2012, pour prendre en charge la gestion multiserveur à distance et aider à augmenter le nombre de serveurs qu’un administrateur peut gérer.

Dans nos tests, le Gestionnaire de serveur dans Windows Server 2012 R2 et Windows Server 2012 peut servir à gérer jusqu'à 100 serveurs qui sont configurés avec une charge de travail standard. Le nombre de serveurs que vous pouvez gérer en utilisant une console du Gestionnaire de serveur unique peut varier en fonction de la quantité de données que vous demandez aux serveurs gérés ainsi que des ressources matérielles et réseau disponibles pour l’ordinateur exécutant le Gestionnaire de serveur. Lorsque la quantité de données à afficher est sur le point d’atteindre la capacité des ressources de cet ordinateur, vous pouvez être confronté à des réactions lentes du Gestionnaire de serveur ainsi qu’à des retards dans la réalisation des actualisations. Pour augmenter le nombre de serveurs que vous pouvez gérer à l’aide du Gestionnaire de serveur, nous vous conseillons de limiter la quantité de données d’événement que le Gestionnaire de serveur obtient de vos serveurs gérés en utilisant les paramètres de la boîte de dialogue **Configurer les données d’événement**. Configurer les données d’événement peut être ouverte à partir du menu **Tâches** dans la vignette **Événements** . Si vous devez gérer un très grand nombre de serveurs dans votre organisation, nous vous recommandons d’évaluer des produits de la [suite Microsoft System Center](https://go.microsoft.com/fwlink/p/?LinkId=239437).

Cette rubrique et ses sous-rubriques fournissent des informations sur l’utilisation des fonctionnalités dans la console du Gestionnaire de serveur. Cette rubrique contient les sections suivantes.

-   [Passez en revue les considérations initiales et la configuration système requise](#BKMK_1.1)

-   [Tâches que vous pouvez effectuer dans le Gestionnaire de serveur](#BKMK_tasks)

-   [Démarrez le Gestionnaire de serveur](#BKMK_start)

-   [Redémarrer les serveurs distants](#BKMK_restart)

-   [Exporter les paramètres du Gestionnaire de serveur à d’autres ordinateurs](#BKMK_export)

## <a name="BKMK_1.1"></a>Passez en revue les considérations initiales et la configuration système requise
Les sections suivantes répertorient certaines considérations initiales que vous devez passer en revue, ainsi que les configurations matérielle et logicielle pour le Gestionnaire de serveur.

### <a name="hardware-requirements"></a>Configuration matérielle requise
Le Gestionnaire de serveur est installé par défaut avec toutes les éditions de Windows Server 2012 R2 et Windows Server 2012. Aucune autre configuration matérielle requise n’existe pour le Gestionnaire de serveur.

### <a name="BKMK_softconfig"></a>Configuration logicielle et la configuration requise
Le Gestionnaire de serveur est installé par défaut avec toutes les éditions de Windows Server 2012. Bien que vous pouvez utiliser le Gestionnaire de serveur pour gérer les [options d’installation Server Core](https://go.microsoft.com/fwlink/p/?LinkID=241573) de Windows Server 2012 et Windows Server 2008 R2 qui sont en cours d’exécution sur des ordinateurs distants, le Gestionnaire de serveur ne s’exécute pas directement sur l’installation Server Core Options.

Pour gérer complètement les serveurs distants qui exécutent Windows Server 2008 ou Windows Server 2008 R2, installez les mises à jour suivantes sur les serveurs que vous souhaitez gérer, dans l’ordre indiqué.

Pour gérer les serveurs qui exécutent Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008 à l’aide du Gestionnaire de serveur dans Windows Server 2012 R2, appliquez les mises à jour suivantes pour les systèmes d’exploitation plus anciens.

-   [.NET framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)

-   [Windows Management Framework 4.0](https://go.microsoft.com/fwlink/?LinkId=293881). Le package de téléchargement Windows Management Framework 4.0 met à jour les fournisseurs Windows Management Instrumentation (WMI) sur Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008. Les fournisseurs WMI mis à jour le Gestionnaire de serveur permettent de collecter des informations sur les rôles et fonctionnalités installés sur les serveurs gérés. Jusqu'à ce que la mise à jour est appliquée, les serveurs qui exécutent Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008 ont l’état de gérabilité **non accessible**.

-   Associé à la mise à jour des performances [article 2682011 de la Base de connaissances](https://go.microsoft.com/fwlink/p/?LinkID=245487) permet le Gestionnaire de serveur pour collecter des données de performances à partir de Windows Server 2008 et Windows Server 2008 R2. Cette mise à jour des performances n’est pas nécessaire sur les serveurs qui exécutent Windows Server 2012.

Pour gérer les serveurs qui exécutent Windows Server 2008 R2 ou Windows Server 2008, appliquer les mises à jour suivantes pour les systèmes d’exploitation plus anciens.

-   [.NET framework 4](https://www.microsoft.com/download/en/details.aspx?id=17718)

-   [Windows Management Framework 3.0](https://go.microsoft.com/fwlink/p/?LinkID=229019) package de téléchargement de The Windows Management Framework 3.0 met à jour les fournisseurs Windows Management Instrumentation (WMI) sur Windows Server 2008 et Windows Server 2008 R2. Les fournisseurs WMI mis à jour le Gestionnaire de serveur permettent de collecter des informations sur les rôles et fonctionnalités installés sur les serveurs gérés. Jusqu'à ce que la mise à jour est appliquée, les serveurs qui exécutent Windows Server 2008 ou Windows Server 2008 R2 ont un état de facilité de **non accessible - vérifier les versions antérieures exécutent Windows Management Framework 3.0**.

-   Associé à la mise à jour des performances [article 2682011 de la Base de connaissances](https://go.microsoft.com/fwlink/p/?LinkID=245487) permet le Gestionnaire de serveur pour collecter des données de performances à partir de Windows Server 2008 et Windows Server 2008 R2.

Le Gestionnaire de serveur s’exécute dans l’Interface graphique serveur minimale ; Autrement dit, lorsque la fonctionnalité interpréteur de commandes graphique de serveur a été désinstallée. La fonctionnalité d’interpréteur de commandes graphique de serveur est installée par défaut sur Windows Server 2012 R2 et Windows Server 2012. Si vous désinstallez l’interpréteur de commandes graphique, la console du Gestionnaire de serveur s’exécute, mais certaines applications ou certains outils disponibles à partir de la console ne sont pas disponibles. Navigateurs Internet ne peut pas s’exécuter sans l’interpréteur de commandes graphique de serveur, c’est le cas pages Web et des applications telles que HTML help (par exemple, l’aide mmc F1) ne peut pas être ouvert. Vous ne pouvez pas ouvrir les boîtes de dialogue pour la configuration de la mise à jour automatique de Windows et des commentaires lors de l’interpréteur de commandes graphique de serveur n’est pas installé ; commandes qui ouvrent ces boîtes de dialogue dans la console du Gestionnaire de serveur sont redirigées pour exécuter **sconfig.cmd**.

#### <a name="manage-remote-computers-from-a-client-computer"></a>Gérer des ordinateurs distants à partir d’un ordinateur client
La console du Gestionnaire de serveur est incluse avec [outils d’Administration de serveur distant](https://go.microsoft.com/fwlink/?LinkID=304145) pour Windows 8.1 et [outils d’Administration de serveur distant](https://go.microsoft.com/fwlink/p/?LinkID=238560) pour Windows 8. Notez que lorsque les outils d’Administration de serveur distant est installé sur un ordinateur client, vous ne pouvez pas gérer l’ordinateur local à l’aide du Gestionnaire de serveur ; Gestionnaire de serveur ne peut pas être utilisé pour gérer les ordinateurs ou périphériques qui exécutent un système d’exploitation du client Windows. Vous pouvez utiliser le Gestionnaire de serveur pour gérer uniquement les serveurs Windows.

|Système d’exploitation de gestionnaire de serveur Source|Ciblé sur Windows Server 2012 R2 |Ciblé sur Windows Server 2012 |Ciblé sur Windows Server 2008 R2 ou Windows Server 2008 |Ciblé sur Windows Server 2003|
|-------------------------------|---------------------------------------|------------------------------------|-----------------------------------------------------------------------|------------------|
|Windows 8 ou Windows Server 2012 |Non pris en charge|Prise en charge complète|Une fois [la configuration logicielle requise et les conditions de configuration](#BKMK_softconfig) satisfaites, la plupart des tâches de gestion peuvent être effectuées, mais pas l’installation ni la désinstallation de rôles ou de fonctionnalités|Prise en charge limitée ; états en ligne et hors connexion uniquement|
|Windows 8.1 ou Windows Server 2012 R2 |Prise en charge complète|Prise en charge complète|Une fois [la configuration logicielle requise et les conditions de configuration](#BKMK_softconfig) satisfaites, la plupart des tâches de gestion peuvent être effectuées, mais pas l’installation ni la désinstallation de rôles ou de fonctionnalités|Prise en charge limitée ; états en ligne et hors connexion uniquement|

###### <a name="to-start-server-manager-on-a-client-computer"></a>Pour démarrer le Gestionnaire de serveur sur un ordinateur client

1.  Suivez les instructions dans [déployer les outils d’Administration à distance du serveur](https://go.microsoft.com/fwlink/?LinkID=238562) pour installer à distance Server Administration Tools pour Windows 8.1 ou Server Administration outils distant pour Windows 8.

2.  Sur le **Démarrer** , cliquez sur **le Gestionnaire de serveur**. La vignette **Gestionnaire de serveur** est disponible après avoir installé Outils d’administration de serveur distant.

3.  Si ni le **outils d’administration** ni le **le Gestionnaire de serveur** vignettes sont affichées sur le **Démarrer** écran après avoir installé les outils d’Administration de serveur distant, et recherche pour le Gestionnaire de serveur sur le **Démarrer** écran n’affiche pas de résultats, vérifiez que le **afficher les outils d’administration** paramètre est activé. Pour afficher ce paramètre, pointez le curseur de la souris sur le coin supérieur droit de la **Démarrer** écran, puis cliquez sur **paramètres**. Si le paramètre **Afficher les outils d’administration** est désactivé, activez-le pour afficher les outils que vous avez installés dans le cadre des Outils d’administration de serveur distant.

Pour plus d’informations sur l’exécution à distance serveur outils d’Administration pour Windows 8 pour gérer des serveurs distants, consultez [outils d’Administration de serveur distant](https://go.microsoft.com/fwlink/?LinkID=221055) sur le TechNet Wiki.

#### <a name="configure-remote-management-on-servers-that-you-want-to-manage"></a>Configurer la gestion à distance sur des serveurs que vous voulez gérer

> [!IMPORTANT]
> Par défaut, le Gestionnaire de serveur et Windows PowerShell de gestion à distance est activée dans Windows Server 2012 R2 et Windows Server 2012.

Pour effectuer des tâches de gestion sur des serveurs distants à l’aide du Gestionnaire de serveur, des serveurs distants que vous souhaitez gérer doivent être configurés pour autoriser la gestion à distance à l’aide de Server Manager et Windows PowerShell. Si la gestion à distance a été désactivée sur Windows Server 2012 R2 ou Windows Server 2012, et que vous souhaitez réactiver, procédez comme suit.

##### <a name="BKMK_windows"></a>Pour configurer la gestion à distance du Gestionnaire de serveur sur Windows Server 2012 R2 ou Windows Server 2012 à l’aide de l’interface Windows

1.  > [!NOTE]
    > Les paramètres qui sont contrôlés par le **configurer la gestion à distance** boîte de dialogue n’affectent pas les parties du Gestionnaire de serveur qui utilisent le modèle DCOM pour les communications distantes.

    Effectuez l’une des opérations suivantes pour ouvrir le Gestionnaire de serveur si elle n’est pas déjà ouverte.

    -   Dans la barre des tâches Windows, cliquez sur le bouton du Gestionnaire de serveur.

    -   Sur le **Démarrer** , cliquez sur **le Gestionnaire de serveur**.

2.  Dans le **propriétés** zone de la **serveurs locaux** , cliquez sur la valeur de lien hypertexte pour le **gestion à distance** propriété.

3.  Effectuez l’une des opérations ci-dessous, puis cliquez sur **OK**.

    -   Pour empêcher cet ordinateur d’être gérés à distance en utilisant le Gestionnaire de serveur (ou Windows PowerShell s’il est installé), désactivez le **activer la gestion à distance de ce serveur à partir d’autres ordinateurs** case à cocher.

    -   Pour permettre à cet ordinateur de gestion à distance en utilisant le Gestionnaire de serveur ou de Windows PowerShell, sélectionnez **activer la gestion à distance de ce serveur à partir d’autres ordinateurs**.

##### <a name="BKMK_ps"></a>Pour activer la gestion à distance du Gestionnaire de serveur sur Windows Server 2012 R2 ou Windows Server 2012 à l’aide de Windows PowerShell

1.  Effectuez l’une des opérations suivantes :

    -   Pour exécuter Windows PowerShell en tant qu’administrateur à partir de la **Démarrer** écran, cliquez sur le **Windows PowerShell** vignette, puis cliquez sur **exécuter en tant qu’administrateur**.

    -   Pour exécuter Windows PowerShell en tant qu’administrateur à partir du bureau, cliquez sur le **Windows PowerShell** raccourci dans la barre des tâches, puis cliquez sur **exécuter en tant qu’administrateur**.

2.  Tapez la commande suivante, puis appuyez sur **entrée** pour activer toutes les exceptions des règles de pare-feu nécessaires.

    **Configure-SMremoting.exe -Enable**

    > [!NOTE]
    > Cette commande fonctionne également dans une invite de commandes ouverte avec des droits d’utilisateur élevés (Exécuter en tant qu’administrateur).

    cas d’échec de l’activation de la gestion à distance, voir [about_remote_Troubleshooting](https://go.microsoft.com/fwlink/p/?LinkID=135188) sur Microsoft TechNet pour la résolution des problèmes de conseils et meilleures pratiques.

###### <a name="to-enable-server-manager-and-windows-powershell-remote-management-on-older-operating-systems"></a>Pour activer la gestion à distance via le Gestionnaire de serveur et Windows PowerShell sur des systèmes d’exploitation plus anciens

-   Effectuez l’une des opérations suivantes :

    -   Pour activer la gestion à distance sur les serveurs qui exécutent Windows Server 2008 R2, consultez [gestion à distance avec le Gestionnaire de serveur](https://go.microsoft.com/fwlink/?LinkID=137378) dans l’aide de Windows Server 2008 R2.

    -   Pour activer la gestion à distance sur des serveurs qui exécutent Windows Server 2008, consultez [activer et utiliser des commandes à distance dans Windows PowerShell](https://go.microsoft.com/fwlink/p/?LinkId=242565).

    -   Pour activer la gestion à distance sur des serveurs qui exécutent Windows Server 2003, activez les exceptions WMI DCOM dans le Pare-feu Windows. Pour plus d’informations sur la procédure à suivre sur des serveurs qui exécutent Windows Server 2003, voir [Connexion via le Pare-feu Windows](https://msdn.microsoft.com/library/aa389286.aspx) sur MSDN.

## <a name="BKMK_tasks"></a>Tâches que vous pouvez effectuer dans le Gestionnaire de serveur
Server Manager rend plus efficace l’administration des serveurs en permettant aux administrateurs d’effectuer des tâches dans le tableau suivant à l’aide d’un seul outil. Dans Windows Server 2012 R2 et Windows Server 2012, les utilisateurs standards d’un serveur et membres du groupe administrateurs peuvent effectuer des tâches de gestion dans le Gestionnaire de serveur, mais par défaut, les utilisateurs standard ne peuvent pas effectuer certaines tâches, comme indiqué dans le tableau suivant.

Les administrateurs peuvent utiliser deux applets de commande Windows PowerShell dans le module d’applet de commande Gestionnaire de serveur, [Enable-ServerManagerStandardUserremoting](https://technet.microsoft.com/library/jj205470.aspx) et [Disable-ServerManagerStandardUserremoting](https://technet.microsoft.com/library/jj205468.aspx), à contrôler l’accès utilisateur standard à d’autres données. Le **Enable-ServerManagerStandardUserremoting** applet de commande peut fournir un ou plusieurs utilisateurs standard, non-administrateurs accès aux événements, service, compteur de performances et les données d’inventaire de rôles et de fonctionnalités.

> [!IMPORTANT]
> Le Gestionnaire de serveur ne peut pas être utilisé pour gérer une version plus récente du système d’exploitation Windows Server. Gestionnaire de serveur s’exécutant sur Windows Server 2012 ou Windows 8 ne peut pas être utilisé pour gérer les serveurs qui exécutent Windows Server 2012 R2.

|Description de la tâche|Administrateurs (y compris le compte Administrateur intégré)|Utilisateurs de serveur standard|
|----------|----------------------------------|-------------|
|ajouter des serveurs à distance à un pool de serveurs que le Gestionnaire de serveur peut être utilisé pour gérer.|Oui|Non|
|créer et modifier des groupes personnalisés de serveurs, tels que les serveurs qui sont dans un emplacement géographique spécifique ou servent un objectif spécifique.|Oui|Oui|
|Installer ou désinstaller des rôles, services de rôle et fonctionnalités sur l’ordinateur local ou sur des serveurs distants qui exécutent Windows Server 2012 R2 ou Windows Server 2012. Pour les définitions de rôles, services de rôle et fonctionnalités, consultez [rôles, Services de rôle et fonctionnalités](https://go.microsoft.com/fwlink/p/?LinkId=239558).|Oui|Non|
|Afficher et modifier les fonctionnalités et rôles de serveur installés sur les serveurs locaux ou distants. **Remarque :** Dans le Gestionnaire de serveur, les données de rôle et fonctionnalité s’affiche dans la langue de base du système, également appelée langue de l’interface graphique utilisateur par défaut du système ou de la langue sélectionnée pendant l’installation du système d’exploitation.|Oui|Les utilisateurs standard peuvent afficher et gérer les fonctionnalités et rôles ainsi qu’effectuer des tâches, telles que l’affichage des événements de rôle, mais ils ne peuvent pas ajouter ni supprimer des services de rôle.|
|Démarrer les outils de gestion tels que Windows PowerShell ou les composants logiciels enfichables mmc. Vous pouvez démarrer une session Windows PowerShell ciblée sur un serveur distant en double-cliquant sur le serveur dans le **serveurs** vignette, puis en cliquant sur **Windows PowerShell**. Vous pouvez démarrer les composants logiciel enfichables mmc à partir de la **outils** menu de la console Gestionnaire de serveur, puis pointer l’élément mmc vers un ordinateur distant après l’utilisation du composant logiciel enfichable est ouvert.|Oui|Oui|
|Gérer les serveurs distants avec différentes informations d’identification en cliquant avec le bouton droit sur un serveur dans la vignette **Serveurs**, puis en cliquant sur **Gérer en tant que**. Vous pouvez utiliser **Gérer en tant que** pour les tâches de gestion de serveur générales et des services de fichiers et de stockage.|Oui|Non|
|Tâches de gestion associées au cycle de vie opérationnel des serveurs, comme le démarrage ou arrêt des services ; et démarrer d’autres outils qui vous permettent de configurer les paramètres réseau, les utilisateurs et les groupes et les connexions Bureau à distance d’un serveur.|Oui|Les utilisateurs standard ne peuvent pas démarrer ni arrêter des services. Ils peuvent modifier le serveur local nom, groupe de travail, ou l’appartenance au domaine et paramètres du Bureau à distance, mais que vous êtes invités par le contrôle de compte d’utilisateur à fournir des informations d’identification administrateur avant de pouvoir effectuer ces tâches. Ils ne peuvent pas modifier les paramètres de gestion à distance.|
|Effectuer des tâches de gestion associées au cycle de vie opérationnel des rôles installés sur les serveurs, notamment l’analyse des rôles pour une mise en conformité avec les meilleures pratiques.|Oui|Les utilisateurs standard ne peut pas exécuter des analyses Best Practices Analyzer.|
|Déterminer l’état d’un serveur, identifier des événements critiques, analyser et dépanner des problèmes ou des échecs de configuration.|Oui|Oui|
|Personnaliser les événements, les données de performance, services et les résultats de Best Practices Analyzer sur lequel vous voulez être alerté sur le tableau de bord du Gestionnaire de serveur.|Oui|Oui|
|Redémarrer les serveurs.|Oui|Non|
|Actualiser les données qui s’affiche dans la console du Gestionnaire de serveur sur les serveurs gérés.|Oui|Non|

> [!NOTE]
> Le Gestionnaire de serveur peut recevoir uniquement des informations sur l’état en ligne ou hors connexion en provenance de serveurs exécutant Windows Server 2003. Gestionnaire de serveur ne peut pas être utilisé pour ajouter des rôles et fonctionnalités aux serveurs qui exécutent Windows Server 2008 R2, Windows Server 2008 ou Windows Server 2003.

## <a name="BKMK_start"></a>Démarrez le Gestionnaire de serveur
Le Gestionnaire de serveur démarre automatiquement par défaut sur les serveurs qui exécutent Windows Server 2012, lorsqu’un membre du groupe Administrateurs ouvre une session sur un serveur. Si vous fermez le Gestionnaire de serveur, le redémarrage de l’une des manières suivantes. Cette section présente également les étapes pour modifier le comportement par défaut et pour empêcher le Gestionnaire de serveur de démarrer automatiquement.

#### <a name="to-start-server-manager-from-the-start-screen"></a>Pour démarrer le Gestionnaire de serveur à partir de l’écran d’accueil

-   Sur le Windows **Démarrer** , cliquez sur le **le Gestionnaire de serveur** vignette.

#### <a name="to-start-server-manager-from-the-windows-desktop"></a>Pour démarrer le Gestionnaire de serveur à partir du Bureau Windows

-   Dans la barre des tâches Windows, cliquez sur **Gestionnaire de serveur**.

#### <a name="to-prevent-server-manager-from-starting-automatically"></a>Pour empêcher le démarrage automatique du Gestionnaire de serveur

1.  Dans la console Gestionnaire de serveur, sur le **gérer** menu, cliquez sur **propriétés du Gestionnaire de serveur**.

2.  Dans la boîte de dialogue **Propriétés du Gestionnaire de serveur** , activez la case à cocher **Ne pas démarrer automatiquement le Gestionnaire de serveur à l’ouverture de session**. Cliquez sur **OK**.

3.  Vous pouvez également empêcher le Gestionnaire de serveur de démarrer automatiquement en activant le paramètre de stratégie de groupe, **ne pas démarrer le Gestionnaire de serveur automatiquement à l’ouverture de session**. Le chemin d’accès à ce paramètre de stratégie, dans la console de l’éditeur de stratégie de groupe locale, est un ordinateur Configuration ordinateur\Modèles d’Administration\système\gestionnaire.

## <a name="BKMK_restart"></a>Redémarrer les serveurs distants
Vous pouvez redémarrer un serveur distant à partir de la **serveurs** vignette d’une page de rôle ou un groupe dans le Gestionnaire de serveur.

> [!IMPORTANT]
> Le redémarrage d’un serveur distant force le serveur à redémarrer, même si des utilisateurs sont toujours connectés au serveur distant et que des programmes avec des données non enregistrées sont toujours ouverts. Ce comportement est différent de la fermeture ou du redémarrage de l’ordinateur local sur lequel vous êtes invité à enregistrer les données de programme non enregistrées et à confirmer que vous voulez forcer les utilisateurs connectés à fermer la session. Assurez-vous de pouvoir forcer d’autres utilisateurs à se déconnecter des serveurs distants et de pouvoir ignorer les données non enregistrées dans les programmes exécutés sur les serveurs distants.
> 
> Si une actualisation automatique se produit dans le Gestionnaire de serveur alors qu’un serveur géré est en cours d’arrêt et le redémarrage, état de l’actualisation et la facilité de gestion erreurs peuvent se produire pour le serveur géré, car le Gestionnaire de serveur ne peut pas se connecter au serveur distant qu’elle soit terminée le redémarrage.

#### <a name="to-restart-remote-servers-in-server-manager"></a>Pour redémarrer des serveurs distants dans le Gestionnaire de serveur

1.  Ouvrez une page d’accueil de groupe serveurs ou de rôles dans le Gestionnaire de serveur.

2.  Sélectionnez un ou plusieurs serveurs distants que vous avez ajoutés au Gestionnaire de serveur. Appuyez sur la touche **Ctrl** et maintenez-la enfoncée pendant que vous cliquez pour sélectionner plusieurs serveurs simultanément. Pour plus d’informations sur l’ajout de serveurs au pool de serveurs de gestionnaire de serveur, consultez [ajouter des serveurs au Gestionnaire de serveur](add-servers-to-server-manager.md).

3.  Cliquez avec le bouton droit sur les serveurs sélectionnés, puis cliquez sur **Redémarrer le serveur**.

## <a name="BKMK_export"></a>Exporter les paramètres du Gestionnaire de serveur à d’autres ordinateurs
Dans le Gestionnaire de serveur, la liste des serveurs gérés, modifications apportées aux paramètres de console de gestionnaire de serveur et des groupes personnalisés que vous avez créés sont stockés dans les deux fichiers suivants. Vous pouvez réutiliser ces paramètres sur d’autres ordinateurs qui exécutent la même version du Gestionnaire de serveur (pas les ordinateurs qui exécutent l’option d’installation Server Core) ou Windows 8. Outils d’Administration de serveur distant doit être en cours d’exécution sur les ordinateurs client Windows pour exporter les paramètres du Gestionnaire de serveur à ces ordinateurs.

-   %*appdata*%\Microsoft\Windows\ServerManager\Serverlist.xml

-   %*appdata*%\Local\Microsoft_Corporation\ServerManager.exe_StrongName_*GUID*\6.2.0.0\user.config

> [!NOTE]
> -   Les informations d’identification Gérer en tant que (ou autres) pour les serveurs de votre pool de serveurs ne sont pas stockées dans le profil itinérant. Les utilisateurs du Gestionnaire de serveur doivent les ajouter à chaque ordinateur à partir duquel ils veulent effectuer la gestion.
> -   Le profil itinérant du partage réseau n’est pas créé tant qu’un utilisateur ne s’est pas connecté au réseau, puis déconnecté pour la première fois. Le fichier **Serverlist.xml** est créé à ce stade.

Vous pouvez exporter les paramètres du Gestionnaire de serveur, améliorer la portabilité des paramètres du Gestionnaire de serveur ou les utiliser sur d’autres ordinateurs dans un des deux manières suivantes.

-   Pour exporter des paramètres vers un autre ordinateur joint au domaine, configurez l’utilisateur du Gestionnaire de serveur pour qu’un profil itinérant dans active directory utilisateurs et ordinateurs. Vous devez être un administrateur de domaine pour modifier les ordinateurs et les propriétés de l’utilisateur dans active directory les utilisateurs.

-   Pour exporter les paramètres vers un autre ordinateur dans un groupe de travail, copiez les deux fichiers précédents au même emplacement sur l’ordinateur à partir de laquelle vous souhaitez gérer à l’aide du Gestionnaire de serveur.

#### <a name="to-export-server-manager-settings-to-other-domain-joined-computers"></a>Pour exporter les paramètres du Gestionnaire de serveur vers d’autres ordinateurs appartenant à un domaine

1.  Dans les ordinateurs et utilisateurs active directory, ouvrez le **propriétés** boîte de dialogue pour un utilisateur du Gestionnaire de serveur.

2.  Sur le **profil** onglet, ajouter un chemin d’accès à un partage réseau pour stocker le profil utilisateur.

3.  Effectuez l’une des opérations suivantes :

    -   Sur les versions Anglais (en-us), les modifications apportées à la **Serverlist.xml** fichier sont automatiquement enregistrées dans le profil. Passez à l’étape suivante.

    -   Sur les autres versions, copiez les deux fichiers suivants à partir de l’ordinateur qui exécute le Gestionnaire de serveur vers le partage réseau qui fait partie du profil itinérant de l’utilisateur.

        -   %*appdata*%\Microsoft\Windows\ServerManager\Serverlist.xml

        -   %*localappdata*%\Microsoft_Corporation\ServerManager.exe_StrongName_*GUID*\6.2.0.0\user.config

4.  Cliquez sur **OK** pour enregistrer vos modifications et fermer la boîte de dialogue **Propriétés** .

#### <a name="to-export-server-manager-settings-to-computers-in-workgroups"></a>Pour exporter les paramètres du Gestionnaire de serveur vers des ordinateurs dans des groupes de travail

-   Sur un ordinateur à partir de laquelle vous souhaitez gérer les serveurs distants, remplacer les deux fichiers suivants avec les mêmes fichiers à partir d’un autre ordinateur qui exécute le Gestionnaire de serveur, et qui a les paramètres souhaités.

    -   %*appdata*%\Microsoft\Windows\ServerManager\Serverlist.xml

    -   %*localappdata*%\Microsoft_Corporation\ServerManager.exe_StrongName_*GUID*\6.2.0.0\user.config


