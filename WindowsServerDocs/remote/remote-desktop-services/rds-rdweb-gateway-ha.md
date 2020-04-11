---
title: Ajouter la haute disponibilité au serveur frontal d’accès web et de passerelle des services Bureau à distance
description: Fournit les étapes de l’installation des serveurs d’accès web et de passerelle Bureau à distance dans un déploiement de services Bureau à distance.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 11/08/2016
manager: dongill
ms.openlocfilehash: e98bbda5460311dd379eab6f5a5bde0ec3845d5c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860282"
---
# <a name="add-high-availability-to-the-rd-web-and-gateway-web-front"></a>Ajouter la haute disponibilité au serveur frontal d’accès web et de passerelle des services Bureau à distance

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016


Vous pouvez déployer une batterie de serveurs d’accès web et de passerelle des services Bureau à distance (Accès Bureau à distance par le web et Passerelle des services Bureau à distance) pour améliorer la disponibilité et la mise à l’échelle d’un déploiement des services Bureau à distance de Windows Server. 

Utilisez les étapes suivantes pour ajouter un serveur de passerelle et d’accès web de Bureau à distance à un déploiement de base des services Bureau à distance existant.  

## <a name="pre-requisites"></a>Prérequis

Configurez un serveur pour jouer le rôle d’accès web et de passerelle supplémentaires des services Bureau à distance ; il peut s’agir d’un serveur physique ou d’une machine virtuelle. Y sont incluses la jonction du serveur au domaine et l’activation de la gestion à distance.

## <a name="step-1-configure-the-new-server-to-be-part-of-the-rds-environment"></a>Étape 1 : Configurer le nouveau serveur comme faisant partie de l’environnement des services Bureau à distance

1. Connectez-vous au serveur RDMS dans le portail Azure à l’aide du client Connexion Bureau à distance.
2. Ajoutez le nouveau serveur de passerelle et d’accès web Bureau à distance au Gestionnaire de serveur :
    1. Lancez le Gestionnaire de serveur, cliquez sur **Gérer > Ajouter des serveurs**.   
    2. Dans la boîte de dialogue Ajouter des serveurs, cliquez sur **Rechercher maintenant**.   
    3. Sélectionnez le serveur de passerelle et d’accès web des services Bureau à distance nouvellement créé (par exemple, Contoso-WebGw2), puis cliquez sur **OK**.
3. Ajouter au déploiement des serveurs de passerelle et d’accès web de Bureau à distance  
    1. Lancez le Gestionnaire de serveur.  
    2. Cliquez sur **Services Bureau à distance > Vue d’ensemble > Serveurs de déploiement > Tâches > Ajouter des serveurs d’accès Bureau à distance par le web**.   
    3. Sélectionnez le serveur nouvellement créé (par exemple, Contoso-WebGw2), puis cliquez sur **Suivant**.  
    4. Dans la page de confirmation, sélectionnez **Redémarrer les ordinateurs distants si nécessaire**, puis cliquez sur **Ajouter**.  
    5. Répétez ces étapes pour ajouter le serveur de passerelle Bureau à distance, mais choisissez **Serveurs de passerelle Bureau à distance** à l’étape b.
4. Réinstallez les certificats des serveurs de passerelle Bureau à distance :
   1. Dans le Gestionnaire de serveur sur le serveur RDMS, cliquez sur **Services Bureau à distance > Vue d’ensemble > Tâches > Modifier les propriétés de déploiement**.  
   2. Développez **Certificats**.  
   3. Faites défiler jusqu’à la table. Pour les services Bureau à distance, cliquez sur **Service de rôle Passerelle > Sélectionner un certificat existant**.  
   4. Cliquez sur **Choisir un autre certificat**, puis accédez à l’emplacement du certificat. Par exemple, \Contoso-CB1\Certificates). Sélectionnez le fichier de certificat pour le serveur de passerelle et d’accès web de Bureau à distance créé lors des prérequis (par exemple, ContosoRdGwCert), puis cliquez sur **Ouvrir**.  
   5. Entrez le mot de passe du certificat, sélectionnez **Autoriser l’ajout du certificat au magasin de certificats Autorités de certification racines de confiance sur les machines de destination**, puis cliquez sur **OK**.  
   6. Cliquez sur **Appliquer**.
      > [!NOTE] 
      > Vous devrez peut-être redémarrer manuellement le service TSGateway exécuté sur chaque serveur de passerelle Bureau à distance, via le Gestionnaire de serveur ou le Gestionnaire des tâches.
   7. Répétez les étapes a à f pour le service de rôle Accès Bureau à distance par le web.

## <a name="step-2-configure-rd-web-and-rd-gateway-properties-on-the-new-server"></a>Étape 2 : Configurer les propriétés de la passerelle et de l’accès web des services Bureau à distance sur le nouveau serveur
1. Configurez le serveur comme faisant partie d’une batterie de serveurs de passerelle des services Bureau à distance :
    1.  Dans le Gestionnaire de serveur, sur le serveur RDMS, cliquez sur **Tous les serveurs**. Cliquez avec le bouton droit sur un des serveurs de passerelle Bureau à distance, puis cliquez sur **Connexion Bureau à distance**.
    2.  Connectez-vous au serveur de passerelle Bureau à distance au moyen d’un compte d’administrateur de domaine.  
    3.  Dans le Gestionnaire de serveur, sur le serveur de passerelle Bureau à distance, cliquez sur **Outils > Services Bureau à distance > Gestionnaire de passerelle Bureau à distance**.  
    4.  Dans le volet de navigation, cliquez sur l’ordinateur local (par exemple, Contoso-WebGw1).  
    5.  Cliquez sur **Ajouter des membres de batterie de serveurs de passerelle Bureau à distance**.  
    6.  Sous l’onglet **Batterie de serveurs**, entrez le nom de chaque serveur de passerelle Bureau à distance, cliquez sur **Ajouter** puis sur **Appliquer**.  
    7.  Répétez les étapes a à f sur chaque serveur de passerelle Bureau à distance, afin que les serveurs se reconnaissent mutuellement en tant que serveurs de passerelle Bureau à distance dans une batterie de serveurs. Ne vous inquiétez pas de la présence d’avertissements, car la propagation des paramètres DNS peut prendre du temps.
2. Configurez le serveur comme faisant partie d’une batterie de serveurs d’accès Bureau à distance par le web. Les étapes ci-dessous configurent à l’identique les clés d’ordinateur de validation et de déchiffrement sur les deux sites RDWeb.
    1.  Dans le Gestionnaire de serveur, sur le serveur RDMS, cliquez sur **Tous les serveurs**. Cliquez avec le bouton droit sur le premier serveur d’accès Bureau à distance par le web (par exemple, Contoso-WebGw1), puis cliquez sur **Connexion Bureau à distance**.  
    2.  Connectez-vous au serveur d’accès Bureau à distance par le web au moyen d’un compte d’administrateur de domaine.  
    3.  Dans le Gestionnaire de serveur, sur le serveur d’accès Bureau à distance par le web, cliquez sur **Outils > Gestionnaire des services Internet (IIS)** .  
    4.  Dans le volet gauche du Gestionnaire des services Internet, développez le **Serveur (par exemple, Contoso-WebGw1) > Sites > Site web par défaut**, puis cliquez sur **RDWeb**.  
    5.  Cliquez avec le bouton droit sur **Clé d’ordinateur**, puis cliquez sur **Ouvrir la fonctionnalité**.
    6.  Dans le volet **Actions** de la page Clé de l’ordinateur, sélectionnez **Générer les clés**, puis cliquez sur **Appliquer**.
    7.  Copiez la clé de validation (vous pouvez cliquer avec le bouton droit sur la clé, puis cliquer sur **Copier**.)
    8.  Dans le Gestionnaire des services Internet, sous **Site web par défaut**, sélectionnez successivement **Feed**, **FeedLogon** et **Pages**.
    9. Pour chacun de ces éléments :
        1.  Cliquez avec le bouton droit sur **Clé d’ordinateur**, puis cliquez sur **Ouvrir la fonctionnalité**.
        2.  Pour la clé de validation, décochez l’option **Générer automatiquement au moment de l’exécution**, puis collez la clé que vous avez copiée à l’étape g.
    10.  Réduisez la fenêtre de connexion du Bureau à distance à ce serveur web de Bureau à distance.  
    11.  Répétez les étapes b à e pour le second serveur d’accès Bureau à distance par le web, en terminant par l’affichage de la fonctionnalité **Clé de l’ordinateur**.
    12. Pour la clé de validation, décochez l’option **Générer automatiquement au moment de l’exécution**, puis collez la clé que vous avez copiée à l’étape g.
    13. Cliquez sur **Appliquer**.
    14. Reproduisez ce processus pour les pages **RDWeb**, **Feed**, **FeedLogon** et **Pages**.
    15. Réduisez la fenêtre de connexion du Bureau à distance au second serveur d’accès Bureau à distance par le web, puis agrandissez la fenêtre de connexion du Bureau à distance au premier serveur d’accès Bureau à distance par le web.  
    16. Répétez les étapes g à n pour copier la clé de déchiffrement.
    17. Lorsque les clés de validation et de déchiffrement sont identiques sur les deux serveurs d’accès Bureau à distance par le web dans les pages **RDWeb**, **Feed**, **FeedLogon** et **Pages**, déconnectez-vous de toutes les fenêtres de connexion Bureau à distance.

## <a name="step-3-configure-load-balancing-for-the-rd-web-and-rd-gateway-servers"></a>Étape 3 : Configurer l’équilibrage de charge pour les serveurs de passerelle et d’accès web des services Bureau à distance

Si vous utilisez l’infrastructure Azure, vous pouvez créer un équilibreur de charge Azure externe, sinon vous pouvez configurer un équilibreur de charge matérielle ou logicielle distinct. L’équilibrage de charge est primordial pour la répartition uniforme du trafic sur des connexions longue durée, depuis des clients Bureau à distance transitant par la passerelle Bureau à distance vers les serveurs sur lesquels les utilisateurs exécuteront leurs charges de travail.

> [!NOTE] 
> Si votre précédent serveur, exécutant la passerelle et l’accès web des services Bureau à distance, a déjà été installé derrière un équilibreur de charge externe, passez directement à l’étape 4, sélectionnez le pool principal existant et ajoutez le nouveau serveur au pool.

1.  Créez un équilibreur de charge Azure :  
    1.  Dans le portail Azure, cliquez sur **Parcourir > Équilibreurs de charge > Ajouter**.  
    2.  Entrez un nom, par exemple **WebGwLB**.  
    3.  Sélectionnez **Public** pour le **Schéma**.
    4.  Sous **Adresse IP publique**, sélectionnez **Choisir une adresse IP publique**, puis sélectionnez une adresse IP publique existante ou créez-en une.
    5.  Sélectionnez l’**Abonnement**, le **Groupe de ressources** et l’**Emplacement** appropriés.
    6.  Cliquez sur **Créer**.  
2. Créez une [sonde](https://azure.microsoft.com/documentation/articles/load-balancer-custom-probe-overview/) pour superviser les serveurs qui sont actifs :  
    1.  Dans le portail Azure, sélectionnez **Parcourir** > **Équilibreurs de charge**, puis choisissez l’équilibreur de charge que vous avez créé à l’étape précédente.
    2.  Sélectionnez **Tous les paramètres** > **Sondes** > **Ajouter**.  
    3.  Entrez un nom, par exemple **HTTPS**, pour la sonde. Sélectionnez **TCP** en guise de **Protocole** et entrez **443** pour le **Port**, puis cliquez sur **OK**.   
3.  Créez les règles d’équilibrage de charge UDP et HTTPS :  
    1.  Dans **Paramètres**, cliquez sur **Règles d’équilibrage de charge**.  
    2.  Sélectionnez **Ajouter** pour la **Règle HTTPS**.  
    3.  Entrez un nom pour la règle, par exemple HTTPS, puis sélectionnez **TCP** pour le **Protocole**. Entrez **443** à la fois pour le **Port** et le **Port principal**, puis cliquez sur **OK**.  
    4.  Dans les **Règles d’équilibrage de charge**, cliquez sur **Ajouter** pour la **Règle UDP**.  
    5.  Entrez un nom pour la règle, par exemple **UDP**, puis sélectionnez **UDP** pour le **Protocole**. Entrez **3391** à la fois pour le **Port** et le **Port principal**, puis cliquez sur **OK**.  
4. Créez le pool principal pour les serveurs de passerelle et d’accès web des services Bureau à distance :
      1. Dans **Paramètres**, cliquez sur **Pools d’adresses principaux > Ajouter**.   
      2. Entrez un nom (par exemple, **WebGwBackendPool**), puis cliquez sur **Ajouter une machine virtuelle**.  
      3. Choisissez un groupe à haute disponibilité (par exemple, WebGwAvSet), puis cliquez sur **OK**.   
      4. Cliquez sur **Choisir les machines virtuelles**, sélectionnez chaque machine virtuelle, puis cliquez sur **Sélectionner > OK > OK**.
