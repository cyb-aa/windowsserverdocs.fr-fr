---
title: Toujours sur la vue d’ensemble de la technologie VPN
description: 'Cette page propose une vue d’ensemble des technologies VPN Always On avec des liens vers d’autres documents détaillés. '
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: fd3f7c6ca8555e270aabf04bbee6800ed284080c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821260"
---
# <a name="always-on-vpn-technology-overview"></a>Vue d’ensemble de la technologie VPN Always On
>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

&#171;  [**Précédent :** En savoir plus sur les améliorations de VPN Always On](always-on-vpn-enhancements.md)<br>
&#187;  [**prochain :** En savoir plus sur les fonctionnalités avancées de VPN Always On](deploy/always-on-vpn-adv-options.md)

Pour ce déploiement, vous devez installer un nouveau serveur d’accès à distance qui exécute Windows Server 2016, mais aussi modifier une partie de votre infrastructure existante pour le déploiement.

L’illustration suivante montre l’infrastructure qui est nécessaire au déploiement VPN Always On.

![Toujours sur l’Infrastructure VPN](../../../media/Always-On-Vpn/Ao-Vpn-Overview.jpg)

Le processus de connexion décrit dans cette illustration se compose des étapes suivantes :

1. Serveurs DNS publics, le client VPN Windows 10 effectue une requête de résolution de nom pour l’adresse IP de la passerelle VPN.

2. À l’aide de l’adresse IP renvoyée par le système DNS, le client VPN envoie une demande de connexion à la passerelle VPN.

3. La passerelle VPN est également configurée comme un Remote Authentication Dial-In User Service \(RADIUS\) Client ; le Client RADIUS VPN envoie la demande de connexion au serveur NPS organisation / d’entreprise pour le traitement de demande de connexion.

4. Le serveur NPS traite la demande de connexion, notamment l’autorisation et l’authentification et détermine s’il faut autoriser ou refuser la demande de connexion.

5. Le serveur NPS transmet une réponse d’acceptation d’accès ou de refus d’accès à la passerelle VPN.

6. La connexion est démarre ou se termine en fonction de la réponse reçu par le serveur VPN à partir du serveur NPS.

Pour plus d’informations sur chaque composant d’infrastructure décrit dans l’illustration ci-dessus, consultez les sections suivantes.

>[!NOTE]
>Si vous avez déjà certaines de ces technologies déployés sur votre réseau, vous pouvez utiliser les instructions fournies dans ce guide de déploiement pour effectuer une configuration supplémentaire des technologies à cet effet de déploiement.

## <a name="domain-name-system-dns"></a>DNS (Domain Name System)

Les zones système DNS (Domain Name) internes et externes sont nécessaires, ce qui suppose que la zone interne est un sous-domaine délégué de la zone externe (par exemple, corp.contoso.com et contoso.com).

En savoir plus sur [système DNS (Domain Name)](../../../../networking/dns/dns-top.md) ou [Guide du réseau](../../../../networking/core-network-guide/core-network-guide.md).




>[!NOTE] 
>Autres DNS conçoit, tels que DNS « split brain » (en utilisant le même nom de domaine interne et externe dans les zones DNS distinctes) ou non liée interne et des domaines externes (par exemple, contoso.local et contoso.com) sont également possibles. Pour plus d’informations sur le déploiement de DNS « split brain », consultez [utiliser une stratégie DNS pour le déploiement de DNS SplitBrain/](../../../../networking/dns/deploy/split-brain-DNS-deployment.md).

## <a name="firewalls"></a>Pare-feu

Assurez-vous que votre pare-feu autorise le trafic qui est nécessaire pour les communications de VPN et RADIUS fonctionner correctement.

Pour plus d’informations, consultez [configurer le pare-feu pour le trafic RADIUS](../../../../networking/technologies/nps/nps-firewalls-configure.md).

## <a name="remote-access-as-a-ras-gateway-vpn-server"></a>Accès à distance comme un serveur VPN de passerelle RAS

Dans Windows Server 2016, le rôle de serveur d’accès à distance est conçu pour effectuer ainsi qu’un routeur et un serveur d’accès à distance ; Par conséquent, il prend en charge un large éventail de fonctionnalités. Pour ce guide de déploiement, vous avez besoin uniquement un petit sous-ensemble de ces fonctionnalités : prise en charge des connexions VPN IKEv2 et le routage de réseau local.

IKEv2 est décrite dans la Internet Engineering Task Force demande de commentaires 7296 de protocole de tunnel VPN. Le principal avantage de IKEv2 est qu’il tolère les interruptions de la connexion réseau sous-jacente. Par exemple, si la connexion est perdue temporairement ou si un utilisateur déplace un ordinateur client à partir d’un réseau à un autre, IKEv2 restaure automatiquement la connexion VPN lorsque la connexion réseau est rétablie, tout cela sans intervention de l’utilisateur.

À l’aide de passerelle RAS, vous pouvez déployer des connexions VPN pour fournir aux utilisateurs finaux un accès à distance au réseau et les ressources de votre organisation. Déploiement de VPN Always On tient à jour une connexion persistante entre les clients et le réseau de votre organisation chaque fois que les ordinateurs distants sont connectés à Internet. Avec cette passerelle, vous pouvez également créer une connexion VPN de site à site entre deux serveurs situés à différents emplacements, comme entre votre bureau principal et une filiale et utiliser la traduction d’adresses réseau \(NAT\) afin que les utilisateurs à l’intérieur de la réseau permettre accéder à des ressources externes, tels qu’Internet. En outre, passerelle RAS prend en charge le protocole BGP (Border Gateway), qui fournit des services de routage dynamiques lorsque vos sites distants ont également des passerelles de périmètre qui prennent en charge le protocole BGP.

Vous pouvez gérer les passerelles d’accès à distance (RAS) à l’aide des commandes Windows PowerShell et la distant accès Console (MMC).



## <a name="network-policy-server-nps"></a>Serveur NPS (Network Policy Server)

NPS vous permet de créer et appliquer des stratégies d’accès réseau de l’entreprise pour l’authentification de demande de connexion et l’autorisation. Lorsque vous utilisez NPS en tant qu’un serveur d’authentification Dial-In Service RADIUS (Remote User), vous configurez les serveurs d’accès réseau, tels que les serveurs VPN, en tant que clients RADIUS dans NPS.

Vous devez également configurer les stratégies réseau utilisées par NPS pour autoriser les demandes de connexion. Vous pouvez éventuellement configurer la gestion de comptes RADIUS de sorte que NPS enregistre les informations de gestion de comptes dans des fichiers journaux sur le disque dur local ou dans une base de données Microsoft SQL Server.

Pour plus d’informations, consultez [serveur NPS (Network Policy Server)](../../../../networking/technologies/nps/nps-top.md).


## <a name="active-directory-certificate-services"></a>Services de certificats Active Directory

Le serveur de l’autorité de Certification (CA) est une autorité de certification qui exécute les Services de certificat Active Directory. La configuration de VPN nécessite une basée sur Active Directory infrastructure à clé publique (PKI).

Les organisations peuvent utiliser AD CS pour améliorer la sécurité en liant l’identité d’une personne, un appareil ou un service à une clé publique correspondante. Les services AD CS comprend également des fonctionnalités qui vous permettent de gérer l’inscription de certificats et la révocation d’une variété d’environnements évolutifs. Pour plus d’informations, consultez [présentation des Services de certificats Active Directory](https://technet.microsoft.com/library/hh831740.aspx) et [Guide de conception d’Infrastructure de clé publique](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx).

À la fin du déploiement, vous allez configurer les modèles de certificat suivants sur l’autorité de certification.

-   Le modèle de certificat authentification de l’utilisateur

-   Le modèle de certificat d’authentification du serveur VPN

-   Le modèle de certificat d’authentification du serveur NPS

### <a name="certificate-templates"></a>Modèles de certificats

Modèles de certificats peuvent considérablement simplifier la tâche d’administration d’une autorité de certification (CA) en vous permettant d’émettre des certificats qui sont préconfigurés pour les tâches sélectionnées. Le composant logiciel enfichable MMC des modèles de certificat permet de vous permettent d’effectuer les tâches suivantes.

-   Afficher les propriétés de chaque modèle de certificat.

-   Copier et modifier des modèles de certificats.

-   Contrôler les utilisateurs et ordinateurs peuvent lire des modèles et s’inscrire aux certificats.

-   Effectuer d’autres tâches d’administration relatives aux modèles de certificats.

Modèles de certificats font partie intégrante d’une autorité de certification d’entreprise (CA). Ils sont un élément important de la stratégie de certificat pour un environnement, qui est l’ensemble de règles et des formats pour l’inscription de certificats, l’utilisation et gestion.

Pour plus d’informations, consultez [modèles de certificats](https://technet.microsoft.com/library/cc730705.aspx).

### <a name="digital-server-certificates"></a>Certificats de serveur numériques

Ce guide de déploiement fournit des instructions sur l’utilisation des Services de certificats Active Directory (AD CS) pour les inscrire et inscrire automatiquement des certificats pour l’accès à distance et les serveurs d’infrastructure de serveur NPS. Les services AD CS vous permet de générer une infrastructure à clé publique (PKI) et de fournir un chiffrement à clé publique, les certificats numériques et les fonctions de signature numérique pour votre organisation.

Lorsque vous utilisez des certificats de serveur numériques pour l’authentification entre les ordinateurs de votre réseau, les certificats fournissent :

1.  Confidentialité grâce au cryptage.

2.  Intégrité via des signatures numériques.

3.  Authentification en associant des clés de certificat avec un compte utilisateur, ordinateur ou un périphérique sur un réseau d’ordinateurs.

Pour plus d’informations, consultez [AD CS étape Guide pas à pas : À deux niveaux d’un déploiement de la hiérarchie PKI](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx).

## <a name="active-directory-domain-services-ad-ds"></a>Services de domaine Active Directory (AD DS)

Les services AD DS fournissent une base de données distribuée qui stocke et gère des informations sur les ressources réseau et les données spécifiques à des applications provenant d’applications utilisant un annuaire. Les administrateurs peuvent utiliser AD DS pour organiser les éléments d’un réseau, tels que les utilisateurs, les ordinateurs et les autres périphériques, en une structure hiérarchique de type contenant-contenu. La structure hiérarchique de type contenant-contenu inclut la forêt Active Directory, les domaines inclus dans la forêt et les unités d’organisation de chaque domaine. Un serveur qui exécute les services AD DS est appelé contrôleur de domaine.

Services AD DS contiennent les comptes d’utilisateur, les comptes d’ordinateurs et les propriétés du compte qui sont requises par Extensible authentification PEAP (Protected Protocol) pour authentifier les informations d’identification utilisateur et pour évaluer d’autorisation des demandes de connexion VPN. Pour plus d’informations sur le déploiement des services AD DS, consultez Windows Server 2016 [Guide du réseau](../../../../networking/core-network-guide/Core-Network-Guide.md).



À la fin de la procédure décrite dans ce déploiement, vous allez configurer les éléments suivants sur le contrôleur de domaine.

-   Activer l’inscription automatique de certificat dans la stratégie de groupe pour les ordinateurs et utilisateurs

-   Créer le groupe d’utilisateurs VPN

-   Créer le groupe de serveurs VPN

-   Créer le groupe de serveurs NPS

### <a name="active-directory-users-and-computers"></a>Utilisateurs et ordinateurs Active Directory

Active Directory Users and Computers est un composant des services AD DS qui contient les comptes qui représentent des entités physiques, comme un ordinateur, une personne ou un groupe de sécurité. Un groupe de sécurité est une collection de comptes d’utilisateur ou un ordinateur qui les administrateurs peuvent gérer comme une seule unité. Comptes d’utilisateur et ordinateur qui appartiennent à un groupe particulier sont appelées membres du groupe.

Comptes d’utilisateur dans Active Directory Users and Computers ont des propriétés dans l’accès à NPS évalue pendant le processus d’autorisation -, sauf si le **l’autorisation d’accès réseau** propriété du compte d’utilisateur est définie sur **contrôle accès via la stratégie de réseau de NPS**. Il s’agit du paramètre par défaut pour tous les comptes d’utilisateur. Dans certains cas, toutefois, ce paramètre peut-être avoir une configuration différente qui empêche l’utilisateur de se connecter à l’aide de VPN. Pour vous protéger contre cette éventualité, vous pouvez configurer le serveur NPS afin d’ignorer les propriétés d’accès à distance du compte d’utilisateur.

Pour plus d’informations, consultez [configurer NPS aux propriétés d’ignorer le compte utilisateur](../../../../networking/technologies/nps/nps-np-configure.md#configure-nps-to-ignore-user-account-dial-in-properties).



### <a name="group-policy-management"></a>Gestion des stratégies de groupe

Gestion des stratégies de groupe permet de gestion des modifications et la configuration basée sur le répertoire des paramètres utilisateur et ordinateur, y compris les informations de sécurité et d’utilisateur. Stratégie de groupe vous permet de définir des configurations pour des groupes d’utilisateurs et ordinateurs.

Avec la stratégie de groupe, vous pouvez spécifier des paramètres pour les entrées de Registre, sécurité, installation de logiciels, des scripts, la redirection de dossiers, les services d’installation à distance et maintenance de Internet Explorer. Les paramètres de stratégie de groupe que vous créez sont contenus dans un objet de stratégie de groupe (GPO). En associant un objet de stratégie de groupe à des conteneurs de système Active Directory sélectionnés, sites, domaines et unités d’organisation, vous pouvez appliquer des paramètres de l’objet de stratégie de groupe aux utilisateurs et ordinateurs dans ces conteneurs Active Directory. Pour gérer les objets de stratégie de groupe dans une entreprise, vous pouvez utiliser le groupe de stratégie de gestion éditeur Console MMC (Microsoft Management).


## <a name="windows-10-vpn-clients"></a>Clients VPN Windows 10

Outre les composants serveur, assurez-vous que les ordinateurs clients que vous configurez pour utiliser des VPN sont en cours d’exécution à jour anniversaire Windows 10 (version 1607). Les clients VPN Windows 10 doivent être joints au domaine à votre domaine Active Directory.


Le client VPN Windows 10 est hautement configurable et offre de nombreuses options. Pour mieux illustrer ce scénario utilise des fonctionnalités spécifiques, le tableau 1 identifie les catégories de fonctionnalités VPN et les configurations spécifiques qui fait référence à ce déploiement. Vous allez configurer les paramètres individuels pour ces fonctionnalités en utilisant le fournisseur de services de configuration (CSP) VPNv2 décrits plus loin dans ce déploiement. 

Tableau 1. Fonctionnalités VPN et les Configurations décrites dans ce déploiement

| **Fonctionnalité VPN** | **Configuration de scénario de déploiement**         |
|-----------------|-----------------------------------------------|
| Type de connexion | IKEv2 natif                                  |
| Routage         | Le tunneling fractionné                               |
| Résolution de noms | Liste d’informations de nom de domaine et le suffixe DNS   |
| Déclenchement      | Détection de réseau Always On et approuvé       |
| Authentification  | PEAP-TLS avec des certificats utilisateur protégé par le module de plateforme sécurisée |
---

>[!NOTE] 
>PEAP-TLS et le module de plateforme sécurisée sont « Protected Extensible Authentication Protocol avec Transport Layer Security » et « Module de plateforme sécurisée », respectivement.

### <a name="vpnv2-csp-nodes"></a>VPNv2 CSP nœuds

Dans ce déploiement, vous utilisez le nœud ProfileXML VPNv2 CSP pour créer le profil VPN qui est remis aux ordinateurs clients Windows 10. Fournisseurs de services de configuration (CSP) sont des interfaces qui exposent les différentes fonctionnalités de gestion dans le client Windows ; Conceptuellement, fournisseurs de services cryptographiques travail similaire au fonctionnement de la stratégie de groupe. Chaque fournisseur CSP a des nœuds qui représentent les paramètres individuels de la configuration. Également comme paramètres de stratégie de groupe, vous pouvez lier des paramètres CSP aux clés de Registre, des fichiers, des autorisations et ainsi de suite. Similaire à l’utilisation de l’éditeur de gestion de stratégie de groupe pour configurer les objets de stratégie de groupe (GPO), vous configurer les nœuds CSP à l’aide d’une solution de gestion (MDM) des appareils mobiles comme Microsoft Intune. Produits de gestion des appareils mobiles comme Intune offrent une option de configuration convivial qui configure le CSP dans le système d’exploitation.

![Gestion des appareils mobiles pour la configuration du fournisseur CSP](../../../media/Always-On-Vpn/Vpn-Mdm.jpg)

Toutefois, vous ne pouvez pas configurer certains nœuds CSP directement par le biais d’une interface utilisateur (IU) comme la Console d’administration Intune. Dans ce cas, vous devez configurer les paramètres de Open Mobile Alliance Uniform Resource Identifier (OMA-URI) manuellement. Vous configurez OMA-URI en utilisant le protocole de gestion des appareils OMA (OMA-DM), une spécification de gestion de dispositif universel qui prennent en charge de la plupart des appareils Apple, Android et Windows. Tant qu’elles respectent la spécification OMA-DM, tous les produits de gestion des appareils mobiles doivent interagir avec ces systèmes d’exploitation de la même façon.

Windows 10 offre de nombreux fournisseurs de services cloud, mais ce déploiement se concentre sur l’utilisation de la VPNv2 CSP pour configurer le client VPN. Le VPNv2 CSP permet de configurer chaque paramètre de profil VPN dans Windows 10 via un unique nœud CSP. Également contenue dans le VPNv2 CSP est un nœud appelé *ProfileXML*, qui vous permet de configurer tous les paramètres dans un seul nœud plutôt qu’individuellement. Pour plus d’informations sur ProfileXML, consultez la section « Vue d’ensemble ProfileXML » plus loin dans ce déploiement. Pour plus d’informations sur chaque nœud VPNv2 CSP, consultez le [VPNv2 CSP](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-csp).



## <a name="next-steps"></a>Étapes suivantes

- [En savoir plus sur certaines des fonctionnalités avancées VPN Always On](deploy/always-on-vpn-adv-options.md)

- [Commencer à planifier votre déploiement VPN Always On](deploy/always-on-vpn-deploy-deployment.md)


---

## <a name="related-topics"></a>Rubriques connexes
- [Prise en charge du logiciel Microsoft server pour machines virtuelles Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines): Cet article décrit la politique de support pour l’exécution de logiciels serveur Microsoft dans l’environnement de machine virtuelle Microsoft Azure (infrastructure-as-a-service).

- [Accès à distance](../../Remote-Access.md): Cette rubrique fournit une vue d’ensemble du rôle de serveur d’accès à distance dans Windows Server 2016.

- [Guide technique de Windows 10 VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-guide): Ce guide pas à pas vous explique les décisions à que prendre pour les clients Windows 10 dans votre solution VPN d’entreprise et comment configurer votre déploiement. Ce guide fait référence le fournisseur de services de Configuration (CSP) VPNv2 et fournit des instructions de configuration (MDM) à l’aide de Microsoft Intune et le modèle de profil VPN pour Windows 10 de gestion des appareils mobiles.

- [Guide du réseau](../../../../networking/core-network-guide/Core-Network-Guide.md): Ce guide fournit des instructions sur la façon de planifier et déployer les composants centraux requis pour un réseau pleinement fonctionnel et un nouveau domaine Active Directory dans une nouvelle forêt.

- [Domain Name System (DNS)](../../../../networking/dns/dns-top.md): Cette rubrique fournit une vue d’ensemble de systèmes DNS (Domain Name). Dans Windows Server 2016, DNS est un rôle de serveur que vous pouvez installer à l’aide des commandes de gestionnaire de serveur ou Windows PowerShell. Si vous installez une nouvelle forêt Active Directory et le même domaine, DNS est automatiquement installé avec Active Directory que le serveur de Catalogue Global pour la forêt et le domaine. 

- [Vue d’ensemble de Services de certificats Active Directory](https://technet.microsoft.com/library/hh831740.aspx): Ce document fournit une vue d’ensemble des Services de certificats Active Directory (AD CS) dans Windows Server® 2012. Les services AD CS correspondent au rôle serveur qui vous permet de générer une infrastructure à clé publique (PKI) et de fournir un chiffrement à clé publique, des certificats numériques et des fonctions de signature numérique pour votre organisation.

- [Guide de conception d’Infrastructure à clé publique](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx):  Ce wiki fournit des conseils sur la conception d’Infrastructures à clé publique (PKI). Avant de configurer une hiérarchie d’autorité de certification PKI et de certification, vous devez connaître de votre organisation sécurité stratégie et certificat pratique la déclaration (CPS).

- [Guide détaillé de AD CS : À deux niveaux d’un déploiement de la hiérarchie PKI](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx): Ce guide pas à pas décrit les étapes nécessaires pour configurer une configuration de base des Services de certificats Active Directory® (AD CS) dans un environnement de laboratoire. Les services AD CS dans Windows Server® 2008 R2 fournit des services personnalisables pour créer et gérer des certificats de clé publiques utilisés dans les systèmes de sécurité logiciels employant des technologies de clé publique.

- [Network Policy Server (NPS)](../../../../networking/technologies/nps/nps-top.md): Cette rubrique fournit une vue d’ensemble du serveur de stratégie réseau dans Windows Server 2016. Un serveur NPS \(Network Policy Server\) vous permet de créer et d’appliquer des stratégies d’accès réseau valides pour toute l’organisation, qui régissent les demandes d’authentification et l’autorisation des demandes de connexion. 

---
