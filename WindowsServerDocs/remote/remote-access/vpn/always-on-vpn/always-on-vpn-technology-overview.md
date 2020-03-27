---
title: Présentation de la technologie VPN Always On
description: 'Cette page fournit un bref aperçu des technologies VPN de Always On avec des liens vers des documents détaillés. '
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.date: 11/05/2018
ms.author: lizross
author: eross-msft
ms.localizationpriority: medium
ms.openlocfilehash: a5d65dd9f8fb6328bd00be8a46af37ee69ccd2bf
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80313394"
---
# <a name="always-on-vpn-technology-overview"></a>Présentation de la technologie VPN Always On

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Précédent :** En savoir plus sur les améliorations apportées au VPN Always On](always-on-vpn-enhancements.md)
- [**Ensuite :** En savoir plus sur les fonctionnalités avancées de Always On VPN](deploy/always-on-vpn-adv-options.md)

Pour ce déploiement, vous devez installer un nouveau serveur d’accès à distance qui exécute Windows Server 2016 et modifier une partie de votre infrastructure existante pour le déploiement.

L’illustration suivante montre l’infrastructure requise pour déployer Always On VPN.

![Infrastructure VPN Always On](../../../media/Always-On-Vpn/Ao-Vpn-Overview.jpg)

Le processus de connexion décrit dans cette illustration est constitué des étapes suivantes :

1. À l’aide de serveurs DNS publics, le client VPN Windows 10 effectue une requête de résolution de noms pour l’adresse IP de la passerelle VPN.

2. À l’aide de l’adresse IP renvoyée par DNS, le client VPN envoie une demande de connexion à la passerelle VPN.

3. La passerelle VPN est également configurée comme un client protocole RADIUS (Remote Authentication Dial-In User Service) (RADIUS). le client RADIUS VPN envoie la demande de connexion au serveur NPS de l’organisation/de l’entreprise pour le traitement des demandes de connexion.

4. Le serveur NPS traite la demande de connexion, y compris l’autorisation et l’authentification, et détermine s’il faut autoriser ou refuser la demande de connexion.

5. Le serveur NPS transmet une réponse d’accès/acceptation ou de refus d’accès à la passerelle VPN.

6. La connexion est établie ou arrêtée en fonction de la réponse que le serveur VPN a reçue du serveur NPS.

Pour plus d’informations sur chaque composant d’infrastructure décrit dans l’illustration ci-dessus, consultez les sections suivantes.

>[!NOTE]
>Si certaines de ces technologies sont déjà déployées sur votre réseau, vous pouvez utiliser les instructions de ce guide de déploiement pour effectuer une configuration supplémentaire des technologies pour cet objectif de déploiement.

## <a name="domain-name-system-dns"></a>DNS (Domain Name System)

Les zones DNS (Domain Name System) internes et externes sont requises, ce qui suppose que la zone interne est un sous-domaine délégué de la zone externe (par exemple, corp.contoso.com et contoso.com).

En savoir plus sur le [système DNS (Domain Name System)](../../../../networking/dns/dns-top.md) ou le [Guide du réseau de base](../../../../networking/core-network-guide/core-network-guide.md).

>[!NOTE]
>D’autres conceptions DNS, telles que le DNS split-brain (utilisant le même nom de domaine en interne et en externe dans des zones DNS distinctes) ou des domaines internes et externes non liés (par exemple, contoso. local et contoso.com) sont également possibles. Pour plus d’informations sur le déploiement du DNS split-brain, consultez [utiliser une stratégie DNS pour le déploiement DNS avec fractionnement Brain](../../../../networking/dns/deploy/split-brain-DNS-deployment.md).

## <a name="firewalls"></a>Pare-feu

Assurez-vous que vos pare-feu autorisent le trafic qui est nécessaire pour que les communications VPN et RADIUS fonctionnent correctement.

Pour plus d’informations, consultez [configurer des pare-feu pour le trafic RADIUS](../../../../networking/technologies/nps/nps-firewalls-configure.md).

## <a name="remote-access-as-a-ras-gateway-vpn-server"></a>Accès à distance en tant que serveur VPN de passerelle RAS

Dans Windows Server 2016, le rôle serveur d’accès à distance est conçu pour fonctionner correctement à la fois comme un routeur et un serveur d’accès à distance. par conséquent, il prend en charge un vaste éventail de fonctionnalités. Pour ce guide de déploiement, vous n’avez besoin que d’un petit sous-ensemble de ces fonctionnalités : la prise en charge des connexions VPN IKEv2 et du routage LAN.

IKEv2 est un protocole de tunneling VPN décrit dans Internet Engineering Task Force Request for Comments 7296. Le principal avantage de IKEv2 est qu’il tolère les interruptions de la connexion réseau sous-jacente. Par exemple, si la connexion est temporairement perdue ou si un utilisateur déplace un ordinateur client d’un réseau vers un autre, IKEv2 restaure automatiquement la connexion VPN lorsque la connexion réseau est rétablie, le tout sans intervention de l’utilisateur.

À l’aide de la passerelle RAS, vous pouvez déployer des connexions VPN pour fournir aux utilisateurs finaux un accès à distance au réseau et aux ressources de votre organisation. Le déploiement d’Always On VPN maintient une connexion permanente entre les clients et le réseau de votre organisation chaque fois que des ordinateurs distants sont connectés à Internet. Avec la passerelle RAS, vous pouvez également créer une connexion VPN de site à site entre deux serveurs situés à différents emplacements, par exemple entre votre bureau principal et une filiale, et utiliser la traduction d’adresses réseau (NAT) afin que les utilisateurs du réseau puissent accéder à l’extérieur. ressources, telles qu’Internet. En outre, la passerelle RAS prend en charge Border Gateway Protocol (BGP), qui fournit des services de routage dynamique lorsque vos sites distants disposent également de passerelles de périphérie qui prennent en charge le protocole BGP.

Vous pouvez gérer les passerelles du service d’accès à distance (RAS) à l’aide des commandes Windows PowerShell et de la console MMC (Microsoft Management Console) d’accès à distance.

## <a name="network-policy-server-nps"></a>Serveur NPS (Network Policy Server)

NPS vous permet de créer et d’appliquer des stratégies d’accès réseau à l’ensemble de l’Organisation pour l’authentification et l’autorisation des demandes de connexion. Lorsque vous utilisez NPS en tant que serveur protocole RADIUS (Remote Authentication Dial-In User Service) (RADIUS), vous configurez les serveurs d’accès réseau, tels que les serveurs VPN, en tant que clients RADIUS dans NPS.

Vous devez également configurer les stratégies réseau utilisées par NPS pour autoriser les demandes de connexion. Vous pouvez éventuellement configurer la gestion de comptes RADIUS de sorte que NPS enregistre les informations de gestion de comptes dans des fichiers journaux sur le disque dur local ou dans une base de données Microsoft SQL Server.

Pour plus d’informations, consultez [serveur NPS (Network Policy Server)](../../../../networking/technologies/nps/nps-top.md).

## <a name="active-directory-certificate-services"></a>Services de certificats Active Directory

Le serveur de l’autorité de certification (CA) est une autorité de certification qui exécute Active Directory les services de certificats. La configuration VPN requiert une infrastructure de clé publique (PKI) basée sur la Active Directory.

Les organisations peuvent utiliser les services AD CS pour améliorer la sécurité en liant l’identité d’une personne, d’un périphérique ou d’un service à une clé publique correspondante. Les services AD CS incluent également des fonctionnalités qui vous permettent de gérer l’inscription et la révocation de certificats dans divers environnements évolutifs. Pour plus d’informations, consultez [Active Directory vue d’ensemble des services de certificats](https://technet.microsoft.com/library/hh831740.aspx) et guide de conception de l’infrastructure à [clé publique](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx).

Au terme du déploiement, vous allez configurer les modèles de certificat suivants sur l’autorité de certification.

- Modèle de certificat d’authentification utilisateur

- Modèle de certificat d’authentification du serveur VPN

- Modèle de certificat d’authentification de serveur NPS

### <a name="certificate-templates"></a>Modèles de certificats

Les modèles de certificats peuvent simplifier de manière considérable la tâche d’administration d’une autorité de certification en vous permettant d’émettre des certificats préconfigurés pour les tâches sélectionnées. Le composant logiciel enfichable MMC modèles de certificats vous permet d’effectuer les tâches suivantes.

- Affichez les propriétés de chaque modèle de certificat.

- Copiez et modifiez les modèles de certificats.

- Contrôler les utilisateurs et les ordinateurs qui peuvent lire des modèles et s’inscrire pour obtenir des certificats.

- Effectuez d’autres tâches d’administration relatives aux modèles de certificats.

Les modèles de certificats font partie intégrante d’une autorité de certification d’entreprise. Il s’agit d’un élément important de la stratégie de certificat pour un environnement, qui est l’ensemble de règles et de formats pour l’inscription, l’utilisation et la gestion des certificats.

Pour plus d’informations, consultez [modèles de certificats](https://technet.microsoft.com/library/cc730705.aspx).

### <a name="digital-server-certificates"></a>Certificats de serveur numérique

Ce guide de déploiement fournit des instructions sur l’utilisation des services de certificats Active Directory (AD CS) pour inscrire et inscrire automatiquement des certificats sur des serveurs d’infrastructure NPS et d’accès à distance. Les services AD CS vous permettent de créer une infrastructure à clé publique (PKI) et de fournir un chiffrement à clé publique, des certificats numériques et des fonctionnalités de signature numérique pour votre organisation.

Lorsque vous utilisez des certificats de serveur numérique pour l’authentification entre des ordinateurs de votre réseau, les certificats fournissent les éléments suivants :

1. Confidentialité par le biais du chiffrement.

2. L’intégrité par le biais de signatures numériques.

3. Authentification en associant des clés de certificat à un ordinateur, un utilisateur ou des comptes de périphériques sur un réseau informatique.

Pour plus d’informations, consultez [Guide pas à pas des services AD CS : déploiement d’une hiérarchie PKI à deux niveaux](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx).

## <a name="active-directory-domain-services-ad-ds"></a>Services de domaine Active Directory (AD DS)

Les services AD DS fournissent une base de données distribuée qui stocke et gère des informations sur les ressources réseau et les données spécifiques à des applications provenant d’applications utilisant un annuaire. Les administrateurs peuvent utiliser AD DS pour organiser les éléments d’un réseau, tels que les utilisateurs, les ordinateurs et les autres périphériques, en une structure hiérarchique de type contenant-contenu. La structure hiérarchique de type contenant-contenu inclut la forêt Active Directory, les domaines inclus dans la forêt et les unités d’organisation de chaque domaine. Un serveur qui exécute les services AD DS est appelé contrôleur de domaine.

AD DS contient les comptes d’utilisateur, les comptes d’ordinateur et les propriétés de compte requis par le protocole PEAP (Protected Extensible Authentication Protocol) pour authentifier les informations d’identification de l’utilisateur et pour évaluer l’autorisation pour les demandes de connexion VPN. Pour plus d’informations sur le déploiement de AD DS, consultez le [Guide du réseau de base](../../../../networking/core-network-guide/Core-Network-Guide.md)de Windows Server 2016.

Au terme de l’exécution des étapes de ce déploiement, vous allez configurer les éléments suivants sur le contrôleur de domaine.

- Activer l’inscription automatique des certificats dans stratégie de groupe pour les ordinateurs et les utilisateurs

- Créer le groupe d’utilisateurs VPN

- Créer le groupe de serveurs VPN

- Créer le groupe de serveurs NPS

### <a name="active-directory-users-and-computers"></a>Utilisateurs et ordinateurs Active Directory

Active Directory utilisateurs et ordinateurs est un composant de AD DS qui contient des comptes qui représentent des entités physiques, telles qu’un ordinateur, une personne ou un groupe de sécurité. Un groupe de sécurité est un ensemble de comptes d’utilisateurs ou d’ordinateurs que les administrateurs peuvent gérer comme une seule unité. Les comptes d’utilisateurs et d’ordinateurs qui appartiennent à un groupe particulier sont appelés membres du groupe.

Les comptes d’utilisateur dans Active Directory utilisateurs et ordinateurs ont des propriétés de numérotation que NPS évalue pendant le processus d’autorisation, sauf si la propriété d' **autorisation d’accès réseau** du compte d’utilisateur est définie pour **contrôler l’accès via la stratégie de réseau NPS**. Il s’agit du paramètre par défaut pour tous les comptes d’utilisateur. Toutefois, dans certains cas, ce paramètre peut avoir une configuration différente qui empêche l’utilisateur de se connecter à l’aide d’un VPN. Pour vous protéger contre cette éventualité, vous pouvez configurer le serveur NPS de sorte qu’il ignore les propriétés de connexion d’un compte d’utilisateur.

Pour plus d’informations, consultez [configurer NPS pour ignorer les propriétés d’accès à distance de compte d’utilisateur](../../../../networking/technologies/nps/nps-np-configure.md#configure-nps-to-ignore-user-account-dial-in-properties).

### <a name="group-policy-management"></a>Gestion de groupes de règles

La gestion des stratégie de groupe permet la gestion des modifications et de la configuration des paramètres de l’utilisateur et de l’ordinateur à partir de l’annuaire, y compris la sécurité et les informations utilisateur. Vous utilisez stratégie de groupe pour définir des configurations pour des groupes d’utilisateurs et d’ordinateurs.

Avec stratégie de groupe, vous pouvez spécifier des paramètres pour les entrées de Registre, la sécurité, l’installation de logiciels, les scripts, la redirection de dossiers, les services d’installation à distance et la maintenance d’Internet Explorer. Les paramètres de stratégie de groupe que vous créez sont contenus dans un objet stratégie de groupe (GPO). En associant un objet de stratégie de groupe à des conteneurs système Active Directory sélectionnés (sites, domaines et unités d’organisation), vous pouvez appliquer les paramètres de l’objet de stratégie de groupe aux utilisateurs et ordinateurs dans ces Active Directory conteneurs. Pour gérer les objets stratégie de groupe au sein d’une entreprise, vous pouvez utiliser la console MMC (Microsoft Management Console) Éditeur de gestion des stratégies de groupe.

## <a name="windows-10-vpn-clients"></a>Clients VPN Windows 10

Outre les composants serveur, assurez-vous que les ordinateurs clients que vous configurez pour utiliser le VPN exécutent la mise à jour anniversaire Windows 10 (version 1607). Les clients VPN Windows 10 doivent être joints au domaine de votre Active Directory.


Le client VPN Windows 10 est hautement configurable et offre de nombreuses options. Pour mieux illustrer les fonctionnalités spécifiques utilisées par ce scénario, le tableau 1 identifie les catégories de fonctionnalités VPN et les configurations spécifiques que ce déploiement fait référence. Vous allez configurer les paramètres individuels de ces fonctionnalités à l’aide du fournisseur de services de configuration (CSP) VPNv2 abordé plus loin dans ce déploiement. 

Tableau 1. Fonctionnalités et configurations VPN abordées dans ce déploiement

| Fonctionnalité VPN     |     Configuration du scénario de déploiement         |
|-----------------|-----------------------------------------------|
| Type de connexion |                 IKEv2 natif                  |
|     Routage     |                Tunneling fractionné                |
| Résolution de noms |  Liste d’informations de nom de domaine et suffixe DNS  |
|   Déclenchement    |    Always On et détection de réseau approuvé    |
| Authentification  | PEAP-TLS avec des certificats d’utilisateur protégés par un module de plateforme sécurisée |

>[!NOTE]
>PEAP-TLS et TPM sont respectivement « protocole EAP (Protected Extensible Authentication Protocol) avec Transport Layer Security » et « Module de plateforme sécurisée (TPM) ».

### <a name="vpnv2-csp-nodes"></a>Nœuds CSP VPNv2

Dans ce déploiement, vous utilisez le nœud CSP ProfileXML VPNv2 pour créer le profil VPN fourni aux ordinateurs clients Windows 10. Les fournisseurs de services de configuration (CSP) sont des interfaces qui exposent différentes fonctionnalités de gestion au sein du client Windows. Conceptuellement, les fournisseurs de services de chiffrement fonctionnent de la même façon que stratégie de groupe. Chaque CSP possède des nœuds de configuration qui représentent des paramètres individuels. À l’instar des paramètres stratégie de groupe, vous pouvez lier des paramètres CSP à des clés de Registre, des fichiers, des autorisations, etc. À l’instar de la façon dont vous utilisez la Éditeur de gestion des stratégies de groupe pour configurer des objets stratégie de groupe (GPO), vous configurez des nœuds CSP à l’aide d’une solution de gestion des appareils mobiles (MDM) telle que Microsoft Intune. Les produits MDM comme Intune offrent une option de configuration conviviale qui configure le CSP dans le système d’exploitation.

![Configuration de la gestion des appareils mobiles vers CSP](../../../media/Always-On-Vpn/Vpn-Mdm.jpg)

Toutefois, vous ne pouvez pas configurer certains nœuds CSP directement par le biais d’une interface utilisateur (IU) telle que la console d’administration Intune. Dans ce cas, vous devez configurer les paramètres de l’Uniform Resource Identifier Open Mobile Alliance (OMA-URI) manuellement. Vous configurez les URI OMA à l’aide du protocole OMA-DM (OMA-DM), une spécification de la gestion des appareils universelle qui est prise en charge par les appareils Apple, Android et Windows modernes. Tant qu’ils adhèrent à la spécification OMA-DM, tous les produits MDM doivent interagir de la même façon avec ces systèmes d’exploitation.

Windows 10 offre de nombreux CSP, mais ce déploiement se concentre sur l’utilisation du CSP VPNv2 pour configurer le client VPN. Le CSP VPNv2 autorise la configuration de chaque paramètre de profil VPN dans Windows 10 par le biais d’un nœud CSP unique. Également contenu dans le CSP VPNv2, il s’agit d’un nœud appelé *ProfileXML*, qui vous permet de configurer tous les paramètres dans un nœud plutôt que individuellement. Pour plus d’informations sur ProfileXML, consultez la section « vue d’ensemble du ProfileXML » plus loin dans ce déploiement. Pour plus d’informations sur chaque nœud CSP VPNv2, consultez le [CSP VPNv2](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-csp).

## <a name="next-steps"></a>Étapes suivantes :

- [En savoir plus sur les fonctionnalités avancées Always On VPN](deploy/always-on-vpn-adv-options.md)

- [Démarrer la planification de votre déploiement Always On VPN](deploy/always-on-vpn-deploy-deployment.md)

## <a name="related-topics"></a>Rubriques connexes

- [Prise en charge des logiciels serveur Microsoft pour les machines virtuelles Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines): cet article décrit la stratégie de prise en charge pour l’exécution de logiciels serveur Microsoft dans le Microsoft Azure environnement de machine virtuelle (infrastructure-as-a-service).

- [Accès à distance](../../Remote-Access.md): cette rubrique fournit une vue d’ensemble du rôle de serveur d’accès à distance dans Windows Server 2016.

- [Guide technique du VPN Windows 10](https://docs.microsoft.com/windows/access-protection/vpn/vpn-guide): ce guide vous guide tout au long des décisions que vous allez prendre pour les clients Windows 10 dans votre solution VPN d’entreprise et comment configurer votre déploiement. Ce guide fait référence au fournisseur de services de configuration (CSP) VPNv2 et fournit des instructions de configuration de la gestion des appareils mobiles (MDM) à l’aide de Microsoft Intune et du modèle de profil VPN pour Windows 10.

- [Guide du réseau de base](../../../../networking/core-network-guide/Core-Network-Guide.md): ce guide fournit des instructions sur la planification et le déploiement des composants principaux requis pour un réseau pleinement fonctionnel et un nouveau domaine de Active Directory dans une nouvelle forêt.

- [DNS (Domain Name System)](../../../../networking/dns/dns-top.md): cette rubrique fournit une vue d’ensemble des systèmes de noms de domaine (DNS). Dans Windows Server 2016, DNS est un rôle serveur que vous pouvez installer à l’aide de Gestionnaire de serveur ou de commandes Windows PowerShell. Si vous installez une nouvelle forêt et un nouveau domaine Active Directory, DNS est automatiquement installé avec Active Directory en tant que serveur de catalogue global pour la forêt et le domaine.

- [Active Directory vue d’ensemble des services de certificats](https://technet.microsoft.com/library/hh831740.aspx): ce document fournit une vue d’ensemble des services de certificats Active Directory (AD CS) dans Windows Server® 2012. Les services AD CS correspondent au rôle serveur qui vous permet de générer une infrastructure à clé publique (PKI) et de fournir un chiffrement à clé publique, des certificats numériques et des fonctions de signature numérique pour votre organisation.

- [Guide de conception de l’infrastructure à clé publique](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx): ce wiki fournit des conseils sur la conception d’infrastructures à clé publique (PKI). Avant de configurer une hiérarchie d’infrastructure à clé publique et d’autorité de certification, vous devez être conscient de la stratégie de sécurité et du CPS (Certificate Practice Statement) de votre organisation.

- [Guide pas à pas des services AD CS : déploiement de la hiérarchie PKI à deux niveaux](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx): ce guide pas à pas décrit les étapes nécessaires pour configurer une configuration de base de Active Directory® les services de certificats (AD CS) dans un environnement Lab. Les services AD CS dans Windows Server® 2008 R2 fournissent des services personnalisables pour la création et la gestion des certificats de clé publique utilisés dans les systèmes de sécurité logiciels employant des technologies de clé publique.

- [Serveur NPS (Network Policy Server)](../../../../networking/technologies/nps/nps-top.md): cette rubrique fournit une vue d’ensemble du serveur NPS (Network Policy Server) dans Windows Server 2016. Un serveur NPS \(Network Policy Server\) vous permet de créer et d’appliquer des stratégies d’accès réseau valides pour toute l’organisation, qui régissent les demandes d’authentification et l’autorisation des demandes de connexion.
