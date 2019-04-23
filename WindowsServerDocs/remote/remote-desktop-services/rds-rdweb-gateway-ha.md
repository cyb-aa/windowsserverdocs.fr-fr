---
title: Ajouter une haute disponibilité pour le serveur web frontal Web de bureau à distance et passerelle
description: Fournit les étapes pour l’installation des serveurs Web de bureau à distance et passerelle dans un déploiement services Bureau à distance.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 11/08/2016
manager: dongill
ms.openlocfilehash: fa09532e0b327b24ebb1c0e155c26c25d1043b63
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854600"
---
# <a name="add-high-availability-to-the-rd-web-and-gateway-web-front"></a>Ajouter une haute disponibilité pour le serveur web frontal Web de bureau à distance et passerelle

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016


Vous pouvez déployer une batterie de serveurs de passerelle Bureau à distance (passerelle RD) et de l’accès Web aux services Bureau à distance (RD Web Access) pour améliorer la disponibilité et la mise à l’échelle d’un déploiement Services Bureau à distance (RDS) de Windows Server 

Utilisez les étapes suivantes pour ajouter un serveur de passerelle et Web de bureau à distance à un déploiement de base des Services Bureau à distance existant.  

## <a name="pre-requisites"></a>Conditions préalables

Configurer un serveur d’agir en tant que supplémentaires Web Bureau à distance et passerelle Bureau à distance - cela peut être un serveur physique ou une machine virtuelle. Cela inclut joindre le serveur au domaine et l’activation de la gestion à distance.

## <a name="step-1-configure-the-new-server-to-be-part-of-the-rds-environment"></a>Étape 1 : Configurer le nouveau serveur comme faisant partie de l’environnement de services Bureau à distance

1. Se connecter au serveur RDM dans le portail Azure, à l’aide du client Connexion Bureau à distance.
2. Ajoutez le nouveau serveur de passerelle et Web de bureau à distance au Gestionnaire de serveur :
    1. Lancez le Gestionnaire de serveur, cliquez sur **gérer > ajouter des serveurs**.   
    2. Dans la boîte de dialogue Ajouter des serveurs, cliquez sur **Rechercher maintenant**.   
    3. Sélectionnez le serveur Web de bureau à distance et passerelle nouvellement créé (par exemple, Contoso-WebGw2) et cliquez sur **OK**.
3. Ajouter des serveurs Web de bureau à distance et de passerelle pour le déploiement  
    1. Lancez le Gestionnaire de serveur.  
    2. Cliquez sur **des Services Bureau à distance > vue d’ensemble > serveurs de déploiement > tâches > ajouter des serveurs d’accès Bureau à distance Web**.   
    3. Sélectionnez le serveur nouvellement créé (par exemple, Contoso-WebGw2), puis cliquez sur **suivant**.  
    4. Dans la page de Confirmation, sélectionnez **redémarrer des ordinateurs distants en fonction des besoins**, puis cliquez sur **ajouter**.  
    5. Répétez ces étapes pour ajouter le serveur de passerelle Bureau à distance, mais choisissez **serveurs de passerelle Bureau à distance** à l’étape b.
4. Réinstallez les certificats pour les serveurs de passerelle Bureau à distance :
    1.  Cliquez sur le Gestionnaire de serveur sur le serveur de RDM, **Services Bureau à distance > vue d’ensemble > tâches > modifier les propriétés de déploiement**.  
    2.  Développez **certificats**.  
    3.  Faites défiler jusqu'à la table. Cliquez sur le Bureau à distance **Service de rôle passerelle > Sélectionner un certificat existant.**  
    4.  Cliquez sur **choisir un autre certificat** puis accédez à l’emplacement du certificat. Par exemple, \Contoso-CB1\Certificates). Sélectionnez le fichier de certificat pour le serveur Web de bureau à distance et passerelle créé au cours de la configuration requise (par exemple, ContosoRdGwCert), puis cliquez sur **Open**.  
    5.  Entrez le mot de passe du certificat, sélectionnez **autoriser le certificat doit être ajouté au magasin de certificats Autorités de certification racine de confiance sur les ordinateurs de destination**, puis cliquez sur **OK**.  
    6.  Cliquez sur **Appliquer**.
    > [!Note] 
    > Vous devrez peut-être redémarrer manuellement le service TSGateway en cours d’exécution sur chaque serveur de passerelle Bureau à distance, via le Gestionnaire de serveur ou le Gestionnaire des tâches.
    7.  Répétez les étapes a à f pour le Service de rôle accès Web Bureau à distance.

## <a name="step-2-configure-rd-web-and-rd-gateway-properties-on-the-new-server"></a>Étape 2 : Configurer les propriétés Web du Bureau à distance et passerelle Bureau à distance sur le nouveau serveur
1. Configurer le serveur comme faisant partie d’une batterie de serveurs de passerelle Bureau à distance :
    1.  Cliquez sur le Gestionnaire de serveur sur le serveur de RDM, **tous les serveurs**. Cliquez sur un des serveurs de passerelle Bureau à distance, puis cliquez sur **connexion Bureau à distance**.
    2.  Connectez-vous au serveur de passerelle Bureau à distance à l’aide d’un compte d’administrateur de domaine.  
    3.  Dans le Gestionnaire de serveur sur le serveur de passerelle Bureau à distance, cliquez sur **Outils > Services Bureau à distance > Gestionnaire de passerelle Bureau à distance**.  
    4.  Dans le volet de navigation, cliquez sur l’ordinateur local (par exemple, Contoso-WebGw1).  
    5.  Cliquez sur **membres de la batterie de serveurs de passerelle Bureau à distance ajouter**.  
    6.  Sur le **batterie de serveurs** onglet, entrez le nom de chaque serveur de passerelle Bureau à distance, puis cliquez sur **ajouter** et **appliquer**.  
    7.  Répétez les étapes a à f sur chaque serveur de passerelle Bureau à distance pour qu’ils reconnaissent mutuellement en tant que serveurs de passerelle Bureau à distance dans une batterie de serveurs. Processus s’il existe des avertissements, car il peut prendre un temps de propagation des paramètres DNS.
2. Configurer le serveur comme faisant partie d’une batterie de serveurs d’accès Web de bureau à distance. Les étapes ci-dessous configurent la Validation et le déchiffrement des clés d’ordinateur à l’identique sur les deux sites RDWeb.
    1.  Cliquez sur le Gestionnaire de serveur sur le serveur de RDM, **tous les serveurs**. Cliquez sur le premier serveur d’accès Web de bureau à distance (par exemple, Contoso-WebGw1), puis sur **connexion Bureau à distance**.  
    2.  Connectez-vous au serveur d’accès Web de bureau à distance à l’aide d’un compte d’administrateur de domaine.  
    3.  Dans le Gestionnaire de serveur sur le serveur d’accès Web de bureau à distance, cliquez sur **Outils > Gestionnaire des Services Internet (IIS)**.  
    4.  Dans le volet gauche du Gestionnaire des services Internet, développez le **serveur (par exemple, Contoso-WebGw1) > Sites > Site Web par défaut**, puis cliquez sur **RDWeb**.  
    5.  Avec le bouton droit **clé Machine**, puis cliquez sur **ouvrir la fonctionnalité**.
    6.  Dans la page clé de l’ordinateur, dans le **Actions** volet, sélectionnez **générer les clés**, puis cliquez sur **appliquer**.
    7.  Copiez la clé de validation (vous pouvez avec le bouton droit de la clé, puis sur **copie**.)
    8.  Dans le Gestionnaire des services Internet, sous **Site Web par défaut**, sélectionnez **flux**, **FeedLogon** et **Pages** à son tour.
    9. Pour chaque :
        1.  Avec le bouton droit **clé Machine**, puis cliquez sur **ouvrir la fonctionnalité**.
        2.  Pour la clé de Validation, désactivez **générer automatiquement lors de l’exécution**, puis collez la clé que vous avez copié à l’étape g.
    10.  Réduisez la fenêtre de connexion du Bureau à distance à ce serveur Web de bureau à distance.  
    11.  Répétez les étapes b à e pour le second serveur d’accès Bureau à distance par le Web, se terminant par l’affichage de la fonctionnalité de **clé Machine**.
    12. Pour la clé de Validation, désactivez **générer automatiquement lors de l’exécution**, puis collez la clé que vous avez copié à l’étape g.
    13. Cliquez sur **Appliquer**.
    14. Terminer ce processus pour le **RDWeb**, **flux**, **FeedLogon** et **Pages** pages.
    15. Réduire la fenêtre de connexion du Bureau à distance pour le second serveur d’accès Web de bureau à distance et agrandissez la fenêtre de connexion du Bureau à distance pour le premier serveur d’accès Web de bureau à distance.  
    16. Répétez les étapes g à n pour copier la clé de déchiffrement.
    17. Lorsque les clés de validation et de déchiffrement sont identiques sur les deux serveurs d’accès Web de bureau à distance pour le **RDWeb**, **flux**, **FeedLogon** et **Pages**pages, déconnectez-vous de toutes les fenêtres de connexion du Bureau à distance.

## <a name="step-3-configure-load-balancing-for-the-rd-web-and-rd-gateway-servers"></a>Étape 3 : Configurer l’équilibrage de charge pour les serveurs Web de bureau à distance et passerelle Bureau à distance

Si vous utilisez l’infrastructure Azure, vous pouvez créer un équilibreur de charge Azure externe ; Si ce n’est pas le cas, vous pouvez configurer un équilibreur de charge matérielle ou logicielle distinct. L’équilibrage de charge est clé afin que le trafic sera uniformément distribués les connexions de longue durées à partir de clients de bureau à distance, via la passerelle Bureau à distance, aux serveurs que les utilisateurs exécutent leurs charges de travail.

> [!Note] 
> Si votre serveur précédent en cours d’exécution Web Bureau à distance et passerelle Bureau à distance a été déjà installé derrière un équilibreur de charge externe, passez directement pour l’étape 4, sélectionnez le pool principal existant et ajoutez le nouveau serveur au pool.

1.  Créer un équilibreur de charge Azure :  
    1.  Dans le portail Azure, cliquez sur **Parcourir > équilibreurs de charge > ajouter**.  
    2.  Entrez un nom, par exemple **WebGwLB**.  
    3.  Sélectionnez **Public** pour le **schéma**, **adresse IP publique**et un **adresse IP publique**. Vous pouvez sélectionner une adresse IP publique existante ou créez-en un. 
    4.  Sélectionnez l’option appropriée **abonnement**, **groupe de ressources**, et **emplacement**.
    5.  Cliquez sur **Create (Créer)**.  
2. Créer un [sonde](https://azure.microsoft.com/documentation/articles/load-balancer-custom-probe-overview/) à analyser les serveurs qui sont actifs :  
    1.  Dans le portail Azure, cliquez sur **Parcourir > équilibrages de charge**., l’équilibreur de charge vous venez de créer, par exemple, WebGwLB et paramètres  
    2.  Cliquez sur **sondes > ajouter**.  
    3.  Entrez un nom, par exemple, **HTTPS**, de la sonde. Sélectionnez **TCP** en tant que le **protocole**, puis entrez **443** pour le **Port**, puis cliquez sur **OK**.   
3.  Créer la règles d’équilibrage de charge HTTPS et UDP :  
    1.  Dans **paramètres**, cliquez sur **règles d’équilibrage de charge**.  
    2.  Sélectionnez **ajouter** pour le **règle HTTPS**.  
    3.  Entrez un nom pour la règle, par exemple, HTTPS, puis sélectionnez **TCP** pour le **protocole**. Entrez **443** pour les deux **Port** et **port principal**, puis cliquez sur **OK**.  
    4.  Dans **règles d’équilibrage de charge**, cliquez sur **ajouter** pour le **UDP rule**.  
    5.  Entrez un nom pour la règle, par exemple, **UDP**, puis sélectionnez **UDP** pour le **protocole**. Entrez **3391** pour les deux **Port** et **port principal**, puis cliquez sur **OK**.  
4. Créer le pool principal pour les serveurs Web de bureau à distance et passerelle Bureau à distance :
      1. Dans **paramètres**, cliquez sur **pools d’adresses principales > ajouter**.   
      2. Entrez un nom (par exemple, **WebGwBackendPool**), puis cliquez sur **ajouter une machine virtuelle**.  
      3. Choisissez un groupe à haute disponibilité (par exemple, WebGwAvSet), puis cliquez sur **OK**.   
      4. Cliquez sur **choisir les machines virtuelles**, sélectionnez chaque machine virtuelle, puis cliquez sur **Sélectionner > OK > OK**.
