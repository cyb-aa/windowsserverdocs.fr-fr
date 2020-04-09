---
title: Services ADFS dans Azure | Microsoft Docs
description: Dans ce document, vous allez apprendre à déployer AD FS dans Azure pour une haute disponibilité.
author: billmath
manager: mtillman
ms.assetid: 692a188c-badc-44aa-ba86-71c0e8074510
ms.topic: get-started-article
ms.date: 10/28/2018
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: 5701e7955a3baff248c0f7efc0b1de03088a8aa0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854952"
---
# <a name="deploying-active-directory-federation-services-in-azure"></a>Déploiement d’Services ADFS dans Azure
AD FS fournit des fonctionnalités de Fédération d’identité et d’authentification unique Web (SSO) simplifiées et sécurisées. La Fédération avec Azure AD ou O365 permet aux utilisateurs de s’authentifier à l’aide des informations d’identification locales et d’accéder à toutes les ressources dans le Cloud. Par conséquent, il est important de disposer d’une infrastructure de AD FS hautement disponible pour garantir l’accès aux ressources locales et dans le Cloud. Le déploiement d’AD FS dans Azure peut vous aider à bénéficier de la haute disponibilité requise avec un minimum d’efforts.
Le déploiement d’AD FS dans Azure présente plusieurs avantages. certains d’entre eux sont répertoriés ci-dessous :

* **Haute disponibilité** : avec la puissance des groupes à haute disponibilité Azure, vous garantissez une infrastructure hautement disponible.
* **Facile à mettre à l’échelle : vous** avez besoin de davantage de performances ? Migrez facilement vers des machines plus puissantes en seulement quelques clics dans Azure
* **Redondance entre géo-géolocalisation** : avec la redondance géographique Azure, vous pouvez être certain que votre infrastructure est hautement disponible dans le monde entier
* **Facilité de** gestion : grâce à des options de gestion très simplifiées dans portail Azure, la gestion de votre infrastructure est très simple et sans soucis. 

## <a name="design-principles"></a>Principes de conception
![Conception du déploiement](./media/how-to-connect-fed-azure-adfs/deployment.png)

Le diagramme ci-dessus illustre la topologie de base recommandée pour commencer le déploiement de votre infrastructure AD FS dans Azure. Les principes qui sous-tendent les différents composants de la topologie sont répertoriés ci-dessous :

* **Serveurs DC/ADFS**: Si vous avez moins de 1 000 utilisateurs, vous pouvez simplement installer AD FS rôle sur vos contrôleurs de domaine. Si vous ne souhaitez pas avoir d’impact sur les performances des contrôleurs de domaine ou si vous avez plus de 1 000 utilisateurs, déployez AD FS sur des serveurs distincts.
* **Serveur WAP** : il est nécessaire de déployer des serveurs proxy d’application Web, afin que les utilisateurs puissent accéder au AD FS lorsqu’ils ne sont pas également sur le réseau de l’entreprise.
* **Zone DMZ**: les serveurs proxy d’application Web sont placés dans la zone DMZ et seul l’accès TCP/443 est autorisé entre la zone DMZ et le sous-réseau interne.
* **Équilibreurs de charge**: pour garantir la haute disponibilité des AD FS et des serveurs proxy d’application Web, nous vous recommandons d’utiliser un équilibreur de charge interne pour les serveurs AD FS et les Azure load balancer pour les serveurs proxy d’application Web.
* **Groupes à haute disponibilité**: pour assurer la redondance de votre déploiement AD FS, il est recommandé de regrouper au moins deux machines virtuelles dans un groupe à haute disponibilité pour des charges de travail similaires. Cette configuration garantit qu’au moins une des machines virtuelles sera disponible pendant un événement de maintenance planifiée ou non planifiée.
* **Comptes de stockage**: il est recommandé de disposer de deux comptes de stockage. Le fait de disposer d’un seul compte de stockage peut entraîner la création d’un point de défaillance unique et peut entraîner l’indisponibilité du déploiement dans un scénario improbable où le compte de stockage tombe en panne. Deux comptes de stockage vous aident à associer un compte de stockage pour chaque ligne d’erreur.
* **Ségrégation du réseau**: les serveurs proxy d’application Web doivent être déployés dans un réseau DMZ distinct. Vous pouvez diviser un réseau virtuel en deux sous-réseaux, puis déployer le ou les serveurs proxy d’application Web dans un sous-réseau isolé. Vous pouvez simplement configurer les paramètres du groupe de sécurité réseau pour chaque sous-réseau et autoriser uniquement la communication requise entre les deux sous-réseaux. Plus de détails sont fournis par scénario de déploiement ci-dessous

## <a name="steps-to-deploy-ad-fs-in-azure"></a>Étapes de déploiement d’AD FS dans Azure
Les étapes mentionnées dans cette section décrivent le Guide de déploiement de l’infrastructure AD FS décrite ci-dessous dans Azure.

### <a name="1-deploying-the-network"></a>1. déploiement du réseau
Comme indiqué ci-dessus, vous pouvez soit créer deux sous-réseaux dans un même réseau virtuel, soit créer deux réseaux virtuels complètement différents. Cet article se concentre sur le déploiement d’un seul réseau virtuel et sa division en deux sous-réseaux. Il s’agit actuellement d’une approche plus simple, car deux réseaux virtuels distincts requièrent une passerelle de réseau virtuel à réseau virtuel pour les communications.

**1,1 créer un réseau virtuel**

![Créer un réseau virtuel](./media/how-to-connect-fed-azure-adfs/deploynetwork1.png)

Dans le Portail Azure, sélectionnez réseau virtuel et vous pouvez déployer le réseau virtuel et un sous-réseau immédiatement en un seul clic. Le sous-réseau INT est également défini et est maintenant prêt pour les machines virtuelles à ajouter.
L’étape suivante consiste à ajouter un autre sous-réseau au réseau, c.-à-d. le sous-réseau DMZ. Pour créer le sous-réseau DMZ, il suffit

* Sélectionner le réseau nouvellement créé
* Dans les propriétés, sélectionnez sous-réseau
* Dans le volet de sous-réseau, cliquez sur le bouton Ajouter.
* Fournir le nom du sous-réseau et les informations d’espace d’adressage pour créer le sous-réseau

![Sous-réseau](./media/how-to-connect-fed-azure-adfs/deploynetwork2.png)

![DMZ de sous-réseau](./media/how-to-connect-fed-azure-adfs/deploynetwork3.png)

**1,2. création des groupes de sécurité réseau**

Un groupe de sécurité réseau (NSG) contient une liste de règles de liste de Access Control (ACL) qui autorisent ou refusent le trafic réseau vers vos instances de machine virtuelle dans un réseau virtuel. Les groupes peuvent être associés à des sous-réseaux ou à des instances de machine virtuelle individuelles au sein de ce sous-réseau. Lorsqu’un groupe de sécurité réseau est associé à un sous-réseau, les règles ACL s’appliquent à toutes les instances de machine virtuelle de ce sous-réseau.
Dans le cadre de ce guide, nous allons créer deux groupes : un pour un réseau interne et un DMZ. Ils seront étiquetés NSG_INT et NSG_DMZ respectivement.

![Créer un groupe de sécurité réseau](./media/how-to-connect-fed-azure-adfs/creatensg1.png)

Une fois le groupe de sécurité réseau créé, il y aura 0 règle entrante et 0. Une fois les rôles sur les serveurs respectifs installés et fonctionnels, les règles entrantes et sortantes peuvent être établies en fonction du niveau de sécurité souhaité.

![Initialiser NSG](./media/how-to-connect-fed-azure-adfs/nsgint1.png)

Une fois les groupes créés, associez NSG_INT au sous-réseau INT et NSG_DMZ avec la zone DMZ de sous-réseau. Vous trouverez un exemple de capture d’écran ci-dessous :

![Configuration de NSG](./media/how-to-connect-fed-azure-adfs/nsgconfigure1.png)

* Cliquez sur sous-réseaux pour ouvrir le panneau des sous-réseaux.
* Sélectionner le sous-réseau à associer au groupe de sécurité réseau 

Après la configuration, le panneau des sous-réseaux doit ressembler à ceci :

![Sous-réseaux après NSG](./media/how-to-connect-fed-azure-adfs/nsgconfigure2.png)

**1,3. création d’une connexion au site local**

Nous aurons besoin d’une connexion locale pour déployer le contrôleur de domaine (DC) dans Azure. Azure offre différentes options de connectivité pour connecter votre infrastructure locale à votre infrastructure Azure.

* Point à site
* Réseau virtuel de site à site
* ExpressRoute

Nous vous recommandons d’utiliser ExpressRoute. ExpressRoute vous permet de créer des connexions privées entre les centres de gestion et l’infrastructure Azure qui se trouvent dans votre environnement local ou dans un environnement de colocalisation. Les connexions ExpressRoute ne sont pas établies via l’Internet public. Ils offrent une plus grande fiabilité, des vitesses supérieures, des latences moindres et une sécurité accrue par rapport aux connexions classiques sur Internet.
Bien qu’il soit recommandé d’utiliser ExpressRoute, vous pouvez choisir n’importe quelle méthode de connexion adaptée à votre organisation. Pour en savoir plus sur ExpressRoute et les différentes options de connectivité à l’aide de ExpressRoute, consultez [Présentation technique de ExpressRoute](https://aka.ms/Azure/ExpressRoute).

### <a name="2-create-storage-accounts"></a>2. créer des comptes de stockage
Pour maintenir une haute disponibilité et éviter de dépendre d’un seul compte de stockage, vous pouvez créer deux comptes de stockage. Divisez les machines de chaque groupe à haute disponibilité en deux groupes, puis affectez un compte de stockage distinct à chaque groupe.

![Créer des comptes de stockage](./media/how-to-connect-fed-azure-adfs/storageaccount1.png)

### <a name="3-create-availability-sets"></a>3. créer des groupes à haute disponibilité
Pour chaque rôle (DC/AD FS et WAP), créez des groupes à haute disponibilité qui contiendront 2 machines chacun au minimum. Cela permet d’obtenir une disponibilité plus élevée pour chaque rôle. Lors de la création des groupes à haute disponibilité, il est essentiel de choisir les éléments suivants :

* **Domaines d’erreur**: les machines virtuelles d’un même domaine d’erreur partagent la même source d’alimentation et le même commutateur réseau physique. Au moins 2 domaines d’erreur sont recommandés. La valeur par défaut est 3 et vous pouvez la conserver comme dans le cadre de ce déploiement
* **Domaines de mise à jour**: les machines appartenant au même domaine de mise à jour sont redémarrées ensemble pendant une mise à jour. Vous souhaitez disposer d’au moins 2 domaines de mise à jour. La valeur par défaut est 5 et vous pouvez la conserver comme dans le cadre de ce déploiement

![Groupes à haute disponibilité](./media/how-to-connect-fed-azure-adfs/availabilityset1.png)

Créer les groupes à haute disponibilité suivants

| Groupe à haute disponibilité | Role | Domaines d’erreur | Mettre à jour les domaines |
|:---:|:---:|:---:|:--- |
| contosodcset |DC/ADFS |3 |5 |
| contosowapset |WAP |3 |5 |

### <a name="4-deploy-virtual-machines"></a>4. déployer des machines virtuelles
L’étape suivante consiste à déployer des machines virtuelles qui hébergeront les différents rôles dans votre infrastructure. Au moins deux ordinateurs sont recommandés dans chaque groupe à haute disponibilité. Créez quatre machines virtuelles pour le déploiement de base.

| Ordinateur | Role | Sous-réseau | Groupe à haute disponibilité | Compte de stockage | Adresse IP |
|:---:|:---:|:---:|:---:|:---:|:---:|
| nommé contosodc1 |DC/ADFS |TIERS |contosodcset |contososac1 |Static |
| contosodc2 |DC/ADFS |TIERS |contosodcset |contososac2 |Static |
| contosowap1 |WAP |DMZ |contosowapset |contososac1 |Static |
| contosowap2 |WAP |DMZ |contosowapset |contososac2 |Static |

Comme vous l’avez peut-être remarqué, aucun NSG n’a été spécifié. Cela est dû au fait qu’Azure vous permet d’utiliser NSG au niveau du sous-réseau. Vous pouvez ensuite contrôler le trafic réseau de l’ordinateur à l’aide du NSG individuel associé au sous-réseau ou à l’objet NIC. En savoir plus sur [ce qu’est un groupe de sécurité réseau (NSG)](https://aka.ms/Azure/NSG).
L’adresse IP statique est recommandée si vous gérez le DNS. Vous pouvez utiliser Azure DNS et à la place dans les enregistrements DNS de votre domaine, reportez-vous aux nouveaux ordinateurs par leur FQDN Azure.
Le volet de votre machine virtuelle doit ressembler à l’exemple ci-dessous une fois le déploiement terminé :

![Machines virtuelles déployées](./media/how-to-connect-fed-azure-adfs/virtualmachinesdeployed_noadfs.png)

### <a name="5-configuring-the-domain-controller--ad-fs-servers"></a>5. Configuration du contrôleur de domaine/des serveurs de AD FS
 Pour authentifier une demande entrante, AD FS devrez contacter le contrôleur de domaine. Pour enregistrer le trajet coûteux d’Azure vers le contrôleur de domaine local pour l’authentification, il est recommandé de déployer un réplica du contrôleur de domaine dans Azure. Pour atteindre une haute disponibilité, il est recommandé de créer un groupe à haute disponibilité d’au moins deux contrôleurs de domaine.

| Contrôleur de domaine | Role | Compte de stockage |
|:---:|:---:|:---:|
| nommé contosodc1 |Réplica |contososac1 |
| contosodc2 |Réplica |contososac2 |

* Promouvoir les deux serveurs en tant que contrôleurs de domaine de réplica avec DNS
* Configurez les serveurs AD FS en installant le rôle AD FS à l’aide du gestionnaire de serveur.

### <a name="6-deploying-internal-load-balancer-ilb"></a>6. déploiement d’Load Balancer internes (ILB)
**6,1. création du ILB**

Pour déployer un ILB, sélectionnez équilibrages de charge dans le Portail Azure, puis cliquez sur Ajouter (+).

> [!NOTE]
> Si vous ne voyez pas les **équilibrages de charge** dans votre menu, cliquez sur **Parcourir** dans la partie inférieure gauche du portail et faites défiler jusqu’à ce que vous voyez **équilibreurs de charge**.  Cliquez ensuite sur l’étoile jaune pour l’ajouter à votre menu. Sélectionnez maintenant l’icône nouvel équilibreur de charge pour ouvrir le panneau afin de commencer la configuration de l’équilibreur de charge.
> 
> 

![Parcourir l’équilibreur de charge](./media/how-to-connect-fed-azure-adfs/browseloadbalancer.png)

* **Nom**: attribuez un nom approprié à l’équilibreur de charge
* **Schéma**: dans la mesure où cet équilibreur de charge sera placé devant les serveurs AD FS et est destiné uniquement aux connexions réseau internes, sélectionnez « interne ».
* **Réseau virtuel**: choisissez le réseau virtuel sur lequel vous déployez votre AD FS
* **Sous-réseau**: choisissez le sous-réseau interne ici
* **Attribution d’adresse IP**: statique

![Équilibreur de charge interne](./media/how-to-connect-fed-azure-adfs/ilbdeployment1.png)

Une fois que vous avez cliqué sur créer et que le ILB est déployé, vous devez le voir dans la liste des équilibrages de charge :

![Équilibreurs de charge après ILB](./media/how-to-connect-fed-azure-adfs/ilbdeployment2.png)

L’étape suivante consiste à configurer le pool principal et la sonde backend.

**6,2. configurer le pool backend ILB**

Sélectionnez le ILB nouvellement créé dans le panneau équilibrages de charge. Le panneau paramètres s’ouvre. 

1. Sélectionner des pools principaux dans le panneau Paramètres
2. Dans le panneau ajouter un pool principal, cliquez sur Ajouter une machine virtuelle.
3. Un panneau s’affiche, dans lequel vous pouvez choisir un groupe à haute disponibilité.
4. Choisir le groupe à haute disponibilité AD FS

![Configurer le pool backend ILB](./media/how-to-connect-fed-azure-adfs/ilbdeployment3.png)

**6,3. Configuration de la sonde**

Dans le panneau Paramètres ILB, sélectionnez sondes d’intégrité.

1. Cliquez sur Ajouter.
2. Fournissez les détails de la sonde a. **Nom**: nom de la sonde b. **Protocole**: http c. **Port**: 80 (http) d. **Chemin d’accès**:/ADFS/Probe e. **Intervalle**: 5 (valeur par défaut) : il s’agit de l’intervalle auquel ILB doit sonder les machines dans le pool principal f. **Limite de seuils non intègres**: 2 (valeur par défaut) : il s’agit du seuil d’échecs de sonde consécutifs après lequel ILB déclare un ordinateur dans le pool principal non réactif et cesse d’envoyer du trafic vers celui-ci.

![Configurer la sonde ILB](./media/how-to-connect-fed-azure-adfs/ilbdeployment4.png)

Nous utilisons le point de terminaison/ADFS/Probe qui a été créé de façon explicitement pour les contrôles d’intégrité dans un environnement AD FS dans lequel une vérification de chemin HTTPs complète ne peut pas se produire.  C’est beaucoup mieux qu’un contrôle de base du port 443, qui ne reflète pas précisément l’état d’un déploiement AD FS moderne.  Pour plus d’informations, consultez https://blogs.technet.microsoft.com/applicationproxyblog/2014/10/17/hardware-load-balancer-health-checks-and-web-application-proxy-ad-fs-2012-r2/.

**6,4. créer des règles d’équilibrage de charge**

Pour équilibrer efficacement le trafic, ILB doit être configuré avec des règles d’équilibrage de charge. Pour créer une règle d’équilibrage de charge, 

1. Sélectionnez règle d’équilibrage de charge dans le panneau Paramètres de la ILB
2. Cliquez sur Ajouter dans le panneau règle d’équilibrage de charge.
3. Dans le panneau ajouter une règle d’équilibrage de charge, a. **Nom**: attribuez un nom à la règle b. **Protocole**: sélectionnez TCP c. **Port**: 443 d. **Port principal**: 443 e. **Pool principal**: sélectionnez le pool que vous avez créé pour le cluster AD FS précédent f. **Sonde**: sélectionnez la sonde créée pour les serveurs AD FS précédemment

![Configurer les règles d’équilibrage ILB](./media/how-to-connect-fed-azure-adfs/ilbdeployment5.png)

**6,5. mettre à jour DNS avec ILB**

Accédez à votre serveur DNS et créez un enregistrement CNAME pour ILB. Le CNAMe doit être pour le service de Fédération avec l’adresse IP pointant vers l’adresse IP du ILB. Par exemple, si l’adresse DIP ILB est 10.3.0.8, et que le service de Fédération installé est fs.contoso.com, créez un enregistrement CNAME pour fs.contoso.com pointant vers 10.3.0.8.
Cela permet de s’assurer que toutes les communications relatives à fs.contoso.com finissent au ILB et sont routées de manière appropriée.

### <a name="7-configuring-the-web-application-proxy-server"></a>7. Configuration du serveur proxy d’application Web
**7,1. Configuration des serveurs proxy d’application Web pour qu’ils atteignent les serveurs AD FS**

Afin de s’assurer que les serveurs proxy d’application Web sont en mesure d’atteindre les serveurs AD FS derrière le ILB, créez un enregistrement dans le%SystemRoot%\system32\drivers\etc\hosts pour ILB. Notez que le nom unique (DN) doit être le nom du service de Fédération, par exemple fs.contoso.com. Et l’entrée IP doit être celle de l’adresse IP de ILB (10.3.0.8 comme dans l’exemple).

**7,2. installation du rôle de proxy d’application Web**

Après vous être assuré que les serveurs proxy d’application Web sont en mesure d’atteindre les serveurs AD FS derrière ILB, vous pouvez ensuite installer les serveurs proxy d’application Web. Les serveurs proxy d’application Web n’ont pas besoin d’être joints au domaine. Installez les rôles de proxy d’application Web sur les deux serveurs proxy d’application Web en sélectionnant le rôle accès à distance. Le gestionnaire de serveur vous guidera pour terminer l’installation du WAP.
Pour plus d’informations sur le déploiement du WAP, consultez [installer et configurer le serveur proxy d’application Web](https://technet.microsoft.com/library/dn383662.aspx).

### <a name="8--deploying-the-internet-facing-public-load-balancer"></a>8. déploiement du Load Balancer accessible sur Internet (public)
**8,1. créer un Load Balancer accessible sur Internet (public)**

Dans le Portail Azure, sélectionnez équilibrages de charge, puis cliquez sur Ajouter. Dans le panneau créer un équilibreur de charge, entrez les informations suivantes :

1. **Nom**: nom de l’équilibreur de charge
2. **Scheme**: public : cette option indique à Azure que cet équilibreur de charge a besoin d’une adresse publique.
3. **Adresse IP**: créer une nouvelle adresse IP (dynamique)

![Équilibreur de charge accessible sur Internet](./media/how-to-connect-fed-azure-adfs/elbdeployment1.png)

Après le déploiement, l’équilibreur de charge s’affiche dans la liste des équilibrages de charge.

![Liste d’équilibrage de charge](./media/how-to-connect-fed-azure-adfs/elbdeployment2.png)

**8,2. attribution d’un nom DNS à l’adresse IP publique**

Cliquez sur l’entrée d’équilibrage de charge nouvellement créée dans le panneau équilibreurs de charge pour afficher le panneau de configuration. Suivez les étapes ci-dessous pour configurer l’étiquette DNS pour l’adresse IP publique :

1. Cliquez sur l’adresse IP publique. Le panneau correspondant à l’adresse IP publique et à ses paramètres s’ouvre.
2. Cliquez sur Configuration
3. Indiquez une étiquette DNS. Il s’agit de l’étiquette DNS publique à laquelle vous pouvez accéder partout, par exemple contosofs.westus.cloudapp.azure.com. Vous pouvez ajouter une entrée dans le DNS externe pour le service de Fédération (par exemple, fs.contoso.com) qui correspond à l’étiquette DNS de l’équilibreur de charge externe (contosofs.westus.cloudapp.azure.com).

![Configurer l’équilibreur de charge accessible sur Internet](./media/how-to-connect-fed-azure-adfs/elbdeployment3.png) 

![Configurer l’équilibrage de charge accessible sur Internet (DNS)](./media/how-to-connect-fed-azure-adfs/elbdeployment4.png)

**8,3. configurer le pool principal pour une Load Balancer accessible sur Internet (public)** 

Suivez les mêmes étapes que pour la création de l’équilibreur de charge interne, afin de configurer le pool principal pour le Load Balancer accessible sur Internet (public) en tant que groupe à haute disponibilité pour les serveurs WAP. Par exemple, contosowapset.

![Configurer le pool principal de Load Balancer accessibles sur Internet](./media/how-to-connect-fed-azure-adfs/elbdeployment5.png)

**8,4. Configuration de la sonde**

Suivez les mêmes étapes que dans Configuration de l’équilibreur de charge interne pour configurer la sonde pour le pool principal de serveurs WAP.

![Configurer la sonde de l’accès à Internet Load Balancer](./media/how-to-connect-fed-azure-adfs/elbdeployment6.png)

**8,5. création d’une ou plusieurs règles d’équilibrage de charge**

Suivez les mêmes étapes que dans ILB pour configurer la règle d’équilibrage de charge pour TCP 443.

![Configurer les règles d’équilibrage de l’accès à Internet Load Balancer](./media/how-to-connect-fed-azure-adfs/elbdeployment7.png)

### <a name="9-securing-the-network"></a>9. sécurisation du réseau
**9,1. sécurisation du sous-réseau interne**

En général, vous avez besoin des règles suivantes pour sécuriser efficacement votre sous-réseau interne (dans l’ordre indiqué ci-dessous).

| Règle | Description | Flux |
|:--- |:--- |:---:|
| AllowHTTPSFromDMZ |Autoriser la communication HTTPs à partir de la zone DMZ |Entrant |
| DenyInternetOutbound |Aucun accès à Internet |Sortant |

![Règles d’accès INT (entrant)](./media/how-to-connect-fed-azure-adfs/nsg_int.png)

**9,2. sécurisation du sous-réseau DMZ**

| Règle | Description | Flux |
|:--- |:--- |:---:|
| AllowHTTPSFromInternet |Autoriser le protocole HTTPs à partir d’Internet vers la zone DMZ |Entrant |
| DenyInternetOutbound |Tout sauf le protocole HTTPs sur Internet est bloqué |Sortant |

![Règles d’accès EXT (entrantes)](./media/how-to-connect-fed-azure-adfs/nsg_dmz.png)

> [!NOTE]
> Si l’authentification par certificat utilisateur client (authentification clientTLS à l’aide de certificats utilisateur x509) est requise, AD FS nécessite l’activation du port TCP 49443 pour l’accès entrant.
> 
> 

### <a name="10-test-the-ad-fs-sign-in"></a>10. tester la connexion AD FS
Le moyen le plus simple consiste à tester AD FS à l’aide de la page IdpInitiatedSignon. aspx. Pour pouvoir effectuer cette opération, vous devez activer IdpInitiatedSignOn sur les propriétés de AD FS. Suivez les étapes ci-dessous pour vérifier la configuration de votre AD FS

1. Exécutez l’applet de commande ci-dessous sur le serveur AD FS, à l’aide de PowerShell, pour lui affecter la valeur activé.
   Set-AdfsProperties-EnableIdPInitiatedSignonPage $true 
2. À partir de n’importe quel ordinateur externe, accédez à https :\//adfs-server.contoso.com/adfs/ls/IdpInitiatedSignon.aspx.  
3. Vous devez voir la page AD FS comme ci-dessous :

![Page tester la connexion](./media/how-to-connect-fed-azure-adfs/test1.png)

Une fois la connexion établie, un message de réussite s’affiche, comme indiqué ci-dessous :

![Réussite du test](./media/how-to-connect-fed-azure-adfs/test2.png)

## <a name="template-for-deploying-ad-fs-in-azure"></a>Modèle de déploiement d’AD FS dans Azure
Le modèle déploie une configuration de 6 ordinateurs, 2 pour les contrôleurs de domaine, AD FS et WAP.

[AD FS dans le modèle de déploiement Azure](https://github.com/paulomarquesc/adfs-6vms-regular-template-based)

Vous pouvez utiliser un réseau virtuel existant ou créer un nouveau réseau virtuel lors du déploiement de ce modèle. Les différents paramètres disponibles pour personnaliser le déploiement sont répertoriés ci-dessous, avec la description de l’utilisation du paramètre dans le processus de déploiement. 

| Paramètre | Description |
|:--- |:--- |
| Location |Région dans laquelle déployer les ressources, par exemple est des États-Unis. |
| StorageAccountType |Type de compte de stockage créé |
| VirtualNetworkUsage |Indique si un nouveau réseau virtuel est créé ou s’il utilise un réseau existant |
| VirtualNetworkName |Nom du réseau virtuel à créer, obligatoire à la fois pour l’utilisation d’un réseau virtuel existant ou nouveau |
| VirtualNetworkResourceGroupName |Spécifie le nom du groupe de ressources dans lequel se trouve le réseau virtuel existant. Lorsque vous utilisez un réseau virtuel existant, ce paramètre devient obligatoire afin que le déploiement puisse trouver l’ID du réseau virtuel existant. |
| VirtualNetworkAddressRange |Plage d’adresses du nouveau réseau virtuel, obligatoire si vous créez un nouveau réseau virtuel |
| InternalSubnetName |Nom du sous-réseau interne, obligatoire sur les deux options d’utilisation de réseau virtuel (nouveau ou existant) |
| InternalSubnetAddressRange |La plage d’adresses du sous-réseau interne, qui contient les contrôleurs de domaine et les serveurs ADFS, obligatoire si vous créez un nouveau réseau virtuel. |
| DMZSubnetAddressRange |Plage d’adresses du sous-réseau DMZ, qui contient les serveurs proxy d’application Windows, obligatoire si vous créez un nouveau réseau virtuel. |
| DMZSubnetName |Nom du sous-réseau interne, obligatoire sur les deux options d’utilisation de réseau virtuel (nouveau ou existant). |
| ADDC01NICIPAddress |L’adresse IP interne du premier contrôleur de domaine, cette adresse IP est affectée de manière statique au contrôleur de domaine et doit être une adresse IP valide au sein du sous-réseau interne |
| ADDC02NICIPAddress |L’adresse IP interne du second contrôleur de domaine, cette adresse IP est affectée de manière statique au contrôleur de domaine et doit être une adresse IP valide au sein du sous-réseau interne |
| ADFS01NICIPAddress |L’adresse IP interne du premier serveur ADFS, cette adresse IP est affectée de manière statique au serveur ADFS et doit être une adresse IP valide au sein du sous-réseau interne |
| ADFS02NICIPAddress |L’adresse IP interne du second serveur ADFS, cette adresse IP est affectée de manière statique au serveur ADFS et doit être une adresse IP valide au sein du sous-réseau interne |
| WAP01NICIPAddress |L’adresse IP interne du premier serveur WAP, cette adresse IP est affectée de manière statique au serveur WAP et doit être une adresse IP valide au sein du sous-réseau DMZ |
| WAP02NICIPAddress |L’adresse IP interne du second serveur WAP, cette adresse IP est affectée de manière statique au serveur WAP et doit être une adresse IP valide au sein du sous-réseau DMZ |
| ADFSLoadBalancerPrivateIPAddress |L’adresse IP interne de l’équilibreur de charge ADFS, cette adresse IP est affectée de manière statique à l’équilibreur de charge et doit être une adresse IP valide au sein du sous-réseau interne |
| ADDCVMNamePrefix |Préfixe du nom de machine virtuelle pour les contrôleurs de domaine |
| ADFSVMNamePrefix |Préfixe du nom de machine virtuelle pour les serveurs ADFS |
| WAPVMNamePrefix |Préfixe du nom de machine virtuelle pour les serveurs WAP |
| ADDCVMSize |Taille de machine virtuelle des contrôleurs de domaine |
| ADFSVMSize |Taille de machine virtuelle des serveurs ADFS |
| WAPVMSize |Taille de machine virtuelle des serveurs WAP |
| AdminUserName |Nom de l’administrateur local des machines virtuelles |
| AdminPassword |Mot de passe du compte d’administrateur local des machines virtuelles |

## <a name="additional-resources"></a>Ressources supplémentaires
* [Groupes à haute disponibilité](https://aka.ms/Azure/Availability) 
* [Azure Load Balancer](https://aka.ms/Azure/ILB)
* [Load Balancer interne](https://aka.ms/Azure/ILB/Internal)
* [Load Balancer accessible sur Internet](https://aka.ms/Azure/ILB/Internet)
* [Comptes de stockage](https://aka.ms/Azure/Storage)
* [Réseaux virtuels Azure](https://aka.ms/Azure/VNet)
* [AD FS et liens de proxy d’application Web](https://aka.ms/ADFSLinks) 

## <a name="next-steps"></a>Étapes suivantes :
* [Intégration de vos identités locales avec Azure Active Directory](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-hybrid-identity)
* [Configuration et gestion de votre AD FS à l’aide de Azure AD Connect](/azure/active-directory/hybrid/how-to-connect-fed-whatis)
* [Déploiement des AD FS inter-géographiques à haute disponibilité dans Azure avec Azure Traffic Manager](active-directory-adfs-in-azure-with-azure-traffic-manager.md)
