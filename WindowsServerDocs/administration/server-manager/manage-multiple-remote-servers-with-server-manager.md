---
title: Gérer plusieurs serveurs distants avec Gestionnaire de serveur
description: Gestionnaire de serveur
ms.prod: windows-server
ms.technology: manage-server-manager
ms.topic: article
ms.assetid: 3a17e686-e7f2-47e2-b7af-733777c38b5f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4b7a33e15287f24ee5b259618dfcfef3c6245564
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851502"
---
# <a name="manage-multiple-remote-servers-with-server-manager"></a>Gérer plusieurs serveurs distants avec Gestionnaire de serveur

>S’applique à Windows Server 2016

Gestionnaire de serveur est une console de gestion dans Windows Server 2012 R2 et Windows Server 2012 qui aide les professionnels de l’informatique à approvisionner et gérer des serveurs Windows locaux et distants à partir de leurs ordinateurs de bureau, sans avoir besoin d’un accès physique aux serveurs ni d’activer des connexions Bureau à distance protocole (rdP) sur chaque serveur. Bien que Gestionnaire de serveur soit disponible dans Windows Server 2008 R2 et Windows Server 2008, Gestionnaire de serveur a été mis à jour dans Windows Server 2012, pour prendre en charge la gestion multiserveur à distance et pour augmenter le nombre de serveurs qu’un administrateur peut gérer.

Dans nos tests, Gestionnaire de serveur dans Windows Server 2012 R2 et Windows Server 2012 peut être utilisé pour gérer jusqu’à 100 serveurs configurés avec une charge de travail classique. Le nombre de serveurs que vous pouvez gérer en utilisant une console du Gestionnaire de serveur unique peut varier en fonction de la quantité de données que vous demandez aux serveurs gérés ainsi que des ressources matérielles et réseau disponibles pour l’ordinateur exécutant le Gestionnaire de serveur. Lorsque la quantité de données à afficher est sur le point d’atteindre la capacité des ressources de cet ordinateur, vous pouvez être confronté à des réactions lentes du Gestionnaire de serveur ainsi qu’à des retards dans la réalisation des actualisations. Pour augmenter le nombre de serveurs que vous pouvez gérer à l’aide du Gestionnaire de serveur, nous vous conseillons de limiter la quantité de données d’événement que le Gestionnaire de serveur obtient de vos serveurs gérés en utilisant les paramètres de la boîte de dialogue **Configurer les données d’événement**. Configurer les données d’événement peut être ouverte à partir du menu **Tâches** dans la vignette **Événements**. Si vous devez gérer un très grand nombre de serveurs dans votre organisation, nous vous recommandons d’évaluer des produits de la [suite Microsoft System Center](https://go.microsoft.com/fwlink/p/?LinkId=239437).

Cette rubrique et ses sous-rubriques fournissent des informations sur l’utilisation des fonctionnalités de la console Gestionnaire de serveur. Cette rubrique contient les sections suivantes.

-   [Examiner les considérations initiales et la configuration système requise](#BKMK_1.1)

-   [Tâches que vous pouvez effectuer dans Gestionnaire de serveur](#BKMK_tasks)

-   [Démarrer Gestionnaire de serveur](#BKMK_start)

-   [Redémarrer les serveurs distants](#BKMK_restart)

-   [Exporter les paramètres de Gestionnaire de serveur vers d’autres ordinateurs](#BKMK_export)

## <a name="review-initial-considerations-and-system-requirements"></a><a name=BKMK_1.1></a>Examiner les considérations initiales et la configuration système requise
Les sections suivantes répertorient certaines considérations initiales que vous devez examiner, ainsi que la configuration matérielle et logicielle requise pour Gestionnaire de serveur.

### <a name="hardware-requirements"></a>Configuration matérielle
Gestionnaire de serveur est installé par défaut avec toutes les éditions de Windows Server 2012 R2 et Windows Server 2012. Aucune configuration matérielle supplémentaire n’existe pour Gestionnaire de serveur.

### <a name="software-and-configuration-requirements"></a><a name=BKMK_softconfig></a>Logiciels et configuration requise
Gestionnaire de serveur est installé par défaut avec toutes les éditions de Windows Server 2012. Bien que vous puissiez utiliser Gestionnaire de serveur pour gérer les [options d’installation minimale](https://go.microsoft.com/fwlink/p/?LinkID=241573) de windows server 2012 et windows Server 2008 R2 qui s’exécutent sur des ordinateurs distants, gestionnaire de serveur ne s’exécute pas directement sur les options d’installation minimale.

Pour gérer entièrement les serveurs distants qui exécutent Windows Server 2008 ou Windows Server 2008 R2, installez les mises à jour suivantes sur les serveurs que vous souhaitez gérer, dans l’ordre indiqué.

Pour gérer les serveurs qui exécutent Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008 à l’aide d’Gestionnaire de serveur dans Windows Server 2012 R2, appliquez les mises à jour suivantes aux systèmes d’exploitation plus anciens.

-   [.NET Framework 4,5](https://www.microsoft.com/download/details.aspx?id=30653)

-   [Windows Management Framework 4.0](https://go.microsoft.com/fwlink/?LinkId=293881). Windows Management Framework 4,0 Télécharger les fournisseurs de mises à jour du package Windows Management Instrumentation (WMI) sur Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008. Les fournisseurs WMI mis à jour permettent Gestionnaire de serveur collecter des informations sur les rôles et les fonctionnalités qui sont installés sur les serveurs gérés. Tant que la mise à jour n’est pas appliquée, les serveurs qui exécutent Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008 ont l’état de facilité de gestion **non accessible**.

-   La mise à jour des performances associée à [l’article 2682011](https://go.microsoft.com/fwlink/p/?LinkID=245487) de la base de connaissances permet gestionnaire de serveur de collecter des données de performances à partir de windows Server 2008 et windows Server 2008 R2. Cette mise à jour des performances n’est pas nécessaire sur les serveurs qui exécutent Windows Server 2012.

Pour gérer les serveurs qui exécutent Windows Server 2008 R2 ou Windows Server 2008, appliquez les mises à jour suivantes aux systèmes d’exploitation plus anciens.

-   [.NET Framework 4](https://www.microsoft.com/download/en/details.aspx?id=17718)

-   [Windows Management Framework 3,0](https://go.microsoft.com/fwlink/p/?LinkID=229019) Windows Management Framework 3,0 Télécharger les fournisseurs de mises à jour du package Windows Management Instrumentation (WMI) sur Windows Server 2008 et Windows Server 2008 R2. Les fournisseurs WMI mis à jour permettent Gestionnaire de serveur collecter des informations sur les rôles et les fonctionnalités qui sont installés sur les serveurs gérés. Tant que la mise à jour n’est pas appliquée, les serveurs qui exécutent Windows Server 2008 ou Windows Server 2008 R2 ont l’état de facilité de gestion **non accessible. Vérifiez que les versions antérieures exécutent Windows Management Framework 3,0**.

-   La mise à jour des performances associée à [l’article 2682011](https://go.microsoft.com/fwlink/p/?LinkID=245487) de la base de connaissances permet gestionnaire de serveur de collecter des données de performances à partir de windows Server 2008 et windows Server 2008 R2.

Gestionnaire de serveur s’exécute dans l’interface graphique serveur minimale ; autrement dit, lorsque la fonctionnalité de l’interpréteur de commandes graphique de serveur a été désinstallée. La fonctionnalité d’interpréteur de commandes graphique de serveur est installée par défaut sur Windows Server 2012 R2 et Windows Server 2012. Si vous désinstallez l’interpréteur de commandes graphique de serveur, la console Gestionnaire de serveur s’exécute, mais certaines applications ou certains outils disponibles à partir de la console ne sont pas disponibles. Les navigateurs Internet ne peuvent pas s’exécuter sans interpréteur de commandes graphique de serveur, de sorte que les pages Web et les applications telles que l’aide HTML (l’aide F1 de la console MMC, par exemple) ne peuvent pas être ouvertes. Vous ne pouvez pas ouvrir de boîtes de dialogue pour la configuration de la mise à jour automatique et des commentaires Windows lorsque l’interpréteur de commandes graphique de serveur n’est pas installé. les commandes qui ouvrent ces boîtes de dialogue dans la console Gestionnaire de serveur sont redirigées pour exécuter **sconfig. cmd**.

#### <a name="manage-remote-computers-from-a-client-computer"></a>Gérer des ordinateurs distants à partir d’un ordinateur client
La console Gestionnaire de serveur est fournie avec [Outils d’administration de serveur distant](https://go.microsoft.com/fwlink/?LinkID=304145) pour Windows 8.1 et [Outils d’administration de serveur distant](https://go.microsoft.com/fwlink/p/?LinkID=238560) pour Windows 8. Notez que lorsque Outils d’administration de serveur distant est installé sur un ordinateur client, vous ne pouvez pas gérer l’ordinateur local à l’aide de Gestionnaire de serveur ; Gestionnaire de serveur ne peut pas être utilisé pour gérer des ordinateurs ou des périphériques qui exécutent un système d’exploitation client Windows. Vous pouvez utiliser Gestionnaire de serveur pour gérer uniquement les serveurs Windows.

|Gestionnaire de serveur système d’exploitation source|Ciblé sur Windows Server 2012 R2 |Ciblé sur Windows Server 2012 |Ciblé sur Windows Server 2008 R2 ou Windows Server 2008 |Ciblé sur Windows Server 2003|
|-------------------------------|---------------------------------------|------------------------------------|-----------------------------------------------------------------------|------------------|
|Windows 8 ou Windows Server 2012 |Non pris en charge|Prise en charge complète|Une fois [la configuration logicielle requise et les conditions de configuration](#BKMK_softconfig) satisfaites, la plupart des tâches de gestion peuvent être effectuées, mais pas l’installation ni la désinstallation de rôles ou de fonctionnalités|Prise en charge limitée ; états en ligne et hors connexion uniquement|
|Windows 8.1 ou Windows Server 2012 R2 |Prise en charge complète|Prise en charge complète|Une fois [la configuration logicielle requise et les conditions de configuration](#BKMK_softconfig) satisfaites, la plupart des tâches de gestion peuvent être effectuées, mais pas l’installation ni la désinstallation de rôles ou de fonctionnalités|Prise en charge limitée ; états en ligne et hors connexion uniquement|

###### <a name="to-start-server-manager-on-a-client-computer"></a>Pour démarrer le Gestionnaire de serveur sur un ordinateur client

1.  Suivez les instructions dans [déployer outils d’administration de serveur distant](https://go.microsoft.com/fwlink/?LinkID=238562) pour installer Outils d’administration de serveur distant pour Windows 8.1 ou outils d’administration de serveur distant pour Windows 8.

2.  Dans l’écran d' **Accueil** , cliquez sur **Gestionnaire de serveur**. La vignette **Gestionnaire de serveur** est disponible après avoir installé Outils d’administration de serveur distant.

3.  Si ni les **Outils d’administration** ni les vignettes **Gestionnaire de serveur** ne s’affichent dans l’écran d' **accueil** après l’installation de outils d’administration de serveur distant, et que la recherche de gestionnaire de serveur sur l’écran d' **Accueil** n’affiche pas les résultats, vérifiez que le paramètre **afficher les outils d’administration** est activé. Pour afficher ce paramètre, placez le curseur de la souris dans l’angle supérieur droit de l’écran d' **Accueil** , puis cliquez sur **paramètres**. Si le paramètre **Afficher les outils d’administration** est désactivé, activez-le pour afficher les outils que vous avez installés dans le cadre des Outils d’administration de serveur distant.

Pour plus d’informations sur l’exécution de Outils d’administration de serveur distant pour Windows 8 afin de gérer des serveurs distants, consultez [Outils d’administration de serveur distant](https://go.microsoft.com/fwlink/?LinkID=221055) sur le wiki technet.

#### <a name="configure-remote-management-on-servers-that-you-want-to-manage"></a>Configurer la gestion à distance sur des serveurs que vous voulez gérer

> [!IMPORTANT]
> Par défaut, Gestionnaire de serveur et la gestion à distance de Windows PowerShell sont activés dans Windows Server 2012 R2 et Windows Server 2012.

Pour effectuer des tâches de gestion sur des serveurs distants à l’aide de Gestionnaire de serveur, les serveurs distants que vous voulez gérer doivent être configurés pour autoriser la gestion à distance à l’aide de Gestionnaire de serveur et Windows PowerShell. Si la gestion à distance a été désactivée sur Windows Server 2012 R2 ou Windows Server 2012 et que vous souhaitez la réactiver, procédez comme suit.

##### <a name="to-configure-server-manager-remote-management-on--windows-server-2012-r2--or--windows-server-2012--by-using-the-windows-interface"></a><a name=BKMK_windows></a>Pour configurer Gestionnaire de serveur la gestion à distance sur Windows Server 2012 R2 ou Windows Server 2012 à l’aide de l’interface Windows

1.  > [!NOTE]
    > Les paramètres contrôlés par la boîte de dialogue **configurer l’administration à distance** n’affectent pas les parties de gestionnaire de serveur qui utilisent DCOM pour les communications à distance.

    Effectuez l’une des opérations suivantes pour ouvrir Gestionnaire de serveur s’il n’est pas déjà ouvert.

    -   Dans la barre des tâches Windows, cliquez sur le bouton Gestionnaire de serveur.

    -   Dans l’écran d' **Accueil** , cliquez sur **Gestionnaire de serveur**.

2.  Dans la zone **Propriétés** de la page **serveurs locaux** , cliquez sur la valeur de lien hypertexte pour la propriété **gestion à distance** .

3.  Effectuez l’une des opérations ci-dessous, puis cliquez sur **OK**.

    -   Pour empêcher la gestion à distance de cet ordinateur à l’aide de Gestionnaire de serveur (ou Windows PowerShell s’il est installé), désactivez la case à cocher **activer la gestion à distance de ce serveur à partir des autres ordinateurs** .

    -   Pour permettre la gestion à distance de cet ordinateur à l’aide de Gestionnaire de serveur ou de Windows PowerShell, sélectionnez **activer la gestion à distance de ce serveur à partir d’autres ordinateurs**.

##### <a name="to-enable-server-manager-remote-management-on--windows-server-2012-r2--or--windows-server-2012--by-using-windows-powershell"></a><a name=BKMK_ps></a>Pour activer la gestion à distance Gestionnaire de serveur sur Windows Server 2012 R2 ou Windows Server 2012 à l’aide de Windows PowerShell

1.  Effectuez l'une des opérations suivantes :

    -   Pour exécuter Windows PowerShell en tant qu’administrateur à partir de l’écran d' **Accueil** , cliquez avec le bouton droit sur la vignette **Windows PowerShell** , puis cliquez sur **exécuter en tant qu’administrateur**.

    -   Pour exécuter Windows PowerShell en tant qu’administrateur à partir du bureau, cliquez avec le bouton droit sur le raccourci **Windows PowerShell** dans la barre des tâches, puis cliquez sur **exécuter en tant qu’administrateur**.

2.  tapez ce qui suit, puis appuyez sur **entrée** pour activer toutes les exceptions de règle de pare-feu requises.

    **Configure-SMRemoting. exe-Enable**

    > [!NOTE]
    > Cette commande fonctionne également dans une invite de commandes ouverte avec des droits d’utilisateur élevés (Exécuter en tant qu’administrateur).

    en cas d’échec de l’activation de la gestion à distance, consultez [about_remote_Troubleshooting](https://go.microsoft.com/fwlink/p/?LinkID=135188) sur Microsoft TechNet pour obtenir des conseils de dépannage et des meilleures pratiques.

###### <a name="to-enable-server-manager-and-windows-powershell-remote-management-on-older-operating-systems"></a>Pour activer la gestion à distance via le Gestionnaire de serveur et Windows PowerShell sur des systèmes d’exploitation plus anciens

-   Effectuez l'une des opérations suivantes :

    -   Pour activer la gestion à distance sur les serveurs qui exécutent Windows Server 2008 R2, consultez [gestion à distance avec Gestionnaire de serveur](https://go.microsoft.com/fwlink/?LinkID=137378) dans l’aide de windows Server 2008 R2.

    -   Pour activer la gestion à distance sur les serveurs qui exécutent Windows Server 2008, consultez [activer et utiliser des commandes distantes dans Windows PowerShell](https://go.microsoft.com/fwlink/p/?LinkId=242565).

    -   Pour activer la gestion à distance sur des serveurs qui exécutent Windows Server 2003, activez les exceptions WMI DCOM dans le Pare-feu Windows. Pour plus d’informations sur la procédure à suivre sur des serveurs qui exécutent Windows Server 2003, voir [Connexion via le Pare-feu Windows](https://msdn.microsoft.com/library/aa389286.aspx) sur MSDN.

## <a name="tasks-that-you-can-perform-in-server-manager"></a><a name=BKMK_tasks></a>Tâches que vous pouvez effectuer dans Gestionnaire de serveur
Gestionnaire de serveur rend l’administration du serveur plus efficace en permettant aux administrateurs d’effectuer les tâches du tableau suivant à l’aide d’un seul outil. Dans Windows Server 2012 R2 et Windows Server 2012, les utilisateurs standard d’un serveur et les membres du groupe administrateurs peuvent effectuer des tâches de gestion dans Gestionnaire de serveur, mais par défaut, les utilisateurs standard ne peuvent pas effectuer certaines tâches, comme indiqué dans le tableau suivant.

Les administrateurs peuvent utiliser deux applets de commande Windows PowerShell dans le module d’applet de commande Gestionnaire de serveur, [Enable-ServerManagerStandardUserremoting](https://technet.microsoft.com/library/jj205470.aspx) et [Disable-ServerManagerStandardUserremoting](https://technet.microsoft.com/library/jj205468.aspx), pour contrôler davantage l’accès utilisateur standard à certaines données supplémentaires. L’applet de commande **Enable-ServerManagerStandardUserremoting** peut fournir à un ou plusieurs utilisateurs standard et non-administrateurs l’accès aux données d’inventaire des événements, des services, des compteurs de performances et des rôles et des fonctionnalités.

> [!IMPORTANT]
> Le Gestionnaire de serveur ne peut pas être utilisé pour gérer une version plus récente du système d’exploitation Windows Server. Gestionnaire de serveur s’exécutant sur Windows Server 2012 ou Windows 8 ne peut pas être utilisé pour gérer les serveurs qui exécutent Windows Server 2012 R2.

|Description de la tâche|Administrateurs (y compris le compte Administrateur intégré)|Utilisateurs de serveur standard|
|----------|----------------------------------|-------------|
|Ajoutez des serveurs distants à un pool de serveurs que Gestionnaire de serveur pouvez utiliser pour gérer.|Oui|Non|
|Créez et modifiez des groupes de serveurs personnalisés, tels que des serveurs qui se trouvent dans un emplacement géographique spécifique ou qui servent un objectif spécifique.|Oui|Oui|
|Installer ou désinstaller des rôles, des services de rôle et des fonctionnalités sur l’ordinateur local ou sur des serveurs distants qui exécutent Windows Server 2012 R2 ou Windows Server 2012. Pour obtenir les définitions des rôles, des services de rôle et des fonctionnalités, consultez [rôles, services de rôle et fonctionnalités](https://go.microsoft.com/fwlink/p/?LinkId=239558).|Oui|Non|
|Afficher et modifier les fonctionnalités et rôles de serveur installés sur les serveurs locaux ou distants. **Remarque :** Dans Gestionnaire de serveur, les données de rôle et de fonctionnalité s’affichent dans la langue de base du système, également appelée langue de l’interface utilisateur graphique par défaut du système, ou la langue sélectionnée lors de l’installation du système d’exploitation.|Oui|Les utilisateurs standard peuvent afficher et gérer les fonctionnalités et rôles ainsi qu’effectuer des tâches, telles que l’affichage des événements de rôle, mais ils ne peuvent pas ajouter ni supprimer des services de rôle.|
|Démarrez les outils de gestion, tels que les composants logiciels enfichables Windows PowerShell ou MMC. Vous pouvez démarrer une session Windows PowerShell ciblée sur un serveur distant en cliquant avec le bouton droit sur le serveur dans la vignette **serveurs** , puis en cliquant sur **Windows PowerShell**. Vous pouvez démarrer les composants logiciels enfichables MMC à partir du menu **Outils** de la console Gestionnaire de serveur, puis pointer la console MMC vers un ordinateur distant après avoir ouvert le composant logiciel enfichable.|Oui|Oui|
|Gérer les serveurs distants avec différentes informations d’identification en cliquant avec le bouton droit sur un serveur dans la vignette **Serveurs**, puis en cliquant sur **Gérer en tant que**. Vous pouvez utiliser **Gérer en tant que** pour les tâches de gestion de serveur générales et des services de fichiers et de stockage.|Oui|Non|
|Effectuer des tâches de gestion associées au cycle de vie opérationnel des serveurs, tels que le démarrage ou l’arrêt des services ; et de démarrer d’autres outils qui vous permettent de configurer les paramètres réseau, les utilisateurs et les groupes d’un serveur, ainsi que les connexions Bureau à distance.|Oui|Les utilisateurs standard ne peuvent pas démarrer ni arrêter des services. Ils peuvent modifier le nom du serveur local, le groupe de travail ou l’appartenance au domaine et les paramètres de Bureau à distance, mais ils sont invités par le contrôle de compte d’utilisateur à fournir des informations d’identification d’administrateur avant de pouvoir effectuer ces tâches. Ils ne peuvent pas modifier les paramètres de gestion à distance.|
|Effectuer des tâches de gestion associées au cycle de vie opérationnel des rôles installés sur les serveurs, notamment l’analyse des rôles pour une mise en conformité avec les meilleures pratiques.|Oui|Les utilisateurs standard ne peuvent pas exécuter des analyses de Best Practices Analyzer.|
|Déterminer l’état d’un serveur, identifier des événements critiques, analyser et dépanner des problèmes ou des échecs de configuration.|Oui|Oui|
|Personnaliser les événements, les données de performances, les services et les Best Practices Analyzer les résultats à propos desquels vous voulez être alerté dans le tableau de bord Gestionnaire de serveur.|Oui|Oui|
|Redémarrer les serveurs.|Oui|Non|
|Actualisez les données affichées dans la console Gestionnaire de serveur sur les serveurs gérés.|Oui|Non|

> [!NOTE]
> Le Gestionnaire de serveur peut recevoir uniquement des informations sur l’état en ligne ou hors connexion en provenance de serveurs exécutant Windows Server 2003. Gestionnaire de serveur ne peut pas être utilisé pour ajouter des rôles et des fonctionnalités aux serveurs qui exécutent Windows Server 2008 R2, Windows Server 2008 ou Windows Server 2003.

## <a name="start-server-manager"></a><a name=BKMK_start></a>Démarrer Gestionnaire de serveur
Gestionnaire de serveur démarre automatiquement par défaut sur les serveurs qui exécutent Windows Server 2012 lorsqu’un membre du groupe administrateurs se connecte à un serveur. Si vous fermez Gestionnaire de serveur, redémarrez-le de l’une des manières suivantes. Cette section contient également les étapes permettant de modifier le comportement par défaut et d’empêcher le démarrage automatique de Gestionnaire de serveur.

#### <a name="to-start-server-manager-from-the-start-screen"></a>Pour démarrer le Gestionnaire de serveur à partir de l’écran d’accueil

-   Sur l’écran d' **Accueil** de Windows, cliquez sur la vignette **Gestionnaire de serveur** .

#### <a name="to-start-server-manager-from-the-windows-desktop"></a>Pour démarrer le Gestionnaire de serveur à partir du Bureau Windows

-   Dans la barre des tâches Windows, cliquez sur **Gestionnaire de serveur**.

#### <a name="to-prevent-server-manager-from-starting-automatically"></a>Pour empêcher le démarrage automatique du Gestionnaire de serveur

1.  Dans la console Gestionnaire de serveur, dans le menu **gérer** , cliquez sur Propriétés de la **Gestionnaire de serveur**.

2.  Dans la boîte de dialogue **Propriétés du Gestionnaire de serveur**, activez la case à cocher **Ne pas démarrer automatiquement le Gestionnaire de serveur à l’ouverture de session**. Cliquez sur **OK**.

3.  Vous pouvez également empêcher le démarrage automatique de Gestionnaire de serveur en activant le paramètre stratégie de groupe, **ne pas démarrer automatiquement gestionnaire de serveur à l’ouverture de session**. Le chemin d’accès à ce paramètre de stratégie, dans la console de l’éditeur de stratégie de groupe local, est Computer Configuration\Administrative système \ Manager.

## <a name="restart-remote-servers"></a><a name=BKMK_restart></a>Redémarrer les serveurs distants
Vous pouvez redémarrer un serveur distant à partir de la vignette **serveurs** d’une page de rôle ou de groupe dans Gestionnaire de serveur.

> [!IMPORTANT]
> Le redémarrage d’un serveur distant force le serveur à redémarrer, même si des utilisateurs sont toujours connectés au serveur distant et que des programmes avec des données non enregistrées sont toujours ouverts. Ce comportement est différent de la fermeture ou du redémarrage de l’ordinateur local sur lequel vous êtes invité à enregistrer les données de programme non enregistrées et à confirmer que vous voulez forcer les utilisateurs connectés à fermer la session. Assurez-vous de pouvoir forcer d’autres utilisateurs à se déconnecter des serveurs distants et de pouvoir ignorer les données non enregistrées dans les programmes exécutés sur les serveurs distants.
> 
> Si une actualisation automatique se produit dans Gestionnaire de serveur alors qu’un serveur géré est en cours d’arrêt et de redémarrage, des erreurs d’état de la gestion et de l’actualisation peuvent se produire pour le serveur géré, car Gestionnaire de serveur ne peut pas se connecter au serveur distant tant que le redémarrage n’est pas terminé.

#### <a name="to-restart-remote-servers-in-server-manager"></a>Pour redémarrer des serveurs distants dans le Gestionnaire de serveur

1.  Ouvrez une page d’hébergement de groupe de serveurs ou de rôles dans Gestionnaire de serveur.

2.  Sélectionnez un ou plusieurs serveurs distants que vous avez ajoutés à Gestionnaire de serveur. Appuyez sur la touche **Ctrl** et maintenez-la enfoncée pendant que vous cliquez pour sélectionner plusieurs serveurs simultanément. Pour plus d’informations sur l’ajout de serveurs au pool de serveurs Gestionnaire de serveur, voir [Ajouter des serveurs à des gestionnaire de serveur](add-servers-to-server-manager.md).

3.  Cliquez avec le bouton droit sur les serveurs sélectionnés, puis cliquez sur **Redémarrer le serveur**.

## <a name="export-server-manager-settings-to-other-computers"></a><a name=BKMK_export></a>Exporter les paramètres de Gestionnaire de serveur vers d’autres ordinateurs
Dans Gestionnaire de serveur, votre liste de serveurs gérés, les modifications apportées aux paramètres de la console Gestionnaire de serveur et les groupes personnalisés que vous avez créés sont stockés dans les deux fichiers suivants. Vous pouvez réutiliser ces paramètres sur d’autres ordinateurs qui exécutent la même version de Gestionnaire de serveur (pas les ordinateurs qui exécutent l’option d’installation minimale) ou Windows 8. Outils d’administration de serveur distant doit s’exécuter sur des ordinateurs clients Windows pour exporter les paramètres de Gestionnaire de serveur vers ces ordinateurs.

-   %*AppData*% \ Microsoft\Windows\ServerManager\Serverlist.Xml

-   %*AppData*% \ Local \ Microsoft_Corporation \Servermanager. exe_StrongName_*GUID*\6.2.0.0\User.config

> [!NOTE]
> -   Les informations d’identification Gérer en tant que (ou autres) pour les serveurs de votre pool de serveurs ne sont pas stockées dans le profil itinérant. Les utilisateurs du Gestionnaire de serveur doivent les ajouter à chaque ordinateur à partir duquel ils veulent effectuer la gestion.
> -   Le profil itinérant du partage réseau n’est pas créé tant qu’un utilisateur ne s’est pas connecté au réseau, puis déconnecté pour la première fois. Le fichier **Serverlist.xml** est créé à ce stade.

Vous pouvez exporter des paramètres de Gestionnaire de serveur, rendre les paramètres Gestionnaire de serveur portables ou les utiliser sur d’autres ordinateurs de l’une des deux manières suivantes.

-   Pour exporter les paramètres vers un autre ordinateur appartenant à un domaine, configurez l’utilisateur Gestionnaire de serveur pour qu’il dispose d’un profil itinérant dans utilisateurs et ordinateurs Active Directory. Vous devez être administrateur de domaine pour modifier les propriétés de l’utilisateur dans utilisateurs et ordinateurs Active Directory.

-   Pour exporter les paramètres vers un autre ordinateur d’un groupe de travail, copiez les deux fichiers précédents au même emplacement sur l’ordinateur à partir duquel vous souhaitez effectuer la gestion à l’aide de Gestionnaire de serveur.

#### <a name="to-export-server-manager-settings-to-other-domain-joined-computers"></a>Pour exporter les paramètres du Gestionnaire de serveur vers d’autres ordinateurs appartenant à un domaine

1.  Dans utilisateurs et ordinateurs Active Directory, ouvrez la boîte de dialogue **Propriétés** pour un utilisateur gestionnaire de serveur.

2.  Sous l’onglet **Profil** , ajoutez un chemin d’accès à un partage réseau pour stocker le profil de l’utilisateur.

3.  Effectuez l'une des opérations suivantes :

    -   Sur les builds de l’anglais américain (en-US), les modifications apportées au fichier **serverlist. xml** sont automatiquement enregistrées dans le profil. Passez à l’étape suivante.

    -   Sur les autres versions, copiez les deux fichiers suivants de l’ordinateur qui exécute Gestionnaire de serveur vers le partage réseau qui fait partie du profil itinérant de l’utilisateur.

        -   %*AppData*% \ Microsoft\Windows\ServerManager\Serverlist.Xml

        -   %*LocalAppData*% \ Microsoft_Corporation \Servermanager. exe_StrongName_*GUID*\6.2.0.0\User.config

4.  Cliquez sur **OK** pour enregistrer vos modifications et fermer la boîte de dialogue **Propriétés**.

#### <a name="to-export-server-manager-settings-to-computers-in-workgroups"></a>Pour exporter les paramètres du Gestionnaire de serveur vers des ordinateurs dans des groupes de travail

-   Sur un ordinateur à partir duquel vous voulez gérer des serveurs distants, remplacez les deux fichiers suivants par les mêmes fichiers d’un autre ordinateur qui exécute Gestionnaire de serveur, et dont les paramètres vous intéressent.

    -   %*AppData*% \ Microsoft\Windows\ServerManager\Serverlist.Xml

    -   %*LocalAppData*% \ Microsoft_Corporation \Servermanager. exe_StrongName_*GUID*\6.2.0.0\User.config


