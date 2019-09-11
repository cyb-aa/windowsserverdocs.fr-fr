---
title: Déployer un accès sans fil authentifié 802.1X basé sur des mots de passe
description: Cette rubrique fait partie du Guide de mise en réseau de Windows Server 2016 « déployer l’accès sans fil authentifié 802.1 X basé sur un mot de passe »
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ff06ba23-9c0f-49ec-8f7b-611cf8d73a1b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ee20e002aa8dad29eefcda87a7b949ffb0bb6e9b
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868938"
---
# <a name="deploy-password-based-8021x-authenticated-wireless-access"></a>Déployer un\-accès sans fil authentifié 802.1 x basé sur un mot de passe

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Il s’agit d’un guide d’accompagnement du&reg; Guide du réseau de base de Windows Server 2016. Le Guide du réseau de base fournit des instructions pour la planification et le déploiement des composants requis pour un réseau pleinement fonctionnel et un nouveau Active Directory® domaine dans une nouvelle forêt.

Ce guide explique comment créer un réseau de base en fournissant des instructions sur le déploiement \(\) d’un accès sans fil IEEE 802,11 x authentifié IEEE\-802.1 x avec Protected Extensible Authentication Protocol – Microsoft Challenge Handshake Authentication Protocol \(version\-2\-PEAP MS\)CHAP v2.

Étant donné\-que\-PEAP MS CHAP v2 exige que les\-utilisateurs fournissent des informations d’identification basées sur un mot de passe plutôt qu’un certificat au cours du processus d’authentification, il est généralement plus facile et moins coûteux à déployer que le protocole EAP\-.TLS TLS ou\-PEAP.

>[!NOTE]
>Dans ce guide, l’accès sans fil authentifié IEEE 802.1 x avec\-PEAP\-MS CHAP v2 est abrégé en « accès sans fil » et « accès WiFi ».

## <a name="about-this-guide"></a>À propos de ce guide
Ce guide, associé aux guides de conditions préalables décrits ci-dessous, fournit des instructions sur le déploiement de l’infrastructure d’accès Wi-Fi suivante.  

- Un ou plusieurs\- pointsd'\(accès sans fil 802,11 x points d’accès.\)

- Active Directory Domain Services \(ADDS\) les utilisateurs et les ordinateurs.

- Gestion des stratégies de groupe

- Un ou plusieurs serveurs NPS NPS \(\) (Network Policy Server).

- Certificats de serveur pour les ordinateurs exécutant NPS (Network Policy Server)

- Ordinateurs clients sans fil exécutant&reg; windows 10, Windows 8.1 ou Windows 8.

### <a name="dependencies-for-this-guide"></a>Dépendances pour ce guide

Pour déployer avec succès des réseaux sans fil authentifiés avec ce guide, vous devez disposer d’un environnement réseau et de domaine avec toutes les technologies requises déployées. Vous devez également déployer des certificats de serveur sur votre NPSs d’authentification.

Les sections suivantes fournissent des liens vers la documentation qui vous montre comment déployer ces technologies.

#### <a name="network-and-domain-environment-dependencies"></a>Dépendances de l’environnement réseau et de domaine

Ce guide est conçu pour les administrateurs réseau et système qui ont suivi les instructions du **Guide du réseau de base** Windows Server 2016 pour déployer un réseau de base, ou pour ceux qui ont déjà déployé les principales technologies incluses dans le cœur. réseau, y compris AD DS \(, DNS\)Domain Name System, protocole \(DHCP DHCP\), TCP\/IP, NPS et Windows Internet Name Service \(WINS\).  

Le Guide du réseau de base est disponible aux emplacements suivants :

- Le [Guide du réseau de base](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide) windows server 2016 est disponible dans la bibliothèque technique de windows server 2016. 

- Le [Guide du réseau de base](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683) est également disponible au format Word dans la Galerie [https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683)TechNet, à l’adresse.

#### <a name="server-certificate-dependencies"></a>Dépendances de certificat de serveur
Deux options sont disponibles pour inscrire des serveurs d’authentification avec des certificats de serveur pour une utilisation avec l’authentification 802.1 x-déployer votre propre infrastructure de clé publique à \(l’aide\) de Active Directory Services de certificats AD CS ou Utilisez les certificats de serveur inscrits par une autorité de certification d’autorité \(\)de certification publique.

##### <a name="ad-cs"></a>AD CS
Les administrateurs système et réseau qui déploient des réseaux sans fil authentifiés doivent suivre les instructions du Guide d’accompagnement du réseau de base Windows Server 2016, **déployer des certificats de serveur pour les déploiements sans fil et câblés 802.1 x**. Ce guide explique comment déployer et utiliser les services AD CS pour inscrire automatiquement des certificats de serveur sur des ordinateurs exécutant NPS.

Ce guide est disponible à l’emplacement suivant.

- Le Guide d’accompagnement du réseau de base Windows Server 2016 [déploie des certificats de serveur pour les déploiements sans fil et câblés 802.1 x](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments?f=255&MSPPError=-2147217396) au format HTML dans la bibliothèque technique. 

##### <a name="public-ca"></a>Autorité de certification publique
Vous pouvez acheter des certificats de serveur auprès d’une autorité de certification publique, telle que VeriSign, à laquelle les ordinateurs clients sont déjà autorisés. 

Un ordinateur client approuve une autorité de certification lorsque le certificat d’autorité de certification est installé dans le magasin de certificats autorités de certification racines de confiance. Par défaut, plusieurs certificats d’autorité de certification publics sont installés sur les ordinateurs exécutant Windows dans leur magasin de certificats autorités de certification racines de confiance.  

Il est recommandé de consulter les guides de conception et de déploiement pour chacune des technologies utilisées dans ce scénario de déploiement. Ces guides peuvent vous aider à déterminer si ce scénario de déploiement fournit les services et la configuration dont vous avez besoin pour le réseau de votre organisation.

### <a name="requirements"></a>Configuration requise
Voici la configuration requise pour le déploiement d’une infrastructure d’accès sans fil à l’aide du scénario décrit dans ce guide :

- Avant de déployer ce scénario, vous devez d’abord acheter des\-points d’accès sans fil 802.1 x pour fournir une couverture sans fil aux emplacements souhaités sur votre site. La section planification de ce guide vous aide à déterminer les fonctionnalités que vos APs doivent prendre en charge.

- Active Directory Domain Services \(ADDS\) est installé, tout comme les autres technologies réseau requises, conformément aux instructions du Guide du réseau de base de Windows Server 2016.  

- Les services AD CS sont déployés et les certificats de serveur sont inscrits auprès de NPSs. Ces certificats sont requis lorsque vous déployez la\-méthode\-d’authentification par\-certificat PEAP MS CHAP v2 qui est utilisée dans ce guide.

- Un membre de votre organisation est familiarisé avec les normes IEEE 802,11 qui sont prises en charge par vos points d’accès sans fil et les cartes réseau sans fil qui sont installées sur les ordinateurs clients et les périphériques de votre réseau. Par exemple, un membre de votre organisation est familiarisé avec les types de fréquence radio, \(l’authentification sans\)fil 802,11 WPA2 ou \(WPA et le\)chiffrement AES ou TKIP.

## <a name="what-this-guide-does-not-provide"></a>Ce que ce guide ne contient pas  
Voici quelques éléments que ce guide ne fournit pas :

### <a name="comprehensive-guidance-for-selecting-8021x-capable-wireless-access-points"></a>Conseils complets pour la sélection de\-points d’accès sans fil 802.1 x

Étant donné que de nombreuses différences existent entre les marques et\-les modèles de points d’accès sans fil 802.1 x, ce guide ne fournit pas d’informations détaillées sur les éléments suivants :  

- Détermination de la méthode ou du modèle de point d’accès sans fil le mieux adapté à vos besoins.

- Le déploiement physique des points d’accès sans fil sur votre réseau.

- Configuration avancée des points d’accès sans fil, par exemple pour les \(réseaux\)locaux virtuels sans fil.

- Instructions sur la configuration des attributs spécifiques au\-fournisseur de points d’accès sans fil dans NPS.

En outre, la terminologie et les noms des paramètres varient entre les marques et les modèles de points d’accès sans fil et peuvent ne pas correspondre aux noms de paramètres génériques utilisés dans ce guide. Pour plus d’informations sur la configuration du point d’accès sans fil, vous devez consulter la documentation du produit fournie par le fabricant de vos points d’accès sans fil.

### <a name="instructions-for-deploying-nps-certificates"></a>Instructions pour le déploiement de certificats NPS
  
Il existe deux alternatives pour le déploiement de certificats NPS. Ce guide ne fournit pas d’instructions complètes pour vous aider à déterminer l’alternative qui correspond le mieux à vos besoins. En général, toutefois, les choix que vous rencontrez sont les suivants :

- Achat de certificats auprès d’une autorité de certification publique, telle que VeriSign, déjà approuvée\-par les clients Windows. Cette option est généralement recommandée pour les réseaux plus petits.

- Déploiement d’une infrastructure \(à clé publique PKI\) sur votre réseau à l’aide des services AD CS. Cela est recommandé pour la plupart des réseaux et les instructions de déploiement des certificats de serveur avec les services AD CS sont disponibles dans le Guide de déploiement mentionné précédemment.

### <a name="nps-network-policies-and-other-nps-settings"></a>Stratégies réseau NPS et autres paramètres NPS

À l’exception des paramètres de configuration définis lors de l’exécution de l’Assistant **configuration de 802.1 x** , comme indiqué dans ce guide, ce guide ne fournit pas d’informations détaillées sur la configuration manuelle des conditions, des contraintes ou d’autres paramètres NPS.

### <a name="dhcp"></a>DHCP

Ce guide de déploiement ne fournit pas d’informations sur la conception ou le déploiement de sous-réseaux DHCP pour les réseaux locaux sans fil.

## <a name="technology-overviews"></a>Vues d’ensemble des technologies
Voici les vues d’ensemble des technologies pour le déploiement de l’accès sans fil :

### <a name="ieee-8021x"></a>IEEE 802.1X
La norme IEEE 802.1 x définit le contrôle\-d’accès réseau basé sur les ports qui est utilisé pour fournir l’accès réseau authentifié aux réseaux Ethernet. Ce contrôle\-d’accès réseau basé sur les ports utilise les caractéristiques physiques de l’infrastructure de réseau local commuté pour authentifier les appareils connectés à un port LAN. L'accès au port peut être refusé en cas d'échec du processus d'authentification. Bien que cette norme ait été conçue pour les réseaux Ethernet câblés, elle a été adaptée pour une utilisation sur les réseaux locaux sans fil 802,11.

### <a name="8021x-capable-wireless-access-points-aps"></a>\- points\(d’accès sans fil 802.1 x\)
Ce scénario nécessite le déploiement d’un ou de plusieurs points\-d’accès sans fil compatibles 802.1 x qui sont compatibles avec\-le protocole Radius \(\) de l’authentification à distance en tant que service utilisateur.

les points d’accès\-conformes à 802.1 x et RADIUS, lorsqu’ils sont déployés dans une infrastructure RADIUS avec un serveur RADIUS tel qu’un serveur NPS, sont appelés *clients RADIUS*.

### <a name="wireless-clients"></a>Clients sans fil
Ce guide fournit des informations complètes sur la configuration pour fournir un accès authentifié 802.1\-x pour les utilisateurs membres du domaine qui se connectent au réseau avec des ordinateurs clients sans fil exécutant Windows 10, Windows 8.1 et Windows 8. Les ordinateurs doivent être joints au domaine pour établir correctement l’accès authentifié.

>[!NOTE]
>Vous pouvez également utiliser des ordinateurs qui exécutent Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012 en tant que clients sans fil.

### <a name="support-for-ieee-80211-standards"></a>Prise en charge des normes IEEE 802,11
Les systèmes d’exploitation Windows et Windows Server pris\-en charge offrent une prise en charge intégrée de la mise en réseau sans fil 802,11. Dans ces systèmes d’exploitation, une carte réseau sans fil 802,11 installée apparaît en tant que connexion réseau sans fil dans le centre réseau et partage. 

Bien qu’il existe\-une prise en charge intégrée de la mise en réseau sans fil 802,11, les composants sans fil de Windows dépendent des éléments suivants :

- Les fonctionnalités de la carte réseau sans fil. La carte réseau sans fil installée doit prendre en charge les normes de sécurité réseau sans fil ou sans fil dont vous avez besoin. Par exemple, si la carte réseau sans fil ne prend pas\-en charge l' \(accès\)WPA protégé Wi-Fi, vous ne pouvez pas activer ni configurer les options de sécurité WPA.

- Les fonctionnalités du pilote de carte réseau sans fil. Pour vous permettre de configurer des options de réseau sans fil, le pilote de la carte réseau sans fil doit prendre en charge la création de rapports de toutes ses fonctionnalités dans Windows. Vérifiez que le pilote de votre carte réseau sans fil est écrit pour les fonctionnalités de votre système d’exploitation. Assurez-vous également que le pilote est la version la plus récente en vérifiant Microsoft Update ou le site Web du fournisseur de la carte réseau sans fil.

Le tableau suivant indique les vitesses de transmission et les fréquences pour les normes sans fil IEEE 802,11 courantes.

|Normes|Different|Taux de transmission bits|Utilisation|
|-------------|---------------|--------------------------|---------|  
|802,11|Plage\- \(\) de fréquences ISM, scientifique et médicale de la bande S 2,4 à 2,5 GHz \(\)|2 mégabits par seconde \((Mbits/s)\)|Obsolète. Rarement utilisé.|  
|réseaux|ISM\-de la bande S|11 Mbits/s|Couramment utilisé.|  
|802.11 a|C\-bande ISM \(5,725 à 5,875 GHz\)|54 Mbits/s|Rarement utilisé en raison des dépenses et des limites limitées.|  
|spécification|ISM\-de la bande S|54 Mbits/s|Largement utilisé. les appareils 802.11 g sont compatibles avec les périphériques 802.11 b.|  
|802.11 n \ 2,4 et 5,0 GHz|La\-bande C et\-la bande S|250 Mbits/s|Les appareils basés sur la\-norme de prératification IEEE 802.11 n sont devenus disponibles en août 2007. De nombreux appareils 802.11 n sont compatibles avec les appareils 802.11 a, b et g.|
|802.11ac |5 GHz |6,93 Gbits/s |802.11 AC, approuvé par l’IEEE dans 2014, est plus évolutif et plus rapide que 802.11 n, et est déployé là où les APs et les clients sans fil le prennent en charge.|

### <a name="wireless-network-security-methods"></a>Méthodes de sécurité du réseau sans fil
Les **méthodes de sécurité de réseau sans fil** sont un regroupement informel d’authentification \(sans fil, parfois appelé sécurité\) sans fil et chiffrement de sécurité sans fil. L’authentification et le chiffrement sans fil sont utilisés par paires pour empêcher les utilisateurs non autorisés d’accéder au réseau sans fil et pour protéger les transmissions sans fil. 

Lors de la configuration des paramètres de sécurité sans fil dans les stratégies de réseau sans fil de stratégie de groupe, vous avez le choix entre plusieurs combinaisons. Toutefois, seules les normes\-d’authentification WPA2\-Enterprise, WPA Enterprise et Open with 802.1 x sont prises en charge pour les déploiements sans fil authentifiés 802.1 x.

>[!NOTE]
>Lors de la configuration des stratégies de réseau sans fil, vous devez sélectionner **WPA2\-Enterprise**, **WPA\-Enterprise**ou **Ouvrir avec 802.1 x** afin d’accéder aux paramètres EAP requis pour l’authentification 802.1 x. déploiements sans fil.  

#### <a name="wireless-authentication"></a>Authentification sans fil
Ce guide recommande l’utilisation des normes d’authentification sans fil suivantes pour les déploiements sans fil authentifiés 802.1 X.  

**\(Wi\--Fi Protected Access –\-Enterprise\) WPA Enterprise** WPA est une norme temporaire développée par The WIFI Alliance pour se conformer au protocole de sécurité sans fil 802,11. Le protocole WPA a été développé en réponse à un certain nombre de failles graves découvertes dans le protocole WEP \(\) équivalent de confidentialité.

WPA\-Enterprise offre une sécurité accrue par rapport à WEP :  

1. Exiger une authentification qui utilise l’infrastructure 802.1 X EAP dans le cadre de l’infrastructure qui garantit une authentification mutuelle centralisée et une gestion de clés dynamique  

2. Amélioration \(de la valeur de contrôle d’intégrité ICV\) avec un \(micro\)de vérification de l’intégrité des messages pour protéger l’en-tête et la charge utile  

3. Implémentation d’un compteur de frames pour décourager les attaques par relecture  

**\(Wi\--Fi Protected Access 2 :\-Enterprise\) WPA2 Enterprise** comme\-la norme WPA Enterprise\-, WPA2 Enterprise utilise 802.1 x et EAP Framework. WPA2\-Enterprise offre une protection renforcée des données pour plusieurs utilisateurs et réseaux gérés de grande taille. WPA2\-Enterprise est un protocole robuste conçu pour empêcher tout accès réseau non autorisé en vérifiant les utilisateurs réseau via un serveur d’authentification.  

#### <a name="wireless-security-encryption"></a>Chiffrement de sécurité sans fil
Le chiffrement de sécurité sans fil est utilisé pour protéger les transmissions sans fil envoyées entre le client sans fil et le point d’accès sans fil. Le chiffrement de sécurité sans fil est utilisé conjointement avec la méthode d’authentification de sécurité réseau sélectionnée. Par défaut, les ordinateurs exécutant Windows 10, Windows 8.1 et Windows 8 prennent en charge deux normes de chiffrement :

1. **Protocole d’intégrité de la clé temporelle** Le protocole TKIP\) est un ancien protocole de chiffrement conçu à l’origine pour fournir un chiffrement sans fil plus sécurisé que celui fourni par le protocole\) WEP privé de confidentialité de la confidentialité \(. \( TKIP a été conçu par le groupe de tâches IEEE 802.11 i et\-Wi Fi Alliance pour remplacer le chiffrement WEP sans nécessiter le remplacement du matériel hérité. TKIP est une suite d’algorithmes qui encapsule la charge utile WEP et permet aux utilisateurs de l’équipement Wi-Fi hérité d’effectuer la mise à niveau vers TKIP sans remplacer le matériel. Comme WEP, TKIP utilise l’algorithme de chiffrement de flux RC4 comme base. Toutefois, le nouveau protocole chiffre chaque paquet de données à l’aide d’une clé de chiffrement unique, et les clés sont plus puissantes que celles de WEP. Bien que TKIP soit utile pour la mise à niveau de la sécurité sur des appareils plus anciens qui ont été conçus pour utiliser uniquement le protocole WEP, il ne traite pas tous les problèmes de sécurité auxquels sont confrontés les réseaux locaux sans fil et, dans la plupart des cas, n’est pas suffisamment robuste pour protéger les données d’entreprise ou les administrations sensibles. transmissions.  

2. **Advanced Encryption Standard** \(AES\) est le protocole de chiffrement préféré pour le chiffrement des données commerciales et gouvernementales. AES offre un niveau plus élevé de sécurité de transmission sans fil que TKIP ou WEP. Contrairement à TKIP et WEP, AES requiert un matériel sans fil prenant en charge la norme AES. AES est une norme\-de chiffrement à clé symétrique qui utilise trois chiffrements\-par blocs,\-AES 128,\-AES 192 et AES 256.

Dans Windows Server 2016, les méthodes de\-chiffrement sans fil AES suivantes sont disponibles pour la configuration dans les propriétés du profil sans fil lorsque vous\-sélectionnez une méthode d’authentification de WPA2 Enterprise, ce qui est recommandé.

1. **AES\-CCMP**. Le chaînage de blocs de chiffrement en \(mode\) compteur code d’Authentification de message le protocole CCMP implémente la norme 802.11 i et est conçu pour un chiffrement de sécurité plus élevé que celui fourni par WEP et utilise des clés de chiffrement AES 128 bits.
2. **GCMP\-AES**. Le protocole \(Galois Counter mode\) GCMP est pris en charge par 802.11 AC, est plus\-efficace qu’AES CCMP et offre de meilleures performances pour les clients sans fil. GCMP utilise des clés de chiffrement AES 256 bits.

> [!IMPORTANT]
> Le chiffrement WEP \(\) de la confidentialité des équivalences est la norme de sécurité sans fil d’origine qui a été utilisée pour chiffrer le trafic réseau. Vous ne devez pas déployer le protocole WEP sur votre réseau,\-car il existe des vulnérabilités bien connues dans cette forme de sécurité obsolète.

### <a name="active-directorydoman-services-adds"></a>Active Directory les services \(domaine AD DS\)
AD DS fournit une base de données distribuée qui stocke et gère des informations\-sur les ressources réseau\-et les données spécifiques à l’application à partir d’applications compatibles avec l’annuaire. Les administrateurs peuvent utiliser AD DS pour organiser les éléments d’un réseau, tels que les utilisateurs, les ordinateurs et les autres périphériques, en une structure hiérarchique de type contenant-contenu. La structure hiérarchique de la relation contenant-contenu comprend la forêt Active Directory, les domaines de la forêt \(et\) les unités d’organisation de chaque domaine. Un serveur qui exécute AD DS est appelé contrôleur de *domaine*.  

AD DS contient les comptes d’utilisateur, les comptes d’ordinateur et les propriétés de compte requis par IEEE 802.1 x\-et\-PEAP MS CHAP v2 pour authentifier les informations d’identification de l’utilisateur et pour évaluer l’autorisation pour les connexions sans fil.

### <a name="active-directory-users-and-computers"></a>Utilisateurs et ordinateurs Active Directory
Active Directory utilisateurs et ordinateurs est un composant de AD DS qui contient des comptes qui représentent des entités physiques, telles qu’un ordinateur, une personne ou un groupe de sécurité. Un *groupe de sécurité* est un ensemble de comptes d’utilisateurs ou d’ordinateurs que les administrateurs peuvent gérer comme une seule unité. Les comptes d’utilisateurs et d’ordinateurs qui appartiennent à un groupe particulier sont appelés *membres du groupe*.  

### <a name="group-policy-management"></a>Gestion des stratégies de groupe
La gestion des stratégie de groupe\-permet la gestion des modifications de configuration et des paramètres des utilisateurs et des ordinateurs, notamment les informations relatives à la sécurité et aux utilisateurs. Vous utilisez stratégie de groupe pour définir des configurations pour des groupes d’utilisateurs et d’ordinateurs. Avec stratégie de groupe, vous pouvez spécifier des paramètres pour les entrées de Registre, la sécurité, l’installation de logiciels, les scripts, la redirection de dossiers, les services d’installation à distance et la maintenance d’Internet Explorer. Les paramètres de stratégie de groupe que vous créez sont contenus dans un objet \(de\)stratégie de groupe d’objets stratégie de groupe. En associant un objet de stratégie de groupe à des conteneurs système Active Directory sélectionnés (sites, domaines et unités d’organisation), vous pouvez appliquer les paramètres de l’objet de stratégie de groupe aux utilisateurs et ordinateurs dans ces Active Directory conteneurs. Pour gérer les objets stratégie de groupe au sein d’une entreprise, vous pouvez utiliser la console \(MMC\)éditeur de gestion des stratégies de groupe Microsoft Management Console.  

Ce guide fournit des instructions détaillées sur la façon de spécifier des paramètres dans \(l’extension\) stratégies de réseau sans fil IEEE 802,11 de la gestion des stratégie de groupe. Les stratégies de \(réseau sans\) fil IEEE 802,11\-configurent les ordinateurs clients sans fil membres du domaine avec la connectivité et les paramètres sans fil nécessaires pour l’accès sans fil authentifié 802.1 x.

### <a name="server-certificates"></a>Certificats de serveur
Ce scénario de déploiement nécessite des certificats de serveur pour chaque serveur NPS qui effectue l’authentification 802.1 X.  

Un certificat de serveur est un document numérique couramment utilisé pour l’authentification et pour sécuriser les informations sur les réseaux ouverts. Un certificat associe de manière sécurisée une clé publique à l'entité qui détient la clé privée correspondante. Les certificats sont signés numériquement par l’autorité de certification émettrice et peuvent être émis pour un utilisateur, un ordinateur ou un service.  

Une autorité de \(certification\) d’autorité de certification est une entité responsable de l’établissement et de la garantie de l’authenticité \(des clés publiques appartenant\) à des sujets généralement des utilisateurs ou des ordinateurs ou d’autres autorités de certification. Les activités d’une autorité de certification peuvent inclure la liaison de clés publiques à des noms uniques par le biais de certificats signés, la gestion des numéros de série de certificats et la révocation de certificats.  

Active Directory Services \(de certificats ad\) CS est un rôle de serveur qui émet des certificats en tant qu’autorité de certification réseau. Une infrastructure de certificat AD CS, également appelée infrastructure à  *\(clé publique (PKI\)* ), fournit des services personnalisables pour l’émission et la gestion des certificats pour l’entreprise.

### <a name="eap-peap-and-peap-ms-chap-v2"></a>EAP, PEAP et PEAP\-MS\-CHAP v2
Le protocole EAP \(\) (Extensible Authentication Protocol) \(étend\) le protocole PPP de point\-à\-point en autorisant des méthodes d’authentification supplémentaires qui utilisent des informations d’identification et des informations échanges de longueurs arbitraires. Avec l’authentification EAP, le client d’accès réseau et l' \(authentificateur, tels que\) le serveur NPS, doivent prendre en charge le même type EAP pour que l’authentification réussisse. Windows Server 2016 comprend une infrastructure EAP, prend en charge deux types EAP et la possibilité de transmettre des messages EAP à NPSs. À l’aide d’EAP, vous pouvez prendre en charge des schémas d’authentification supplémentaires, connus sous le nom de *types EAP*. Les types EAP pris en charge par Windows Server 2016 sont les suivants :  

- \(TLS TLS\)

- Microsoft Challenge Handshake Authentication Protocol version 2 \(MS\-CHAP v2\)

>[!IMPORTANT]
>Les types \(EAP forts tels que ceux basés sur des certificats\) offrent une meilleure sécurité contre\-les attaques par force brute, les attaques de dictionnaire et les attaques\-par déduction de mot de passe que le mot de passe. protocoles \(d’authentification tels que CHAP ou\-MS CHAP version\)1.

Protected \(EAP\) PEAP utilise TLS pour créer un canal chiffré entre un client PEAP d’authentification, tel qu’un ordinateur sans fil, et un authentificateur PEAP, tel qu’un serveur NPS ou d’autres serveurs RADIUS. PEAP ne spécifie pas de méthode d’authentification, mais fournit une sécurité supplémentaire pour d’autres \(protocoles d’authentification\-EAP tels que\) EAP MS\-CHAP v2 pouvant fonctionner via le canal chiffré TLS. fourni par PEAP. PEAP est utilisé comme méthode d’authentification pour les clients d’accès qui se connectent au réseau de votre organisation via les types suivants de \(serveurs\)d’accès réseau (NAS) :

- points d'\-accès sans fil 802.1 x

- commutateurs\-d’authentification 802.1 x

- Ordinateurs exécutant Windows Server \(2016 et le service d’accès à distance (RAS\) ) qui sont configurés\) en tant que serveurs VPN de réseau \(privé virtuel, serveurs DirectAccess, ou les deux

- Ordinateurs exécutant Windows Server 2016 et Services Bureau à distance

PEAP\-MS\-\- \(\)CHAP v2 est plus facile à déployer que EAP TLS, car l’authentification des utilisateurs s’effectue à l’aide du nom d’utilisateur et du mot de passe des informations d’identification basées sur un mot de passe, au lieu de\- certificats ou cartes à puce. Seuls les serveurs NPS ou d’autres serveurs RADIUS doivent disposer d’un certificat. Le certificat NPS est utilisé par le serveur NPS pendant le processus d’authentification pour prouver son identité aux clients PEAP.

Ce guide fournit des instructions pour configurer vos clients sans fil et\(votre\) NPS pour utiliser\-PEAP\-MS CHAP v2 pour l’accès authentifié 802.1 x.

### <a name="network-policy-server"></a>Serveur NPS (Network Policy Server)
NPS \(\) \(\-(Network Policy Server) vous permet de configurer et de gérer de manière centralisée les stratégies réseau à l’aide de l’authentification à distance sur le serveur RADIUS du service utilisateur et le proxy RADIUS.\) NPS est requis lorsque vous déployez l’accès sans fil 802.1 X.

Quand vous configurez vos points d’accès sans fil 802.1 X en tant que clients RADIUS dans NPS, NPS traite les demandes de connexion envoyées par les APs. Pendant le traitement des demandes de connexion, NPS effectue l’authentification et l’autorisation. L’authentification détermine si le client a présenté des informations d’identification valides. Si NPS réussit à authentifier le client à l’origine de la demande, le serveur NPS détermine si le client est autorisé à effectuer la connexion demandée, et autorise ou refuse la connexion. Cette procédure est expliquée plus en détail comme suit :

#### <a name="authentication"></a>Authentication

L’authentification MS\-\-CHAP v2 Mutual PEAP a deux parties principales :

1.  Le client authentifie le serveur NPS.  Pendant cette phase de l’authentification mutuelle, le serveur NPS envoie son certificat de serveur à l’ordinateur client afin que le client puisse vérifier l’identité du serveur NPS avec le certificat. Pour authentifier le serveur NPS, l’ordinateur client doit approuver l’autorité de certification qui a émis le certificat NPS. Le client approuve cette autorité de certification lorsque le certificat de l’autorité de certification est présent dans le magasin de certificats des autorités de certification racines de confiance sur l’ordinateur client.

    Si vous déployez votre propre autorité de certification privée, le certificat d’autorité de certification est automatiquement installé dans le magasin de certificats des autorités de certification racines de confiance pour l’utilisateur actuel et pour l’ordinateur local lorsque stratégie de groupe est actualisé sur l’ordinateur client membre du domaine. Si vous décidez de déployer des certificats de serveur à partir d’une autorité de certification publique, assurez-vous que le certificat d’autorité de certification publique figure déjà dans le magasin de certificats des autorités de certification racines de confiance.  

2.  Le serveur NPS authentifie l’utilisateur. Une fois que le client a correctement authentifié le serveur NPS, le client envoie les\-informations d’identification basées sur le mot de passe de l’utilisateur au serveur NPS, qui vérifie les informations d’identification de l' \(utilisateur par rapport à la base de données des comptes d’utilisateur dans Active Directory ad des services domaine DS\).

Si les informations d’identification sont valides et que l’authentification est réussie, le serveur NPS commence la phase d’autorisation du traitement de la demande de connexion. Si les informations d’identification ne sont pas valides et que l’authentification échoue, le serveur NPS envoie un message de refus d’accès et la demande de connexion est refusée.  

#### <a name="authorization"></a>Authorization

Le serveur exécutant NPS effectue l’autorisation comme suit :  

1. NPS vérifie les restrictions dans les propriétés du compte d’utilisateur\-ou d’ordinateur en se connectant dans AD DS. Chaque compte d’utilisateur et d’ordinateur dans Active Directory utilisateurs et ordinateurs comprend plusieurs propriétés, y compris celles figurant dans l’onglet **accès à distance\-** . Dans cet onglet, dans **autorisation d’accès réseau**, si la valeur est **autoriser l’accès**, l’utilisateur ou l’ordinateur est autorisé à se connecter au réseau. Si la valeur est **refuser l’accès**, l’utilisateur ou l’ordinateur n’est pas autorisé à se connecter au réseau. Si la valeur est **contrôler l’accès via la stratégie de réseau NPS**, NPS évalue les stratégies réseau configurées pour déterminer si l’utilisateur ou l’ordinateur est autorisé à se connecter au réseau. 

2. NPS traite ensuite ses stratégies réseau pour rechercher une stratégie qui correspond à la demande de connexion. Si une stratégie de correspondance est trouvée, le serveur NPS accorde ou refuse la connexion en fonction de la configuration de cette stratégie.  

Si l’authentification et l’autorisation réussissent et que la stratégie réseau correspondante accorde l’accès, le serveur NPS accorde l’accès au réseau, et l’utilisateur et l’ordinateur peuvent se connecter aux ressources réseau pour lesquelles ils disposent d’autorisations.  

>[!NOTE]
>Pour déployer un accès sans fil, vous devez configurer des stratégies NPS. Ce guide fournit des instructions sur l’utilisation de l' **Assistant Configuration de 802.1 x** dans NPS pour créer des stratégies NPS pour l’accès sans fil authentifié 802.1 x.  

### <a name="bootstrap-profiles"></a>Profils bootstrap
Dans les réseaux\-sans fil authentifiés 802.1 x, les clients sans fil doivent fournir des informations d’identification de sécurité qui sont authentifiées par un serveur RADIUS afin de se connecter au réseau. Pour protected \[EAP\]PEAP\-Microsoft Challenge Handshake Authentication Protocol version \[2\-MS CHAP\]v2, les informations d’identification de sécurité sont un nom d’utilisateur et un mot de passe. Pour TLS \[\] Transport Layer Security TLS ou PEAP\-, les informations d’identification de sécurité sont des certificats, tels que des certificats d’utilisateur et d’ordinateur client ou des cartes à puce.\-

Lors de la connexion à un réseau configuré pour exécuter l'\-authentification\-PEAP MS CHAP v2\-, PEAP TLS ou\-EAP TLS, par défaut, les clients sans fil Windows doivent également valider un certificat d’ordinateur qui est envoyé par le serveur RADIUS. Le certificat d’ordinateur envoyé par le serveur RADIUS pour chaque session d’authentification est communément appelé certificat de serveur.

Comme mentionné précédemment, vous pouvez émettre vos serveurs RADIUS comme certificat de serveur de l’une des deux manières suivantes : à \(partir d’une autorité de certification commerciale\)telle que VeriSign, Inc., ou à partir d’une autorité de certification privée que vous déployez sur votre réseau. Si le serveur RADIUS envoie un certificat d’ordinateur émis par une autorité de certification commerciale qui a déjà un certificat racine installé dans le magasin de certificats des autorités de certification racines de confiance du client, le client sans fil peut valider le serveur RADIUS certificat d’ordinateur, que le client sans fil soit joint ou non au domaine Active Directory. Dans ce cas, le client sans fil peut se connecter au réseau sans fil, puis vous pouvez joindre l’ordinateur au domaine.

>[!NOTE]
>Le comportement nécessitant le client pour valider le certificat de serveur peut être désactivé, mais la désactivation de la validation de certificat de serveur n’est pas recommandée dans les environnements de production.

Les profils de démarrage sans fil sont des profils temporaires qui sont configurés de façon à permettre aux utilisateurs de clients sans fil de\-se connecter au réseau sans fil authentifié 802.1 x avant que l’ordinateur soit\/joint au domaine, et ou avant l’utilisateur a réussi à se connecter au domaine à l’aide d’un ordinateur sans fil donné pour la première fois.  Cette section résume le problème rencontré lors de la tentative de joindre un ordinateur sans fil au domaine, ou pour qu’un utilisateur utilise un ordinateur\-sans fil joint à un domaine pour la première fois pour ouvrir une session sur le domaine.

Pour les déploiements dans lesquels l’utilisateur ou l’administrateur informatique ne peut pas connecter physiquement un ordinateur au réseau Ethernet câblé pour joindre l’ordinateur au domaine et l’ordinateur n’a pas le certificat d’autorité de certification racine émettrice nécessaire installé dans sa **racine approuvée** Le magasin de certificats des autorités de certification, vous pouvez configurer des clients sans fil avec un profil de connexion sans fil temporaire, appelé *profil de démarrage*, pour se connecter au réseau sans fil.

Un *profil de démarrage* supprime la nécessité de valider le certificat d’ordinateur du serveur RADIUS. Cette configuration temporaire permet à l’utilisateur sans fil de joindre l’ordinateur au domaine, auquel cas les stratégies IEEE \(802,11\) du réseau sans fil sont appliquées et le certificat d’autorité de certification racine approprié est automatiquement installé sur le informatisé.

Lorsque stratégie de groupe est appliqué, un ou plusieurs profils de connexion sans fil qui appliquent la configuration requise pour l’authentification mutuelle sont appliqués sur l’ordinateur ; le profil de démarrage n’est plus nécessaire et est supprimé. Une fois l’ordinateur joint au domaine et le redémarrage de l’ordinateur, l’utilisateur peut se connecter au domaine à l’aide d’une connexion sans fil.

Pour une vue d’ensemble du processus de déploiement de l’accès sans fil à l’aide de ces technologies, consultez [vue d’ensemble du déploiement de l’accès sans fil](b-wireless-access-deploy-overview.md).
