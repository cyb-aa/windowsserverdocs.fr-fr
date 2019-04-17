---
title: Déployer le mot de passe-802. 1 X accès sans fil authentifié
description: Cette rubrique fait partie du guide de mise en réseau de Windows Server2016 «Accès sans fil authentifié 802. 1-mot de passe de déployer X»
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ff06ba23-9c0f-49ec-8f7b-611cf8d73a1b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0ded8a273a9ad464b44fa7245db58d0fd05f06a2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-password-based-8021x-authenticated-wireless-access"></a>Déployer Password\-802. 1 X accès sans fil authentifié

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Il s’agit d’un guide d’accompagnement pour le serveur Windows&reg; 2016 Guide du réseau de base. Le Guide de réseau de base fournit des instructions pour la planification et déploiement des composants requis pour un réseau pleinement fonctionnel et un nouveau domaine ActiveDirectory® dans une nouvelle forêt.

Ce guide explique comment développer un réseau de base en fournissant des instructions sur le déploiement Institute de Electrical and Electronics Engineers \(IEEE\) 802.1X\-accès sans fil IEEE 802.11 à l’aide de Protected Extensible Authentication Protocol – MicrosoftChallenge Handshake Authentication Protocol version2 authentifié \ (v2\ PEAP\-MS\-CHAP).

Étant donné que PEAP\-MS\-CHAP v2 requiert que les utilisateurs fournissent des informations d’identification password\ au lieu d’un certificat pendant le processus d’authentification, il est généralement plus facile et moins coûteux à déployer que EAP\-TLS ou PEAP\-TLS.

>[!NOTE]
>Dans ce guide, IEEE 802. 1 X accès sans fil authentifié avec PEAP\-MS\-CHAP v2 est abrégée en «accès sans fil» et «Accès Wi-Fi.»

## <a name="about-this-guide"></a>À propos de ce guide
Ce guide, conjointement avec les guides de la configuration requise décrite ci-dessous, fournit des instructions sur la façon de déployer l’infrastructure d’accès Wi-Fi suivante.  

- Un ou plusieurs 802.1X\-capable 802.11accès sans fil points \(APs\).

- \(ADDS\) utilisateurs et ordinateurs des Services de domaine ActiveDirectory.

- Gestion des stratégies de groupe.

- Un ou plusieurs serveurs NPS \(NPS\).

- Certificats de serveur pour les ordinateurs exécutant NPS.

- Les ordinateurs clients exécutant Windows sans fil&reg; 10, Windows8.1 ou Windows8.

### <a name="dependencies-for-this-guide"></a>Dépendances de ce guide

Pour déployer correctement authentifié sans fil avec ce guide, vous devez disposer d’un environnement réseau et de domaine avec toutes les technologies requises déployés. Vous devez également avoir déployé sur vos serveurs NPS authentification des certificats de serveur.

Les sections suivantes fournissent des liens vers la documentation qui vous explique comment déployer ces technologies.

#### <a name="network-and-domain-environment-dependencies"></a>Dépendances d’environnement réseau et du domaine

Ce guide est conçu pour les administrateurs système et réseau ayant suivi les instructions fournies dans Windows Server2016 **Guide réseau de base** pour déployer un réseau de base, ou pour ceux ayant déjà déployé les technologies de base inclus dans le réseau de base, y compris les services ADDS, Domain Name System \(DNS\), Dynamic Host Configuration Protocol \(DHCP\), TCP\/IP, NPS et WindowsInternetNameService \(WINS\).  

Le Guide du réseau principal est disponible aux emplacements suivants:

- Windows Server2016 [Guide réseau de base](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide) est disponible dans la bibliothèque technique Windows Server2016. 

- Le [Guide réseau de base](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683) est également disponible au format Word dans la Galerie TechNet, à [https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683).

#### <a name="server-certificate-dependencies"></a>Dépendances de certificat de serveur
Il existe deux options disponibles pour l’inscription de serveurs d’authentification avec des certificats de serveur pour une utilisation avec l’authentification 802. 1 X - déployer votre propre infrastructure à clé publique à l’aide des Services de certificats ActiveDirectory \(ADCS\) ou utiliser des certificats de serveur qui sont inscrits par une autorité de certification publique \(CA\).

##### <a name="ad-cs"></a>SERVICES ADCS
Les administrateurs réseau et système de déploiement sans fil authentifié doivent suivre les instructions fournies dans le Windows Server2016 Core Network Companion Guide, **déployer des certificats de serveur pour les déploiements sans fil et de 802.1 X câblé**. Ce guide explique comment déployer et utiliser les services ADCS pour l’inscription automatique des certificats de serveur sur les ordinateurs exécutant NPS.

Ce guide est disponible à l’emplacement suivant.

- Le Guide d’accompagnement du réseau Windows Server2016 Core [déployer des certificats de serveur pour 802.1 X câblé et sans fil déploiements](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments?f=255&MSPPError=-2147217396) au format HTML dans la bibliothèque technique. 

##### <a name="public-ca"></a>Autorité de certification publique
Vous pouvez acheter des certificats de serveur à partir d’une autorité de certification publique, telle que VeriSign, que les ordinateurs clients approuvent déjà. 

Un ordinateur client approuve une autorité de certification lorsque le certificat d’autorité de certification est installé dans le magasin de certificats Autorités de Certification racine de confiance. Par défaut, les ordinateurs exécutant Windows ont plusieurs certificats d’autorité de certification publiques installés dans leur certificat autorités de Certification racine de confiance stocker.  

Il est recommandé de consulter les guides de conception et de déploiement pour chacune des technologies qui sont utilisés dans ce scénario de déploiement. Ces guides peuvent vous aider à déterminer si ce scénario de déploiement fournit les services et la configuration dont vous avez besoin pour le réseau de votre organisation.

### <a name="requirements"></a>Configuration requise
Voici la configuration requise pour le déploiement d’une infrastructure d’accès sans fil en utilisant le scénario décrit dans ce guide:

- Avant de déployer ce scénario, vous devez tout d’abord acheter 802.1X\-points d’accès sans fil compatibles pour fournir une couverture sans fil dans les emplacements de votre choix sur votre site. La section de planification de ce guide aide à déterminer les fonctionnalités de que vos points d’accès doivent prendre en charge.

- Les \(ADDS\) Services de domaine ActiveDirectory est installé, tout comme les autres technologies de réseau nécessaires, en suivant les instructions dans le Guide du réseau Windows Server2016 Core.  

- Les services ADCS est déployé, et les certificats de serveur inscrits sur les serveurs NPS. Ces certificats sont requis lorsque vous déployez la méthode d’authentification basée sur les certificate\ PEAP\-MS\-CHAP v2 qui est utilisée dans ce guide.

- Un membre de votre organisation est familiarisé avec les normes IEEE 802.11 qui sont pris en charge par vos points d’accès sans fil et les cartes réseau sans fil qui sont installés dans les ordinateurs clients et les périphériques sur votre réseau. Par exemple, une personne de votre organisation est familiarisée avec les types de fréquence radio, l’authentification sans fil 802.11 \ (WPA2 ou WPA\) et les chiffrements \(AES or TKIP\).

## <a name="what-this-guide-does-not-provide"></a>Ce que ce guide ne fournit pas  
Voici certains éléments de que ce guide ne fournit pas:

### <a name="comprehensive-guidance-for-selecting-8021x-capable-wireless-access-points"></a>Guide complet de sélection 802.1X\-points d’accès sans fil compatibles

Car il existent de nombreuses différences entre les modèles et marques de 802.1X\-points d’accès sans fil compatibles, ce guide ne fournit pas des informations détaillées sur:  

- Pour déterminer quelle marque ou le modèle du point d’accès sans fil est meilleure adapté à vos besoins.

- Le déploiement des points d’accès sans fil sur votre réseau physique.

- Avancé configuration du point d’accès sans fil, par exemple pour sans fil \(VLANs\) réseaux locaux virtuels.

- Obtenir des instructions sur la façon de configurer des attributs spécifiques au vendor\ point d’accès sans fil dans NPS.

En outre, la terminologie et les noms de paramètres varient entre les modèles et marques de point d’accès sans fil et peut ne pas correspondent les noms des paramètres génériques qui sont utilisés dans ce guide. Pour plus d’informations de configuration de point d’accès sans fil, vous devez passer en revue la documentation fournie par le fabricant de vos points d’accès sans fil.

### <a name="instructions-for-deploying-nps-server-certificates"></a>Instructions pour le déploiement de certificats de serveur NPS
  
Il existe deux possibilités pour le déploiement de certificats de serveur NPS. Ce guide ne fournit pas de conseils détaillés pour vous aider à déterminer quel autre répondra le mieux à vos besoins. En règle générale, toutefois, les choix que vous êtes confronté sont:

- Achetez des certificats à partir d’une autorité de certification publique, telle que VeriSign, qui sont déjà approuvé par les clients Windows. Cette option est généralement recommandée pour les réseaux plus petits.

- Déploiement d’un \(PKI\) Infrastructure à clé publique sur votre réseau à l’aide des services ADCS. Cela est recommandé pour la plupart des réseaux, et les instructions pour le déploiement de certificats de serveur avec les services ADCS sont disponibles dans le guide de déploiement mentionnés précédemment.

### <a name="nps-network-policies-and-other-nps-settings"></a>Stratégies de réseau NPS et d’autres paramètres de NPS

À l’exception des paramètres de configuration effectuées lorsque vous exécutez le **configurer 802. 1 X** Assistant, comme indiqué dans ce guide, ce guide ne fournit pas des informations détaillées pour la configuration manuelle des conditions NPS, contraintes ou autres paramètres NPS.

### <a name="dhcp"></a>DHCP

Ce guide de déploiement ne fournit pas d’informations sur la conception ou déploiement de sous-réseaux DHCP pour les réseaux locaux sans fil.

## <a name="technology-overviews"></a>Vues d’ensemble de la technologie
Voici les vues d’ensemble des technologies de déploiement de l’accès sans fil:

### <a name="ieee-8021x"></a>IEEE 802. 1 X
La norme IEEE 802. 1 X standard définit le contrôle d’accès réseau basé sur les LPT1\ qui est utilisé pour fournir un accès réseau authentifié à des réseaux Ethernet. Ce contrôle d’accès réseau basé sur LPT1\ utilise les caractéristiques physiques de l’infrastructure de réseau local commuté pour authentifier les périphériques connectés à un port LAN. L’accès au port peut être refusé en cas d’échec du processus d’authentification. Bien que cette norme ait été conçue pour les réseaux Ethernet câblés, elle a été adaptée aux réseaux locaux sans fil 802.11.

### <a name="8021x-capable-wireless-access-points-aps"></a>802.1X\-\(APs\) les points d’accès sans fil compatibles
Ce scénario nécessite le déploiement d’un ou plusieurs 802.1X\-compatibles avec points d’accès sans fil qui sont compatibles avec le protocole Remote Authentication Dial\-In utilisateur Service \(RADIUS\).

802. 1 X et les points d’accès conforme RADIUS\, lors du déploiement dans une infrastructure RADIUS avec un serveur RADIUS tel qu’un serveur NPS, sont appelés *clients RADIUS*.

### <a name="wireless-clients"></a>Clients sans fil
Ce guide fournit des détails de la configuration complète de fournir 802. 1 X un accès authentifié pour les utilisateurs membres domain\ qui se connectent au réseau avec les ordinateurs clients sans fil exécutant Windows10, Windows8.1 et Windows8. Ordinateurs doivent être joints au domaine afin d’établir un accès authentifié.

>[!NOTE]
>Vous pouvez également utiliser des ordinateurs qui exécutent Windows Server2016, Windows Server2012R2 et Windows Server2012 en tant que clients sans fil.

### <a name="support-for-ieee-80211-standards"></a>Prise en charge pour IEEE 802.11normes
Pris en charge des systèmes d’exploitation Windows et Windows Server fournissent une prise en charge intégrée de mise en réseau sans fil 802.11. Dans ces systèmes d’exploitation, une carte réseau sans fil 802.11 s’affiche en tant qu’une connexion réseau sans fil dans le Centre réseau et partage. 

Bien qu’il soit intégrée la prise en charge pour la mise en réseau sans fil 802.11, les composants de Windows sans fil dépendent des éléments suivants:

- Les fonctionnalités de la carte réseau sans fil. La carte réseau sans fil installée doit prendre en charge le réseau local sans fil ou les normes de sécurité sans fil dont vous avez besoin. Par exemple, si la carte réseau sans fil ne prend pas en charge \(WPA\) Wi\-Fi Protected Access, vous ne pouvez pas activer ou configurer les options de sécurité WPA.

- Les fonctionnalités du pilote de carte réseau sans fil. Pour vous permettre de configurer les options de réseau sans fil, le pilote de la carte réseau sans fil doit prendre en charge la génération de rapports de toutes ses fonctionnalités de Windows. Vérifiez que le pilote de votre carte réseau sans fil est écrit pour les fonctionnalités de votre système d’exploitation. Assurez-vous également que le pilote est la version la plus récente en vérifiant MicrosoftUpdate ou le site Web du fournisseur de carte réseau sans fil.

Le tableau suivant affiche les vitesses de transmission et les fréquences pour des normes communes de sans fil IEEE 802.11.

|Normes|Fréquences|Débits de Transmission|Utilisation|
|-------------|---------------|--------------------------|---------|  
|802.11|Bande S\ industrielle, scientifique et médical fréquences \(ISM\) \ (2.4 à 2.5GHz\)|2mégabits par seconde \(Mbps\)|Obsolète. Couramment utilisés.|  
|802. 11 b|ISM S\ hors-bande|11Mbits/s|Couramment utilisés.|  
|802. 11 a|ISM hors-bande C\ \ (5,725 à 5.875GHz\)|54Mbits/s|Pas couramment utilisé dû à une plage limitée et de dépenses.|  
|802. 11g|ISM S\ hors-bande|54Mbits/s|Largement utilisé. Périphériques 802. 11g sont compatibles avec 802. 11 b périphériques.|  
|802.11n \2.4 et 5.0GHz|ISM S\-bande et C\|250Mbits/s|Les périphériques basés sur pre\ ratification IEEE 802.11n standard est désormais disponibles en août2007. 802.11n de nombreux appareils sont compatibles avec 802. 11 a, b et les périphériques g.|
|802.11ac |5GHz |6,93Gbits/s |802.11ac, approuvé par la norme IEEE en 2014, plus évolutive et plus rapide que 802.11n et est déployé dans lequel les deux points d’accès et les clients sans fil prennent en charge.|

### <a name="wireless-network-security-methods"></a>Méthodes de sécurité réseau sans fil
**Sans fil des méthodes de sécurité réseau** est un regroupement d’authentification sans fil informel \ (parfois appelé sécurité\ sans fil) et le chiffrement de sécurité sans fil. Chiffrement et authentification sans fil sont utilisés dans des paires pour empêcher les utilisateurs non autorisés d’accéder au réseau sans fil et pour protéger les transmissions sans fil. 

Lorsque vous configurez les paramètres de sécurité sans fil dans la stratégie réseau sans fil des stratégies de groupe, il existe plusieurs combinaisons à choisir. Toutefois, uniquement le WPA2\-entreprise WPA\-Enterprise et ouvert avec 802. 1 X normes d’authentification sont pris en charge pour les déploiements sans fil 802.1 X authentifié.

>[!NOTE]
>Lors de la configuration des stratégies de réseau sans fil, vous devez sélectionner **WPA2\-entreprise**, **WPA\-entreprise**, ou **ouvert avec 802. 1 X** afin d’accéder aux paramètres EAP qui sont obligatoires pour 802. 1 X authentifié déploiements sans fil.  

#### <a name="wireless-authentication"></a>Authentification sans fil
Ce guide recommande que l’utilisation des normes suivantes, l’authentification sans fil 802. 1 X authentifié déploiements sans fil.  

**Wi\-Fi Protected Access – entreprise \(WPA\-Enterprise\)** WPA est une norme intérimaire développée par l’Alliance Wi-Fi pour se conformer le protocole de sécurité sans fil 802.11. Le protocole WPA a été développé en réponse à un certain nombre de défauts graves qui ont été détectées dans le protocole Wired Equivalent Privacy \(WEP\) précédent.

Entreprise-WPA\ offre une sécurité accrue sur WEP par:  

1. Exiger l’authentification qui utilise l’infrastructure X EAP 802.1dans le cadre de l’infrastructure qui garantit une authentification mutuelle centralisée et la gestion de clés dynamique  

2. Amélioration de la valeur de contrôle d’intégrité \(ICV\) avec un \(MIC\) vérification de l’intégrité de Message, pour protéger l’en-tête et la charge utile  

3. Implémentation d’un compteur d’images pour empêcher les attaques par relecture  

**Wi\-Fi Protected Access 2 – entreprise \(WPA2\-Enterprise\)** comme le WPA\-entreprise standard, entreprise WPA2\ utilise 802. 1 X et infrastructure EAP. WPA2\-entreprise fournit une protection renforcée des données pour plusieurs utilisateurs et les grands réseaux gérés. Entreprise-WPA2\ est un protocole robuste qui est conçu pour empêcher tout accès non autorisé réseau en vérifiant les utilisateurs du réseau par le biais d’un serveur d’authentification.  

#### <a name="wireless-security-encryption"></a>Chiffrement de sécurité sans fil
Chiffrement de sécurité sans fil est utilisé pour protéger les transmissions sans fil qui sont envoyées entre le client sans fil et le point d’accès sans fil. Chiffrement de sécurité sans fil est utilisé conjointement avec la méthode d’authentification de sécurité réseau sélectionnée. Par défaut, les ordinateurs exécutant Windows10, Windows8.1 et Windows8 prend en charge deux normes de chiffrement:

1. **Temporal Key Integrity Protocol** \(TKIP\) est un protocole de chiffrement plus ancien qui a été conçu pour fournir un chiffrement plus sécurisé sans fil celui fourni par le protocole \(WEP\) Wired Equivalent Privacy inefficace. TKIP a été conçu par la norme IEEE 802.11i groupe et l’Alliance Wi\-Fi pour remplacer WEP sans nécessiter le remplacement de matériel hérité de tâches. TKIP est une suite d’algorithmes qui encapsule la charge utile WEP et permet aux utilisateurs de matériel Wi-Fi d’ancienne génération mise à niveau vers TKIP sans remplacer du matériel. Tout comme WEP, TKIP utilise l’algorithme de chiffrement RC4 flux comme base. Le nouveau protocole chiffre Toutefois, chaque paquet de données avec une clé de chiffrement unique, et les clés sont beaucoup plus puissant que celles par WEP. Bien que TKIP est utile pour la mise à niveau de sécurité sur des appareils plus anciens qui ont été conçus pour utiliser le WEP uniquement, il ne traite pas tous les problèmes de sécurité d’accès au réseau local sans fil et dans la plupart des cas n’est pas suffisamment robuste pour protéger le gouvernement sensible ou des transmissions de données d’entreprise.  

2. **Norme de chiffrement avancé** \(AES\) est le protocole de chiffrement par défaut pour le chiffrement des données commerciales et gouvernementales. AES offre un niveau élevé de sécurité des transmissions sans fil que WEP ou TKIP. Contrairement aux TKIP et WEP, AES nécessite du matériel sans fil qui prend en charge la norme AES. AES est un chiffrement à clé symmetric\ standard qui utilise trois chiffrements par bloc, AES\-128, 192-AES\ et AES\-256.

Dans Windows Server2016, les méthodes de chiffrement sans fil AES\ suivantes sont disponibles pour la configuration dans les propriétés du profil sans fil lorsque vous sélectionnez une méthode d’authentification de l’entreprise WPA2\, qui est recommandée.

1. **AES\-CCMP**. Compteur \(CCMP\) Mode Cipher Block chaînage Message Authentication Code Protocol implémente la norme 802.11i et est conçu pour le chiffrement de sécurité plus élevé que celui fourni par WEP utilise les clés de chiffrement AES 128bits.
2. **AES\-GCMP**. \(GCMP\) GALOIS Counter Mode Protocol est pris en charge par 802.11ac, il est plus efficace que AES\-CCMP et offre de meilleures performances pour les clients sans fil. GCMP utilise les clés de chiffrement AES 256bits.

> [!IMPORTANT]
> Câblé équivalence de confidentialité \(WEP\) était la sécurité de sans fil d’origine standard qui a été utilisée pour chiffrer le trafic réseau. Vous ne devez pas déployer WEP sur votre réseau, car il existe bien connu des vulnérabilités dans ce formulaire obsolète de la sécurité.

### <a name="active-directory-doman-services-ad-ds"></a>\(ADDS\) des Services de domaine ActiveDirectory
Les services ADDS fournit une base de données distribuée qui stocke et gère des informations sur les ressources réseau et l’appel à leurs propres données à partir d’applications prenant en charge Directory.. Les administrateurs peuvent utiliser ADDS pour organiser les éléments d’un réseau, tels que les utilisateurs, ordinateurs et autres appareils, en une structure hiérarchique de type contenant-contenu. La structure hiérarchique de type contenant-contenu inclut la forêt ActiveDirectory, domaines dans la forêt et les unités d’organisation \(OUs\) dans chaque domaine. Un serveur qui exécute les services ADDS est appelé un *contrôleur de domaine*.  

Services ADDS contiennent les comptes d’utilisateurs, les comptes d’ordinateurs et les propriétés du compte qui sont requis par IEEE 802. 1 X et PEAP\-MS\-CHAP v2 pour authentifier les informations d’identification utilisateur et d’évaluer d’autorisation pour les connexions sans fil.

### <a name="active-directory-users-and-computers"></a>Utilisateurs et ordinateurs ActiveDirectory
Les utilisateurs et ordinateurs Active est un composant des services ADDS qui contient les comptes qui représentent des entités physiques, comme un ordinateur, une personne ou un groupe de sécurité. Un *groupe de sécurité* est un ensemble de comptes d’utilisateur ou d’ordinateur que les administrateurs peuvent gérer comme une seule unité. Comptes d’utilisateurs et d’ordinateurs qui appartiennent à un groupe particulier sont appelés *membres de groupes de*.  

### <a name="group-policy-management"></a>Gestion des stratégies de groupe
Gestion des stratégies de groupe permet Directory.-modifications et configuration Gestion des paramètres utilisateur et ordinateur, y compris les informations de sécurité et d’utilisateur. Stratégie de groupe vous permet de définir la configuration de groupes d’utilisateurs et ordinateurs. Avec la stratégie de groupe, vous pouvez spécifier des paramètres pour les entrées de Registre, sécurité, installation logicielle, scripts, la redirection de dossiers, les services d’installation à distance et maintenance de Internet Explorer. La stratégie de groupe de paramètres que vous créez sont contenus dans une stratégie de groupe objet \(GPO\). En associant un objet de stratégie de groupe à des conteneurs de système ActiveDirectory sélectionnés: sites, des domaines et des unités d’organisation, vous pouvez appliquer des paramètres de l’objet de stratégie de groupe aux utilisateurs et ordinateurs dans ces conteneurs ActiveDirectory. Pour gérer les objets de stratégie de groupe dans l’entreprise, vous pouvez utiliser l’éditeur de gestion de stratégie de groupe MicrosoftManagement Console \(MMC\).  

Ce guide fournit des instructions détaillées sur la spécification des paramètres du réseau sans fil \ IEEE (802.11\) l’extension des stratégies de gestion des stratégies de groupe. Le réseau sans fil \ IEEE (802.11\) stratégies configurer des ordinateurs clients sans fil membres de domain\ avec la connectivité nécessaire et les paramètres sans fil 802. 1 X de l’accès sans fil authentifié.

### <a name="server-certificates"></a>Certificats de serveur
Ce scénario de déploiement requiert des certificats de serveur pour chaque serveur NPS qui effectue l’authentification de 802. 1 X.  

Un certificat de serveur est un document numérique couramment utilisé pour l’authentification et de protéger les informations sur les réseaux ouverts. Un certificat lie de manière sécurisée une clé publique à l’entité qui détient la clé privée correspondante. Les certificats sont signés numériquement par l’autorité de certification émettrice et ils peuvent être émis pour un utilisateur, un ordinateur ou un service.  

Une autorité de certification \(CA\) est une entité responsable de l’établissement et la garantie de l’authenticité des clés publiques appartenant aux sujets \ (généralement des utilisateurs ou Directory\) ou d’autres autorités de certification. Les activités d’une autorité de certification figurent la liaison de clés publiques à des noms uniques par le biais de certificats auto-signés, la gestion des numéros de série et la révocation de certificats.  

Les \(ADCS\) Services de certificats ActiveDirectory est un rôle de serveur qui émet des certificats en tant qu’un autorité de certification de réseau. Un ADCS certificat infrastructure, également appelé un *infrastructure à clé publique \(PKI\)*, fournit des services personnalisables pour l’émission et la gestion des certificats pour l’entreprise.

### <a name="eap-peap-and-peap-ms-chap-v2"></a>EAP et PEAP PEAP\-MS\-CHAP v2
Extensible Authentication Protocol \(EAP\) étend Point\-celle-ci-Point Protocol \(PPP\) en autorisant des méthodes d’authentification supplémentaires qui utilisent les informations d’identification et échange de longueurs arbitraires. Avec l’authentification EAP, à la fois l’accès réseau client et l’authentificateur \ (par exemple, le serveur NPS) doit prendre en charge le même type EAP pour l’authentification réussie pour se produire. Windows Server2016 inclut une infrastructure EAP, prend en charge deux types EAP et la possibilité de transmettre des messages EAP pour les serveurs NPS. En utilisant le protocole EAP, vous pouvez prendre en charge les schémas d’authentification supplémentaire, appelés *types de protocoles EAP*. Les types EAP sont pris en charge par Windows Server2016 sont:  

- Sécurité de la couche de transport \(TLS\)

- MicrosoftChallenge Handshake Authentication Protocol version2 \ (v2\ MS\-CHAP)

>[!IMPORTANT]
>Les types EAP forts \ (telles que celles qui sont basées sur certificates\) offrent une meilleure sécurité contre les attaques en force brute\, aux attaques par dictionnaire et mot de passe de déchiffrement des attaques de protocoles d’authentification password\ \ (par exemple, CHAP ou MS\-CHAP 1\ version).

Protégé EAP \(PEAP\) utilise TLS pour créer un canal chiffré entre un client PEAP, par exemple, un ordinateur sans fil et un authentificateur PEAP, par exemple, un serveur NPS ou d’autres serveurs RADIUS. PEAP ne spécifie pas une méthode d’authentification, mais il offre une sécurité supplémentaire pour d’autres protocoles d’authentification EAP \ (par exemple, v2\ EAP\-MS\-CHAP) qui peut fonctionner par le biais du canal chiffré TLS fourni par le protocole PEAP. PEAP est utilisée comme méthode d’authentification pour les clients d’accès qui sont connectent au réseau de votre organisation via les types de réseau accès serveurs \(NASs\) suivants:

- 802.1X\-points d’accès sans fil compatibles

- 802.1X\-commutateurs d’authentification

- Les ordinateurs exécutant Windows Server2016 et le Service d’accès à distance \(RAS\) sont configurés en tant que réseau privé virtuel réseau \(VPN\) serveurs, les serveurs DirectAccess ou les deux

- Ordinateurs exécutant Windows Server2016 et des Services Bureau à distance

PEAP\-MS\-CHAP v2 est plus facile à déployer que EAP\-TLS, car l’authentification des utilisateurs est effectuée à l’aide des informations d’identification password\ \ (nom d’utilisateur et password\), au lieu de certificats ou des cartes à puce. Uniquement NPS ou autres serveurs RADIUS doivent posséder un certificat. Le certificat de serveur NPS est utilisé par le serveur NPS pendant le processus d’authentification pour prouver son identité aux clients PEAP.

Ce guide fournit des instructions pour configurer vos clients sans fil et votre server\(s\) NPS pour utiliser PEAP\-MS\-CHAP v2 pour 802. 1 X un accès authentifié.

### <a name="network-policy-server"></a>Serveur de stratégie réseau
Réseau serveur NPS \(NPS\) vous permet de configurer et gérer des stratégies de réseau à l’aide de serveur distant authentification Dial\-In utilisateur Service \(RADIUS\) et proxy RADIUS de manière centralisée. NPS est nécessaire lorsque vous déployez 802. 1 X accès sans fil.

Lorsque vous configurez vos points de l’accès sans fil 802. 1 X en tant que clients RADIUS dans NPS, NPS traite les demandes de connexion envoyées par les points d’accès. Pendant le traitement de demande de connexion, NPS effectue l’authentification et l’autorisation. Authentification détermine si le client a présenté les informations d’identification valides. Si NPS authentifie correctement le client demandeur, NPS détermine ensuite si le client est autorisé à établir la connexion demandée et autorise ou refuse la connexion. Cela est expliqué plus en détail comme suit:

#### <a name="authentication"></a>Authentification

Authentification mutuelle PEAP\-MS\-CHAP v2 de réussie comporte deux parties principales:

1.  Le client authentifie le serveur NPS.  Pendant cette phase de l’authentification mutuelle, le serveur NPS envoie son certificat de serveur à l’ordinateur client afin que le client peut vérifier l’identité du serveur NPS avec le certificat. Pour authentifier correctement le serveur NPS, l’ordinateur client doit approuver l’autorité de certification qui a émis le certificat de serveur NPS. Le client approuve cette autorité de certification lorsque le certificat de l’autorité de certification est présent dans le magasin de certificats Autorités de Certification racine de confiance sur l’ordinateur client.

    Si vous déployez votre propre autorité de certification privée, le certificat d’autorité de certification est automatiquement installé dans le magasin de certificats Autorités de Certification racine de confiance pour l’utilisateur actuel et de l’ordinateur Local lorsque la stratégie de groupe est actualisée sur l’ordinateur client membre du domaine. Si vous décidez de déployer des certificats de serveur à partir d’une autorité de certification publique, assurez-vous que le certificat d’autorité de certification publique est déjà dans le magasin de certificats Autorités de Certification racine de confiance.  

2.  Le serveur NPS authentifie l’utilisateur. Une fois que le client s’authentifie correctement le serveur NPS, le client envoie les informations d’identification de l’utilisateur password\ sur le serveur NPS, qui vérifie les informations d’identification de l’utilisateur par rapport à la base de données de comptes d’utilisateur dans les Services de domaine ActiveDirectory \(ADDS\).

Si les informations d’identification sont valides et que l’authentification réussit, le serveur NPS commence la phase d’autorisation de traitement de la demande de connexion. Si les informations d’identification ne sont pas valides et que l’authentification échoue, NPS envoie un message de refuser l’accès et la demande de connexion est refusée.  

#### <a name="authorization"></a>Autorisation

Le serveur exécutant NPS effectue l’autorisation comme suit:  

1. NPS vérifie les restrictions de l’utilisateur ou ordinateur dial\ dans Propriétés du compte dans ADDS. Chaque compte d’utilisateur et d’ordinateur dans ActiveDirectory Users and Computers inclut plusieurs propriétés, y compris celles qui figurent sur la **Dial\ dans** onglet. Dans cet onglet, dans **autorisation d’accès réseau**, si la valeur est **autoriser l’accès**, l’utilisateur ou l’ordinateur est autorisé à se connecter au réseau. Si la valeur est **refuser l’accès**, l’utilisateur ou l’ordinateur n’est pas autorisé à se connecter au réseau. Si la valeur est **contrôler l’accès via la stratégie de réseau NPS**, NPS évalue les stratégies réseau configurées pour déterminer si l’utilisateur ou l’ordinateur est autorisé à se connecter au réseau. 

2. NPS traite ensuite ses stratégies de réseau pour rechercher une stratégie qui correspond à la demande de connexion. Si une stratégie correspondante est trouvée, NPS accorde ou refuse la connexion en fonction de configuration de cette stratégie.  

Si l’authentification et autorisation sont réussies, si la stratégie réseau correspondante accorde l’accès, NPS accorde l’accès au réseau et l’utilisateur et l’ordinateur peuvent se connecter aux ressources réseau pour lesquels ils disposent d’autorisations.  

>[!NOTE]
>Pour déployer l’accès sans fil, vous devez configurer les stratégies du serveur NPS. Ce guide fournit des instructions pour utiliser le **configurer 802. 1 X wizard** dans NPS pour créer des stratégies de serveur NPS pour 802. 1 X authentifiés l’accès sans fil.  

### <a name="bootstrap-profiles"></a>Profils d’amorçage
Dans 802.1X\-authentifiés des réseaux sans fil, les clients sans fil doivent fournir des informations d’identification de sécurité qui sont authentifiées par un serveur RADIUS pour vous connecter au réseau. Pour EAP protégé \[PEAP\]\-MicrosoftChallenge Handshake Authentication Protocol version2 \[MS\-CHAP v2\], les informations d’identification de sécurité sont un nom d’utilisateur et un mot de passe. Pour \[TLS\ EAP\-Transport Layer Security] ou PEAP\-TLS, les informations d’identification de sécurité sont des certificats, tels que les certificats d’utilisateur et l’ordinateur client ou de cartes à puce.

Lors de la connexion à un réseau qui est configuré pour exécuter PEAP\-MS\-CHAP v2, PEAP\-TLS ou l’authentification EAP\-TLS, par défaut, les clients sans fil Windows doivent également valider un certificat d’ordinateur qui est envoyé par le serveur RADIUS. Le certificat d’ordinateur qui est envoyé par le serveur RADIUS pour chaque session de l’authentification est communément appelé un certificat de serveur.

Comme mentionné précédemment, vous pouvez émettre vos serveurs RADIUS de leur certificat de serveur dans un des deux manières: à partir d’une autorité de certification commerciale \ (telles que VeriSign, Inc., \), ou à partir d’une autorité de certification privée que vous déployez sur votre réseau. Si le serveur RADIUS envoie un certificat d’ordinateur qui a été émis par une autorité de certification commerciale qui dispose déjà d’un certificat racine est installé dans le magasin de certificats Autorités de Certification racine de confiance du client, le client sans fil permettre valider certificat d’ordinateur du serveur RADIUS, quelle que soit l’indique si le client sans fil a rejoint le domaine ActiveDirectory. Dans ce cas, le client sans fil permettre se connecter au réseau sans fil, puis vous pouvez joindre l’ordinateur au domaine.

>[!NOTE]
>Le comportement que le client valider le certificat de serveur peut être désactivé, mais la désactivation de la validation de certificat de serveur n’est pas recommandée dans les environnements de production.

Profils d’amorçage sans fil sont des profils temporaires qui sont configurés de manière à permettre aux utilisateurs de clients sans fil pour se connecter à la 802.1X\-réseau sans fil authentifié avant de l’ordinateur est joint au domaine, and\ / ou avant que l’utilisateur a correctement connecté au domaine à l’aide d’un ordinateur donné sans fil pour la première fois.  Cette section résume la nature du problème se produit lorsque vous essayez de joindre un ordinateur sans fil au domaine ou à un utilisateur d’utiliser un ordinateur sans fil appartenant à un domain\ pour la première fois pour vous connecter au domaine.

Pour les déploiements dans lesquels l’utilisateur ou un administrateur informatique ne peut pas connecter physiquement un ordinateur au réseau Ethernet câblé pour joindre l’ordinateur au domaine et l’ordinateur ne dispose pas de l’émission nécessaires racine certificat d’autorité de certification installé dans son **Trusted Root Certification Authorities** magasin de certificats, vous pouvez configurer les clients sans fil avec un profil de connexion sans fil temporaire, appelé un *amorçage profil*, pour se connecter au réseau sans fil.

Un *amorçage profil* supprime la nécessité de valider le certificat d’ordinateur du serveur RADIUS. Cette configuration temporaire permet à l’utilisateur sans fil joindre l’ordinateur au domaine, à quel moment les réseau sans fil \ IEEE (802.11\) les stratégies sont appliquées et le certificat d’autorité de certification de racine appropriée est automatiquement installé sur l’ordinateur.

Lors de la stratégie de groupe est appliquée, un ou plusieurs profils de connexion sans fil qui appliquent la configuration requise pour l’authentification mutuelle sont appliquées sur l’ordinateur; le profil de démarrage n’est plus nécessaire et est supprimé. Après avoir joint l’ordinateur au domaine et le redémarrage de l’ordinateur, l’utilisateur peut utiliser une connexion sans fil pour se connecter au domaine.

Pour une vue d’ensemble du processus de déploiement de l’accès sans fil à l’aide de ces technologies, voir [vue d’ensemble du déploiement de l’accès sans fil](b-wireless-access-deploy-overview.md).
