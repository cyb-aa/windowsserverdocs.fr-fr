---
title: Déployer un accès sans fil authentifié 802.1X basé sur des mots de passe
description: Cette rubrique fait partie de la « Accès sans fil authentifié 802. 1 mot de passe de déployer X » du guide de mise en réseau de Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ff06ba23-9c0f-49ec-8f7b-611cf8d73a1b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f34580f4a0fd92c6f059d19d09a6926fc2775960
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840010"
---
# <a name="deploy-password-based-8021x-authenticated-wireless-access"></a>Déployer le mot de passe\-en fonction de 802. 1 X un accès sans fil authentifié

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Il s’agit d’un guide d’accompagnement pour le serveur Windows&reg; 2016 Guide réseau de base. Le Guide du réseau fournit des instructions pour la planification et déploiement des composants requis pour un réseau pleinement fonctionnel et un nouveau domaine Active dans une nouvelle forêt.

Ce guide explique comment reposent sur un réseau de base en fournissant des instructions sur le déploiement Institute of Electrical and Electronics Engineers \(IEEE\) 802. 1 X\-authentifié accès sans fil IEEE 802.11 à l’aide Protected Extensible Authentication Protocol – Microsoft Challenge Handshake Authentication Protocol version 2 \(PEAP\-MS\-CHAP v2\).

Étant donné que PEAP\-MS\-CHAP v2 requiert que les utilisateurs fournissent le mot de passe\-en informations d’identification au lieu d’un certificat pendant le processus d’authentification, il est généralement plus facile et moins coûteux à déployer que EAP\-TLS ou PEAP\-TLS.

>[!NOTE]
>Dans ce guide, IEEE 802. 1 X authentifiés l’accès sans fil avec PEAP\-MS\-CHAP v2 est abrégé en « accès sans fil » et « Accès Wi-Fi ».

## <a name="about-this-guide"></a>À propos de ce guide
Ce guide, conjointement avec les guides de configuration requise décrite ci-dessous, fournit des instructions sur la façon de déployer l’infrastructure d’accès Wi-Fi suivante.  

- Un ou plusieurs 802. 1 X\-points d’accès sans fil 802.11 \(APs\).

- Les Services de domaine Active Directory \(AD DS\) utilisateurs et ordinateurs.

- Gestion des stratégies de groupe

- Un ou plusieurs serveurs de stratégie réseau \(NPS\) serveurs.

- Certificats de serveur pour les ordinateurs exécutant NPS (Network Policy Server)

- Les ordinateurs clients exécutant Windows sans fil&reg; 10, Windows 8.1 ou Windows 8.

### <a name="dependencies-for-this-guide"></a>Dépendances de ce guide

Pour déployer correctement authentifié sans fil avec ce guide, vous devez disposer d’un environnement réseau et de domaine avec toutes les technologies requises déployés. Vous devez également disposer des certificats de serveur déployés sur votre authentification NPSs.

Les sections suivantes fournissent des liens vers la documentation qui vous montre comment déployer ces technologies.

#### <a name="network-and-domain-environment-dependencies"></a>Dépendances de l’environnement réseau et de domaine

Ce guide est conçu pour les administrateurs système et réseau ayant suivi les instructions fournies dans Windows Server 2016 **Guide du réseau** pour déployer un réseau de base, ou pour ceux qui ont déjà déployé les technologies de base inclus dans le réseau de base, y compris les services AD DS, Domain Name System \(DNS\), Dynamic Host Configuration Protocol \(DHCP\), TCP\/IP, NPS et Windows Internet Name Service \(WINS\).  

Le Guide du réseau est disponible aux emplacements suivants :

- Windows Server 2016 [Guide du réseau](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide) est disponible dans la bibliothèque technique Windows Server 2016. 

- Le [Guide du réseau](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683) est également disponible au format Word dans la Galerie TechNet, à [ https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683 ](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683).

#### <a name="server-certificate-dependencies"></a>Dépendances de certificat de serveur
Deux options sont disponibles pour l’inscription de serveurs d’authentification avec certificats de serveur pour une utilisation avec l’authentification 802. 1 X - déployer votre propre infrastructure à clé publique à l’aide des Services de certificats Active Directory \(AD CS\) ou utiliser des certificats de serveur qui sont inscrits par une autorité de certification publique \(autorité de certification\).

##### <a name="ad-cs"></a>AD CS
Réseau et les administrateurs système déploiement sans fil authentifié doivent suivre les instructions fournies dans le Windows Server 2016 Core Network Companion Guide, **déployer des certificats de serveur pour les déploiements sans fil et câblé à 802.1 X**. Ce guide explique comment déployer et utiliser les services AD CS pour inscrire automatiquement les certificats de serveur sur des ordinateurs exécutant NPS.

Ce guide est disponible à l’emplacement suivant.

- Le Guide d’accompagnement du réseau Windows Server 2016 Core [déployer des certificats de serveur pour les déploiements de sans fil et câblé à 802.1 X](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments?f=255&MSPPError=-2147217396) au format HTML dans la bibliothèque technique. 

##### <a name="public-ca"></a>Autorité de certification publique
Vous pouvez acheter des certificats de serveur à partir d’une autorité de certification publique, telle que VeriSign, que les ordinateurs clients approuvent déjà. 

Un ordinateur client fait confiance à une autorité de certification lorsque le certificat d’autorité de certification est installé dans le magasin de certificats Autorités de Certification racine de confiance. Par défaut, les ordinateurs exécutant Windows ont plusieurs certificats d’autorité de certification publiques installés dans leur certificat Trusted Root Certification Authorities store.  

Il est recommandé de consulter les guides de conception et de déploiement pour chacune des technologies utilisées dans ce scénario de déploiement. Ces guides peuvent vous aider à déterminer si ce scénario de déploiement fournit les services et la configuration dont vous avez besoin pour le réseau de votre organisation.

### <a name="requirements"></a>Configuration requise
Voici la configuration requise pour le déploiement d’une infrastructure d’accès sans fil en utilisant le scénario décrit dans ce guide :

- Avant de déployer ce scénario, vous devez tout d’abord acheter 802. 1 X\-points accès sans fil capable de fournir une couverture sans fil aux emplacements souhaités sur votre site. La section de ce guide de planification aide à déterminer les caractéristiques de que vos points d’accès doivent prendre en charge.

- Les Services de domaine Active Directory \(AD DS\) est installé, comme les autres technologies de réseau requise, en suivant les instructions dans le Guide du réseau Windows Server 2016 Core.  

- Les services AD CS est déployé, et des certificats de serveur sont inscrit auprès de NPSs. Ces certificats sont requis lorsque vous déployez le PEAP\-MS\-CHAP v2 certificat\-en fonction de méthode d’authentification qui est utilisé dans ce guide.

- Un membre de votre organisation est familiarisé avec les normes IEEE 802.11 pris en charge par vos points d’accès sans fil et les cartes réseau sans fil qui sont installés dans les ordinateurs clients et les périphériques sur votre réseau. Par exemple, une personne de votre organisation est familiarisée avec les types de fréquence radio, l’authentification sans fil 802.11 \(WPA2 ou WPA\)et les chiffrements \(AES ou TKIP\).

## <a name="what-this-guide-does-not-provide"></a>Ce que ce guide ne contient pas  
Voici certains éléments de que ce guide ne fournit pas :

### <a name="comprehensive-guidance-for-selecting-8021x-capable-wireless-access-points"></a>Des instructions complètes pour la sélection de 802. 1 X\-points d’accès sans fil compatible

Car il existent de nombreuses différences entre les modèles et marques de 802. 1 X\-capable AP sans fil accessibles, ce guide ne fournit pas des informations détaillées sur :  

- Détermination de qui marque ou le modèle du point d’accès sans fil convient le mieux adapté à vos besoins.

- Le déploiement physique des points d’accès sans fil sur votre réseau.

- Avancées de configuration de point d’accès sans fil, telles que pour les réseaux locaux virtuels sans fil \(VLAN\).

- Obtenir des instructions sur la configuration de fournisseur de point d’accès sans fil\-des attributs spécifiques dans NPS.

En outre, la terminologie et les noms de paramètres varient entre les modèles et marques de point d’accès sans fil et peuvent ne pas correspondent les noms des paramètres génériques qui sont utilisés dans ce guide. Pour plus d’informations de configuration de point d’accès sans fil, vous devez passer en revue la documentation du produit fournie par le fabricant de vos points d’accès sans fil.

### <a name="instructions-for-deploying-nps-certificates"></a>Instructions de déploiement des certificats de serveur NPS
  
Il existe deux alternatives pour le déploiement de certificats de serveur NPS. Ce guide ne fournit pas de conseils détaillés pour vous aider à déterminer quels alternative répondra le mieux à vos besoins. En général, toutefois, les choix que vous êtes confronté à sont :

- Achat de certificats à partir d’une autorité de certification publique, telle que VeriSign, qui sont déjà approuvées par Windows\-en fonction des clients. Cette option est généralement recommandée pour les réseaux de petite taille.

- Déploiement d’une Infrastructure à clé publique \(PKI\) sur votre réseau à l’aide des services AD CS. Cela est recommandé pour la plupart des réseaux, et les instructions pour savoir comment déployer des certificats de serveur avec les services AD CS sont disponibles dans le guide de déploiement mentionnées précédemment.

### <a name="nps-network-policies-and-other-nps-settings"></a>Stratégies de réseau NPS et d’autres paramètres de NPS

Sauf pour les paramètres de configuration effectuées lorsque vous exécutez le **configurer 802. 1 X** Assistant, comme indiqué dans ce guide, ce guide ne fournit pas les informations détaillées pour configurer manuellement les conditions, les contraintes ou les autres NPS NPS Paramètres.

### <a name="dhcp"></a>DHCP

Ce guide de déploiement ne fournit pas d’informations sur la conception ou déploiement de sous-réseaux DHCP pour les réseaux locaux sans fil.

## <a name="technology-overviews"></a>Vues d’ensemble des technologies
Voici des présentations des technologies de déploiement de l’accès sans fil :

### <a name="ieee-8021x"></a>IEEE 802.1X
La norme IEEE 802. 1 X définit le port\-en fonction de contrôle d’accès réseau qui est utilisé pour fournir un accès réseau authentifié à des réseaux Ethernet. Ce port\-contrôle d’accès réseau utilise les caractéristiques physiques de l’infrastructure LAN commutée pour authentifier les appareils connectés à un port LAN. L'accès au port peut être refusé en cas d'échec du processus d'authentification. Bien que cette norme ait été conçue pour les réseaux Ethernet câblés, elle a été adaptée aux réseaux locaux sans fil 802.11.

### <a name="8021x-capable-wireless-access-points-aps"></a>802. 1 X\-points d’accès sans fil capable \(APs\)
Ce scénario nécessite le déploiement d’un ou plusieurs 802. 1 X\-capables points d’accès sans fil qui sont compatibles avec Remote Authentication Dial\-In User Service \(RADIUS\) protocole.

802. 1 X et RADIUS\-conformes APs, lors du déploiement dans une infrastructure RADIUS avec un serveur RADIUS tel un serveur NPS, sont appelés *clients RADIUS*.

### <a name="wireless-clients"></a>Clients sans fil
Ce guide fournit des détails de la configuration complète pour fournir de 802. 1 X un accès authentifié pour le domaine\-utilisateurs membres qui se connectent au réseau avec les ordinateurs clients sans fil exécutant Windows 10, Windows 8.1 et Windows 8. Ordinateurs doivent être joints au domaine afin d’établir un accès authentifié.

>[!NOTE]
>Vous pouvez également utiliser des ordinateurs qui exécutent Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012 en tant que clients sans fil.

### <a name="support-for-ieee-80211-standards"></a>Prise en charge pour IEEE 802.11 normes
Prise en charge des systèmes d’exploitation Windows et Windows Server fournissent généré\-prise en charge des 802.11 sans fil mise en réseau. Dans ces systèmes d’exploitation, une carte réseau sans fil 802.11 installée s’affiche en tant qu’une connexion de réseau sans fil dans le Centre réseau et partage. 

Bien qu’il repose\-prise en charge des réseaux sans fil 802.11, les composants sans fil de Windows dépendent de ce qui suit :

- Les fonctionnalités de la carte réseau sans fil. La carte réseau sans fil installée doit prendre en charge le réseau local sans fil ou des normes de sécurité sans fil dont vous avez besoin. Par exemple, si la carte réseau sans fil ne prend pas en charge Wi\-Fi Protected Access \(WPA\), vous ne pouvez pas activer ou configurer les options de sécurité WPA.

- Les fonctionnalités du pilote de carte réseau sans fil. Pour vous permettre de configurer les options de réseau sans fil, le pilote pour la carte réseau sans fil doit prendre en charge la création de rapports de toutes ses fonctionnalités à Windows. Vérifiez que le pilote de votre carte réseau sans fil est écrit pour les fonctionnalités de votre système d’exploitation. Vérifiez également que le pilote est la version la plus récente en vérifiant Microsoft Update ou le site Web du fournisseur de carte réseau sans fil.

Le tableau suivant présente les vitesses de transmission et les fréquences pour les normes sans fil IEEE 802.11 communes.

|Normes|Fréquences|Vitesses de Transmission|Utilisation|
|-------------|---------------|--------------------------|---------|  
|802.11|S\-bande industriel, scientifique et médical \(ISM\) plage de fréquences \(2.4 à 2,5 GHz\)|2 mégabits par seconde \(Mbits/s\)|Obsolète. Couramment utilisées.|  
|802.11b|S\-bande ISM|11 Mbits/s|Couramment utilisé.|  
|802. 11 a|C\-bande ISM \(5,725 à 5.875 GHz\)|54 Mbits/s|Généralement pas utilisé en raison de dépenses et la plage limitée.|  
|802. 11 g|S\-bande ISM|54 Mbits/s|Largement utilisé. les périphériques 802. 11 g sont compatibles avec 802. 11 b appareils.|  
|802.11n \2.4 et 5.0 GHz|C\-hors bande et S\-bande ISM|250 Mbits/s|Appareils en fonction de la version antérieure de\-ratification IEEE 802.11n standard est disponible depuis août 2007. 802.11n de nombreux périphériques sont compatibles avec 802. 11 a, b et g périphériques.|
|802.11ac |5 GHz |6,93 Gbits/s |802.11ac, approuvé par l’IEEE en 2014, plus évolutif et plus rapide que 802.11n et est déployé dans lequel les deux points d’accès et les clients sans fil prennent en charge.|

### <a name="wireless-network-security-methods"></a>Méthodes de sécurité de réseau sans fil
**Sans fil de méthodes de sécurité réseau** est un regroupement informel de l’authentification sans fil \(parfois qualifié de sécurité sans fil\) et le chiffrement de sécurité sans fil. Chiffrement et l’authentification sans fil sont utilisés dans les paires pour empêcher les utilisateurs non autorisés d’accéder au réseau sans fil et pour protéger les transmissions sans fil. 

Lorsque vous configurez les paramètres de sécurité sans fil dans la stratégie réseau sans fil des stratégies de groupe, il existe plusieurs combinaisons sélectionnables. Toutefois, seuls le WPA2\-Enterprise, WPA\-Enterprise et ouvrir avec 802. 1 X standards d’authentification sont pris en charge pour les déploiements sans fil 802.1 X authentifié.

>[!NOTE]
>Lors de la configuration des stratégies de réseau sans fil, vous devez sélectionner **WPA2\-Enterprise**, **WPA\-Enterprise**, ou **ouvrir avec 802. 1 X** dans ordre pour accéder aux paramètres EAP qui sont requis pour 802. 1 X authentifié des déploiements sans fil.  

#### <a name="wireless-authentication"></a>Authentification sans fil
Ce guide recommande que l’utilisation des normes suivantes, l’authentification sans fil 802. 1 X authentifié des déploiements sans fil.  

**Wi\-Fi Protected Access – Enterprise \(WPA\-Enterprise\)**  WPA est une norme intérimaire développée par Wi-Fi Alliance pour être conforme avec le protocole de sécurité sans fil 802.11. Le protocole WPA a été développé en réponse à un nombre de défauts graves qui ont été détectés dans le précédent Wired Equivalent Privacy \(WEP\) protocole.

WPA\-Enterprise offre une sécurité accrue sur WEP par :  

1. Nécessite une authentification qui utilise le framework X EAP 802.1 dans le cadre de l’infrastructure qui garantit l’authentification mutuelle centralisée et gestion dynamique de clés  

2. Amélioration de la valeur de vérifier l’intégrité \(ICV\) avec un Message Integrity Check \(MIC\), pour protéger l’en-tête et la charge utile  

3. Implémentation d’un compteur de trames pour décourager les attaques par relecture  

**Wi\-Fi Protected Access 2 – Enterprise \(WPA2\-Enterprise\)**  comme le WPA\-standard de l’entreprise, WPA2\-entreprise utilise le 802. 1 X et infrastructure EAP. WPA2\-Enterprise offre une protection des données renforcée pour plusieurs utilisateurs et grand des réseaux gérés. WPA2\-Enterprise est un protocole fiable qui est conçu pour empêcher tout accès réseau non autorisé en vérifiant les utilisateurs du réseau via un serveur d’authentification.  

#### <a name="wireless-security-encryption"></a>Chiffrement de sécurité sans fil
Chiffrement de sécurité sans fil est utilisé pour protéger les transmissions sans fil qui sont envoyées entre le client sans fil et le point d’accès sans fil. Chiffrement de sécurité sans fil est utilisé conjointement avec la méthode d’authentification de sécurité réseau sélectionné. Par défaut, les ordinateurs qui exécutent Windows 10, Windows 8.1 et Windows 8 prennent en charge deux normes de chiffrement :

1. **Temporal Key Integrity Protocol** \(TKIP\) est un ancien protocole de chiffrement qui a été conçu pour fournir un chiffrement plus sécurisé sans fil celui fourni par la faiblesse inhérente Wired Equivalent Privacy \(WEP\) protocole. TKIP a été conçu par l’IEEE 802.11i tâches groupe et le Wi\-Fi Alliance pour remplacer WEP sans nécessiter le remplacement du matériel hérité. TKIP est une suite d’algorithmes qui encapsule la charge utile WEP et permet aux utilisateurs d’un équipement hérité Wi-Fi de mise à niveau vers TKIP sans remplacer du matériel. Comme WEP, TKIP utilise l’algorithme de chiffrement de flux RC4 comme base. Le nouveau protocole chiffre Toutefois, chaque paquet de données avec une clé de chiffrement unique, et les clés sont beaucoup plus puissant que celles par WEP. Bien que TKIP est utile pour la mise à niveau de sécurité sur des appareils plus anciens qui ont été conçues pour utiliser WEP uniquement, il ne traite pas tous les problèmes de sécurité accessible sur des réseaux locaux sans fil et dans la plupart des cas n’est pas suffisamment robuste pour protéger government sensible ou des données d’entreprise transmissions.  

2. **Advanced Encryption Standard** \(AES\) est le protocole de chiffrement par défaut pour le chiffrement des données commerciales et publiques. AES offre un niveau de sécurité de la transmission sans fil supérieur TKIP ou WEP. Contrairement aux TKIP et WEP, AES requiert du matériel sans fil qui prend en charge la norme AES. AES est symétrique\-chiffrement à clé standard qui utilise trois chiffrements par bloc, AES\-128, AES\-192 et AES\-256.

Dans Windows Server 2016, l’algorithme AES suivant\-les méthodes de chiffrement sans fil sont disponibles pour la configuration dans Propriétés du profil sans fil lorsque vous sélectionnez une méthode d’authentification de WPA2\-entreprise, ce qui est recommandé.

1. **AES\-CCMP**. Compteur Mode Cipher Block Chaining Message Authentication Code Protocol \(CCMP\) implémente la norme 802.11i est conçu pour le chiffrement de sécurité plus élevé que celle fournie par WEP et utilise des clés de chiffrement AES 128 bits.
2. **AES\-GCMP**. Protocole de Mode de compteur GALOIS \(GCMP\) est pris en charge par 802.11ac, est plus efficace que AES\-CCMP et offre de meilleures performances pour les clients sans fil. GCMP utilise des clés de chiffrement AES 256 bits.

> [!IMPORTANT]
> Wired Equivalency Privacy \(WEP\) a été la norme de sécurité sans fil d’origine qui a été utilisée pour chiffrer le trafic réseau. Ne déployez pas WEP sur votre réseau, car il existe bien\-des vulnérabilités connues dans cette forme obsolète de la sécurité.

### <a name="active-directorydoman-services-adds"></a>Les Services de domaine Active Directory \(AD DS\)
Les services AD DS fournit une base de données distribuée qui stocke et gère les informations sur les ressources réseau et d’application\-des données spécifiques à partir du répertoire\-applications compatibles. Les administrateurs peuvent utiliser AD DS pour organiser les éléments d’un réseau, tels que les utilisateurs, les ordinateurs et les autres périphériques, en une structure hiérarchique de type contenant-contenu. La structure hiérarchique de relation contenant-contenu inclut la forêt Active Directory, domaines dans la forêt et les unités d’organisation \(unités d’organisation\) dans chaque domaine. Un serveur qui exécute les services AD DS est appelé un *contrôleur de domaine*.  

Services AD DS contiennent les comptes d’utilisateur, les comptes d’ordinateurs et les propriétés du compte qui sont requis par IEEE 802. 1 X et PEAP\-MS\-CHAP v2 pour authentifier les informations d’identification de l’utilisateur et pour évaluer d’autorisation pour les connexions sans fil.

### <a name="active-directory-users-and-computers"></a>Utilisateurs et ordinateurs Active Directory
Active Directory Users and Computers est un composant des services AD DS qui contient les comptes qui représentent des entités physiques, comme un ordinateur, une personne ou un groupe de sécurité. Un *groupe de sécurité* est une collection de comptes d’utilisateur ou ordinateur que les administrateurs peuvent gérer comme une seule unité. Comptes d’utilisateur et ordinateur qui appartiennent à un groupe particulier sont appelés *membres du groupe*.  

### <a name="group-policy-management"></a>Gestion des stratégies de groupe
Gestion des stratégies de groupe Active directory\-en fonction de gestion des modifications et de la configuration des paramètres utilisateur et ordinateur, y compris les informations de sécurité et d’utilisateur. Stratégie de groupe vous permet de définir des configurations pour des groupes d’utilisateurs et ordinateurs. Avec la stratégie de groupe, vous pouvez spécifier des paramètres pour les entrées de Registre, sécurité, installation de logiciels, des scripts, la redirection de dossiers, les services d’installation à distance et maintenance de Internet Explorer. Les paramètres de stratégie de groupe que vous créez sont contenus dans un objet de stratégie de groupe \(GPO\). En associant un objet de stratégie de groupe à des conteneurs de système Active Directory sélectionnés, sites, domaines et unités d’organisation, vous pouvez appliquer des paramètres de l’objet de stratégie de groupe aux utilisateurs et ordinateurs dans ces conteneurs Active Directory. Pour gérer les objets de stratégie de groupe dans une entreprise, vous pouvez utiliser l’éditeur de gestion de stratégie de groupe Microsoft Management Console \(MMC\).  

Ce guide fournit des instructions détaillées sur la façon de spécifier les paramètres dans le réseau sans fil \(IEEE 802.11\) l’extension des stratégies de gestion des stratégies de groupe. Le réseau sans fil \(IEEE 802.11\) configurent des stratégies de domaine\-ordinateurs membres du client sans fil avec la connectivité nécessaire et les paramètres sans fil 802. 1 X authentifiés l’accès sans fil.

### <a name="server-certificates"></a>Certificats de serveur
Ce scénario de déploiement requiert des certificats de serveur pour chaque serveur NPS qui effectue l’authentification de 802. 1 X.  

Un certificat de serveur est un document numérique couramment utilisé pour l’authentification et de protéger les informations sur les réseaux ouverts. Un certificat associe de manière sécurisée une clé publique à l'entité qui détient la clé privée correspondante. Les certificats sont signés numériquement par l’autorité de certification émettrice, et ils peuvent être émis pour un utilisateur, un ordinateur ou un service.  

Une autorité de certification \(autorité de certification\) est une entité chargée d’établir et de se porter garante de l’authenticité des clés publiques appartenant aux sujets \(généralement des utilisateurs ou ordinateurs\) ou d’autres autorités de certification. Activités d’une autorité de certification figurent la liaison des clés publiques à des noms uniques par le biais des certificats auto-signés, la gestion des numéros de série et la révocation de certificats.  

Services de certificats Active Directory \(AD CS\) est un rôle de serveur qui émet des certificats en tant que réseau autorité de certification. Un AD CS de certificat infrastructure, également appelé un *infrastructure à clé publique \(PKI\)*, fournit des services personnalisables pour l’émission et la gestion des certificats pour l’entreprise.

### <a name="eap-peap-and-peap-ms-chap-v2"></a>PEAP, EAP et PEAP\-MS\-CHAP v2
Extensible Authentication Protocol \(EAP\) étend Point\-à\-Point Protocol \(PPP\) en autorisant des méthodes d’authentification supplémentaires qui utilisent des informations d’identification et échanges de longueurs arbitraires. Avec l’authentification EAP, le réseau d’accès client et l’authentificateur \(tels que le serveur NPS\) doit de prendre en charge le même type EAP pour l’authentification doit avoir lieu. Windows Server 2016 inclut une infrastructure EAP, prend en charge deux types EAP et la possibilité de transmettre des messages EAP à NPSs. En utilisant le protocole EAP, vous pouvez prendre en charge les schémas d’authentification supplémentaires, connu sous le nom *types EAP*. Les types EAP sont prises en charge par Windows Server 2016 sont :  

- Transport Layer Security \(TLS\)

- Microsoft Challenge Handshake Authentication Protocol version 2 \(MS\-CHAP v2\)

>[!IMPORTANT]
>Les types EAP forts \(telles que celles qui sont basées sur les certificats\) offrent une meilleure sécurité contre les attaques en\-forcer des attaques, les attaques par dictionnaire et attaques de mot de passe de déchiffrement\-en fonction protocoles d’authentification \(tels que CHAP ou MS\-CHAP version 1\).

Protected EAP \(PEAP\) utilise TLS pour créer un canal chiffré entre un client PEAP, par exemple un ordinateur sans fil et un authentificateur PEAP, par exemple un serveur NPS ou d’autres serveurs RADIUS. PEAP ne spécifie pas une méthode d’authentification, mais il fournit une sécurité supplémentaire pour d’autres protocoles d’authentification EAP \(telles que EAP\-MS\-CHAP v2\) qui peut fonctionner via le canal chiffré TLS fourni par le protocole PEAP. PEAP est utilisé comme méthode d’authentification pour les clients d’accès qui sont connectent au réseau de votre organisation via les types de serveurs d’accès réseau suivants \(NAS\):

- 802. 1 X\-points d’accès sans fil compatible

- 802. 1 X\-commutateurs d’authentification

- Ordinateurs exécutant Windows Server 2016 et le Service d’accès à distance \(RAS\) qui sont configurés en tant que réseau privé virtuel \(VPN\) serveurs, les serveurs DirectAccess ou les deux

- Ordinateurs exécutant Windows Server 2016 et les Services Bureau à distance

PEAP\-MS\-CHAP v2 est plus facile à déployer que EAP\-TLS, car l’authentification utilisateur est effectuée à l’aide du mot de passe\-en fonction des informations d’identification \(nom d’utilisateur et mot de passe\), au lieu de les certificats ou des cartes à puce. Uniquement NPS ou autres serveurs RADIUS doit être un certificat. Le certificat de serveur NPS est utilisé par le serveur NPS pendant le processus d’authentification pour prouver son identité aux clients PEAP.

Ce guide fournit des instructions pour configurer vos clients sans fil et le serveur NPS\(s\) à utiliser le protocole PEAP\-MS\-CHAP v2 pour 802. 1 X de l’accès authentifié.

### <a name="network-policy-server"></a>Serveur NPS (Network Policy Server)
Network Policy Server \(NPS\) vous permet de configurer et gérer des stratégies de réseau à l’aide de Remote Authentication Dial de manière centralisée\-In User Service \(RADIUS\) serveur et proxy RADIUS. NPS est nécessaire lorsque vous déployez 802. 1 X accès sans fil.

Lorsque vous configurez vos points de l’accès sans fil 802. 1 X en tant que clients RADIUS dans NPS, NPS traite les demandes de connexion envoyées par les points d’accès. Pendant le traitement de demande de connexion, NPS effectue l’authentification et l’autorisation. L’authentification détermine si le client a présenté des informations d’identification valides. Si NPS authentifie correctement le client demandeur, NPS détermine ensuite si le client est autorisé à établir la connexion demandée et autorise ou refuse la connexion. Cela est expliqué plus en détail comme suit :

#### <a name="authentication"></a>Authentification

PEAP mutuelle réussie\-MS\-authentification CHAP v2 comporte deux parties principales :

1.  Le client s’authentifie le serveur NPS.  Pendant cette phase de l’authentification mutuelle, le serveur NPS envoie son certificat de serveur à l’ordinateur client afin que le client peut vérifier l’identité du serveur NPS avec le certificat. Pour s’authentifier le serveur NPS, l’ordinateur client doit approuver l’autorité de certification qui a émis le certificat de serveur NPS. Le client approuve cette autorité de certification lorsque le certificat de l’autorité de certification est présent dans le magasin de certificats Autorités de Certification racine de confiance sur l’ordinateur client.

    Si vous déployez votre propre autorité de certification privée, le certificat d’autorité de certification est automatiquement installé dans le magasin de certificats Autorités de Certification racine de confiance pour l’utilisateur actuel et pour l’ordinateur Local lorsque la stratégie de groupe est actualisée sur l’ordinateur client membre du domaine. Si vous décidez de déployer des certificats de serveur à partir d’une autorité de certification publique, assurez-vous que le certificat d’autorité de certification publique est déjà dans le magasin de certificats Autorités de Certification racine de confiance.  

2.  Le serveur NPS authentifie l’utilisateur. Une fois que l’authentification du client au serveur NPS, le client envoie le mot de passe utilisateur\-en fonction des informations d’identification au serveur NPS, qui vérifie les informations d’identification de l’utilisateur par rapport à la base de données de comptes utilisateur dans les Services de domaine Active Directory \(AD DS\).

Si les informations d’identification sont valides et que l’authentification réussit, le serveur NPS commence la phase d’autorisation de traitement de la demande de connexion. Si les informations d’identification ne sont pas valides et que l’authentification échoue, NPS envoie un message de rejet d’accès et la demande de connexion est refusée.  

#### <a name="authorization"></a>Authorization

Le serveur NPS effectue d’autorisation comme suit :  

1. NPS vérifie pour connaître les restrictions dans le cadran de compte utilisateur ou ordinateur\-dans Propriétés dans AD DS. Chaque compte utilisateur et ordinateur dans Active Directory Users and Computers comprend plusieurs propriétés, y compris celles que l'on trouve sur le **Dial\-dans** onglet. Dans cet onglet, dans **l’autorisation d’accès réseau**, si la valeur est **autoriser l’accès**, l’utilisateur ou l’ordinateur est autorisé à se connecter au réseau. Si la valeur est **refuser l’accès**, l’utilisateur ou l’ordinateur n’est pas autorisé à se connecter au réseau. Si la valeur est **contrôler l’accès via la stratégie de réseau de NPS**, NPS évalue les stratégies réseau configurées pour déterminer si l’utilisateur ou l’ordinateur est autorisé à se connecter au réseau. 

2. NPS traite ensuite ses stratégies de réseau pour rechercher une stratégie qui correspond à la demande de connexion. Si une stratégie de correspondance est trouvée, NPS accorde ou refuse la connexion en fonction de configuration de cette stratégie.  

Si l’authentification et autorisation sont réussies, et si la stratégie réseau correspondante accorde l’accès, NPS accorde l’accès au réseau, et l’utilisateur et l’ordinateur peuvent se connecter aux ressources réseau pour lesquels ils disposent d’autorisations.  

>[!NOTE]
>Pour déployer l’accès sans fil, vous devez configurer les stratégies du serveur NPS. Ce guide fournit des instructions pour utiliser le **configurer 802. 1 X wizard** dans NPS pour créer des stratégies de serveur NPS pour 802. 1 X de l’accès sans fil authentifié.  

### <a name="bootstrap-profiles"></a>Profils d’amorçage
802. 1 X\-authentifié des réseaux sans fil, les clients sans fil doivent fournir des informations d’identification de sécurité qui sont authentifiées par un serveur RADIUS afin de vous connecter au réseau. Pour EAP protégé \[PEAP\]\-Microsoft Challenge Handshake Authentication Protocol version 2 \[MS\-CHAP v2\], les informations d’identification de sécurité sont un nom d’utilisateur et le mot de passe. Pour le protocole EAP\-Transport Layer Security \[TLS\] ou PEAP\-TLS, les informations d’identification de sécurité sont des certificats, tels que les certificats utilisateur et ordinateur client ou de cartes à puce.

Lorsque vous vous connectez à un réseau qui est configuré pour effectuer le PEAP\-MS\-CHAP v2, PEAP\-TLS ou EAP\-l’authentification TLS, par défaut, les clients sans fil doivent également valider un certificat d’ordinateur qui est de Windows envoyé par le serveur RADIUS. Le certificat d’ordinateur envoyé par le serveur RADIUS pour chaque session d’authentification est communément appelé un certificat de serveur.

Comme mentionné précédemment, vous pouvez émettre vos serveurs RADIUS de leur certificat de serveur dans un des deux façons : à partir d’une autorité de certification commerciale \(telle que VeriSign, Inc.,\), ou à partir d’une autorité de certification privée que vous déployez sur votre réseau. Si le serveur RADIUS envoie un certificat d’ordinateur qui a été émis par une autorité de certification commerciale qui possède déjà un certificat racine installé dans le magasin de certificats des autorités de Certification racine de confiance du client, puis le client sans fil peut valider le serveur RADIUS certificat d’ordinateur, indépendamment de si le client sans fil a rejoint le domaine Active Directory. Dans ce cas, le client sans fil peut se connecter au réseau sans fil, et vous pouvez ensuite joindre l’ordinateur au domaine.

>[!NOTE]
>Le comportement que le client valider le certificat de serveur peut être désactivé, mais la désactivation de la validation de certificat de serveur n’est pas recommandée dans les environnements de production.

Les profils de démarrage sans fil sont des profils temporaires qui sont configurés de manière à permettre aux utilisateurs de client sans fil pour se connecter à la 802. 1 X\-réseau sans fil d’authentifié avant de l’ordinateur est joint au domaine, et\/ou avant l’utilisateur a correctement connecté au domaine à l’aide d’un ordinateur sans fil donné pour la première fois.  Cette section résume le problème se produit lorsque vous tentez de joindre un ordinateur sans fil au domaine, ou pour un utilisateur d’utiliser un domaine\-joint à un ordinateur sans fil pour la première fois ouvrir une session le domaine.

Pour les déploiements dans lesquels l’utilisateur ou un administrateur informatique ne peut pas connecter physiquement un ordinateur au réseau Ethernet câblé à joindre l’ordinateur au domaine et l’ordinateur n’a pas la délivrance nécessaire racine du certificat d’autorité de certification installé dans son  **Autorités de Certification racine** magasin de certificats, vous pouvez configurer les clients sans fil avec un profil de connexion sans fil temporaire, appelé un *amorcer profil*pour se connecter au réseau sans fil.

Un *amorcer profil* supprime la nécessité de valider le certificat d’ordinateur du serveur RADIUS. Cette configuration temporaire permet à l’utilisateur sans fil à joindre l’ordinateur au domaine, moment auquel les réseau sans fil \(IEEE 802.11\) stratégies sont appliquées et le certificat d’autorité de certification de racine approprié est automatiquement installé sur le ordinateur.

Lors de la stratégie de groupe est appliquée, un ou plusieurs profils de connexion sans fil qui appliquent la configuration requise pour l’authentification mutuelle sont appliquées sur l’ordinateur ; le profil de démarrage n’est plus nécessaire et est supprimé. Après avoir joint l’ordinateur au domaine et le redémarrage de l’ordinateur, l’utilisateur peut utiliser une connexion sans fil pour se connecter au domaine.

Pour une vue d’ensemble du processus de déploiement de l’accès sans fil à l’aide de ces technologies, consultez [vue d’ensemble du déploiement de l’accès sans fil](b-wireless-access-deploy-overview.md).
