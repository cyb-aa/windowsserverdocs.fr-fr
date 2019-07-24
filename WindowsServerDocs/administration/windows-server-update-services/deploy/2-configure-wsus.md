---
title: 'Étape 2 : configurer WSUS'
description: Rubrique de Windows Server Update Service (WSUS) - configurer WSUS est l’étape 2 dans un processus en quatre étapes pour le déploiement de WSUS
ms.prod: windows-server-threshold
ms.reviewer: na
ms.technology: manage-wsus
ms.topic: article
ms.assetid: d4adc568-1f23-49f3-9a54-12a7bec5f27c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ae151c2a6f0f5ef72b263fbe7f1f0d26933452af
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66810995"
---
# <a name="step-2-configure-wsus"></a>Étape 2 : Configurer WSUS

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Après l’installation du rôle serveur WSUS sur votre serveur, vous devez le configurer correctement. La liste de vérification suivante résume les étapes impliquées dans la réalisation de la configuration initiale de votre serveur WSUS.

|Tâche|Description|
|----|--------|
|[2.1. Configurer des connexions réseau](#21-configure-network-connections)|Configurez le réseau de clusters en utilisant l’Assistant Configuration du réseau.|
|[2.2. Configurer WSUS à l’aide de l’Assistant Configuration WSUS](#22-configure-wsus-by-using-the-wsus-configuration-wizard)|Utilisez l’Assistant Configuration WSUS pour effectuer la configuration WSUS de base.|
|[2.3. Configurer des groupes d’ordinateurs WSUS](#23-configure-wsus-computer-groups)|Créez des groupes d’ordinateurs dans la console d’administration WSUS pour gérer les mises à jour dans votre organisation.|
|[2.4. Configurer les mises à jour du client](#24-configure-client-updates)|Spécifiez comment et quand les mises à jour automatiques sont appliquées aux ordinateurs clients.|
|[2.5. Sécuriser WSUS avec le protocole Secure Sockets Layer](#25-secure-wsus-with-the-secure-sockets-layer-protocol)|Configurez le protocole Secure Sockets Layer (SSL) pour protéger Windows Server Update Services (WSUS).|

## <a name="21-configure-network-connections"></a>2.1. Configurer des connexions réseau
Avant de commencer le processus de configuration, assurez-vous de connaître les réponses aux questions suivantes :

1.  Le pare-feu du serveur est-il configuré pour autoriser les clients à accéder au serveur ?

2.  Cet ordinateur peut-il se connecter au serveur en amont (par exemple le serveur désigné pour télécharger des mises à jour de Microsoft Update) ?

3.  Connaissez-vous le nom du serveur proxy et les informations d’identification de l’utilisateur nécessaires pour s’y connecter, le cas échéant ?

Par défaut, WSUS est configuré pour utiliser Microsoft Update comme emplacement de récupération des mises à jour. Si vous avez un serveur proxy sur le réseau, vous pouvez configurer WSUS pour utiliser le serveur proxy. s’il existe un pare-feu d’entreprise entre WSUS et Internet, vous devrez peut-être configurer le pare-feu pour vous assurer que WSUS peut obtenir les mises à jour.

> [!TIP]
> Bien qu’une connexion Internet soit nécessaire pour télécharger les mises à jour à partir de Microsoft Update, WSUS offre la possibilité d’importer des mises à jour sur des réseaux non connectés à Internet.

Lorsque vous disposez des réponses à ces questions, vous pouvez commencer à configurer les paramètres réseau WSUS suivants :

-   **Mises à jour** Indiquez comment ce serveur doit obtenir les mises à jour (de Microsoft Update ou d’un autre serveur WSUS).

-   **Proxy** si vous avez déterminé que WSUS doit utiliser un serveur proxy pour accéder à Internet, vous devez configurer les paramètres de proxy dans le serveur WSUS.

-   **Pare-feu** si vous avez déterminé que WSUS se trouve derrière un pare-feu d’entreprise, il existe quelques étapes supplémentaires qui doivent être effectuées au niveau du périphérique de périmètre pour autoriser le trafic WSUS.

### <a name="211-connection-from-the-wsus-server-to-the-internet"></a>2.1.1. Connexion Internet à partir du serveur WSUS
Si un pare-feu d’entreprise est placé entre WSUS et Internet, il peut être nécessaire de le configurer afin de garantir que WSUS peut obtenir les mises à jour. Pour obtenir des mises à jour de Microsoft Update, le serveur WSUS utilise le port 443 pour le protocole HTTPS. Bien que la plupart des pare-feu d’entreprise autorisent ce type de trafic, il existe quelques sociétés qui limitent l’accès à Internet à partir des serveurs en raison des stratégies de sécurité de la société. Si votre société limite l’accès, vous devez obtenir l’autorisation de permettre l’accès Internet à partir de WSUS à la liste des URL suivante :

- http://windowsupdate.microsoft.com

- http://*.windowsupdate.microsoft.com

- https://*.windowsupdate.microsoft.com

- http://*.update.microsoft.com

- https://*.update.microsoft.com

- http://*.windowsupdate.com

- http://download.windowsupdate.com

- https://download.microsoft.com

- http://*.download.windowsupdate.com

- http://wustat.windows.com

- http://ntservicepack.microsoft.com

- http://go.microsoft.com

- http://dl.delivery.mp.microsoft.com

- https://dl.delivery.mp.microsoft.com

> [!IMPORTANT]
> Pour un scénario dans lequel WSUS ne parvient pas à obtenir les mises à jour en raison des configurations de pare-feu, consultez [article 885819](https://support.microsoft.com/kb/885819) dans la Base de connaissances Microsoft.

La section suivante décrit comment configurer un pare-feu d’entreprise placé entre WSUS et Internet. Du fait que WSUS initie tout le trafic réseau, il n’est pas nécessaire configurer le pare-feu de Windows sur le serveur WSUS. Bien que la connexion entre Microsoft Update et WSUS impose que les ports 80 et 443 soient ouverts, vous pouvez configurer plusieurs serveurs WSUS pour qu’ils se synchronisent avec un port personnalisé.

### <a name="212-connection-between-wsus-servers"></a>2.1.2. Connexion entre les serveurs WSUS
Les serveurs WSUS en amont et en aval se synchronisent sur le port configuré par l’administrateur WSUS. Par défaut, ces ports sont configurés comme suit :

-   Sur WSUS 3.2 et versions antérieures, port 80 pour HTTP et 443 pour HTTPS

-   Sur WSUS 6.2 et versions ultérieures (au moins Windows Server 2012), le port 8530 pour HTTP et 8531 pour HTTPS sont utilisés.

Le pare-feu sur le serveur WSUS doit être configuré pour autoriser le trafic entrant sur ces ports.

### <a name="213-connection-between-clients-windows-update-agent-and-wsus-servers"></a>2.1.3. Connexion entre les clients (Windows Update Agent) et les serveurs WSUS
Les interfaces et les ports d’écoute sont configurés dans les sites IIS pour WSUS et dans les paramètres de stratégie de groupe utilisés pour configurer les ordinateurs clients. Les ports par défaut sont les mêmes que ceux spécifiés à la section précédente **Connexion entre les serveurs WSUS**et le pare-feu du serveur WSUS doit également être configuré pour autoriser le trafic entrant sur ces ports.

## <a name="configure-the-proxy-server"></a>Configuration du serveur proxy
Si le réseau d’entreprise utilise des serveurs proxy, ces derniers doivent prendre en charge les protocoles HTTP et SSL et utiliser l’authentification de base ou Windows. Ces exigences peuvent être satisfaites en utilisant l’une des configurations suivantes :

1.  Un serveur proxy unique qui prend en charge deux canaux de protocole. Dans ce cas, définissez un canal pour utiliser le protocole HTTP et l’autre canal pour utiliser HTTPS.

    > [!NOTE]
    > Vous pouvez configurer un serveur proxy qui gère les deux protocoles pour WSUS pendant l’installation du logiciel serveur WSUS.

2.  Deux serveurs proxy, chacun d’eux prend en charge un seul protocole. Dans ce cas, un serveur proxy est configuré pour utiliser le protocole HTTP et l’autre serveur proxy est configuré pour utiliser HTTPS.

Pour configurer deux serveurs proxy, chacun d’eux gérant un protocole pour WSUS, procédez comme suit :

#### <a name="to-set-up-wsus-to-use-two-proxy-servers"></a>Pour configurer WSUS pour utiliser deux serveurs proxy

1.  Ouvrez une session sur l’ordinateur qui sera le serveur WSUS à l’aide d’un compte membre du groupe Administrateurs local.

2.  Installez le rôle serveur WSUS. Au cours du déroulement de l’Assistant Configuration WSUS (décrit dans la section suivante), ne spécifiez pas de serveur proxy.

3.  Ouvrez une invite de commandes (Cmd.exe) en tant qu'administrateur. Pour ouvrir une invite de commandes en tant qu’administrateur, accédez à **Démarrer**. Dans **rechercher**, type **invite de commandes**. en haut du menu Démarrer, avec le bouton droit **invite de commandes**, puis cliquez sur **exécuter en tant qu’administrateur**. Si le **contrôle de compte d’utilisateur** boîte de dialogue s’affiche, entrez les informations d’identification appropriées (si nécessaire), vérifiez que l’action affichée est ce que vous souhaitez, puis cliquez sur **continuer**.

4.  Dans la fenêtre d’invite de commandes, accédez au dossier C:\Program Files\Update Services\Tools. Tapez la commande suivante :

    **WSUSUTIL ConfigureSSlproxy [< proxy_server proxy_port >]-activer**, où :

    1.  proxy_server est le nom du serveur proxy qui prend en charge HTTPS.

    2.  proxy_port est le numéro de port du serveur proxy.

5.  Fermez la fenêtre d’invite de commandes.

Pour ajouter le serveur proxy qui utilise le protocole HTTP à la configuration de WSUS, procédez comme suit :

#### <a name="to-add-a-proxy-server-that-uses-the-http-protocol"></a>Pour ajouter un serveur proxy qui utilise le protocole HTTP

1.  Ouvrez la console d’administration WSUS.

2.  Dans le volet gauche, développez le nom du serveur, puis cliquez sur **Options**.

3.  Dans le volet **Options** , cliquez sur **Source de mise à jour et serveur de mise à jour**, puis cliquez sur l’onglet **Serveur proxy** .

4.  Pour modifier la configuration du serveur proxy existante, utilisez les options suivantes :

    ###### <a name="to-change-or-add-a-proxy-server-to-the-wsus-configuration"></a>Pour modifier ou ajouter un serveur proxy à la configuration WSUS

    1.  Activez la case à cocher **Utiliser un serveur proxy lors de la synchronisation**.

    2.  Dans la zone de texte **Nom du serveur proxy**, tapez le nom du serveur proxy.

    3.  Dans la zone de texte **Numéro de port proxy** , tapez le numéro de port du serveur proxy. Le numéro de port par défaut est 80.

    4.  FF le serveur proxy requiert que vous utilisez un compte d’utilisateur spécifique, sélectionnez le **utiliser les informations d’identification de l’utilisateur pour se connecter au serveur proxy** case à cocher. Tapez le nom d’utilisateur requis, le domaine et le mot de passe dans les zones de texte correspondantes.

    5.  Si le serveur proxy prend en charge l’authentification de base, activez la case à cocher **Autoriser l’authentification de base (mot de passe envoyé non codé)** .

    6.  Cliquez sur **OK**.

    ###### <a name="to-remove-a-proxy-server-from-the-wsus-configuration"></a>Pour supprimer un serveur proxy de la configuration WSUS

    1.  Pour supprimer un serveur proxy de la configuration WSUS, désactivez la case à cocher **Utiliser un serveur proxy lors de la synchronisation**.

    2.  Cliquez sur **OK**.

## <a name="22-configure-wsus-by-using-the-wsus-configuration-wizard"></a>2.2. Configurer WSUS en utilisant l’Assistant Configuration de WSUS
Cette procédure part du principe que vous utilisez l’Assistant Configuration de WSUS, qui s’affiche la première fois que vous lancez la console de gestion des services WSUS. Plus loin dans cette rubrique, vous apprendrez à effectuer ces configurations à l’aide de la page **Options** :

#### <a name="to-configure-wsus"></a>Pour configurer WSUS

1.  Dans le volet de navigation du Gestionnaire de serveur, cliquez sur **Tableau de bord**, sur **Outils**, puis sur **Services WSUS (Windows Server Update Services)** .

    > [!NOTE]
    > Si le **terminer l’Installation de WSUS** boîte de dialogue s’affiche, cliquez sur **exécuter**. Dans le **terminer l’Installation de WSUS** boîte de dialogue, cliquez sur **fermer** lorsque l’installation est terminée avec succès.

2.  L’Assistant WSUS (Windows Server Update Services) s’affiche. Dans la page **Avant de commencer** , examinez les informations, puis cliquez sur **Suivant**.

3.  Lisez les instructions de la page **S’inscrire au Programme d’amélioration de Microsoft Update** et déterminez si vous souhaitez participer. Si vous souhaitez participer au programme. Conservez la sélection par défaut, ou désactivez la case à cocher, puis cliquez sur **suivant**.

4.  Sur la page **Choisir le serveur en amont**, il existe deux options :

    1.  Synchroniser les mises à jour avec Microsoft Update

    2.  Synchroniser à partir d’un serveur Windows Server Update Services

        -   Si vous choisissez de synchroniser à partir d’un autre serveur WSUS, vous devez spécifier le nom du serveur et le port sur lequel ce serveur communiquera avec le serveur en amont.

        -   Pour utiliser SSL, activez la case à cocher **Utiliser SSL pour la synchronisation des informations de mise à jour** . Les serveurs utiliseront le port 443 pour la synchronisation. (Vous devez vous assurer que ce serveur et le serveur en amont prennent en charge le protocole SSL).

        -   S’il s’agit d’un serveur réplica, sélectionnez le **il s’agit d’un réplica du serveur en amont** case à cocher.

5.  Après avoir sélectionné les options appropriées pour votre déploiement, cliquez sur **Suivant** pour continuer.

6.  Dans la page **Définir le serveur proxy**, activez la case à cocher **Utiliser un serveur proxy lors de la synchronisation**, puis saisissez le nom du serveur proxy et son numéro de port (80 par défaut) dans les zones correspondantes.

    > [!IMPORTANT]
    > Vous devez effectuer cette étape si vous avez déterminé que WSUS doit utiliser un serveur proxy pour accéder à Internet.

7.  Si vous souhaitez vous connecter au serveur proxy à l’aide des informations d’identification de l’utilisateur spécifique, sélectionnez le **utiliser les informations d’identification de l’utilisateur pour se connecter au serveur proxy** case à cocher, puis tapez le nom d’utilisateur, le domaine et le mot de passe de l’utilisateur dans le correspondantes zones. Si vous souhaitez activer l’authentification de base pour l’utilisateur qui se connecte au serveur proxy, activez la **autoriser l’authentification de base (mot de passe est envoyé en texte clair)** case à cocher.

8.  Cliquez sur **Suivant**. Sur le **se connecter au serveur en amont** , cliquez sur **démarrer connexion**.

9. Une fois la connexion établie, cliquez sur **Suivant** pour continuer.

10. Sur le **choisir les langues** page, que vous avez la possibilité de sélectionner les langues dans lesquelles WSUS recevra les mises à jour - toutes les langues ou un sous-ensemble de langues. sélectionner un sous-ensemble de langues est économiser l’espace disque, mais il est IMPORTANT de choisir toutes les langues qui sont requises par tous les clients de ce serveur WSUS. Si vous choisissez d’obtenir des mises à jour pour quelques langues uniquement, sélectionnez **télécharger les mises à jour dans ces langues uniquement**, puis sélectionnez les langues pour lesquelles vous souhaitez que les mises à jour ; sinon, laissez la sélection par défaut.

    > [!WARNING]
    > Si vous sélectionnez l’option **Télécharger les mises à jour dans ces langues uniquement**et qu’un serveur WSUS en aval est connecté à ce serveur, cette option oblige le serveur en aval à n’utiliser également que les langues sélectionnées.

11. Après avoir sélectionné les options linguistiques appropriées pour votre déploiement, cliquez sur **Suivant** pour continuer.

12. La page **Choisir les produits** vous permet d’indiquer les produits pour lesquels vous souhaitez des mises à jour. Sélectionnez les catégories de produits, tels que Windows, ou des produits spécifiques, tels que Windows Server 2008. sélection d’une catégorie de produit sélectionne tous les produits de cette catégorie.

13. Sélectionnez les options appropriées du produit pour votre déploiement, puis cliquez sur **suivant**.

14. La page **Choisir les classifications** permet de sélectionner les classifications des mises à jour que vous souhaitez obtenir. Choisissez toutes les classifications ou un sous-ensemble d’entre elles, puis cliquez sur **Suivant**.

15. La page **Définir la planification de la synchronisation** permet d’indiquer si vous souhaitez effectuer la synchronisation manuellement ou automatiquement.

    -   Si vous choisissez **synchroniser manuellement**, vous devez démarrer le processus de synchronisation à partir de la Console d’Administration WSUS.

    -   Si vous choisissez **synchroniser automatiquement**, le serveur WSUS effectuera une synchronisation aux intervalles définis.

    Définissez l’heure de la **Première synchronisation**, puis indiquez le nombre de **Synchronisations par jour** que ce serveur doit effectuer. Si, par exemple, vous indiquez quatre synchronisations par jour à compter de 3h00, les synchronisations auront lieu à 3h00, 9h00, 15h00 et 21h00.

16. Après avoir sélectionné les options de synchronisation appropriées pour votre déploiement, cliquez sur **Suivant** pour continuer.

17. Dans la page **Terminé** , vous avez la possibilité de démarrer maintenant la synchronisation en activant la case à cocher **Commencer la synchronisation initiale** . Si vous ne sélectionnez pas cette option, vous devez utiliser la Console d’administration WSUS pour effectuer la synchronisation initiale. Cliquez sur **Suivant** si vous voulez en savoir plus sur d’autres paramètres ou sur **Terminer** pour terminer cet Assistant et la configuration WSUS initiale.

18. Après avoir cliqué sur **Terminer**, la console de gestion des services WSUS s’affiche.

Maintenant que vous avez effectué la configuration WSUS de base, lisez les sections suivantes pour plus d’informations sur la modification des paramètres à l’aide de la console de gestion des services WSUS.

## <a name="23-configure-wsus-computer-groups"></a>2.3. Configurer des groupes d’ordinateurs WSUS
groupes d’ordinateurs constituent une partie importante des déploiements de Windows Server Update Services (WSUS). Ils vous permettent de tester et de cibler des mises à jour sur des ordinateurs spécifiques. Il existe deux groupes d’ordinateurs par défaut : Tous les ordinateurs et ordinateurs non attribués. Par défaut, lorsque chaque ordinateur client contacte pour la première fois le serveur WSUS, ce dernier l’ajoute à ces deux groupes.

Vous pouvez créer autant de groupes d’ordinateurs personnalisés que nécessaire pour gérer les mises à jour dans votre organisation. Il est recommandé de créer au moins un groupe d’ordinateurs pour tester les mises à jour avant de les déployer vers d’autres ordinateurs de votre organisation.

Utilisez la procédure suivante pour créer un groupe et affecter un ordinateur à ce groupe :

#### <a name="to-create-a-computer-group"></a>Pour créer un groupe d’ordinateurs

1.  Dans la Console d’Administration WSUS, sous **les Services de mise à jour**, développez le serveur WSUS, développez **ordinateurs**, avec le bouton droit **tous les ordinateurs**, puis cliquez sur **ajouter l’ordinateur de groupe**.

2.  Dans le **ajouter l’ordinateur de groupe** boîte de dialogue **nom**, spécifiez le nom du nouveau groupe, puis cliquez sur **ajouter**.

3.  Cliquez sur **ordinateurs**, puis sélectionnez les ordinateurs que vous souhaitez affecter à ce nouveau groupe.

4.  Cliquez sur les noms d’ordinateurs que vous avez sélectionné à l’étape précédente, puis cliquez sur **modifier l’appartenance**.

5.  Dans le **définir l’appartenance au groupe d’ordinateur** boîte de dialogue, sélectionnez le test de groupe que vous avez créé, puis cliquez sur **OK**.

## <a name="24-configure-client-updates"></a>2.4. Configurer les mises à jour des clients
Le programme d’installation de WSUS configure automatiquement les services Internet (IIS) pour qu’ils distribuent la version la plus récente de Mises à jour automatiques à chaque ordinateur client qui contacte le serveur WSUS. La meilleure méthode de configuration des Mises à jour automatiques dépend de votre environnement réseau.

-   Dans un environnement qui utilise le service d’annuaire active directory, vous pouvez utiliser un existant basés sur domaine objet (stratégie de groupe) ou créer un nouvel objet GPO.

-   Dans un environnement sans active directory, utilisez l’éditeur de stratégie de groupe locale pour configurer les mises à jour automatiques et pointez les ordinateurs clients sur le serveur WSUS.

> [!IMPORTANT]
> Les procédures suivantes supposent que votre réseau exécute active directory. Elles supposent également que vous connaissez la stratégie de groupe et que vous l’utilisez pour gérer le réseau.

Utilisez les procédures suivantes pour configurer les Mises à jour automatiques pour les ordinateurs clients :

-   [Étape 4 : configurer les paramètres de stratégie de groupe pour les mises à jour automatiques](4-configure-group-policy-settings-for-automatic-updates.md)

-   [2.3. Configurer des groupes d’ordinateurs](#23-configure-wsus-computer-groups) dans cette rubrique

### <a name="configure-automatic-updates-in-group-policy"></a>Configurer les Mises à jour automatiques dans une stratégie de groupe

Si vous avez configuré active directory dans votre réseau, vous pouvez configurer un ou plusieurs ordinateurs simultanément en les incluant dans un objet de stratégie de groupe (GPO), puis en configurant ce GPO avec des paramètres WSUS. Nous vous recommandons de créer un objet de stratégie de groupe qui contient uniquement des paramètres WSUS.

Liez cet objet de stratégie de groupe WSUS à un conteneur active directory qui convient à votre environnement. Dans un environnement simple, vous pouvez lier un seul objet de stratégie de groupe WSUS au domaine. Dans un environnement plus complexe, vous pouvez lier plusieurs objets de stratégie de groupe WSUS à plusieurs unités d’organisation, ce qui vous permet d’appliquer différents paramètres de stratégie WSUS à différents types d’ordinateurs.

##### <a name="to-enable-wsus-through-a-domain-gpo"></a>Pour activer WSUS par le biais d’un objet de stratégie de groupe de domaine

1.  Dans les stratégies de groupe Management Console (GPMC), accédez à l’objet de stratégie de groupe sur lequel vous voulez configurer WSUS, puis cliquez sur **modifier**.

2.  Dans la console GPMC, développez **ordinateur Configuration**, développez **stratégies**, développez **modèles d’administration**, développez **composants Windows**, puis cliquez sur **mise à jour Windows**.

3.  Dans le volet d’informations, double-cliquez sur **Configurer les mises à jour automatiques**. La stratégie **Configuration du service Mises à jour automatiques** s’ouvre.

4.  Cliquez sur **Activé**, puis sélectionnez l’une des options suivantes sous le paramètre **Configuration de la mise à jour automatique** :

    -   **Notification des téléchargements et des installations**. Cette option avertit un administrateur connecté avant de télécharger et d’installer les mises à jour.

    -   **Téléchargement automatique et notification des installations**. Cette option lance automatiquement le téléchargement des mises à jour, puis avertit un administrateur connecté avant d’installer les mises à jour. Cette option est activée par défaut.

    -   **Téléchargement automatique et planification des installations**. Cette option lance automatiquement le téléchargement des mises à jour, puis installe les mises à jour selon le jour et l’heure que vous spécifiez.

    -   **Autoriser l’administrateur local à choisir les paramètres**. Cette option permet aux administrateurs locaux d’utiliser Mises à jour automatiques dans le Panneau de configuration pour sélectionner une option de configuration. Par exemple, ils peuvent choisir une heure d’installation planifiée. Les administrateurs locaux ne sont pas autorisés à désactiver les Mises à jour automatiques.

5.  Sélectionnez **autoriser le ciblage côté client**, sélectionnez **activé**, puis tapez le nom du groupe d’ordinateurs WSUS auquel vous souhaitez ajouter cet ordinateur dans le **nom du groupe cible pour cet ordinateur**  boîte.

    > [!NOTE]
    > L’option**Autoriser le ciblage côté client** permet aux ordinateurs clients de s’ajouter aux groupes d’ordinateurs cibles sur le serveur WSUS lorsque les mises à jour automatiques sont redirigées vers un serveur WSUS. Si l’état est défini sur activé, cet ordinateur s’identifie en tant que membre d’un groupe d’ordinateurs particulier lorsqu’il envoie des informations sur le serveur WSUS, ce qui les utilise pour déterminer quelles mises à jour sont déployées sur cet ordinateur. Ce paramètre indique au serveur WSUS le groupe que l’ordinateur client doit utiliser. Vous devez créer le groupe sur le serveur WSUS et ajouter des ordinateurs membres du domaine à ce groupe.

6.  Cliquez sur **OK** pour fermer la stratégie **Autoriser le ciblage côté client** et revenir au volet de détails Windows Update.

7.  Cliquez sur **OK** pour fermer la stratégie **Configuration du service Mises à jour automatiques** et revenir au volet de détails Windows Update.

8.  Dans le volet d’informations **Windows Update**, double-cliquez sur **Spécifier l’emplacement intranet du service de mise à jour Microsoft**.

9. Cliquez sur **Activé**, puis sur Serveur dans les zones de texte **Configurer le service intranet de mise à jour pour la détection des mises à jour** et **Configurer le serveur intranet de statistiques** . Entrez la même URL que celle du serveur WSUS. Par exemple, tapez *http://servername* dans les deux zones (où *nom_serveur* est le nom du serveur WSUS).

    > [!WARNING]
    > Lorsque vous tapez l’adresse intranet de votre serveur WSUS, veillez à indiquer le port qui sera utilisé. Par défaut, WSUS utilise le port 8530 pour HTTP et le port 8531 pour HTTPS. Par exemple, si vous utilisez HTTP, vous devez taper **http://servername:8530** .

10. Cliquez sur **OK**.

Après avoir configuré un ordinateur client, il prendra plusieurs minutes avant que l’ordinateur s’affiche dans le **ordinateurs** page dans la Console d’Administration WSUS. Pour les ordinateurs clients configurés avec un objet de stratégie de groupe de domaine, environ 20 minutes peuvent être nécessaires à la stratégie de groupe pour qu’elle applique les nouveaux paramètres de stratégie à l’ordinateur client. Par défaut, les mises à jour de la stratégie de groupe en arrière-plan toutes les 90 minutes, avec un décalage aléatoire compris entre 0 et 30 minutes. Si vous souhaitez mettre à jour de stratégie de groupe plus tôt, vous pouvez ouvrir une fenêtre d’invite de commandes sur le client ordinateur et tapez gpupdate /force.

pour les ordinateurs clients qui sont configurés à l’aide de l’éditeur de stratégie de groupe locale, l’objet de stratégie de groupe est appliquée immédiatement, et la mise à jour prend environ 20 minutes. Si vous lancez la détection manuellement, il est inutile d’attendre 20 minutes pour l’ordinateur client contacte WSUS.

Dans la mesure où l’attente du début de la détection peut être longue, vous pouvez utiliser la procédure suivante pour lancer la détection immédiatement.

##### <a name="to-initiate-wsus-detection"></a>Pour lancer la détection par le serveur WSUS

1.  Sur l’ordinateur client, ouvrez une fenêtre d’invite de commandes avec élévation de privilèges.

2.  Tapez wuauclt.exe /detectnow, puis appuyez sur ENTRÉE.

## <a name="25-secure-wsus-with-the-secure-sockets-layer-protocol"></a>2.5. Sécurisation de WSUS avec le protocole SSL (Secure Sockets Layer)

Vous pouvez utiliser le protocole  SSL (Secure Sockets Layer) pour aider à sécuriser le déploiement de WSUS. WSUS utilise SSL pour authentifier les ordinateurs clients et les serveurs WSUS en aval vers le serveur WSUS. WSUS utilise également SSL pour chiffrer les métadonnées de mise à jour.

> [!IMPORTANT]
> Les clients et les serveurs en aval configurés pour utiliser le protocole TLS (Transport Layer Security) ou HTTPS doivent également être configurés pour utiliser un nom de domaine complet (FQDN) pour leur serveur WSUS en amont.

WSUS utilise SSL pour les métadonnées uniquement, pas pour les fichiers de mise à jour. Microsoft Update distribue les mises à jour de la même façon. Microsoft réduit le risque d’envoyer des fichiers de mise à jour sur un canal non chiffré en signant chaque mise à jour. En outre, un hachage est calculé et envoyé avec les métadonnées pour chaque mise à jour. Lorsqu’une mise à jour est téléchargée, WSUS vérifie la signature numérique et le hachage. Si la mise à jour a été modifié, il n’est pas installé.

### <a name="limitations-of-wsus-ssl-deployments"></a>Limitations des déploiements SSL WSUS
Lorsque vous utilisez SSL pour sécuriser un déploiement WSUS, vous devez prendre en compte les limitations suivantes :

1.  L’utilisation de SSL augmente la charge de travail du serveur. Vous devez prévoir une perte de 10 % des performances en raison du coût du chiffrement de toutes les métadonnées envoyées sur le réseau.

2.  Si vous utilisez WSUS avec une base de données SQL Server distante, la connexion entre le serveur WSUS et le serveur de base de données n’est pas sécurisée par SSL. Si la connexion de base de données doit être sécurisée, tenez compte des recommandations suivantes :

-   déplacer la base de données WSUS sur le serveur WSUS.

-   déplacer le serveur de base de données distante et le serveur WSUS à un réseau privé.

-   Déployez le protocole IPsec (Internet Protocol security) pour sécuriser le trafic réseau. Pour plus d’informations sur IPsec, consultez la rubrique [Création et utilisation des stratégies IPsec](https://go.microsoft.com/fwlink/?LinkID=203841).

### <a name="configure-ssl-on-the-wsus-server"></a>Configuration de SSL sur le serveur WSUS
WSUS requiert deux ports SSL : un port qui utilise le protocole HTTPS pour envoyer des métadonnées chiffrées et un port qui utilise le protocole HTTP pour envoyer des mises à jour. Lorsque vous configurez WSUS pour utiliser SSL, procédez comme suit :

-   Vous ne pouvez pas configurer l’ensemble du site web WSUS de manière à ce qu’il exige SSL, car tout le trafic vers le site WSUS devrait alors être chiffré. WSUS chiffre uniquement les métadonnées de mise à jour. Si un ordinateur tente de récupérer des fichiers de mise à jour sur le port HTTPS, le transfert échoue.

    Vous devez exiger SSL pour les racines virtuelles suivantes uniquement :

    -   **SimpleAuthWebService**

    -   **DSSAuthWebService**

    -   **ServerSyncWebService**

    -   **APIremoting30**

    -   **ClientWebService**

    Vous ne devez pas exiger SSL pour les racines virtuelles suivantes :

    -   **Contenu**

    -   **Inventaire**

    -   **ReportingWebService**

    -   **SelfUpdate**

-   Le certificat de l’autorité de certification (AC) doit être importé dans le magasin AC racine de confiance de l’ordinateur local ou dans le magasin AC racine de confiance de Windows Server Update Service sur les serveurs WSUS en aval. Si le certificat est importé uniquement à l’utilisateur Local, autorité de certification racine approuvée, stocker, le serveur WSUS en aval ne sera pas authentifié sur le serveur en amont.

    Pour plus d’informations sur l’utilisation des certificats SSL dans IIS, consultez [nécessitent Secure Sockets Layer (IIS 7)](https://go.microsoft.com/fwlink/?LinkID=203846).

-   Vous devez importer le certificat sur tous les ordinateurs qui communiqueront avec le serveur WSUS. Cela inclut tous les ordinateurs clients, les serveurs en aval et les ordinateurs qui exécutent la console d’administration WSUS. Le certificat doit être importé dans le magasin AC racine de confiance de l’ordinateur local ou dans le magasin AC racine de confiance de Windows Server Update Service.

-   Vous pouvez utiliser n’importe quel port pour SSL. Toutefois, le port que vous définissez pour SSL détermine également le port que WSUS utilise pour envoyer le trafic HTTP non sécurisé. Penchez-vous sur les exemples suivants :

    -   Si vous utilisez le port standard 443 pour le trafic HTTPS, WSUS utilise le port standard 80 pour le trafic HTTP non sécurisé.

    -   Si vous utilisez un port autre que 443 pour le trafic HTTPS, WSUS envoie le trafic HTTP non via le port qui précède numériquement le port pour le protocole HTTPS. Par exemple, si vous utilisez le port 8531 pour HTTPS, WSUS utilise le port 8530 pour HTTP.

-   Vous devez réinitialiser *ClientServicingProxy* si le nom du serveur, la configuration SSL ou le numéro de port sont modifiés.

##### <a name="to-configure-ssl-on-the-wsus-root-server"></a>Pour configurer SSL sur le serveur racine WSUS

1.  Ouvrez une session sur le serveur WSUS à l’aide d’un compte membre du groupe Administrateurs WSUS ou Administrateurs local.

2.  Accédez à **Démarrer**, type **CMD**, avec le bouton droit **invite de commandes**, puis cliquez sur **exécuter en tant qu’administrateur**.

3.  Accédez à la *%ProgramFiles%* **\Update Services\Tools\\* * dossier.

4.  Dans la fenêtre d’invite de commandes, tapez la commande suivante :

    **WSUSUTIL configuressl** *certificateName*

    où :

    *certificateName* est le nom DNS du serveur WSUS.

### <a name="configure-ssl-on-client-computers"></a>Configuration de SSL sur les ordinateurs clients
Lorsque vous configurez SSL sur les ordinateurs clients, vous devez envisager les problèmes suivants :

-   Vous devez inclure une URL pour un port sécurisé sur le serveur WSUS. Étant donné que vous ne pouvez pas exiger SSL sur le serveur, le seul moyen de vous assurer que les ordinateurs clients peuvent utiliser un canal de sécurité est d’utiliser une URL qui spécifie le protocole HTTPS. Si vous utilisez un port autre que 443 pour SSL, vous devez également inclure ce port dans l’URL.

-   Le certificat sur un ordinateur client doit être importé dans le magasin AC racine de confiance de l’ordinateur Local ou d’un magasin d’autorité de certification racine approuvé du Service de mise à jour automatique. Si le certificat est importé dans le magasin AC racine de confiance de l’utilisateur Local uniquement, les mises à jour automatique échoue l’authentification du serveur.

-   Les ordinateurs clients doivent approuver le certificat que vous liez au serveur WSUS. Selon le type de certificat utilisé, vous devrez peut-être configurer un service permettant aux ordinateurs clients d’approuver le certificat lié au serveur WSUS.

### <a name="configure-ssl-for-downstream-wsus-servers"></a>Configuration de SSL pour les serveurs WSUS en aval
Les instructions suivantes configurent un serveur en aval à synchroniser avec un serveur en amont qui utilise SSL.

##### <a name="to-synchronize-a-downstream-server-to-an-upstream-server-that-uses-ssl"></a>Pour synchroniser un serveur en aval avec un serveur en amont qui utilise SSL

1.  Ouvrez une session sur l’ordinateur à l’aide d’un compte d’utilisateur membre du groupe Administrateurs WSUS ou Administrateurs local.

2.  Cliquez sur **Démarrer**, cliquez sur **tous les programmes**, cliquez sur **outils d’administration**, puis cliquez sur **Windows Server Update Service**.

3.  Dans le volet de droite, développez le nom du serveur.

4.  Cliquez sur **Options**, puis sur **Source des mises à jour et serveur proxy**.

5.  Sur la page **Mise à jour de la source** , sélectionnez **Synchroniser à partir d’un autre serveur Windows Server Update Services**.

6.  Tapez le nom du serveur en amont dans le **nom du serveur** zone de texte. Tapez le numéro de port que le serveur utilise pour les connexions SSL dans le **le numéro de Port** zone de texte.

7.  Sélectionnez le **utiliser SSL lors de la synchronisation des informations de mise à jour** case à cocher, puis cliquez sur **OK**.

### <a name="additional-ssl-resources"></a>ressources SSL supplémentaires
Les étapes requises pour configurer une autorité de certification, lier le certificat au site web WSUS et établir une relation d’approbation entre les ordinateurs clients et le certificat n’entrent pas dans le cadre de ce guide. Pour plus d’informations et pour obtenir des instructions sur la façon d’installer des certificats et de configurer cet environnement, consultez les rubriques suivantes :

-   [Guide pas à pas de PKI Suite B](https://go.microsoft.com/fwlink/?LinkID=203858)

-   [Implémentation et administration des modèles de certificats](https://go.microsoft.com/fwlink/?LinkID=203859)

-   [Guide de Migration et d’Active directory mise à niveau des Services de certificat](https://go.microsoft.com/fwlink/?LinkID=203860)

-   [Configurer l’inscription automatique de certificat](https://go.microsoft.com/fwlink/?LinkID=203861)

### <a name="26-complete-iis-configuration"></a>2.6. Achèvement de la configuration d’IIS
Par défaut, l’accès en lecture anonyme est activé pour la valeur par défaut et tous les nouveaux sites web IIS. Certaines applications, notamment Windows SharePoint Services, peuvent supprimer l’accès anonyme. Si cela se produit, vous devez réactiver l’accès en lecture anonyme avant de pouvoir installer et utiliser WSUS.

Pour activer l’accès en lecture anonyme, suivez les étapes de la version applicable d’IIS :

1.  [Activation de l’authentification anonyme (IIS 7)](https://go.microsoft.com/fwlink/?LinkID=205316), comme indiqué dans le Guide des opérations IIS 7.

2.  [Activation de l’authentification anonyme (IIS 6.0)](https://go.microsoft.com/fwlink/?LinkId=211391), comme indiqué dans le Guide des opérations IIS 6.0.

### <a name="27-configure-a-signing-certificate"></a>2.7. Configuration d’un certificat de signature
WSUS a la possibilité de publier des packages de mise à jour personnalisés permettant de mettre à jour des produits Microsoft et non-Microsoft. WSUS peut signer automatiquement ces packages de mises à jour personnalisés pour vous avec un certificat Authenticode. Pour activer la signature personnalisée des mises à jour, vous devez installer un certificat de signature de packages sur votre serveur WSUS. Il existe plusieurs règles associées à la signature personnalisée des mises à jour.

1.  **Distribution des certificats**. La clé privée doit être installée sur le serveur WSUS et la clé publique doit être installée de manière explicite dans le magasin de certificats approuvé sur tous les ordinateurs et serveurs clients qui doivent recevoir des mises à jour signées personnalisées.

2.  **Expiration**. Lorsque le certificat auto-signé expire ou s’approche de l’expiration, WSUS enregistre les événements dans le journal des événements.

3.  **Mises à jour/révocation de certificats**. Si vous souhaitez mettre à jour ou de révoquer un certificat (après avoir découvert qu’il avait expiré), WSUS ne proposait aucune fonctionnalité pour activer cette option. Il s’agissait d’une tâche très difficile à effectuer manuellement ou à automatiser avec succès.


