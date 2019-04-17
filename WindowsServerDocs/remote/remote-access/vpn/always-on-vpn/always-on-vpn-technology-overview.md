---
title: Toujours sur la présentation de la technologie VPN
description: 'Cette page propose une brève présentation des technologies toujours actif (AlwaysOn) avec des liens vers des documents détaillés. '
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: fd3f7c6ca8555e270aabf04bbee6800ed284080c
ms.sourcegitcommit: 4147e092d77d9458696e6686bccefe3788c90508
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/28/2019
ms.locfileid: "9031323"
---
# Présentation de la technologie toujours actif (AlwaysOn)
>S’applique à: Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #171;  [ **Précédent:** obtenir des informations sur les améliorations toujours actif (AlwaysOn)](always-on-vpn-enhancements.md)<br>
& #187;  [ **Suivant:** obtenir des informations sur les fonctionnalités avancées de toujours actif (AlwaysOn)](deploy/always-on-vpn-adv-options.md)

Pour ce déploiement, vous devez installer un nouveau serveur d’accès distant qui exécute Windows Server 2016, mais aussi modifier certains de votre infrastructure existante pour le déploiement.

L’illustration suivante montre l’infrastructure qui est nécessaires pour déployer toujours actif (AlwaysOn).

![Toujours sur l’Infrastructure VPN](../../../media/Always-On-Vpn/Ao-Vpn-Overview.jpg)

Le processus de connexion représenté dans l’illustration suivante est constitué des étapes suivantes:

1. À l’aide de serveurs DNS publics, le client VPN Windows 10 effectue une requête de résolution de nom pour l’adresse IP de la passerelle VPN.

2. À l’aide de l’adresse IP renvoyée par le système DNS, le client VPN envoie une demande de connexion à la passerelle VPN.

3. La passerelle VPN est également configurée comme un \(RADIUS\) Remote Authentication Dial-In User Service Client; le Client de RADIUS VPN envoie la demande de connexion au serveur NPS organisation / d’entreprise pour le traitement de demande de connexion.

4. Le serveur NPS traite la demande de connexion, y compris effectuant l’authentification et l’autorisation et détermine s’il faut autoriser ou refuser la demande de connexion.

5. Le serveur NPS transmet une réponse d’acceptation ou de refus d’accès à la passerelle VPN.

6. La connexion est démarre ou se termine en fonction de la réponse du serveur VPN reçu du serveur NPS.

Pour plus d’informations sur chaque composant d’infrastructure indiqué dans l’illustration ci-dessus, consultez les sections suivantes.

>[!NOTE]
>Si vous avez déjà certaines de ces technologies déployés sur votre réseau, vous pouvez utiliser les instructions fournies dans ce guide de déploiement pour effectuer une configuration supplémentaire des technologies à cet effet de déploiement.

## DNS (Domain Name System)

Zones de nom de domaine (DNS) internes et externes ne sont requises, ce qui suppose que la zone interne est un sous-domaine délégué de la zone externe (par exemple, corp.contoso.com et contoso.com).

En savoir plus sur les [Guide du réseau principal](../../../../networking/core-network-guide/core-network-guide.md)ou le [Système DNS (Domain Name)](../../../../networking/dns/dns-top.md) .




>[!NOTE] 
>Autres DNS conceptions, tels que DNS split-brain (en utilisant le même nom de domaine interne et externe dans des zones DNS distinctes) ou non liés internes et des domaines externes (par exemple, contoso.local et contoso.com) existent également possibles. Pour plus d’informations sur le déploiement DNS split-brain, voir [Utilisation d’une stratégie DNS pour un déploiement DNS Split /-Brain](../../../../networking/dns/deploy/split-brain-DNS-deployment.md).

## Pare-feu

Assurez-vous que votre pare-feu autorise le trafic qui est nécessaire pour les communications VPN et RADIUS fonctionner correctement.

Pour plus d’informations, consultez [Configurer le pare-feu pour le trafic RADIUS](../../../../networking/technologies/nps/nps-firewalls-configure.md).

## Accès à distance comme serveur VPN passerelle RAS

Dans Windows Server 2016, le rôle de serveur d’accès à distance est conçu pour effectuer également comme un routeur et un serveur d’accès à distance; Par conséquent, il prend en charge un large éventail de fonctionnalités. Ce guide de déploiement, vous avez besoin uniquement un petit sous-ensemble de ces fonctionnalités: prise en charge pour les connexions VPN IKEv2 et le routage de réseau local.

IKEv2 est décrite dans Internet Engineering Task Force Request for 7296 de commentaires de protocole de tunnel VPN. Le principal avantage de IKEv2 est qu’il tolère d’interruption de la connexion réseau sous-jacente. Par exemple, si la connexion est temporairement perdue ou si un utilisateur déplace sur un ordinateur client à partir d’un réseau à l’autre, IKEv2 restaure automatiquement la connexion VPN lorsque la connexion réseau est rétablie, sans intervention de l’utilisateur.

À l’aide de RAS passerelle, vous pouvez déployer des connexions VPN pour fournir aux utilisateurs un accès à distance au réseau et les ressources de votre organisation. Déploiement toujours actif (AlwaysOn) conserve une connexion permanente entre les clients et le réseau de votre organisation chaque fois que les ordinateurs distants sont connectés à Internet. Avec RAS passerelle, vous pouvez également créer une connexion VPN de site à site entre deux serveurs à différents emplacements, telles qu’entre votre bureau principal et une filiale et utiliser des \(NAT\) traduction d’adresses réseau afin que les utilisateurs au sein du réseau peuvent accéder externe ressources, comme Internet. En outre, passerelle RAS prend en charge le protocole BGP (Border Gateway), qui fournit des services de routage dynamique lorsque vos sites distants ont également des passerelles edge qui prennent en charge le protocole BGP.

Vous pouvez gérer des passerelles de Service d’accès à distance (RAS) à l’aide de commandes Windows PowerShell et à distance accès Microsoft Management Console (MMC).



## Serveur NPS (Network Policy Server)

NPS vous permet de créer et appliquer des stratégies d’accès à l’échelle de l’organisation du réseau pour l’authentification de demande de connexion et d’autorisation. Lorsque vous utilisez NPS comme serveur RADIUS Remote Authentication Dial-In User Service (), vous configurez des serveurs d’accès réseau, tels que des serveurs VPN, en tant que clients RADIUS dans NPS.

Vous configurez également des stratégies de réseau que NPS utilise pour autoriser les demandes de connexion, et vous pouvez configurer la gestion des comptes RADIUS afin que NPS enregistre les informations de gestion pour enregistrer les fichiers sur le disque dur local ou dans une base de données Microsoft SQL Server.

Pour plus d’informations, voir le [Serveur NPS (Network Policy Server)](../../../../networking/technologies/nps/nps-top.md).


## Services de certificats ActiveDirectory

Le serveur de l’autorité de Certification (CA) est une autorité de certification qui exécute les Services de certificats Active Directory. La configuration VPN nécessite une basée sur Active Directory infrastructure à clé publique (PKI).

Organisations peuvent utiliser les services AD CS pour renforcer la sécurité en liant l’identité d’une personne, un appareil ou un service à une clé publique correspondante. AD CS inclut également des fonctionnalités qui vous permettent de gérer l’inscription de certificat et de révocation dans une variété d’environnements évolutives. Pour plus d’informations, voir [Présentation des Services de certificats Active Directory](https://technet.microsoft.com/library/hh831740.aspx) et [Guide de conception Infrastructure à clé publique](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx).

Au cours de l’achèvement du déploiement, vous allez configurer les modèles de certificat suivants sur l’autorité de certification.

-   Le modèle de certificat d’authentification utilisateur

-   Le modèle de certificat d’authentification du serveur VPN

-   Le modèle de certificat d’authentification du serveur NPS

### Modèles de certificats

Modèles de certificats peuvent considérablement simplifier la tâche d’administration d’une autorité de certification (CA) en vous permettant d’émettre des certificats qui sont préconfigurés pour les tâches sélectionnées. Le composant logiciel enfichable MMC Modèles de certificats vous permet d’effectuer les tâches suivantes.

-   Afficher les propriétés pour chaque modèle de certificat.

-   Copier et modifier des modèles de certificats.

-   Contrôler les utilisateurs et ordinateurs peuvent lire des modèles et s’inscrire pour les certificats.

-   Effectuer d’autres tâches d’administration relatives aux modèles de certificats.

Modèles de certificats qui font partie intégrante d’une autorité de certification d’entreprise (CA). Ils sont un élément important de la stratégie de certificat pour un environnement, qui est l’ensemble des règles et formats pour l’inscription de certificat, utilisation et la gestion.

Pour plus d’informations, voir les [Modèles de certificats](https://technet.microsoft.com/library/cc730705.aspx).

### Certificats de serveur numérique

Ce guide de déploiement fournit des instructions pour les inscrire et inscrire automatiquement des certificats pour l’accès à distance et les serveurs d’infrastructure de serveur NPS à l’aide de Services de certificats Active Directory (AD CS). AD CS vous permet de créer une infrastructure à clé publique (PKI) et de fournir un chiffrement à clé publique, des certificats numériques et les fonctionnalités de signature numérique pour votre organisation.

Lorsque vous utilisez des certificats de serveur numérique pour l’authentification entre les ordinateurs de votre réseau, les certificats fournissent:

1.  Confidentialité par le biais du chiffrement.

2.  Intégrité par le biais de signatures numériques.

3.  Authentification en associant les clés de certificat avec un compte d’ordinateur, utilisateur ou appareil sur un réseau informatique.

Pour plus d’informations, voir [AD CS étape Guide pas à pas: déploiement de hiérarchie de deux niveaux PKI](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx).

## Services de domaine Active Directory (AD DS)

Ils offrent une base de données distribuée qui stocke et gère les informations sur les ressources réseau et des données spécifiques à l’application à partir d’applications d’annuaire. Les administrateurs peuvent utiliser les services AD DS pour organiser les éléments d’un réseau, par exemple, les utilisateurs, les ordinateurs et autres périphériques, dans une structure hiérarchique imbrication. La structure hiérarchique contention inclut la forêt Active Directory, domaines dans la forêt et les unités d’organisation (ou) dans chaque domaine. Un serveur qui exécute les services AD DS est appelé un contrôleur de domaine.

Les services AD DS contient les comptes d’utilisateurs, les comptes d’ordinateur et les propriétés du compte qui sont requises par Extensible Authentication Protocol PEAP (Protected) pour authentifier les informations d’identification utilisateur et pour évaluer l’autorisation des demandes de connexion VPN. Pour plus d’informations sur le déploiement des services AD DS, voir le [Guide du réseau principal](../../../../networking/core-network-guide/Core-Network-Guide.md)de Windows Server 2016.



Pendant la fin de la procédure décrite dans ce déploiement, vous allez configurer les éléments suivants sur le contrôleur de domaine.

-   Activer l’inscription automatique de certificat dans la stratégie de groupe pour les ordinateurs et les utilisateurs

-   Créer le groupe d’utilisateurs VPN

-   Créer le groupe de serveurs VPN

-   Créer le groupe de serveurs NPS

### Utilisateurs et ordinateurs active Directory

Active Directory utilisateurs et ordinateurs est un composant des services AD DS qui contient les comptes qui représentent des entités physiques, comme un ordinateur, une personne ou un groupe de sécurité. Un groupe de sécurité est une collection de comptes d’utilisateur ou l’ordinateur que les administrateurs peuvent gérer comme une seule unité. Comptes d’utilisateur et d’ordinateur qui appartiennent à un groupe donné sont référencés en tant que membres du groupe.

Comptes d’utilisateur dans Active Directory utilisateurs et ordinateurs ont des propriétés dial-in NPS évalue pendant le processus d’autorisation -, sauf si la propriété de **l’Autorisation d’accès réseau** du compte d’utilisateur est définie sur **contrôler l’accès via la stratégie réseau NPS **. Il s’agit du paramètre par défaut pour tous les comptes d’utilisateurs. Toutefois, dans certains cas, ce paramètre peut avoir une configuration différents qui empêche l’utilisateur de se connecter à l’aide de VPN. Pour vous protéger contre cette possibilité, vous pouvez configurer le serveur NPS pour ignorer les propriétés d’accès du compte d’utilisateur.

Pour plus d’informations, consultez [Configurer NPS aux propriétés de compte d’utilisateur Ignorer Dial-in](../../../../networking/technologies/nps/nps-np-configure.md#configure-nps-to-ignore-user-account-dial-in-properties).



### Gestion des stratégies de groupe

Gestion des stratégies de groupe permet de gestion des modifications et la configuration basée sur le répertoire des paramètres utilisateur et d’ordinateur, y compris les informations de sécurité et l’utilisateur. Vous utilisez la stratégie de groupe pour définir des configurations pour des groupes d’utilisateurs et ordinateurs.

Avec la stratégie de groupe, vous pouvez spécifier des paramètres pour les entrées de Registre, sécurité, installation de logiciels, scripts, la redirection de dossiers, les services d’installation à distance et maintenance d’Internet Explorer. Les paramètres de stratégie de groupe que vous créez sont contenus dans un objet de stratégie de groupe (GPO). En associant un objet de stratégie de groupe à des conteneurs système Active Directory sélectionnés: sites, des domaines et des unités d’organisation, vous pouvez appliquer des paramètres de l’objet de stratégie de groupe pour les utilisateurs et ordinateurs dans ces conteneurs Active Directory. Pour gérer les objets de stratégie de groupe dans une entreprise, vous pouvez utiliser le MMC Stratégie de groupe gestion éditeur Microsoft Management Console ().


## Clients VPN Windows 10

Outre les composants du serveur, assurez-vous que les ordinateurs clients que vous configurez pour utiliser un VPN sont en cours d’exécution mise à jour anniversaire de Windows 10 (version1607). Les clients VPN Windows 10 doivent être joints au domaine à votre domaine Active Directory.


Le client VPN Windows 10 est configurable et offre de nombreuses options. Pour mieux illustrer les fonctionnalités spécifiques qu'utilise ce scénario, Table1 identifie les catégories de fonctionnalités VPN et les configurations spécifiques qui fait référence à ce déploiement. Vous allez configurer les paramètres individuels de ces fonctionnalités à l’aide du fournisseur de services de configuration (CSP) VPNv2 décrits plus loin dans ce déploiement. 

Tableau1. Fonctionnalités de VPN et les Configurations décrites dans ce déploiement

| **Fonctionnalité VPN** | **Configuration de scénario de déploiement**         |
|-----------------|-----------------------------------------------|
| Type de connexion | IKEv2 native                                  |
| Routage         | Tunneling fractionné                               |
| Résolution de nom | Suffixe DNS et la liste d’informations de nom de domaine   |
| Déclenchement      | Détection de réseaux toujours activé et de confiance       |
| Authentification  | PEAP-TLS avec des certificats utilisateur protégé par le module de plateforme sécurisée |
---

>[!NOTE] 
>PEAP-TLS et module de plateforme sécurisée sont «Protected Extensible Authentication Protocol avec Transport Layer Security» et «Module de plateforme sécurisée», respectivement.

### Nœuds du fournisseur CSP VPNv2

Dans ce déploiement, vous utilisez le nœud ProfileXML VPNv2 CSP pour créer le profil VPN qui est fourni pour les ordinateurs clients Windows 10. Fournisseurs de services de configuration (CSP) sont des interfaces qui exposent les fonctionnalités de gestion des différents au sein du client Windows; point de vue conceptuel, les fournisseurs CSP fonctionnent similaire au mode de fonctionnement de la stratégie de groupe. Chaque fournisseur CSP a des nœuds de configuration qui représentent les paramètres individuels. Également comme les paramètres de stratégie de groupe, vous pouvez lier les paramètres CSP aux clés de Registre, les fichiers, les autorisations et ainsi de suite. Similaire à l’utilisation de l’éditeur de gestion des stratégies de groupe pour configurer les objets de stratégie de groupe (GPO), vous configurez nœud CSP à l’aide d’une solution de gestion des périphériques mobiles comme Microsoft Intune. Produits de GPM comme Intune offrent une option de configuration convivial qui configure le fournisseur CSP dans le système d’exploitation.

![Gestion des périphériques mobiles à la configuration du fournisseur de solutions cloud](../../../media/Always-On-Vpn/Vpn-Mdm.jpg)

Toutefois, vous ne pouvez pas configurer certains nœuds CSP directement par le biais d’une interface utilisateur (UI), comme la Console d’administration Intune. Dans ces cas, vous devez configurer les paramètres Open Mobile Alliance Uniform Resource Identifier (OMA-URI) manuellement. Vous configurez OMA-URI à l’aide du protocole OMA la gestion des périphériques (DM OMA), une spécification de gestion d’appareils universelle qui prennent en charge des appareils plus modernes de Apple, Android et Windows. Dans la mesure qui respectent la spécification OMA-DM, tous les produits de GPM doivent interagir avec ces systèmes d’exploitation de la même manière.

Windows 10 offre de nombreux fournisseurs CSP, mais ce déploiement se concentre sur le fournisseur CSP VPNv2 pour configurer le client VPN. Le fournisseur CSP VPNv2 permet la configuration de chaque paramètre de profil VPN dans Windows 10 par le biais d’un nœud CSP unique. Également contenus dans le fournisseur CSP VPNv2 est un nœud appelé *ProfileXML*, ce qui vous permet de configurer tous les paramètres dans un nœud au lieu individuellement. Pour plus d’informations sur ProfileXML, consultez la section «Vue d’ensemble ProfileXML» plus loin dans ce déploiement. Pour plus d’informations sur chaque nœud CSP VPNv2, consultez le fournisseur [CSP VPNv2](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-csp).



## Étapes suivantes

- [En savoir plus sur certaines des fonctionnalités avancées toujours actif (AlwaysOn)](deploy/always-on-vpn-adv-options.md)

- [Commencer à planifier votre déploiement de VPN Toujours actif (AlwaysOn)](deploy/always-on-vpn-deploy-deployment.md)


---

## Rubriques associées
- [Le logiciel serveur Microsoft prend en charge pour les machines virtuelles Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines): cet article traite de la politique de support pour le logiciel serveur Microsoft en cours d’exécution dans l’environnement de machine virtuelle Microsoft Azure (infrastructure-as-a-service).

- [Accès à distance](../../Remote-Access.md): cette rubrique fournit une vue d’ensemble du rôle du serveur d’accès à distance dans Windows Server 2016.

- [Guide technique du VPN Windows 10](https://docs.microsoft.com/windows/access-protection/vpn/vpn-guide): ce guide vous explique comment les décisions que vous créerez pour les clients Windows 10 dans votre solution VPN d’entreprise et la configuration de votre déploiement. Ce guide fait référence au fournisseur de services de configuration (CSP) VPNv2 et fournit des instructions de configuration relatives à la gestion des appareils mobiles (GPM) à l’aide de Microsoft Intune et du modèle de profil VPN pour Windows10.

- [Guide du réseau principal](../../../../networking/core-network-guide/Core-Network-Guide.md): ce guide fournit des instructions sur la façon de planifier et déployer les principaux composants requis pour un réseau entièrement fonctionnel et un nouveau domaine Active Directory dans une nouvelle forêt.

- [Nom de domaine (DNS)](../../../../networking/dns/dns-top.md): cette rubrique fournit une vue d’ensemble des systèmes DNS (Domain Name). Dans Windows Server 2016, DNS est un rôle de serveur que vous pouvez installer en utilisant les commandes de gestionnaire de serveur ou Windows PowerShell. Si vous installez une nouvelle forêt Active Directory et le même domaine, DNS est automatiquement installé avec Active Directory que le serveur de Catalogue Global pour la forêt et le domaine. 

- [Présentation des Services de certificats Active Directory](https://technet.microsoft.com/library/hh831740.aspx): ce document fournit une vue d’ensemble des Services de certificats Active Directory (AD CS) dans Windows Server® 2012. AD CS est le rôle de serveur qui vous permet de créer une infrastructure à clé publique (PKI) et de fournir un chiffrement à clé publique, des certificats numériques et les fonctionnalités de signature numérique pour votre organisation.

- [Guide de conception Infrastructure à clé publique](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx): ce wiki fournit des conseils sur la conception d’Infrastructures de clé publique (PKI). Avant de configurer une hiérarchie d’autorité de certification PKI et de certification, vous devez être conscient sécurité stratégie et certificat pratique instruction de votre organisation (CPS).

- [Guide d’étape par étape AD CS: déploiement de hiérarchie de deux niveaux PKI](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx): ce guide pas à pas décrit les étapes nécessaires pour définir une configuration de base des Services de certificats Active Directory® (AD CS) dans un environnement de laboratoire. AD CS dans Windows Server® 2008 R2 fournissent des services personnalisables pour la création et la gestion des certificats de clé publique utilisés dans les systèmes de sécurité logiciels employant des technologies de clé publique.

- [Serveur NPS (Network Policy Server)](../../../../networking/technologies/nps/nps-top.md): cette rubrique fournit une vue d’ensemble du serveur NPS dans Windows Server 2016. Un serveur NPS \(Network Policy Server\) vous permet de créer et d’appliquer des stratégies d’accès réseau valides pour toute l’organisation, qui régissent les demandes d’authentification et l’autorisation des demandes de connexion. 

---
