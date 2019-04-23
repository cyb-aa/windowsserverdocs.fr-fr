---
title: Gérer le serveur local et la console du Gestionnaire de serveur
description: Gestionnaire de serveur
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eeb32f65-d588-4ed5-82ba-1ca37f517139
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1f22578cc54a22464fe5d9208731fe681be30481
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832980"
---
# <a name="manage-the-local-server-and-the-server-manager-console"></a>Gérer le serveur local et la console du Gestionnaire de serveur

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dans Windows Server, le Gestionnaire de serveur vous permet de gérer le serveur local (si vous exécutez le Gestionnaire de serveur sur Windows Server et non sur un système d’exploitation de clients basés sur Windows) et les serveurs distants qui exécutent Windows Server 2008 et les versions plus récentes de la Windows Système d’exploitation de serveur.

Le **serveur Local** page dans le Gestionnaire de serveur affiche les données de compteur propriétés, événements, service et les performances de serveur et les résultats de Best Practices Analyzer (BPA) pour le serveur local. Les vignettes Événements, Services, BPA et Performances fonctionnent comme dans les pages de rôles et de groupes de serveurs. Pour plus d’informations sur la configuration des données affichées dans ces vignettes, voir [View et Configure Performance, Event, et Service Data et [Run Best Practices Analyzer Scans et Manage Scan Results.

Paramètres et commandes de menu dans les barres d’en-tête de console Gestionnaire de serveur s’appliquent globalement à tous les serveurs dans votre pool de serveurs et vous permettent d’utiliser le Gestionnaire de serveur pour gérer le pool de serveurs entier.

Cette rubrique contient les sections suivantes.

-   [Arrêter le serveur local](#BKMK_shutdown)

-   [Configurer les propriétés du Gestionnaire de serveur](#BKMK_props)

-   [Gérer la console du Gestionnaire de serveur](#BKMK_managesm)

-   [Personnaliser les outils qui sont affichés dans le menu Outils](#BKMK_tools)

-   [Gérer les rôles sur les pages d’accueil de rôle](#BKMK_roles)

## <a name="BKMK_shutdown"></a>Arrêter le serveur local
Le **tâches** menu sur le serveur local **propriétés** vignette vous permet de démarrer une session Windows PowerShell sur le serveur local, ouvrez le **ordinateur gestion** mmc snap-in ou d’ouvrir composants logiciel enfichables MMC pour les rôles ou fonctionnalités qui sont installées sur le serveur local. Vous pouvez également arrêter le serveur local en utilisant la commande **Arrêter le serveur local** dans ce menu **Tâches** . La commande **Arrêter le serveur local** est également disponible pour le serveur local dans la vignette **Serveurs** de la page **Tous les serveurs** , ou dans toute page de groupe ou de rôle dans laquelle le serveur local est représenté.

Arrêter le serveur local à l’aide de cette méthode, contrairement à l’arrêt de Windows Server 2016 à partir de la **Démarrer** écran s’ouvre le **arrêté bas Windows** boîte de dialogue qui vous permet de spécifier les raisons d’arrêt dans le **shutdown Event Tracker** zone.

> [!NOTE]
> Seuls les membres du groupe Administrateurs peuvent arrêter ou redémarrer un serveur. Les utilisateurs standard ne peuvent pas arrêter ni redémarrer un serveur. Un clic sur la commande **Arrêter le serveur local** entraîne la fermeture des sessions serveur des utilisateurs standard. Le résultat est le même que lorsqu’un utilisateur standard exécute la commande d’arrêt **Alt+F4** à partir du Bureau du serveur.

## <a name="BKMK_props"></a>Configurer les propriétés du Gestionnaire de serveur
Vous pouvez afficher ou modifier les paramètres suivants dans la vignette **Propriétés** de la page **Serveur local**. Pour modifier une valeur de paramètre, cliquez sur la valeur hypertexte du paramètre.

> [!NOTE]
> En règle générale, les propriétés affichées dans la vignette **Propriétés** de la page Serveur local ne peuvent être modifiées que sur le serveur local. Vous ne pouvez pas modifier les propriétés du serveur local à partir d’un ordinateur distant en utilisant le Gestionnaire de serveur, car le **propriétés** mosaïque peut uniquement obtenir des informations sur l’ordinateur local, pas à distance des ordinateurs.
> 
> Parce que de nombreuses propriétés affichent dans le **propriétés** vignette sont contrôlées par des outils qui ne font pas partie du Gestionnaire de serveur (le panneau de configuration, par exemple), devient **propriétés** paramètres ne sont pas toujours affiché dans le **propriétés** vignette immédiatement. Par défaut, les données de la vignette **Propriétés** sont actualisées toutes les deux minutes. Pour actualiser **propriétés** données de la vignette immédiatement, cliquez sur **Actualiser** dans la barre d’adresses de gestionnaire de serveur.

|Paramètre|Description|
|------|--------|
|Nom de l’ordinateur|Affiche le nom d’ordinateur convivial et ouvre le **propriétés système** boîte de dialogue qui vous permet de modifier le nom du serveur, l’appartenance au domaine et autres paramètres système tels que les profils d’utilisateur.|
|Domaine (ou Groupe de travail, si le serveur n’appartient pas à un domaine)|Affiche le domaine ou groupe de travail duquel le serveur est un membre. Ouvre le **propriétés système** boîte de dialogue qui vous permet de modifier le nom du serveur, l’appartenance au domaine et autres paramètres système tels que les profils d’utilisateur.|
|Pare-feu Windows|Affiche l’état du Pare-feu Windows pour le serveur local. Ouvre **Panneau de configuration\Système et sécurité\Pare-feu Windows**. Pour plus d’informations sur la configuration du Pare-feu Windows, voir [Pare-feu Windows avec fonctions avancées de sécurité et IPsec](https://go.microsoft.com/fwlink/?LinkId=253465).|
|Gestion à distance|Affiche l’état de gestion à distance de Server Manager et Windows PowerShell. Ouvre le **configurer la gestion à distance** boîte de dialogue. Pour plus d’informations sur la gestion à distance, consultez [configurer la gestion à distance dans le Gestionnaire de serveur](configure-remote-management-in-server-manager.md).|
|Bureau à distance|Indique si les utilisateurs peuvent se connecter au serveur à distance à l’aide de sessions Bureau à distance. Ouvre le **distant** onglet de la **propriétés système** boîte de dialogue.|
|Association de cartes réseau|Indique si le serveur local participe à l’association de cartes réseau. Ouvre la boîte de dialogue **Association de cartes réseau** et vous permet de joindre le serveur local à une association de cartes réseau si vous le souhaitez. Pour plus d’informations sur l’association de cartes réseau, voir le [livre blanc relatif à l’association de cartes réseau](https://go.microsoft.com/fwlink/?LinkID=253449).|
|Ethernet|Affiche l’état du réseau du serveur. Ouvre **Panneau de configuration\Réseau et Internet\Connexions réseau**.|
|Version du système d'exploitation|Ce champ en lecture seule affiche le numéro de version du système d’exploitation Windows exécuté par le serveur local.|
|Informations sur le matériel|Ce champ en lecture seule affiche le fabricant ainsi que le nom et le numéro de modèle du matériel de serveur.|
|Dernières mises à jour installées|Affiche le jour et l’heure de la dernière installation des mises à jour de Windows. Ouvre **Panneau de configuration\Système et sécurité\Windows Update**.|
|Windows Update|Affiche les paramètres de Windows Update pour le serveur local. Ouvre **Panneau de configuration\Système et sécurité\Windows Update**.|
|Dernière recherche de mises à jour|Affiche le jour et l’heure de la dernière recherche de mises à jour de Windows disponibles par le serveur. Ouvre **Panneau de configuration\Système et sécurité\Windows Update**.|
|Rapport d’erreurs Windows|Affiche l’état de l’abonnement concernant le rapport d’erreurs Windows. Ouvre la boîte de dialogue **Configuration du rapport d’erreurs Windows**. Pour plus d’informations sur le rapport d’erreurs Windows, ses avantages, les déclarations de confidentialité et les paramètres d’abonnement, voir [Rapport d’erreurs Windows](https://go.microsoft.com/fwlink/?LinkID=245991).|
|Programme d’amélioration du produit|Affiche l’état de l’abonnement concernant le Programme d’amélioration de l’expérience utilisateur Windows. Ouvre la boîte de dialogue **Configuration du Programme d’amélioration de l’expérience utilisateur** . Pour plus d’informations sur le Programme d’amélioration de l’expérience utilisateur Windows, ses avantages et paramètres d’abonnement, voir [Programme d’amélioration de l’expérience utilisateur Windows](https://go.microsoft.com/fwlink/?LinkID=245992).|
|Configuration de sécurité renforcée d’Internet Explorer|Indique si la Configuration de sécurité renforcée d’Internet Explorer (également connue sous le nom de renforcement de IE ou IE ESC) est activée ou désactivée. Ouvre la boîte de dialogue **Configuration de sécurité renforcée d’Internet Explorer**. La Configuration de sécurité renforcée d’Internet Explorer est une mesure de sécurité pour les serveurs qui empêche l’ouverture des pages Web dans Internet Explorer. Pour plus d’informations sur la Configuration de sécurité renforcée d’Internet Explorer, ses avantages et les paramètres, consultez [Internet Explorer : Configuration de sécurité renforcée](https://go.microsoft.com/fwlink/?LinkId=253461).|
|Fuseau horaire|Affiche le fuseau horaire du serveur local. Ouvre le **date et heure** boîte de dialogue.|
|ID de produit|Affiche le numéro d’identification Windows d’activation état et de produit (si Windows a été activé) du système d’exploitation Windows Server 2016. Il ne s’agit pas du même numéro que la clé de produit Windows. Ouvre la boîte de dialogue **Activation de Windows** .|
|Processeurs|Ce champ en lecture seule affiche le fabricant, nom du modèle et vitesse plus d’informations sur les processeurs du serveur local.|
|Mémoire installée (RAM)|Ce champ en lecture seule affiche la quantité de mémoire RAM disponible, en gigaoctets.|
|Espace disque total|Ce champ en lecture seule affiche la quantité d’espace disque disponible, en gigaoctets.|

## <a name="BKMK_managesm"></a>Gérer la console du Gestionnaire de serveur
Paramètres globaux qui s’appliquent à l’intégralité de la console Gestionnaire de serveur et à tous les serveurs distants qui ont été ajoutés au pool de serveurs du Gestionnaire de serveur, sont trouvent dans les barres d’en-tête en haut de la fenêtre de console du Gestionnaire de serveur.

### <a name="add-servers-to-server-manager"></a>ajouter des serveurs au Gestionnaire de serveur
La commande qui ouvre le **ajouter des serveurs** boîte de dialogue et vous permet d’ajouter des serveurs physiques ou virtuels distants au pool de serveurs de gestionnaire de serveur, est dans le **gérer** menu de la console du Gestionnaire de serveur. Pour plus d’informations sur l’ajout de serveurs, consultez [ajouter des serveurs au Gestionnaire de serveur](add-servers-to-server-manager.md).

### <a name="refresh-data-that-is-displayed-in-server-manager"></a>Actualiser les données qui sont affichées dans le Gestionnaire de serveur
Vous pouvez configurer l’intervalle d’actualisation des données qui s’affiche dans le Gestionnaire de serveur sur le **propriétés du Gestionnaire de serveur** boîte de dialogue, que vous pouvez ouvrir à partir de la **gérer** menu.

##### <a name="to-configure-the-refresh-interval-in-server-manager"></a>Pour configurer l’intervalle d’actualisation dans le Gestionnaire de serveur

1.  Sur le **gérer** menu dans la console Gestionnaire de serveur, cliquez sur **propriétés du Gestionnaire de serveur**.

2.  Dans le **propriétés du Gestionnaire de serveur** boîte de dialogue, spécifiez une période de temps, en minutes, correspondant à la durée s’écouler entre les actualisations des données qui s’affiche dans le Gestionnaire de serveur. La valeur par défaut est de 10 minutes. Cliquez sur OK quand vous avez terminé.

#### <a name="refresh-limitations"></a>Limites de l’actualisation
Actualisation s’applique globalement aux données à partir de tous les serveurs que vous avez ajoutés au pool de serveurs de gestionnaire de serveur. Vous ne pouvez pas actualiser des données ni configurer différents intervalles d’actualisation pour des serveurs, rôles ou groupes individuels.

Lorsque les serveurs qui se trouvent dans un cluster sont ajoutés au Gestionnaire de serveur, qu’ils soient des ordinateurs physiques ou virtuels, la première actualisation de données peut échouer ou afficher des données uniquement pour le serveur hôte des objets en cluster. Les actualisations suivantes affichent des données précises pour les serveurs physiques ou virtuels dans un cluster de serveurs.

Données affichées dans les pages d’accueil de rôle dans le Gestionnaire de serveur des Services Bureau à distance, de l’adresse IP de gestion et de fichiers et de Services de stockage ne sont pas actualisée automatiquement. Actualiser les données affichées dans ces pages manuellement, en appuyant sur **F5** ou en cliquant sur **Actualiser** dans le Gestionnaire de serveur en-tête de la console lorsque vous vous trouvez sur ces pages.

### <a name="add-or-remove-roles-or-features"></a>Ajouter ou supprimer des rôles ou des fonctionnalités
Les commandes qui ouvrent les fonctionnalités Assistant Ajouter des rôles et de supprimer des rôles et fonctionnalités Assistant et vous permettent d’ajoutez ou supprimez des rôles, services de rôle et fonctionnalités aux serveurs dans votre pool de serveurs, se trouvent dans le **gérer** menu du Gestionnaire de serveur console et le **tâches** menu de la **des rôles et fonctionnalités** vignette sur les pages de rôle ou groupe. Pour plus d’informations sur l’ajout ou la suppression de rôles ou de fonctionnalités, voir [Installer ou désinstaller des rôles, des services de rôle ou des fonctionnalités](install-or-uninstall-roles-role-services-or-features.md).

Dans le Gestionnaire de serveur, les données de rôle et fonctionnalité s’affiche dans la langue de base du système, également appelée langue de l’interface graphique utilisateur par défaut du système ou de la langue sélectionnée pendant l’installation du système d’exploitation.

### <a name="create-server-groups"></a>créer des groupes de serveurs
La commande qui ouvre le **créer le groupe de serveurs** boîte de dialogue et vous permet de créer des groupes de serveurs personnalisés, est dans le **gérer** menu de la console du Gestionnaire de serveur. Pour plus d’informations sur la création de groupes de serveurs, consultez [créer et gérer des groupes de serveurs](create-and-manage-server-groups.md).

### <a name="prevent-server-manager-from-opening-automatically-at-logon"></a>Empêcher l’ouverture automatique du Gestionnaire de serveur lors de l’ouverture de session
Le **ne pas démarrer le Gestionnaire de serveur automatiquement à l’ouverture de session** case à cocher dans la **propriétés du Gestionnaire de serveur** boîte de dialogue contrôle si le Gestionnaire de serveur s’ouvre automatiquement à l’ouverture de session pour les membres de la Groupe Administrateurs sur un serveur local. Ce paramètre n’affecte pas le comportement du Gestionnaire de serveur lorsqu’il s’exécute sur Windows 10 dans le cadre des outils d’Administration de serveur distant. Pour plus d’informations sur la configuration de ce paramètre, consultez [le Gestionnaire de serveur](server-manager.md).

### <a name="zoom-in-or-out"></a>Zoom avant ou arrière
Pour effectuer un zoom avant ou arrière de l’affichage de la console du Gestionnaire de serveur, vous pouvez utiliser la **Zoom** commandes sur le **vue** menu, ou appuyez sur **Ctrl + signe Plus (+)** pour effectuer un zoom et **Ctrl + signe moins (-)** effectuer un zoom arrière.

## <a name="BKMK_tools"></a>Personnaliser les outils qui sont affichés dans le menu Outils
Le **outils** menu dans le Gestionnaire de serveur inclut des liens virtuels vers des raccourcis dans le **outils d’administration** dossier **Panneau de configuration/système et sécurité**. Le **outils d’administration** dossier contient une liste de raccourcis ou fichiers LNK vers des outils de gestion disponibles, telles que les composants logiciels enfichables mmc. Remplit le Gestionnaire de serveur le **outils** menu avec des liens vers ces raccourcis et copie la structure de dossiers de la **outils d’administration** dossier pour le **outils** menu. Par défaut, les outils du dossier Outils d’administration sont classés dans une liste plate et triés par type et par nom. Dans le Gestionnaire de serveur**outils** menu, les éléments sont triés uniquement par nom, et non par type.

Pour personnaliser le menu **Outils** , copiez les raccourcis d’outils ou de scripts à utiliser dans le dossier **Outils d’administration** . Vous pouvez également organiser vos raccourcis dans des dossiers, qui créent des menus en cascade dans le menu **Outils**. en outre, si vous souhaitez limiter l’accès aux outils personnalisés dans le **outils** menu, vous pouvez définir les droits d’accès utilisateur sur les deux dossiers votre outil personnalisé dans Outils d’administration, ou directement sur les fichiers de script ou l’outil d’origine.

Nous vous déconseillons de réorganiser les outils d’administration et système, et tous les outils de gestion associés aux fonctionnalités et rôles installés sur le serveur local. Le déplacement des outils de gestion des fonctionnalités et rôles peut empêcher la réussite de la désinstallation de ces outils de gestion lorsque cela est nécessaire. Après la désinstallation d’un rôle ou d’une fonctionnalité, un lien non fonctionnel vers un outil dont le raccourci a été déplacé peut demeurer dans le menu **Outils**. Si vous réinstallez le rôle, un lien dupliqué vers le même outil est créé dans le menu **Outils** , mais l’un des liens ne fonctionnera pas.

Les outils des rôles et des fonctionnalités qui sont installés dans le cadre des Outils d’administration de serveur distant sur un ordinateur basé sur le client Windows peuvent toutefois être organisés en dossiers personnalisés. La désinstallation de la fonctionnalité ou du rôle parent n’a aucun effet sur les raccourcis d’outils qui sont disponibles sur un ordinateur distant qui exécute Windows 10.

La procédure suivante décrit comment créer un exemple de dossier nommé *MyTools*, et déplacer les raccourcis pour les deux scripts Windows PowerShell dans le dossier qui sont alors accessibles à partir du menu Outils du Gestionnaire de serveur.

#### <a name="to-customize-the-tools-menu-by-adding-shortcuts-in-administrative-tools"></a>Pour personnaliser le menu Outils en ajoutant des raccourcis dans Outils d’administration

1.  Créez un dossier appelé *MyTools* dans un emplacement pratique.

    > [!NOTE]
    > En raison des droits d’accès limités sur le dossier **Outils d’administration**, vous n’êtes pas autorisé à créer un dossier directement dans le dossier **Outils d’administration** ; vous devez le créer ailleurs (par exemple, sur le Bureau), puis le copier dans le dossier **Outils d’administration**.

2.  déplacer ou copier *MyTools* à **Panneau de configuration/système et sécurité/outils d’administration**. Par défaut, vous devez être membre du groupe Administrateurs sur l’ordinateur pour apporter des modifications au dossier **Outils d’administration**.

3.  Si vous n’avez pas besoin restreindre les droits d’accès utilisateur à vos raccourcis d’outils personnalisés, passez à l’étape 6. Sinon, cliquez avec le bouton droit sur le fichier d’outils (ou le dossier *MyTools*), puis cliquez sur **Propriétés**.

4.  Sur le **sécurité** onglet du fichier **propriétés** boîte de dialogue, cliquez sur **modifier**.

5.  pour les utilisateurs pour lesquels vous souhaitez restreindre l’accès aux outils, désactivez les cases à cocher **lecture & exécution**, **en lecture**, et **écrire** autorisations. Ces autorisations sont héritées par le raccourci d’outils dans le dossier **Outils d’administration** .

    Si vous modifiez les droits d’accès pour un utilisateur alors que l’utilisateur utilise le Gestionnaire de serveur (ou lorsque le Gestionnaire de serveur est ouvrez), vos modifications ne sont pas affichées dans le **outils** menu jusqu'à ce que l’utilisateur redémarre le Gestionnaire de serveur.

    > [!NOTE]
    > Si vous limitez l’accès à un dossier entier que vous avez copié dans Outils d’administration, les utilisateurs avec accès restreint peuvent voir ni le dossier ni son contenu dans le Gestionnaire de serveur**outils** menu.
    > 
    > modifier les autorisations pour le dossier dans le **outils d’administration** dossier. Étant donné que les dossiers et fichiers masqués dans Outils d’administration sont toujours affichés dans le Gestionnaire de serveur**outils** menu, n’utilisez pas le **Hidden** définition sur un dossier ou fichier **propriétés** boîte de dialogue pour restreindre l’accès à vos raccourcis d’outils personnalisés.
    > 
    > Les autorisations **Refuser** remplacent toujours les autorisations **Autoriser**.

6.  Avec le bouton droit de l’outil d’origine, un script ou un fichier exécutable pour lequel vous souhaitez ajouter des entrées dans le **outils** menu, puis sur **créer raccourci**.

7.  Déplacez le raccourci vers le *MyTools* dossier Outils d’administration.

8.  Actualisez ou redémarrez le Gestionnaire de serveur, si nécessaire, pour afficher votre raccourci d’outils personnalisé dans le **outils** menu.

## <a name="BKMK_roles"></a>Gérer les rôles sur les pages d’accueil de rôle
Une fois que vous ajoutez des serveurs au pool de serveurs de gestionnaire de serveur et Server Manager collecte les données d’inventaire sur les serveurs dans votre pool, le Gestionnaire de serveur ajoute des pages au volet de navigation pour les rôles qui sont détectées sur les serveurs gérés. La vignette **Serveurs** dans les pages de rôles répertorie les serveurs gérés qui exécutent le rôle. Par défaut, les vignettes **Événements**, **Best Practices Analyzer**, **Services**et **Performances** affichent des données pour tous les serveurs qui exécutent le rôle ; la sélection de serveurs spécifiques dans la vignette **Serveurs** limite l’étendue des événements, services, compteurs de performance et résultats BPA aux serveurs sélectionnés uniquement. Outils de gestion sont généralement disponibles dans la console du Gestionnaire de serveur **outils** menu, une fois un rôle ou une fonctionnalité a été installée ou détectée sur un serveur géré. Vous pouvez également cliquer avec le bouton droit sur les entrées de serveur dans la vignette **Serveurs** pour un rôle ou groupe, puis lancer l’outil de gestion à utiliser.

Dans Windows Server 2016, les fonctionnalités et rôles suivants disposent d’outils de gestion qui sont intégrées dans la console Gestionnaire de serveur en tant que pages.

-   **Services de fichiers et de stockage** Les pages des services de fichiers et de stockage incluent des commandes et vignettes personnalisées pour la gestion des volumes, partages, disques virtuels iSCSI et pools de stockage. Lorsque vous ouvrez le fichier et page d’accueil de rôle Services de stockage dans le Gestionnaire de serveur, un volet rétractable s’ouvre et affiche les pages d’administration personnalisé pour les Services de fichiers et stockage. Pour plus d’informations sur le déploiement et la gestion des services de fichiers et de stockage, voir [Vue d’ensemble des services de fichiers et de stockage](https://go.microsoft.com/fwlink/p/?LinkId=241530).

-   **Services Bureau à distance** Les pages des services Bureau à distance incluent des commandes et vignettes personnalisées pour la gestion des sessions, licences, passerelles et bureaux virtuels. Pour plus d’informations sur le déploiement et la gestion des Services Bureau à distance, consultez [des Services Bureau à distance (rdS)](https://go.microsoft.com/fwlink/p/?LinkId=241532).

-   **Adresse IP de gestion (IPAM).** La page du rôle IPAM inclut une vignette **Bienvenue** personnalisée contenant des liens vers des tâches de gestion et de configuration IPAM courantes, y compris un Assistant pour l’approvisionnement d’un serveur IPAM. La page d’accueil IPAM inclut également des vignettes pour afficher le réseau géré, le résumé de la configuration et les tâches planifiées.

    Il existe certaines limitations à la gestion d’IPAM dans le Gestionnaire de serveur. À la différence des pages de rôles et de groupes classiques, IPAM n’a aucune vignette **Serveurs**, **Événements**, **Performances**, **Best Practices Analyzer**ni **Services** . Aucun modèle Best Practices Analyzer n’est disponible pour IPAM ; Best Practices Analyzer analyses sur IPAM ne sont pas pris en charge. Pour accéder aux serveurs dans votre pool de serveurs qui exécutent IPAM, créez un groupe personnalisé de ces serveurs qui exécutent IPAM, puis accédez à la liste de serveurs à partir de la vignette **Serveurs** dans la page du groupe personnalisé. Vous pouvez également accéder aux serveurs IPAM à partir de la vignette **Serveurs** dans la page du groupe **Tous les serveurs**.

    Les miniatures du tableau de bord affichent également des lignes limitées pour IPAM, par rapport aux miniatures pour d’autres rôles et groupes. En cliquant sur les lignes des miniatures IPAM, vous pouvez afficher des événements, données de performance et alertes d’état de la gestion pour les serveurs qui exécutent IPAM. Les services associés à IPAM peuvent être gérés à partir de pages pour les groupes de serveurs qui contiennent des serveurs IPAM, par exemple la page pour le groupe **Tous les serveurs**.

    Pour plus d’informations sur le déploiement et la gestion d’IPAM, consultez [l’adresse IP (IPAM) de gestion](https://go.microsoft.com/fwlink/p/?LinkId=241533).

## <a name="see-also"></a>Voir aussi
[Le Gestionnaire de serveur](server-manager.md)
[ajouter des serveurs au Gestionnaire de serveur](add-servers-to-server-manager.md)
[créer et gérer des groupes de serveurs](create-and-manage-server-groups.md)
[afficher et configurer Performances, événements, données et services](view-and-configure-performance-event-and-service-data.md)
[File and Storage Services](https://go.microsoft.com/fwlink/p/?LinkId=241530)
[des Services Bureau à distance (rdS)](https://go.microsoft.com/fwlink/p/?LinkId=241532) 
 [ Adresse IP (IPAM) de gestion](https://go.microsoft.com/fwlink/p/?LinkId=241533)



