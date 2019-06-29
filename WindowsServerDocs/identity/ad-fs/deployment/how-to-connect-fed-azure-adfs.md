---
title: Active Directory Federation Services dans Azure | Microsoft Docs
description: Dans ce document, vous allez apprendre à déployer AD FS dans Azure pour la haute disponibilité.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 692a188c-badc-44aa-ba86-71c0e8074510
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/28/2018
ms.subservice: hybrid
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f075f91e97f806555507bfc0e0c5f3d1589a71e6
ms.sourcegitcommit: 63926404009f9e1330a4a0aa8cb9821a2dd7187e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67469648"
---
# <a name="deploying-active-directory-federation-services-in-azure"></a>Déploiement d’Active Directory Federation Services dans Azure
AD FS fournit la fédération des identités simplifiées et sécurisées et les fonctionnalités de l’authentification unique (SSO) Web. Fédération avec AD Azure ou O365 permet aux utilisateurs de s’authentifier à l’aide des informations d’identification locales et accéder à toutes les ressources dans le cloud. Par conséquent, il devient important de disposer d’une infrastructure AD FS hautement disponible pour garantir l’accès aux ressources locales et dans le cloud. Déploiement d’AD FS dans Azure peut aider à atteindre la haute disponibilité avec un minimum d’efforts.
Il existe plusieurs avantages du déploiement d’AD FS dans Azure, certaines d'entre elles sont répertoriées ci-dessous :

* **Haute disponibilité** -avec la puissance de groupes à haute disponibilité Azure, vous vérifiez une infrastructure hautement disponible.
* **Facile à mettre à l’échelle** – besoin de meilleures performances ? Facilement migrer vers des ordinateurs plus puissants en seulement quelques clics dans Azure
* **Géo-redondance** – avec redondance géographique Azure vous pouvez être sûr que votre infrastructure est hautement disponible dans le monde entier
* **Facilité de gestion** – avec les options de gestion extrêmement simplifiées dans le portail Azure, la gestion de votre infrastructure est simple et automatisée 

## <a name="design-principles"></a>Principes de conception
![Conception du déploiement](./media/how-to-connect-fed-azure-adfs/deployment.png)

Le diagramme ci-dessus illustre la topologie de base recommandée pour commencer à déployer votre infrastructure AD FS dans Azure. Les principes qui sous-tendent les différents composants de la topologie sont répertoriées ci-dessous :

* **Contrôleur de domaine / serveurs ADFS**: Si vous avez moins de 1 000 utilisateurs, que vous pouvez simplement installer le rôle AD FS sur vos contrôleurs de domaine. Si vous ne souhaitez pas que tout impact sur les performances sur les contrôleurs de domaine ou si vous avez plus de 1 000 utilisateurs, puis déployer AD FS sur des serveurs distincts.
* **Serveur WAP** – il est nécessaire de déployer des serveurs Proxy d’Application Web, afin que les utilisateurs peuvent accéder à AD FS quand ils ne sont pas sur le réseau d’entreprise également.
* **ZONE DMZ**: Les serveurs Proxy d’Application Web seront placés dans la zone DMZ et uniquement le port TCP/443 accès est autorisé entre le réseau de périmètre et le sous-réseau interne.
* **Équilibreurs de charge**: Pour garantir une haute disponibilité des serveurs AD FS et Proxy d’Application Web, nous recommandons à l’aide d’un équilibreur de charge interne pour les serveurs AD FS et Azure Load Balancer pour les serveurs Proxy d’Application Web.
* **Groupes à haute disponibilité**: Pour assurer la redondance à votre déploiement AD FS, il est recommandé de regrouper deux ou plusieurs machines virtuelles dans un groupe à haute disponibilité pour les charges de travail similaires. Cette configuration garantit que pendant un événement de maintenance planifié ou non planifié, au moins une machine virtuelle sera disponible
* **Comptes de stockage**: Il est recommandé d’avoir deux comptes de stockage. Avoir un seul compte de stockage peut entraîner la création d’un point de défaillance unique et peut entraîner le déploiement devient indisponible dans l’éventualité où le compte de stockage tombe en panne. Deux comptes de stockage permet d’associer un compte de stockage pour chaque ligne d’erreur.
* **Séparation des réseaux**:  Serveurs de Proxy d’Application Web doivent être déployés dans un réseau DMZ distinct. Vous pouvez diviser un réseau virtuel en deux sous-réseaux, puis déployez l’ou les serveurs Proxy d’Application Web dans un sous-réseau isolé. Vous pouvez simplement configurer les paramètres de groupe de sécurité réseau pour chaque sous-réseau et autoriser uniquement les communications requises entre les deux sous-réseaux. Plus les détails sont fournis par le scénario de déploiement ci-dessous

## <a name="steps-to-deploy-ad-fs-in-azure"></a>Étapes pour déployer AD FS dans Azure
Les étapes présentées dans cette section expliquent comment déployer le ci-dessous infrastructure AD FS représentée dans Azure.

### <a name="1-deploying-the-network"></a>1. Déployer le réseau
Comme indiqué ci-dessus, vous pouvez soit créer deux sous-réseaux dans un seul réseau virtuel ou bien créer deux complètement différents réseaux virtuels (VNet). Cet article vous concentrer sur le déploiement d’un seul réseau virtuel et la diviser en deux sous-réseaux. Il s’agit actuellement une approche plus aisée que deux réseaux virtuels distincts nécessiterait un réseau virtuel à la passerelle de réseau virtuel pour les communications.

**1.1 créer le réseau virtuel**

![Créer le réseau virtuel](./media/how-to-connect-fed-azure-adfs/deploynetwork1.png)

Dans le portail Azure, sélectionnez le réseau virtuel et vous pouvez déployer le réseau virtuel et un sous-réseau immédiatement avec un simple clic. Un sous-réseau INT est également défini et est maintenant prêt pour les machines virtuelles à ajouter.
L’étape suivante consiste à ajouter un autre sous-réseau au réseau, c'est-à-dire le sous-réseau DMZ. Pour créer le sous-réseau de réseau de périmètre, il vous suffit

* Sélectionnez le réseau qui vient d’être créé
* Dans les propriétés, sélectionnez le sous-réseau
* Dans le sous-réseau volet, cliquez sur le bouton Ajouter
* Fournissez les informations sous-réseau nom et l’adresse d’espace pour créer le sous-réseau

![Sous-réseau](./media/how-to-connect-fed-azure-adfs/deploynetwork2.png)

![Sous-réseau DMZ](./media/how-to-connect-fed-azure-adfs/deploynetwork3.png)

**1.2. Création des groupes de sécurité réseau**

Un groupe de sécurité réseau (NSG) contient une liste de règles de liste de contrôle d’accès (ACL) qui autorisent ou refusent le trafic réseau vers vos instances de machine virtuelle dans un réseau virtuel. Groupes de sécurité réseau peuvent être associés à des sous-réseaux ou à des instances de machine virtuelle individuelles au sein de ce sous-réseau. Lorsqu’un groupe de sécurité réseau est associé à un sous-réseau, les règles ACL s’appliquent à toutes les instances de machine virtuelle dans ce sous-réseau.
Pour les besoins de ce guide, nous allons créer deux groupes de sécurité réseau : un pour un réseau interne et un réseau de périmètre. Ils seront étiquetés nommés NSG_INT et NSG_DMZ respectivement.

![Créer le groupe de sécurité réseau](./media/how-to-connect-fed-azure-adfs/creatensg1.png)

Une fois le groupe de sécurité réseau est créé, il y aura des règles de trafic sortant entrants et 0 0. Une fois que les rôles sur les serveurs respectifs sont installés et fonctionnelle, puis les règles de trafic entrants et sortants peuvent être modulées selon le niveau de sécurité souhaité.

![Initialiser le groupe de sécurité réseau](./media/how-to-connect-fed-azure-adfs/nsgint1.png)

Une fois que les groupes de sécurité réseau sont créés, associez NSG_INT au sous-réseau INT et NSG_DMZ à sous-réseau DMZ. Une capture d’écran de l’exemple est donné ci-dessous :

![Configuration du groupe de sécurité réseau](./media/how-to-connect-fed-azure-adfs/nsgconfigure1.png)

* Cliquez sur sous-réseaux pour ouvrir le panneau des sous-réseaux
* Sélectionnez le sous-réseau à associer à un groupe de sécurité réseau 

Après la configuration, le panneau sous-réseaux doit ressembler à ceci :

![Sous-réseaux après un groupe de sécurité réseau](./media/how-to-connect-fed-azure-adfs/nsgconfigure2.png)

**1.3. Créer la connexion au niveau local**

Nous aurons besoin d’une connexion en local afin de déployer le contrôleur de domaine (DC) dans azure. Azure offre différentes options de connectivité pour connecter votre infrastructure locale à votre infrastructure Azure.

* Point-to-site
* Réseau virtuel de Site à site
* ExpressRoute

Il est recommandé d’utiliser ExpressRoute. ExpressRoute vous permet de créer des connexions privées entre les centres de données Azure et d’infrastructure qui se trouve sur votre serveur local ou dans un environnement de colocalisation. Les connexions ExpressRoute ne passent pas par l’Internet public. Ils offrent plus fiabilité, vitesses plus rapides, latence et une sécurité plus élevée que les connexions classiques sur Internet.
S’il est recommandé d’utiliser ExpressRoute, vous pouvez choisir n’importe quelle méthode de connexion plus adapté à votre organisation. Pour en savoir plus sur ExpressRoute et les différentes options de connectivité à l’aide d’ExpressRoute, consultez [présentation technique d’ExpressRoute](https://aka.ms/Azure/ExpressRoute).

### <a name="2-create-storage-accounts"></a>2. Créer des comptes de stockage
Pour maintenir la haute disponibilité et éviter toute dépendance à un seul compte de stockage, vous pouvez créer deux comptes de stockage. Diviser les ordinateurs dans chaque groupe à haute disponibilité en deux groupes, puis affectez chaque groupe à un compte de stockage distinct.

![Créer des comptes de stockage](./media/how-to-connect-fed-azure-adfs/storageaccount1.png)

### <a name="3-create-availability-sets"></a>3. Créer des groupes à haute disponibilité
Pour chaque rôle (contrôleur de domaine/AD FS et WAP), créer des groupes à haute disponibilité qui contiendront les 2 machines au minimum. Cela vous permet de garantir une meilleure disponibilité pour chaque rôle. Création de la disponibilité de jeux, il est essentiel de choisir les éléments suivants :

* **Domaines d’erreur**: Les machines virtuelles dans le même domaine d’erreur partagent la même source d’alimentation et un commutateur réseau physique. Un minimum de 2 domaines d’erreur sont recommandés. La valeur par défaut est 3 et vous pouvez la laisser comme pour les besoins de ce déploiement
* **Mettre à jour des domaines**: Machines appartenant au même domaine de mise à jour sont redémarrées simultanément pendant une mise à jour. Vous souhaitez disposer d’au moins 2 domaines de mise à jour. La valeur par défaut est 5 et vous pouvez la laisser comme pour les besoins de ce déploiement

![Groupes à haute disponibilité](./media/how-to-connect-fed-azure-adfs/availabilityset1.png)

Créer la haute disponibilité suivants :

| Groupe à haute disponibilité | Rôle | Domaines d’erreur | Domaines de mise à jour |
|:---:|:---:|:---:|:--- |
| contosodcset |DC/ADFS |3 |5 |
| contosowapset |WAP |3 |5 |

### <a name="4-deploy-virtual-machines"></a>4. Déployer des ordinateurs virtuels
L’étape suivante consiste à déployer des machines virtuelles qui hébergeront les différents rôles dans votre infrastructure. Un minimum de deux machines sont recommandées dans chaque groupe à haute disponibilité. Créez quatre machines virtuelles pour le déploiement de base.

| Machine | Rôle | Sous-réseau | Groupe à haute disponibilité | Compte de stockage | Adresse IP |
|:---:|:---:|:---:|:---:|:---:|:---:|
| contosodc1 |DC/ADFS |INT |contosodcset |contososac1 |Statique |
| contosodc2 |DC/ADFS |INT |contosodcset |contososac2 |Statique |
| contosowap1 |WAP |DMZ |contosowapset |contososac1 |Statique |
| contosowap2 |WAP |DMZ |contosowapset |contososac2 |Statique |

Comme vous l’avez peut-être remarqué, aucun groupe de sécurité réseau n’a été spécifié. Il s’agit, car azure vous permet d’utiliser le groupe de sécurité réseau au niveau du sous-réseau. Ensuite, vous pouvez contrôler le trafic réseau d’ordinateur à l’aide du groupe de sécurité réseau individuel associé à un sous-réseau ou à l’objet carte réseau. En savoir plus sur [qu’est un groupe de sécurité réseau (NSG)](https://aka.ms/Azure/NSG).
Adresse IP statique est recommandée si vous gérez le serveur DNS. Vous pouvez utiliser Azure DNS et à la place dans les enregistrements DNS pour votre domaine, consultez les nouvelles machines virtuelles par leurs noms de domaine complets Azure.
Une fois le déploiement terminé, votre volet de la machine virtuelle doit ressembler à ceci :

![Machines virtuelles déployées](./media/how-to-connect-fed-azure-adfs/virtualmachinesdeployed_noadfs.png)

### <a name="5-configuring-the-domain-controller--ad-fs-servers"></a>5. Configuration du contrôleur de domaine / serveurs AD FS
 Pour authentifier les demandes entrantes, AD FS devez contacter le contrôleur de domaine. Pour enregistrer les coûteux à partir d’Azure pour le contrôleur de domaine local pour l’authentification, il est recommandé de déployer un réplica du contrôleur de domaine dans Azure. Pour atteindre une haute disponibilité, il est recommandé de créer un ensemble de disponibilité d’au moins 2 contrôleurs de domaine.

| Contrôleur de domaine | Rôle | Compte de stockage |
|:---:|:---:|:---:|
| contosodc1 |Réplica |contososac1 |
| contosodc2 |Réplica |contososac2 |

* Promouvoir les deux serveurs en tant que contrôleurs de domaine réplica avec DNS
* Configurer les serveurs AD FS en installant le rôle AD FS à l’aide du Gestionnaire de serveur.

### <a name="6-deploying-internal-load-balancer-ilb"></a>6. Déploiement de l’équilibreur de charge interne (ILB)
**6.1. Créer l’équilibreur de charge interne**

Pour déployer un équilibreur de charge interne, sélectionnez équilibreurs de charge dans le portail Azure et cliquez sur Ajouter (+).

> [!NOTE]
> Si vous ne voyez pas **équilibreurs de charge** dans votre menu, cliquez sur **Parcourir** dans l’angle inférieur gauche du portail et faites défiler jusqu'à ce que vous voyiez **équilibreurs de charge**.  Cliquez ensuite sur l’étoile jaune pour l’ajouter à votre menu. Sélectionnez maintenant la nouvelle icône d’équilibreur de charge pour ouvrir le panneau pour commencer la configuration de l’équilibreur de charge.
> 
> 

![Parcourir l’équilibreur de charge](./media/how-to-connect-fed-azure-adfs/browseloadbalancer.png)

* **Nom** : Attribuez un nom approprié à l’équilibreur de charge
* **Schéma**: Étant donné que cet équilibreur de charge sera placé devant les serveurs AD FS et le but de fournir uniquement les connexions de réseau interne, sélectionnez « Interne »
* **Réseau virtuel**: Choisissez le réseau virtuel où vous déployez vos services AD FS
* **Sous-réseau** : Choisissez le sous-réseau interne ici
* **Affectation d’adresses IP**: Statique

![Équilibreur de charge interne](./media/how-to-connect-fed-azure-adfs/ilbdeployment1.png)

Après avoir cliqué sur Créer et l’équilibrage de charge interne est déployé, vous devez la voir dans la liste des équilibreurs de charge :

![Équilibreurs de charge après équilibreur de charge interne](./media/how-to-connect-fed-azure-adfs/ilbdeployment2.png)

Étape suivante consiste à configurer le pool principal et la sonde principale.

**6.2. Configurer le pool principal de l’équilibreur de charge interne**

Sélectionnez l’équilibreur de charge interne nouvellement créé dans le panneau équilibreurs de charge. Le panneau Paramètres s’ouvre. 

1. Sélectionner les pools principaux à partir du Panneau de paramètres
2. Dans le panneau de pool principal ajouter, cliquez sur Ajouter une machine virtuelle
3. S’affiche dans le panneau où vous pouvez choisir le groupe à haute disponibilité
4. Choisissez le groupe à haute disponibilité AD FS

![Configurer le pool principal de l’équilibreur de charge interne](./media/how-to-connect-fed-azure-adfs/ilbdeployment3.png)

**6.3. Configuration de la sonde**

Dans le panneau de paramètres d’équilibrage de charge interne, sélectionnez sondes d’intégrité.

1. Cliquez sur Ajouter
2. Fournir les détails de la sonde un. **Nom** : Sonde b de nom. **Protocole** : HTTP c. **Port** : 80 (HTTP) d. **Chemin d’accès**: / adfs/probe e. **Intervalle** : 5 (valeur par défaut) – il s’agit de l’intervalle auquel équilibreur de charge interne interrogera les machines dans le pool principal f. **Limite de seuil de défaillance**: 2 (valeur par défaut) – il s’agit le seuil d’échecs de sonde consécutifs après lequel ILB déclarer un ordinateur dans le pool principal non réactive et cessera de lui envoyer le trafic à celui-ci.

![Configuration de la sonde d’équilibreur de charge interne](./media/how-to-connect-fed-azure-adfs/ilbdeployment4.png)

Nous utilisons le point de terminaison /adfs/probe qui a été créé explicitement pour les contrôles d’intégrité dans un environnement AD FS où une vérification du chemin d’accès HTTPS complète ne peut pas se produire.  Cela est beaucoup mieux que d’une vérification de base port 443, qui ne reflète pas exactement l’état d’un déploiement AD FS modern.  Plus d’informations, consultez https://blogs.technet.microsoft.com/applicationproxyblog/2014/10/17/hardware-load-balancer-health-checks-and-web-application-proxy-ad-fs-2012-r2/.

**6.4. Créer des règles d’équilibrage de charge**

Pour équilibrer le trafic au mieux, l’équilibrage de charge interne doit être configuré avec les règles d’équilibrage de charge. Pour créer une règle d’équilibrage de charge 

1. Sélectionnez la règle à partir du Panneau de paramètres de l’équilibrage de charge interne d’équilibrage de charge
2. Cliquez sur Ajouter dans la panneau de configuration de règle d’équilibrage de charge
3. Dans la panneau de configuration de règle d’équilibrage de charge d’ajouter un. **Nom** : Fournissez un nom pour la règle b. **Protocole** : Sélectionnez TCP c. **Port** : 443 d. **Port principal**: 443 e. **Pool principal**: Sélectionnez le pool que vous avez créé pour le cluster AD FS f antérieures. **Sonde**: Sélectionnez la sonde créée précédemment pour les serveurs AD FS

![Configurer équilibrage règles d’équilibrage de charge interne](./media/how-to-connect-fed-azure-adfs/ilbdeployment5.png)

**6.5. Mise à jour DNS avec équilibrage de charge interne**

Accédez à votre serveur DNS et créez un enregistrement CNAME pour l’équilibrage de charge interne. L’enregistrement CNAME doit être le service de fédération avec l’adresse IP pointant vers l’adresse IP de l’équilibreur de charge interne. Par exemple, si l’adresse DIP d’équilibrage de charge interne est 10.3.0.8 et le service de fédération installé est fs.contoso.com, puis créer un enregistrement CNAME pour fs.contoso.com pointant vers 10.3.0.8.
Cela garantit que toutes les communications concernant fs.contoso.com pointeront l’équilibrage de charge interne vers et sont routées.

### <a name="7-configuring-the-web-application-proxy-server"></a>7. Configuration du serveur Proxy d’Application Web
**7.1. Configuration des serveurs Proxy d’Application Web pour atteindre les serveurs AD FS**

Pour garantir que les serveurs Proxy d’Application Web sont en mesure d’atteindre les serveurs AD FS derrière l’équilibreur de charge interne, créez un enregistrement dans le %systemroot%\system32\drivers\etc\hosts pour l’équilibrage de charge interne. Notez que le nom unique (DN) doit être le nom du service de fédération, par exemple fs.contoso.com. Et l’entrée d’adresse IP doit être celle de l’adresse IP de d’équilibrage de charge interne (10.3.0.8 comme dans l’exemple).

**7.2. L’installation du rôle de Proxy d’Application Web**

Après avoir vérifié que les serveurs Proxy d’Application Web sont en mesure d’atteindre les serveurs AD FS derrière l’équilibreur de charge interne, vous pouvez ensuite installer les serveurs Proxy d’Application Web. Serveurs de Proxy d’Application Web ne doivent pas être joint au domaine. Installer les rôles de Proxy d’Application Web sur les deux serveurs Proxy d’Application Web en sélectionnant le rôle accès à distance. Le Gestionnaire de serveur vous guideront pour terminer l’installation du proxy d’application Web.
Pour plus d’informations sur la façon de déployer le proxy d’application Web, consultez [installer et configurer le serveur de Proxy d’Application Web](https://technet.microsoft.com/library/dn383662.aspx).

### <a name="8--deploying-the-internet-facing-public-load-balancer"></a>8.  Déploiement d’équilibrage de charge (Public) accessible sur Internet
**8.1.  Créer l’équilibreur de charge (Public) accessible sur Internet**

Dans le portail Azure, sélectionnez les équilibreurs de charge, puis cliquez sur Ajouter. Dans le panneau créer un équilibreur, entrez les informations suivantes

1. **Nom** : Nom de l’équilibreur de charge
2. **Schéma**: Public – cette option indique à Azure que cet équilibreur de charge sera besoin d’une adresse publique.
3. **Adresse IP**: Créer une nouvelle adresse IP (dynamique)

![Équilibreur de charge accessible sur Internet](./media/how-to-connect-fed-azure-adfs/elbdeployment1.png)

Après le déploiement, l’équilibreur de charge s’affiche dans la liste des équilibreurs de charge.

![Liste des équilibreurs de charge](./media/how-to-connect-fed-azure-adfs/elbdeployment2.png)

**8.2. Attribuer un nom DNS à l’adresse IP publique**

Cliquez sur l’entrée d’équilibrage de charge qui vient d’être créé dans le panneau équilibreurs de charge pour afficher le panneau de configuration. Suivez les étapes ci-dessous pour configurer le nom DNS pour l’adresse IP publique :

1. Cliquez sur l’adresse IP publique. Le panneau s’ouvre pour l’adresse IP publique et de ses paramètres
2. Cliquez sur la Configuration
3. Indiquez un nom DNS. Cela devient le nom DNS public, vous pouvez accéder depuis n’importe où, par exemple contosofs.westus.cloudapp.azure.com. Vous pouvez ajouter une entrée dans le DNS externe pour le service de fédération (par exemple, fs.contoso.com) qui résout le nom DNS de l’équilibreur de charge externe (contosofs.westus.cloudapp.azure.com).

![Configurer l’équilibreur de charge accessible sur internet](./media/how-to-connect-fed-azure-adfs/elbdeployment3.png) 

![Configurer l’équilibrage de charge (DNS) internet](./media/how-to-connect-fed-azure-adfs/elbdeployment4.png)

**8.3. Configuration du pool principal pour l’équilibreur de charge (Public) accessible sur Internet** 

Suivez les mêmes étapes que pour la création de l’équilibreur de charge interne, pour configurer le pool principal pour l’équilibreur de charge (Public) accessible sur Internet en tant que la haute disponibilité pour les serveurs WAP. Par exemple, contosowapset.

![Configuration du pool principal d’équilibreur de charge accessible sur Internet](./media/how-to-connect-fed-azure-adfs/elbdeployment5.png)

**8.4. Configuration de la sonde**

Suivez les mêmes étapes que pour la configuration de l’équilibreur de charge interne afin de configurer la sonde pour le pool principal de serveurs WAP.

![Configuration de la sonde d’équilibreur de charge accessible sur Internet](./media/how-to-connect-fed-azure-adfs/elbdeployment6.png)

**8.5. Créer une ou plusieurs règles d’équilibrage de charge**

Suivez les mêmes étapes que l’équilibrage de charge interne pour configurer la règle d’équilibrage pour le port TCP 443.

![Configurer des règles d’équilibrage de l’équilibreur de charge accessible sur Internet](./media/how-to-connect-fed-azure-adfs/elbdeployment7.png)

### <a name="9-securing-the-network"></a>9. Sécurisation du réseau
**9.1. Sécurisation du sous-réseau interne**

En général, vous devez les règles suivantes pour sécuriser efficacement votre sous-réseau interne (dans l’ordre indiqué ci-dessous)

| Règle | Description | Flux |
|:--- |:--- |:---:|
| AllowHTTPSFromDMZ |Autoriser la communication HTTPS à partir de la zone DMZ |Entrant |
| DenyInternetOutbound |Aucun accès à internet |Sortant |

![Règles d’accès INT (entrant)](./media/how-to-connect-fed-azure-adfs/nsg_int.png)

**9.2. Sécurisation du sous-réseau DMZ**

| Règle | Description | Flux |
|:--- |:--- |:---:|
| AllowHTTPSFromInternet |Autoriser le trafic HTTPS à partir d’internet vers le réseau de périmètre |Entrant |
| DenyInternetOutbound |N’est pas HTTPS à internet est bloqué |Sortant |

![Règles d’accès EXT (entrant)](./media/how-to-connect-fed-azure-adfs/nsg_dmz.png)

> [!NOTE]
> Si l’authentification du certificat utilisateur client (authentification clientTLS à l’aide de X509 certificats utilisateur) est requise, AD FS nécessite TCP port 49443 être activé pour l’accès entrant.
> 
> 

### <a name="10-test-the-ad-fs-sign-in"></a>10. Tester l’authentification dans AD FS
Le moyen le plus simple est de tester AD FS à l’aide de la page IdpInitiatedSignon.aspx. Afin d’être en mesure d’effectuer, il est nécessaire pour activer l’authentification IdpInitiatedSignOn sur les propriétés AD FS. Suivez les étapes ci-dessous pour vérifier votre configuration AD FS

1. Exécutez l’applet de commande sur le serveur AD FS, à l’aide de PowerShell, pour définir sur activé ci-dessous.
   Set-AdfsProperties -EnableIdPInitiatedSignonPage $true 
2. À partir de n’importe quelle machine externe, accéder à https :\//adfs-server.contoso.com/adfs/ls/IdpInitiatedSignon.aspx.  
3. Vous devez voir la page AD FS comme ci-dessous :

![Page de connexion de test](./media/how-to-connect-fed-azure-adfs/test1.png)

Lors de l’authentification réussie, il vous fournira un message de réussite comme indiqué ci-dessous :

![Réussite du test](./media/how-to-connect-fed-azure-adfs/test2.png)

## <a name="template-for-deploying-ad-fs-in-azure"></a>Modèle de déploiement AD FS dans Azure
Le modèle déploie une installation à 6 machines, 2 contrôleurs de domaine, AD FS et WAP.

[AD FS dans le modèle de déploiement Azure](https://github.com/paulomarquesc/adfs-6vms-regular-template-based)

Vous pouvez utiliser un réseau virtuel existant ou créer un nouveau réseau virtuel lors du déploiement de ce modèle. Les différents paramètres disponibles pour la personnalisation du déploiement sont répertoriés ci-dessous avec la description de l’utilisation du paramètre dans le processus de déploiement. 

| Paramètre | Description |
|:--- |:--- |
| Location |Région dans laquelle déployer les ressources, par exemple, East US. |
| StorageAccountType |Le type de compte de stockage créé |
| VirtualNetworkUsage |Indique si un nouveau réseau virtuel sera créé ou utilisez-en un existant |
| VirtualNetworkName |Le nom du réseau virtuel à créer, obligatoire sur l’utilisation du réseau virtuel nouveau ou existant |
| VirtualNetworkResourceGroupName |Spécifie le nom du groupe de ressources où se trouve le réseau virtuel existant. Lorsque vous utilisez un réseau virtuel existant, ce paramètre est obligatoire pour le déploiement puisse trouver l’ID du réseau virtuel existant |
| VirtualNetworkAddressRange |La plage d’adresses du nouveau réseau virtuel, obligatoire si vous créez un nouveau réseau virtuel |
| InternalSubnetName |Le nom du sous-réseau interne, obligatoire dans les deux options d’utilisation de réseau virtuel (nouveau ou existantes) |
| InternalSubnetAddressRange |La plage d’adresses du sous-réseau interne, qui contient les contrôleurs de domaine et les serveurs ADFS, obligatoires si vous créez un nouveau réseau virtuel. |
| DMZSubnetAddressRange |La plage d’adresses du sous-réseau dmz, qui contient les Windows serveurs proxy d’application, obligatoires si vous créez un nouveau réseau virtuel. |
| DMZSubnetName |Le nom du sous-réseau interne, obligatoire dans les deux options d’utilisation de réseau virtuel (nouveau ou existantes). |
| ADDC01NICIPAddress |L’adresse IP interne du premier contrôleur de domaine, cette adresse IP est affectée de manière statique au contrôleur de domaine et doit être une adresse ip valide au sein du sous-réseau interne |
| ADDC02NICIPAddress |L’adresse IP interne du second contrôleur de domaine, cette adresse IP est affectée de manière statique au contrôleur de domaine et doit être une adresse ip valide au sein du sous-réseau interne |
| ADFS01NICIPAddress |L’adresse IP interne du premier serveur ADFS, cette adresse IP est affectée de manière statique pour le serveur ADFS et doit être une adresse ip valide au sein du sous-réseau interne |
| ADFS02NICIPAddress |L’adresse IP interne du second serveur ADFS, cette adresse IP est affectée de manière statique pour le serveur ADFS et doit être une adresse ip valide au sein du sous-réseau interne |
| WAP01NICIPAddress |L’adresse IP interne du premier serveur WAP, cette adresse IP est affectée de manière statique au serveur WAP et doit être une adresse ip valide au sein du sous-réseau DMZ |
| WAP02NICIPAddress |L’adresse IP interne du second serveur WAP, cette adresse IP est affectée de manière statique au serveur WAP et doit être une adresse ip valide au sein du sous-réseau DMZ |
| ADFSLoadBalancerPrivateIPAddress |L’adresse IP interne de l’équilibreur de charge ADFS, cette adresse IP est affectée de manière statique l’équilibreur de charge et doit être une adresse ip valide au sein du sous-réseau interne |
| ADDCVMNamePrefix |Préfixe de nom de Machine virtuelle pour les contrôleurs de domaine |
| ADFSVMNamePrefix |Préfixe de nom de Machine virtuelle pour les serveurs ADFS |
| WAPVMNamePrefix |Préfixe de nom de Machine virtuelle pour les serveurs WAP |
| ADDCVMSize |La taille de machine virtuelle des contrôleurs de domaine |
| ADFSVMSize |La taille de machine virtuelle des serveurs ADFS |
| WAPVMSize |La taille de machine virtuelle des serveurs WAP |
| AdminUserName |Le nom de l’administrateur local des machines virtuelles |
| AdminPassword |Le mot de passe pour le compte administrateur local des machines virtuelles |

## <a name="additional-resources"></a>Ressources supplémentaires
* [Groupes à haute disponibilité](https://aka.ms/Azure/Availability) 
* [Équilibrage de charge Azure](https://aka.ms/Azure/ILB)
* [Équilibreur de charge interne](https://aka.ms/Azure/ILB/Internal)
* [Équilibreur de charge accessible sur Internet](https://aka.ms/Azure/ILB/Internet)
* [Comptes de stockage](https://aka.ms/Azure/Storage)
* [Réseaux virtuels Azure](https://aka.ms/Azure/VNet)
* [AD FS et les liens de Proxy d’Application Web](https://aka.ms/ADFSLinks) 

## <a name="next-steps"></a>Étapes suivantes
* [L’intégration de vos identités locales avec Azure Active Directory](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-hybrid-identity)
* [Configuration et la gestion des services AD FS à l’aide d’Azure AD Connect](/azure/active-directory/hybrid/how-to-connect-fed-whatis)
* [Déploiement des services haute disponibilité par-delà AD FS dans Azure avec Azure Traffic Manager](active-directory-adfs-in-azure-with-azure-traffic-manager.md)
