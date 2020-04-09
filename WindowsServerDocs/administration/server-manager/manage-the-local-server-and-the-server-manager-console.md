---
title: Gérer le serveur local et la console du Gestionnaire de serveur
description: Gestionnaire de serveur
ms.prod: windows-server
ms.technology: manage-server-manager
ms.topic: article
ms.assetid: eeb32f65-d588-4ed5-82ba-1ca37f517139
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d64d45fec0c48f66da72dfee7ab9f1f9965205ad
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851492"
---
# <a name="manage-the-local-server-and-the-server-manager-console"></a>Gérer le serveur local et la console du Gestionnaire de serveur

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dans Windows Server, Gestionnaire de serveur vous permet de gérer à la fois le serveur local (si vous exécutez Gestionnaire de serveur sur Windows Server et non sur un système d’exploitation client Windows) et des serveurs distants qui exécutent Windows Server 2008 et des versions plus récentes du système d’exploitation Windows Server.

La page **serveur local** de gestionnaire de serveur affiche les propriétés du serveur, les événements, les données des compteurs de performance et de service, ainsi que les résultats de Best Practices Analyzer (BPA) pour le serveur local. Les vignettes Événements, Services, BPA et Performances fonctionnent comme dans les pages de rôles et de groupes de serveurs. Pour plus d’informations sur la configuration des données affichées dans ces vignettes, voir [View et Configure Performance, Event, et Service Data](view-and-configure-performance-event-and-service-data.md) et [Run Best Practices Analyzer Scans et Manage Scan Results](run-best-practices-analyzer-scans-and-manage-scan-results.md).

Les commandes de menu et les paramètres des barres d’en-tête de la console Gestionnaire de serveur s’appliquent globalement à tous les serveurs de votre pool de serveurs, et vous permettent d’utiliser Gestionnaire de serveur pour gérer l’ensemble du pool de serveurs.

Cette rubrique contient les sections suivantes.

-   [Arrêter le serveur local](#BKMK_shutdown)

-   [Configurer les propriétés de Gestionnaire de serveur](#BKMK_props)

-   [Gérer la console Gestionnaire de serveur](#BKMK_managesm)

-   [Personnaliser les outils qui sont affichés dans le menu outils](#BKMK_tools)

-   [Gérer les rôles dans les pages d’hébergement des rôles](#BKMK_roles)

## <a name="shut-down-the-local-server"></a><a name=BKMK_shutdown></a>Arrêter le serveur local
Le menu **tâches** de la vignette **Propriétés** du serveur local vous permet de démarrer une session Windows PowerShell sur le serveur local, d’ouvrir le composant logiciel enfichable MMC Gestion de l' **ordinateur** ou d’ouvrir les composants logiciels enfichables MMC pour les rôles ou les fonctionnalités installés sur le serveur local. Vous pouvez également arrêter le serveur local en utilisant la commande **Arrêter le serveur local** dans ce menu **Tâches**. La commande **Arrêter le serveur local** est également disponible pour le serveur local dans la vignette **Serveurs** de la page **Tous les serveurs**, ou dans toute page de groupe ou de rôle dans laquelle le serveur local est représenté.

L’arrêt du serveur local à l’aide de cette méthode, contrairement à l’arrêt de Windows Server 2016 à partir de l’écran d' **Accueil** , ouvre la boîte de dialogue **arrêt de Windows** , qui vous permet de spécifier les raisons de l’arrêt dans la zone moniteur d’événements de mise **hors tension** .

> [!NOTE]
> Seuls les membres du groupe Administrateurs peuvent arrêter ou redémarrer un serveur. Les utilisateurs standard ne peuvent pas arrêter ni redémarrer un serveur. Un clic sur la commande **Arrêter le serveur local** entraîne la fermeture des sessions serveur des utilisateurs standard. Le résultat est le même que lorsqu’un utilisateur standard exécute la commande d’arrêt **Alt+F4** à partir du Bureau du serveur.

## <a name="configure-server-manager-properties"></a><a name=BKMK_props></a>Configurer les propriétés de Gestionnaire de serveur
Vous pouvez afficher ou modifier les paramètres suivants dans la vignette **Propriétés** de la page **Serveur local**. Pour modifier la valeur d’un paramètre, cliquez sur la valeur hypertexte du paramètre.

> [!NOTE]
> En règle générale, les propriétés affichées dans la vignette **Propriétés** de la page Serveur local ne peuvent être modifiées que sur le serveur local. Vous ne pouvez pas modifier les propriétés du serveur local à partir d’un ordinateur distant à l’aide de Gestionnaire de serveur, car la vignette **Propriétés** ne peut obtenir des informations que sur l’ordinateur local, et non sur les ordinateurs distants.
> 
> Étant donné que de nombreuses propriétés affichées dans la vignette **Propriétés** sont contrôlées par des outils qui ne font pas partie de gestionnaire de serveur (panneau de configuration, par exemple), les modifications apportées aux paramètres de **Propriétés** ne sont pas toujours affichées immédiatement dans la vignette **Propriétés** . Par défaut, les données de la vignette **Propriétés** sont actualisées toutes les deux minutes. Pour actualiser les données de la vignette des **Propriétés** immédiatement, cliquez sur **Actualiser** dans la barre d’adresses gestionnaire de serveur.

|Paramètre|Description|
|------|--------|
|nom de l'ordinateur|Affiche le nom convivial de l’ordinateur et ouvre la boîte de dialogue **Propriétés système** qui vous permet de modifier le nom du serveur, l’appartenance au domaine et d’autres paramètres système tels que les profils utilisateur.|
|Domaine (ou Groupe de travail, si le serveur n’appartient pas à un domaine)|Affiche le domaine ou groupe de travail duquel le serveur est un membre. Ouvre la boîte de dialogue **Propriétés système** qui vous permet de modifier le nom du serveur, l’appartenance au domaine et d’autres paramètres système tels que les profils utilisateur.|
|Pare-feu Windows|Affiche l’état du Pare-feu Windows pour le serveur local. Ouvre **Panneau de configuration\Système et sécurité\Pare-feu Windows**. Pour plus d’informations sur la configuration du Pare-feu Windows, voir [Pare-feu Windows avec fonctions avancées de sécurité et IPsec](https://go.microsoft.com/fwlink/?LinkId=253465).|
|gestion à distance|Affiche Gestionnaire de serveur et l’état de la gestion à distance de Windows PowerShell. Ouvre la boîte de dialogue **configurer l’administration à distance** . Pour plus d’informations sur la gestion à distance, voir [configurer la gestion à distance dans Gestionnaire de serveur](configure-remote-management-in-server-manager.md).|
|Bureau à distance|Indique si les utilisateurs peuvent se connecter au serveur à distance à l’aide de sessions Bureau à distance. Ouvre l’onglet **distant** de la boîte de dialogue **Propriétés système** .|
|Association de cartes réseau|Indique si le serveur local participe à l’association de cartes réseau. Ouvre la boîte de dialogue **Association de cartes réseau** et vous permet de joindre le serveur local à une association de cartes réseau si vous le souhaitez. Pour plus d’informations sur l’association de cartes réseau, voir le [livre blanc relatif à l’association de cartes réseau](https://go.microsoft.com/fwlink/?LinkID=253449).|
|Ethernet|Affiche l’état du réseau du serveur. Ouvre **Panneau de configuration\Réseau et Internet\Connexions réseau**.|
|Version du système d'exploitation|Ce champ en lecture seule affiche le numéro de version du système d’exploitation Windows exécuté par le serveur local.|
|Informations sur le matériel|Ce champ en lecture seule affiche le fabricant ainsi que le nom et le numéro de modèle du matériel de serveur.|
|Dernières mises à jour installées|Affiche le jour et l’heure de la dernière installation des mises à jour de Windows. Ouvre **Panneau de configuration\Système et sécurité\Windows Update**.|
|Windows Update|Affiche les paramètres de Windows Update pour le serveur local. Ouvre **Panneau de configuration\Système et sécurité\Windows Update**.|
|Dernière recherche de mises à jour|Affiche le jour et l’heure de la dernière recherche de mises à jour de Windows disponibles par le serveur. Ouvre **Panneau de configuration\Système et sécurité\Windows Update**.|
|Rapport d'erreurs Windows|Affiche l’état de l’abonnement concernant le rapport d’erreurs Windows. Ouvre la boîte de dialogue **Configuration du rapport d’erreurs Windows**. Pour plus d’informations sur le rapport d’erreurs Windows, ses avantages, les déclarations de confidentialité et les paramètres d’abonnement, voir [Rapport d’erreurs Windows](https://go.microsoft.com/fwlink/?LinkID=245991).|
|Programme d'amélioration du produit|Affiche l’état de l’abonnement concernant le Programme d’amélioration de l’expérience utilisateur Windows. Ouvre la boîte de dialogue **Configuration du Programme d’amélioration de l’expérience utilisateur**. Pour plus d’informations sur le Programme d’amélioration de l’expérience utilisateur Windows, ses avantages et paramètres d’abonnement, voir [Programme d’amélioration de l’expérience utilisateur Windows](https://go.microsoft.com/fwlink/?LinkID=245992).|
|Configuration de sécurité renforcée d’Internet Explorer|Indique si la Configuration de sécurité renforcée d’Internet Explorer (également connue sous le nom de renforcement de IE ou IE ESC) est activée ou désactivée. Ouvre la boîte de dialogue **Configuration de sécurité renforcée d’Internet Explorer**. La Configuration de sécurité renforcée d’Internet Explorer est une mesure de sécurité pour les serveurs qui empêche l’ouverture des pages Web dans Internet Explorer. Pour plus d’informations sur la Configuration de sécurité renforcée d’Internet Explorer, ses avantages et paramètres, voir [Internet Explorer : Configuration de sécurité renforcée](https://go.microsoft.com/fwlink/?LinkId=253461).|
|fuseau horaire|Affiche le fuseau horaire du serveur local. Ouvre la boîte de dialogue **date et heure** .|
|Product ID|Affiche l’état d’activation de Windows et le numéro d’identification de produit (si Windows a été activé) du système d’exploitation Windows Server 2016. Il ne s’agit pas du même numéro que la clé de produit Windows. Ouvre la boîte de dialogue **Activation de Windows**.|
|Processeurs|Ce champ en lecture seule affiche le fabricant, le nom du modèle et les informations sur la vitesse des processeurs du serveur local.|
|Mémoire installée (RAM)|Ce champ en lecture seule affiche la quantité de mémoire RAM disponible, en gigaoctets.|
|Espace disque total|Ce champ en lecture seule affiche la quantité d’espace disque disponible, en gigaoctets.|

## <a name="manage-the-server-manager-console"></a><a name=BKMK_managesm></a>Gérer la console Gestionnaire de serveur
Les paramètres globaux qui s’appliquent à l’ensemble de la console Gestionnaire de serveur, et à tous les serveurs distants qui ont été ajoutés au pool de serveurs Gestionnaire de serveur, se trouvent dans les barres d’en-tête en haut de la fenêtre de console Gestionnaire de serveur.

### <a name="add-servers-to-server-manager"></a>Ajouter des serveurs à Gestionnaire de serveur
La commande qui ouvre la boîte de dialogue **Ajouter des serveurs** et vous permet d’ajouter des serveurs physiques ou virtuels distants au pool de serveurs gestionnaire de serveur, se trouve dans le menu **gérer** de la console Gestionnaire de serveur. Pour plus d’informations sur l’ajout de serveurs, voir [Ajouter des serveurs à des gestionnaire de serveur](add-servers-to-server-manager.md).

### <a name="refresh-data-that-is-displayed-in-server-manager"></a>Actualiser les données qui sont affichées dans le Gestionnaire de serveur
Vous pouvez configurer l’intervalle d’actualisation des données affichées dans Gestionnaire de serveur dans la boîte de dialogue **Propriétés de gestionnaire de serveur** , que vous ouvrez à partir du menu **gérer** .

##### <a name="to-configure-the-refresh-interval-in-server-manager"></a>Pour configurer l’intervalle d’actualisation dans le Gestionnaire de serveur

1.  Dans le menu **gérer** de la console Gestionnaire de serveur, cliquez sur Propriétés de la **Gestionnaire de serveur**.

2.  Dans la boîte de dialogue Propriétés de la **Gestionnaire de serveur** , spécifiez une période, en minutes, pour la durée écoulée entre les actualisations des données affichées dans Gestionnaire de serveur. La valeur par défaut est de 10 minutes. Cliquez sur OK lorsque vous avez terminé.

#### <a name="refresh-limitations"></a>Limites de l’actualisation
L’actualisation s’applique globalement aux données de tous les serveurs que vous avez ajoutés au pool de serveurs Gestionnaire de serveur. Vous ne pouvez pas actualiser des données ni configurer différents intervalles d’actualisation pour des serveurs, rôles ou groupes individuels.

Lorsque des serveurs qui se trouvent dans un cluster sont ajoutés à Gestionnaire de serveur, qu’il s’agisse d’ordinateurs physiques ou de machines virtuelles, la première actualisation des données peut échouer ou afficher des données uniquement pour le serveur hôte des objets en cluster. Les actualisations suivantes affichent des données précises pour les serveurs physiques ou virtuels dans un cluster de serveurs.

Les données affichées dans les pages d’accès aux rôles de Gestionnaire de serveur pour les Services Bureau à distance, la gestion des adresses IP et les services de fichiers et de stockage ne s’actualisent pas automatiquement. Actualisez les données affichées dans ces pages manuellement, en appuyant sur **F5** ou en cliquant sur **Actualiser** dans l’en-tête de la console Gestionnaire de serveur lorsque vous êtes sur ces pages.

### <a name="add-or-remove-roles-or-features"></a>Ajouter ou supprimer des rôles ou des fonctionnalités
Les commandes qui ouvrent l’Assistant Ajout de rôles et de fonctionnalités et suppriment les rôles et les fonctionnalités de l’Assistant et vous permettent d’ajouter ou de supprimer des rôles, des services de rôle et des fonctionnalités sur les serveurs de votre pool de serveurs, dans le menu **gérer** de la console Gestionnaire de serveur et dans le menu **tâches** de la vignette **rôles et fonctionnalités** des pages de rôle ou de groupe. Pour plus d’informations sur l’ajout ou la suppression de rôles ou de fonctionnalités, voir [Installer ou désinstaller des rôles, des services de rôle ou des fonctionnalités](install-or-uninstall-roles-role-services-or-features.md).

Dans Gestionnaire de serveur, les données de rôle et de fonctionnalité s’affichent dans la langue de base du système, également appelée langue de l’interface utilisateur graphique par défaut du système, ou la langue sélectionnée lors de l’installation du système d’exploitation.

### <a name="create-server-groups"></a>créer des groupes de serveurs
La commande qui ouvre la boîte de dialogue **créer un groupe** de serveurs et vous permet de créer des groupes de serveurs personnalisés se trouve dans le menu **gérer** de la console Gestionnaire de serveur. Pour plus d’informations sur la création de groupes de serveurs, consultez [créer et gérer des groupes de serveurs](create-and-manage-server-groups.md).

### <a name="prevent-server-manager-from-opening-automatically-at-logon"></a>Empêcher l’ouverture automatique du Gestionnaire de serveur lors de l’ouverture de session
La case à cocher **ne pas démarrer gestionnaire de serveur automatiquement à l’ouverture de session** de la boîte de dialogue **Propriétés du Gestionnaire de serveur** contrôle si gestionnaire de serveur s’ouvre automatiquement à l’ouverture de session pour les membres du groupe Administrateurs sur un serveur local. Ce paramètre n’affecte pas le comportement de Gestionnaire de serveur lorsqu’il s’exécute sur Windows 10 dans le cadre de Outils d’administration de serveur distant. Pour plus d’informations sur la configuration de ce paramètre, consultez [Gestionnaire de serveur](server-manager.md).

### <a name="zoom-in-or-out"></a>Zoom avant ou arrière
Pour effectuer un zoom avant ou arrière sur l’affichage de la console Gestionnaire de serveur, vous pouvez utiliser les commandes **Zoom** du menu **affichage** ou appuyer sur **CTRL + plus (+)** pour effectuer un zoom avant et sur **CTRL + moins (-)** pour effectuer un zoom arrière.

## <a name="customize-tools-that-are-displayed-in-the-tools-menu"></a><a name=BKMK_tools></a>Personnaliser les outils qui sont affichés dans le menu outils
Le menu **Outils** de gestionnaire de serveur contient des liens souples vers des raccourcis dans le dossier **Outils d’administration** dans Panneau de configuration **/système et sécurité**. Le dossier **Outils d’administration** contient une liste de raccourcis ou de fichiers LNK pour les outils de gestion disponibles, tels que les composants logiciels enfichables mmc. gestionnaire de serveur remplit le menu **Outils** avec des liens vers ces raccourcis et copie la structure des dossiers du dossier **Outils d’administration** dans le menu **Outils** . Par défaut, les outils du dossier Outils d’administration sont classés dans une liste plate et triés par type et par nom. Dans le menu**Outils** de gestionnaire de serveur, les éléments sont triés uniquement par nom, et non par type.

Pour personnaliser le menu **Outils**, copiez les raccourcis d’outils ou de scripts à utiliser dans le dossier **Outils d’administration**. Vous pouvez également organiser vos raccourcis dans des dossiers, qui créent des menus en cascade dans le menu **Outils**. en outre, si vous souhaitez restreindre l’accès aux outils personnalisés dans le menu **Outils** , vous pouvez définir des droits d’accès utilisateur sur vos dossiers d’outils personnalisés dans outils d’administration, ou directement sur les fichiers d’outils ou de scripts d’origine.

Nous vous déconseillons de réorganiser les outils d’administration et système, et tous les outils de gestion associés aux fonctionnalités et rôles installés sur le serveur local. Le déplacement des outils de gestion des fonctionnalités et rôles peut empêcher la réussite de la désinstallation de ces outils de gestion lorsque cela est nécessaire. Après la désinstallation d’un rôle ou d’une fonctionnalité, un lien non fonctionnel vers un outil dont le raccourci a été déplacé peut demeurer dans le menu **Outils**. Si vous réinstallez le rôle, un lien dupliqué vers le même outil est créé dans le menu **Outils**, mais l’un des liens ne fonctionnera pas.

Les outils des rôles et des fonctionnalités qui sont installés dans le cadre des Outils d’administration de serveur distant sur un ordinateur basé sur le client Windows peuvent toutefois être organisés en dossiers personnalisés. La désinstallation du rôle ou de la fonctionnalité parent n’a aucun effet sur les raccourcis d’outils qui sont disponibles sur un ordinateur distant qui exécute Windows 10.

La procédure suivante décrit comment créer un exemple de dossier nommé *mytools*et déplacer les raccourcis de deux scripts Windows PowerShell dans le dossier qui est ensuite accessible à partir du menu outils de gestionnaire de serveur.

#### <a name="to-customize-the-tools-menu-by-adding-shortcuts-in-administrative-tools"></a>Pour personnaliser le menu Outils en ajoutant des raccourcis dans Outils d’administration

1.  Créez un dossier nommé *mytools* à un emplacement pratique.

    > [!NOTE]
    > En raison des droits d’accès limités sur le dossier **Outils d’administration**, vous n’êtes pas autorisé à créer un dossier directement dans le dossier **Outils d’administration** ; vous devez le créer ailleurs (par exemple, sur le Bureau), puis le copier dans le dossier **Outils d’administration**.

2.  Déplacez ou copiez le *mytools* vers panneau de configuration **/système et sécurité/outils d’administration**. Par défaut, vous devez être membre du groupe Administrateurs sur l’ordinateur pour apporter des modifications au dossier **Outils d’administration**.

3.  Si vous n’avez pas besoin de restreindre les droits d’accès utilisateur à vos raccourcis d’outils personnalisés, passez à l’étape 6. Sinon, cliquez avec le bouton droit sur le fichier d’outils (ou le dossier *MyTools*), puis cliquez sur **Propriétés**.

4.  Dans l’onglet **sécurité** de la boîte de dialogue **Propriétés** du fichier, cliquez sur **modifier**.

5.  pour les utilisateurs pour lesquels vous souhaitez restreindre l’accès aux outils, désactivez les cases à cocher pour les autorisations **lecture & exécution**, **lecture**et **écriture** . Ces autorisations sont héritées par le raccourci d’outils dans le dossier **Outils d’administration**.

    Si vous modifiez les droits d’accès d’un utilisateur lorsque celui-ci utilise Gestionnaire de serveur (ou que Gestionnaire de serveur est ouvert), vos modifications ne sont pas affichées dans le menu **Outils** tant que l’utilisateur n’a pas redémarré gestionnaire de serveur.

    > [!NOTE]
    > Si vous limitez l’accès à un dossier entier que vous avez copié dans outils d’administration, les utilisateurs avec accès restreint ne peuvent voir ni le dossier ni son contenu dans le menu**Outils** de gestionnaire de serveur.
    > 
    > Modifiez les autorisations pour le dossier dans le dossier **Outils d’administration** . Étant donné que les dossiers et fichiers masqués dans les outils d’administration sont toujours affichés dans le menu**Outils** de gestionnaire de serveur, n’utilisez pas le paramètre **masqué** de la boîte de dialogue **Propriétés** d’un fichier ou d’un dossier pour limiter l’accès utilisateur à vos raccourcis d’outils personnalisés.
    > 
    > Les autorisations **Refuser** remplacent toujours les autorisations **Autoriser**.

6.  Cliquez avec le bouton droit sur l’outil, le script ou le fichier exécutable d’origine pour lequel vous souhaitez ajouter des entrées dans le menu **Outils** , puis cliquez sur **créer un raccourci**.

7.  Déplacez le raccourci vers le dossier *mytools* dans outils d’administration.

8.  Actualisez ou redémarrez Gestionnaire de serveur, si nécessaire, pour afficher votre raccourci d’outil personnalisé dans le menu **Outils** .

## <a name="manage-roles-on-role-home-pages"></a><a name=BKMK_roles></a>Gérer les rôles dans les pages d’hébergement des rôles
Après avoir ajouté des serveurs au pool de serveurs Gestionnaire de serveur, et Gestionnaire de serveur collecter des données d’inventaire sur les serveurs de votre pool, Gestionnaire de serveur ajoute des pages au volet de navigation pour les rôles qui sont détectés sur les serveurs gérés. La vignette **Serveurs** dans les pages de rôles répertorie les serveurs gérés qui exécutent le rôle. Par défaut, les vignettes **Événements**, **Best Practices Analyzer**, **Services** et **Performances** affichent des données pour tous les serveurs qui exécutent le rôle ; la sélection de serveurs spécifiques dans la vignette **Serveurs** limite l’étendue des événements, services, compteurs de performance et résultats BPA aux serveurs sélectionnés uniquement. Les outils de gestion sont généralement disponibles dans le menu **Outils** de la console Gestionnaire de serveur, une fois qu’un rôle ou une fonctionnalité a été installé ou détecté sur un serveur géré. Vous pouvez également cliquer avec le bouton droit sur les entrées de serveur dans la vignette **Serveurs** pour un rôle ou groupe, puis lancer l’outil de gestion à utiliser.

Dans Windows Server 2016, les rôles et fonctionnalités suivants ont des outils de gestion qui sont intégrés dans Gestionnaire de serveur console en tant que pages.

-   **Services de fichiers et de stockage** Les pages des services de fichiers et de stockage incluent des commandes et vignettes personnalisées pour la gestion des volumes, partages, disques virtuels iSCSI et pools de stockage. Lorsque vous ouvrez la page d’hébergement du rôle Services de fichiers et de stockage dans Gestionnaire de serveur, un volet rétractant s’ouvre et affiche des pages de gestion personnalisée pour les services de fichiers et de stockage. Pour plus d’informations sur le déploiement et la gestion des services de fichiers et de stockage, voir [Vue d’ensemble des services de fichiers et de stockage](https://go.microsoft.com/fwlink/p/?LinkId=241530).

-   **Services Bureau à distance** Les pages des services Bureau à distance incluent des commandes et vignettes personnalisées pour la gestion des sessions, licences, passerelles et bureaux virtuels. Pour plus d’informations sur le déploiement et la gestion des Services Bureau à distance, consultez [services Bureau à distance (rdS)](https://go.microsoft.com/fwlink/p/?LinkId=241532).

-   **Gestion des adresses IP (IPAM).** La page du rôle IPAM inclut une vignette **Bienvenue** personnalisée contenant des liens vers des tâches de gestion et de configuration IPAM courantes, y compris un Assistant pour l’approvisionnement d’un serveur IPAM. La page d’accueil IPAM inclut également des vignettes pour afficher le réseau géré, le résumé de la configuration et les tâches planifiées.

    Il existe certaines limitations à la gestion IPAM dans Gestionnaire de serveur. À la différence des pages de rôles et de groupes classiques, IPAM n’a aucune vignette **Serveurs**, **Événements**, **Performances**, **Best Practices Analyzer** ni **Services**. Aucun modèle de Best Practices Analyzer n’est disponible pour IPAM ; Best Practices Analyzer analyses sur IPAM ne sont pas prises en charge. Pour accéder aux serveurs dans votre pool de serveurs qui exécutent IPAM, créez un groupe personnalisé de ces serveurs qui exécutent IPAM, puis accédez à la liste de serveurs à partir de la vignette **Serveurs** dans la page du groupe personnalisé. Vous pouvez également accéder aux serveurs IPAM à partir de la vignette **Serveurs** dans la page du groupe **Tous les serveurs**.

    Les miniatures du tableau de bord affichent également des lignes limitées pour IPAM, par rapport aux miniatures pour d’autres rôles et groupes. En cliquant sur les lignes des miniatures IPAM, vous pouvez afficher des événements, données de performance et alertes d’état de la gestion pour les serveurs qui exécutent IPAM. Les services associés à IPAM peuvent être gérés à partir de pages pour les groupes de serveurs qui contiennent des serveurs IPAM, par exemple la page pour le groupe **Tous les serveurs**.

    Pour plus d’informations sur le déploiement et la gestion d’IPAM, voir [gestion des adresses IP (IPAM)](https://go.microsoft.com/fwlink/p/?LinkId=241533).

## <a name="see-also"></a>Voir aussi
[Gestionnaire de serveur](server-manager.md)
[Ajouter des serveurs à gestionnaire de serveur](add-servers-to-server-manager.md)
[créer et gérer des groupes de serveurs](create-and-manage-server-groups.md)
[afficher et configurer les données de performances, d’événements et de service](view-and-configure-performance-event-and-service-data.md)
[les services de fichiers et de stockage](https://go.microsoft.com/fwlink/p/?LinkId=241530)
[services Bureau À distance (rdS)](https://go.microsoft.com/fwlink/p/?LinkId=241532)
la [gestion des adresses IP (IPAM)](https://go.microsoft.com/fwlink/p/?LinkId=241533)



