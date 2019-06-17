---
title: Composants du réseau principal
description: Ce guide fournit des instructions sur la façon de planifier et déployer les composants centraux requis pour un réseau pleinement fonctionnel et un nouveau domaine Active Directory dans une nouvelle forêt avec Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: b3cd60f7-d380-4712-9a78-0a8f551e1121
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ef764356c5f74eb0aff15753e7f83a020c68c091
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446530"
---
# <a name="core-network-components"></a>Composants du réseau principal

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Ce guide fournit des instructions sur la façon de planifier et déployer les composants centraux requis pour un réseau pleinement fonctionnel et un nouveau domaine Active Directory dans une nouvelle forêt.

> [!NOTE]
> Ce guide est disponible au téléchargement au format Microsoft Word à partir de la Galerie TechNet. Pour plus d’informations, consultez [Core réseau Guide pour Windows Server 2016](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683).

Ce guide contient les sections suivantes.

- [À propos de ce guide](#BKMK_about)

- [Vue d’ensemble du réseau de base](#BKMK_overview)

- [Planification de réseau de base](#BKMK_planning)

- [Déploiement de réseau de base](#BKMK_deployment)

- [Ressources techniques supplémentaires](#BKMK_resources)

- [Annexes A à E](#BKMK_appendix)

## <a name="BKMK_about"></a>À propos de ce guide
Ce guide s’adresse aux administrateurs système et réseau qui installent un nouveau réseau ou qui souhaitent créer un réseau basé sur des domaines pour remplacer un réseau constitué de groupes de travail. Le scénario de déploiement fourni dans ce guide est particulièrement utile si vous savez déjà que vous aurez besoin d’ajouter des services et des fonctionnalités supplémentaires à votre réseau dans le futur.

Il est recommandé de passer en revue les guides de conception et de développement de chacune des technologies utilisées dans ce scénario de déploiement pour vous aider à déterminer si ce guide propose les services et la configuration dont vous avez besoin.

Un réseau de base est un ensemble de matériels, périphériques et logiciels réseau qui fournissent les services centraux répondant aux besoins en technologies de l’information de votre organisation.

Un réseau de base Windows Server présente de nombreux avantages, notamment :

-   Des protocoles centraux pour la connectivité réseau entre les ordinateurs et d’autres périphériques compatibles avec le protocole TCP/IP (Transmission Control Protocol/Internet Protocol). TCP/IP est une suite de protocoles standard utilisée pour la connexion d’ordinateurs et la création de réseaux. TCP/IP est un logiciel de protocole réseau fourni avec les systèmes d’exploitation de Microsoft Windows qui implémente et prend en charge la suite de protocoles TCP/IP.

-   L’attribution automatique d’adresses IP avec le protocole DHCP (Dynamic Host Configuration Protocol) pour les ordinateurs et autres périphériques configurés en tant que clients DHCP. La configuration manuelle d’adresses IP sur tous les ordinateurs de votre réseau prend du temps et est moins souple que l’attribution dynamique de configurations d’adresses IP aux ordinateurs et autres périphériques avec un serveur DHCP.

-   Un service de résolution de noms (DNS, Domain Name System). Le service DNS permet aux utilisateurs, aux ordinateurs, aux applications et aux services de trouver les adresses IP des ordinateurs et périphériques du réseau d’après le nom de domaine complet (FQDN, Fully Qualified Domain Name) de l’ordinateur ou du périphérique.

-   Une forêt, qui est constituée d’un ou de plusieurs domaines Active Directory partageant les mêmes définitions de classes et d’attributs (schéma), les mêmes informations de site et de réplication (configuration) et des fonctionnalités de recherche à l’échelle de la forêt (catalogue global).

-   Un domaine racine de forêt, qui est le premier domaine de forêt créé dans une nouvelle forêt. Les groupes Administrateurs de l’entreprise et Administrateurs du schéma, qui sont des groupes d’administration s’appliquant à l’ensemble de la forêt, sont situés dans le domaine racine de la forêt. Par ailleurs, un domaine racine de forêt, comme les autres domaines, est une collection d’objets de type ordinateur, utilisateur et groupe qui sont définis par l’administrateur dans les services de domaine Active Directory (AD DS). Ces objets partagent une base de données d’annuaire commune et des stratégies de sécurité. Ils peuvent également partager des relations de sécurité avec d’autres domaines si vous ajoutez des domaines à mesure que votre organisation se développe. Le service d’annuaire stocke également des données d’annuaire et permet aux ordinateurs, applications et utilisateurs autorisés d’accéder aux données.

-   Une base de données de comptes d’utilisateurs et d’ordinateurs. Le service d’annuaire offre une base de données de comptes d’utilisateurs centralisée qui vous permet de créer des comptes d’utilisateurs et d’ordinateurs pour les personnes et les ordinateurs qui sont autorisés à se connecter à votre réseau et à accéder aux ressources réseau, comme les applications, les bases de données, les fichiers et dossiers partagés, ainsi que les imprimantes.

Un réseau de base vous permet également de faire évoluer votre réseau à mesure que votre organisation se développe et que vos besoins en technologies de l’information évoluent. Par exemple, avec un réseau de base, vous pouvez ajouter des domaines, sous-réseaux IP, services d’accès à distance, les services sans fil et autres fonctionnalités et rôles serveur fournis par Windows Server 2016.

### <a name="network-hardware-requirements"></a>Configuration matérielle réseau
Pour réussir le déploiement d’un réseau de base, vous devez déployer du matériel réseau, notamment :

-   un câblage Ethernet, Fast Ethernet ou Gigabyte Ethernet ;

-   un concentrateur, un commutateur de couche 2 ou 3, un routeur, ou un autre périphérique assurant la fonction de relais du trafic réseau entre les ordinateurs et les périphériques ;

-   des ordinateurs conformes à la configuration minimale requise pour leurs systèmes d’exploitation client et serveur respectifs.

## <a name="what-this-guide-does-not-provide"></a>Ce que ce guide ne contient pas
Ce guide ne fournit pas d’instructions sur le déploiement des éléments suivants :

-   Matériel réseau, comme le câblage, les routeurs, les commutateurs et les concentrateurs

-   Ressources réseau supplémentaires, comme les imprimantes et les serveurs de fichiers

-   Connectivité Internet

-   Accès à distance

-   Accès sans fil

-   Déploiement d’ordinateurs clients

> [!NOTE]
> Ordinateurs qui exécutent les systèmes d’exploitation clients Windows sont configurés par défaut pour recevoir des baux d’adresses IP du serveur DHCP. Par conséquent, aucune configuration DHCP ou IPv4 (Internet Protocol version 4) supplémentaire n’est requise sur les ordinateurs clients.

## <a name="technology-overviews"></a>Vues d’ensemble des technologies
Les sections suivantes fournissent de brèves vues d’ensemble des technologies à déployer pour créer un réseau de base.

### <a name="active-directory-domain-services"></a>Services de domaine Active Directory
Un annuaire est une structure hiérarchique où sont stockées les informations relatives aux objets du réseau, tels que les utilisateurs et les ordinateurs. Un service d’annuaire, telles que les services AD DS, fournit les méthodes pour le stockage des données d’annuaire et rendre ces données disponibles pour les administrateurs et les utilisateurs du réseau. Par exemple, les services AD DS stocke des informations sur les comptes d’utilisateur, y compris les noms, les adresses de messagerie, les mots de passe et les numéros de téléphone et permet aux autres utilisateurs autorisés sur le même réseau accéder à ces informations.

### <a name="dns"></a>DNS
DNS est un protocole de résolution de noms pour les réseaux TCP/IP, tels qu’Internet ou le réseau d’une organisation. Un serveur DNS héberge les informations qui permettent aux ordinateurs clients et aux services de résoudre des noms DNS alphanumériques explicites en adresses IP utilisées par les ordinateurs pour communiquer.

### <a name="dhcp"></a>DHCP
DHCP est une norme IP permettant de simplifier la gestion de la configuration IP de l’hôte. Cette norme permet d’utiliser des serveurs DHCP comme moyen de gérer l’allocation dynamique d’adresses IP et d’autres informations de configuration pour les clients DHCP sur votre réseau.

DHCP vous permet d’utiliser un serveur DHCP pour attribuer dynamiquement une adresse IP à un ordinateur ou un autre périphérique, tel qu’une imprimante, sur votre réseau local. Chaque ordinateur d’un réseau TCP/IP doit avoir une adresse IP unique, car cette adresse et son masque de sous-réseau associé sont utilisés pour identifier l’ordinateur hôte et le sous-réseau auquel l’ordinateur est joint. Avec le protocole DHCP, vous avez l’assurance que tous les ordinateurs configurés en tant que clients DHCP reçoivent une adresse IP correspondant à leur emplacement réseau et leur sous-réseau. De plus, en utilisant les options DHCP, telles qu’une passerelle par défaut et des serveurs DNS, vous fournissez automatiquement aux clients DHCP les informations dont ils ont besoin pour fonctionner correctement sur votre réseau.

Pour les réseaux TCP/IP, le réseau DHCP réduit la complexité et le travail d’administration nécessaires pour reconfigurer les ordinateurs.

### <a name="tcpip"></a>TCP/IP
TCP/IP dans Windows Server 2016 est la suivante :

-   un logiciel réseau reposant sur des protocoles réseau standard ;

-   un protocole réseau d’entreprise routable qui prend en charge la connexion de votre ordinateur Windows aux environnements de réseau local (LAN, Local Area Network) et de réseau étendu (WAN, Wide Area Network) ;

-   un ensemble de technologies et d’utilitaires centraux permettant de connecter votre ordinateur Windows à des systèmes hétérogènes pour le partage d’informations ;

-   une base pour pouvoir accéder aux services Internet globaux, comme les serveurs Web et FTP (File Transfer Protocol) ;

-   une infrastructure client/serveur interplateforme, évolutive et robuste.

TCP/IP propose des utilitaires TCP/IP de base qui permettent aux ordinateurs Windows de se connecter à d’autres systèmes d’exploitation Microsoft et non-Microsoft pour partager des informations, notamment :

-    Windows Server 2016

-   Windows 10

-    Windows Server 2012 R2

-   Windows 8.1

-    Windows Server 2012

-   Windows 8

-    Windows Server 2008 R2

-    Windows 7

-    Windows Server 2008

-   Windows Vista

-   Hôtes Internet

-   Systèmes Apple Macintosh

-   Gros systèmes IBM

-   Systèmes UNIX et Linux

-   Systèmes Open VMS

-   Imprimantes réseau

-   Tablettes et téléphones cellulaires avec Ethernet câblé ou technologie 802.11 sans fil activée

## <a name="BKMK_overview"></a>Vue d’ensemble du réseau de base
L’illustration suivante décrit la topologie d’un réseau de base Windows Server.

![Topologie de réseau Windows Server Core](../media/Core-Network-Guide/cng16_overview.jpg)

> [!NOTE]
> Ce guide comprend également des instructions pour l’ajout facultatif de serveurs NPS (Network Policy Server) et Web (IIS) à la topologie de votre réseau. Ces serveurs peuvent servir de base de déploiement des solutions d’accès réseau sécurisées, telles que les connexions sans fil et câblées 802.1X déployées à l’aide des guides d’accompagnement du réseau de base. Pour plus d’informations, voir [Déploiement de fonctionnalités facultatives pour l’authentification d’accès réseau et les services Web](#BKMK_optionalfeatures).

### <a name="core-network-components"></a>Composants d’un réseau de base
Les composants d’un réseau de base sont les suivants :

##### <a name="router"></a>Routeur
Ce guide de déploiement fournit des instructions sur le déploiement d’un réseau de base avec deux sous-réseaux séparés par un routeur sur lequel l’acheminement DHCP est activé. Vous pouvez néanmoins déployer un commutateur de couche 2, un commutateur de couche 3 ou un concentrateur, en fonction de vos exigences et de vos ressources. Si vous déployez un commutateur, celui-ci doit prendre en charge l’acheminement DHCP. Sinon, vous devez placer un serveur DHCP sur chaque sous-réseau. Si vous déployez un concentrateur, vous déployez un seul sous-réseau et vous n’avez pas besoin de l’acheminement DHCP ni d’une seconde étendue sur votre serveur DHCP.

##### <a name="static-tcpip-configurations"></a>Configurations TCP/IP statiques
Les serveurs de ce déploiement sont configurés avec des adresses IPv4 statiques. Les ordinateurs clients sont configurés par défaut pour recevoir des baux d’adresses IP du serveur DHCP.

##### <a name="active-directory-domain-services-global-catalog-and-dns-server-dc1"></a>Catalogue global des services de domaine Active Directory et serveur DNS DC1
Les deux Services de domaine Active Directory (AD DS) et le système DNS (Domain Name) sont installés sur ce serveur, appelé DC1, qui fournit le répertoire et à tous les ordinateurs et périphériques du réseau, les services de résolution de nom.

##### <a name="dhcp-server-dhcp1"></a>Serveur DHCP DHCP1
Le serveur DHCP, appelé DHCP1, est configuré avec une étendue qui fournit des baux d’adresses IP aux ordinateurs sur le sous-réseau local. Le serveur DHCP peut également être configuré avec des étendues supplémentaires pour fournir des baux d’adresses IP aux ordinateurs sur d’autres sous-réseaux si l’acheminement DHCP est configuré sur les routeurs.

##### <a name="client-computers"></a>Ordinateurs clients
Ordinateurs qui exécutent les systèmes d’exploitation clients Windows sont configurés par défaut en tant que clients DHCP, qui obtiennent des adresses IP et des options DHCP automatiquement du serveur DHCP.

## <a name="BKMK_planning"></a>Planification de réseau de base
Avant de déployer un réseau de base, vous devez effectuer les tâches suivantes :

-   [Planification des sous-réseaux](#bkmk_NetFndtn_Pln_Subnt)

-   [Planification de la configuration de base de tous les serveurs](#bkmk_NetFndtn_Pln_AllSrvrs)

-   [Planification du déploiement de DC1](#bkmk_NetFndtn_Pln_AD-DNS-01)

-   [Planification de l’accès de domaine](#bkmk_NetFndtn_Pln_DomAccess)

-   [Planification du déploiement du serveur DHCP1](#bkmk_NetFndtn_Pln_DHCP-01)

Chacun de ces éléments est présenté en détail dans les sections qui suivent.

> [!NOTE]
> Pour aider à planifier votre déploiement, consultez également [annexe E - feuille de préparation de planification réseau Core](#BKMK_E).

### <a name="bkmk_NetFndtn_Pln_Subnt"></a>Planification des sous-réseaux
Dans les réseaux TCP/IP (Transmission Control Protocol/Internet Protocol), des routeurs sont utilisés pour interconnecter le matériel et les logiciels utilisés sur différents segments physiques du réseau, appelés sous-réseaux, et pour acheminer des paquets IP entre ces sous-réseaux. Vous devez déterminer la disposition physique de votre réseau, y compris le nombre de routeurs et de sous-réseaux dont vous avez besoin, avant de suivre les instructions de ce guide.

Par ailleurs, pour configurer les serveurs de votre réseau avec des adresses IP statiques, vous devez déterminer la plage d’adresses IP que vous souhaitez utiliser pour le sous-réseau sur lequel sont situés les serveurs de votre réseau de base. Dans ce guide, plages d’adresses IP privées 10.0.0.1 - 10.0.0.254 et 10.0.1.1 - 10.0.1.254 sont utilisés comme exemples, mais vous pouvez utiliser toute plage d’adresses IP privées que vous préférez.

> [!IMPORTANT]
> Une fois que vous avez sélectionné la plage d’adresses IP à utiliser pour chaque sous-réseau, veillez à configurer votre routeur avec une adresse IP de la même plage d’adresses IP que celle utilisée pour le sous-réseau sur lequel le routeur est installé. Par exemple, si votre routeur est configuré par défaut avec l’adresse IP 192.168.1.1, et que vous l’installez sur un sous-réseau avec la plage d’adresses IP 10.0.0.0/24, vous devez reconfigurer le routeur pour utiliser à la place une adresse IP de la plage d’adresses IP 10.0.0.0/24.

Les plages d’adresses IP privées reconnues sont spécifiées par le document RFC (Request for Comments) Internet 1918 :

-   10.0.0.0 - 10.255.255.255

-   172.16.0.0 - 172.31.255.255

-   192.168.0.0 - 192.168.255.255

Lorsque vous utilisez les plages d’adresses IP privées spécifiées dans le document RFC 1918, vous ne pouvez pas vous connecter directement à Internet en utilisant une adresse IP privée, car les demandes dirigées vers ces adresses ou émanant de celles-ci sont automatiquement ignorées par les routeurs des fournisseurs de services Internet. Pour ajouter ultérieurement la connectivité Internet à votre réseau de base, vous devez prendre un abonnement auprès d’un fournisseur de services Internet de manière à obtenir une adresse IP publique.

> [!IMPORTANT]
> Lorsque vous utilisez des adresses IP privées, vous devez utiliser un serveur proxy ou un serveur NAT (Network Address Translation) quel qu’il soit pour convertir les plages d’adresses IP privées sur votre réseau local en adresse IP publique qui peut être routée sur Internet. La plupart des routeurs fournissant des services NAT, le choix d’un routeur NAT ne devrait pas poser de problème particulier.

Pour plus d’informations, voir [Planification du déploiement du serveur DHCP1](#bkmk_NetFndtn_Pln_DHCP-01).

### <a name="bkmk_NetFndtn_Pln_AllSrvrs"></a>Planification de la configuration de base de tous les serveurs
Pour chaque serveur du réseau de base, vous devez renommer l’ordinateur, lui attribuer une adresse IPv4 statique et configurer d’autres propriétés TCP/IP.

#### <a name="planning-naming-conventions-for-computers-and-devices"></a>Planification des conventions de dénomination pour les ordinateurs et les périphériques
Pour garantir l’homogénéité de votre réseau, il est judicieux d’utiliser des noms cohérents pour les serveurs, les imprimantes et autres périphériques. Des noms d’ordinateurs peuvent être utilisés pour aider les utilisateurs et les administrateurs à identifier le rôle et l’emplacement du serveur, de l’imprimante ou des autres périphériques. Par exemple, si vous avez trois serveurs DNS, un à San Francisco, un à Los Angeles et un à Chicago, vous pouvez utiliser la convention d’affectation de noms *fonction_serveur*-*emplacement* - *nombre*:

-   DNS-DEN-01. Ce nom représente le serveur DNS déployé à Denver (Colorado). Si d’autres serveurs DNS sont déployés à Denver, la valeur numérique du nom peut être incrémentée (DNS-DEN-02, DNS-DEN-03, etc.).

-   DNS-SPAS-01. Ce nom représente le serveur DNS déployé à South Pasadena (Californie).

-   DNS-ORL-01. Ce nom représente le serveur DNS déployé à Orlando (Floride).

Dans ce guide, la convention de dénomination des serveurs utilisée est très simple : le nom se compose de la fonction du serveur principal et d’un numéro. Par exemple, le contrôleur de domaine est appelé DC1 et le serveur DHCP, DHCP1.

Nous vous recommandons de choisir une convention de dénomination avant de procéder à l’installation de votre réseau de base à l’aide de ce guide.

#### <a name="planning-static-ip-addresses"></a>Planification des adresses IP statiques
Avant de configurer chaque ordinateur avec une adresse IP statique, vous devez planifier vos sous-réseaux et les plages d’adresses IP. Par ailleurs, vous devez déterminer les adresses IP de vos serveurs DNS. Si vous prévoyez d’installer un routeur qui fournit l’accès à d’autres routeurs, comme des sous-réseaux supplémentaires ou Internet, vous devez connaître l’adresse IP du routeur, également appelé passerelle par défaut, pour pouvoir configurer les adresses IP statiques.

Le tableau ci-dessous propose des exemples de valeurs pour la configuration des adresses IP statiques.

|Éléments de configuration|Exemples de valeurs|
|-----------------------|------------------|
|Adresse IP|10.0.0.2|
|Masque de sous-réseau|255.255.255.0|
|Passerelle par défaut (adresse IP du routeur)|10.0.0.1|
|Serveur DNS préféré|10.0.0.2|

> [!NOTE]
> Si vous planifiez de déployer plusieurs serveurs DNS, vous pouvez également planifier l’adresse IP du serveur DNS auxiliaire.

### <a name="bkmk_NetFndtn_Pln_AD-DNS-01"></a>Planification du déploiement de DC1
Voici les principales étapes de planification avant d’installer les Services de domaine Active Directory (AD DS) et DNS sur DC1.

#### <a name="planning-the-name-of-the-forest-root-domain"></a>Planification du nom du domaine racine de forêt
Une première étape dans le processus de conception d’AD DS consiste à déterminer le nombre de forêts requiert de votre organisation. Une forêt est le conteneur AD DS de niveau supérieur et se compose d’un ou plusieurs domaines qui partagent un schéma commun et un catalogue global. Une organisation peut avoir plusieurs forêts, même si la plupart préfèrent utiliser un modèle à une seule forêt qui est plus facile à administrer.

Lorsque vous créez le premier contrôleur de domaine dans votre organisation, vous créez en fait le premier domaine (également appelé le domaine racine de forêt) et la première forêt. Toutefois, vous devez avant cela déterminer le nom de domaine le plus approprié pour votre organisation. Le nom de l’organisation est généralement utilisé comme nom de domaine, lequel est bien souvent un nom déposé. Si vous planifiez de déployer des serveurs Web externes basés sur Internet pour fournir des informations et des services à vos clients ou partenaires, choisissez un nom de domaine encore inutilisé, puis déposez ce nom pour que votre organisation en ait la propriété.

#### <a name="planning-the-forest-functional-level"></a>Planification du niveau fonctionnel de la forêt
Lors de l’installation des services AD DS, vous devez choisir le niveau fonctionnel de forêt que vous souhaitez utiliser. La fonctionnalité des domaines et forêts, introduite dans Windows Server 2003 Active Directory, fournit un moyen d’activer les fonctionnalités Active Directory au sein de votre environnement réseau domaine ou forêt échelle. Différents niveaux de fonctionnalités de domaine et de forêt sont disponibles, selon votre environnement.

La fonctionnalité de forêt active les fonctionnalités dans tous les domaines de votre forêt. Les niveaux fonctionnels de forêt suivants sont disponibles :

-    Windows Server 2008 . Ce niveau fonctionnel de forêt prend en charge uniquement les contrôleurs de domaine qui exécutent Windows Server 2008 et versions ultérieures du système d’exploitation Windows Server.

-    Windows Server 2008 R2. Ce niveau fonctionnel de forêt prend en charge les contrôleurs de domaine Windows Server 2008 R2 et les contrôleurs de domaine qui exécutent des versions ultérieures du système d’exploitation Windows Server.

-    Windows Server 2012. Ce niveau fonctionnel de forêt prend en charge les contrôleurs de domaine Windows Server 2012 et les contrôleurs de domaine qui exécutent des versions ultérieures du système d’exploitation Windows Server.

-    Windows Server 2012 R2. Ce niveau fonctionnel de forêt prend en charge les contrôleurs de domaine Windows Server 2012 R2 et les contrôleurs de domaine qui exécutent des versions ultérieures du système d’exploitation Windows Server.

-    Windows Server 2016. Ce niveau fonctionnel de forêt prend en charge uniquement les contrôleurs de domaine Windows Server 2016 et les contrôleurs de domaine qui exécutent des versions ultérieures du système d’exploitation Windows Server.

Si vous déployez un nouveau domaine dans une nouvelle forêt et tous vos contrôleurs de domaine exécutent Windows Server 2016, il est recommandé de configurer les services AD DS avec le niveau fonctionnel de forêt Windows Server 2016 pendant l’installation d’AD DS.

> [!IMPORTANT]
> Une fois que le niveau fonctionnel de la forêt a été augmenté, les contrôleurs de domaine exécutant des systèmes d’exploitation antérieurs ne peuvent plus être inclus dans la forêt. Par exemple, si vous augmentez le niveau fonctionnel de la forêt à Windows Server 2016, les contrôleurs de domaine exécutant Windows Server 2012 R2 ou Windows Server 2008 ne peut pas ajoutés à la forêt.

Exemples d’éléments de configuration pour les services AD DS sont fournies dans le tableau suivant.

|Éléments de configuration|Exemples de valeurs|
|------------------------|-------------------|
|Nom DNS complet|Exemples :<br /><br />-   corp.contoso.com<br />-   example.com|
|Niveau fonctionnel de forêt|-Windows Server 2008 <br />-Windows Server 2008 R2 <br />-Windows Server 2012 <br />-Windows Server 2012 R2 <br />-Windows Server 2016|
|Emplacement du dossier de la base de données des services de domaine Active Directory|E:\Configuration\\<br /><br />Ou acceptez la valeur par défaut.|
|Emplacement du dossier des fichiers journaux des services de domaine Active Directory|E:\Configuration\\<br /><br />Ou acceptez la valeur par défaut.|
|Emplacement du dossier SYSVOL des services de domaine Active Directory|E:\Configuration\\<br /><br />Ou acceptez la valeur par défaut.|
|Mot de passe administrateur de restauration des services d’annuaire|**J\*p2leO4$F**|
|Nom du fichier de réponses (facultatif)|**AD DS_AnswerFile**|

#### <a name="planning-dns-zones"></a>Planification des zones DNS
Sur les serveurs DNS principaux intégrés à Active Directory, une zone de recherche directe est créée par défaut lors de l’installation du rôle serveur DNS. Une zone de recherche directe permet aux ordinateurs et aux périphériques de rechercher l’adresse IP d’un autre ordinateur ou d’un autre périphérique en fonction de son nom DNS. Il est recommandé de créer en parallèle une zone de recherche inversée DNS. Avec une requête de recherche inversée DNS, un ordinateur ou un périphérique peut découvrir le nom d’un autre ordinateur ou périphérique en utilisant son adresse IP. Le déploiement d’une zone de recherche inversée permet généralement d’améliorer les performances DNS et d’assurer un plus grand succès des requêtes DNS.

Lorsque vous créez une zone de recherche inversée, le domaine in-addr.arpa est installé dans les services DNS ; défini dans les normes DNS, ce domaine est réservé dans l’espace de noms DNS Internet afin de fournir un moyen fiable et pratique d’effectuer des requêtes inversées. Pour créer l’espace de noms inversé, des sous-domaines sont formés dans le domaine in-addr.arpa en utilisant le classement inversé des nombres dans la notation décimale séparée par des points des adresses IP.

Le domaine in-addr.arpa s’applique à tous les réseaux TCP/IP basés sur Internet Protocol version 4 (IPv4) d’adressage. L’Assistant Nouvelle zone suppose automatiquement que vous utilisez ce domaine lorsque vous créez une zone de recherche inversée.

Dans l’Assistant Nouvelle zone, nous vous recommandons de sélectionner les options suivantes :

|Éléments de configuration|Exemples de valeurs|
|-----------------------|------------------|
|Type de zone|**Zone principale** et **Enregistrer la zone dans Active Directory** sont sélectionnés|
|Étendue de réplication de la zone Active Directory|**Tous les serveurs DNS dans ce domaine**|
|Première page Nom de la zone de recherche inversée de l’Assistant|**Zone de recherche inversée IPv4**|
|Seconde page Nom de la zone de recherche inversée de l’Assistant|ID réseau = 10.0.0.|
|Mises à jour dynamiques|**Autoriser uniquement les mises à jour dynamiques sécurisées**|

### <a name="bkmk_NetFndtn_Pln_DomAccess"></a>Planification de l’accès de domaine
Pour vous connecter au domaine, l’ordinateur doit être un ordinateur membre du domaine et le compte d’utilisateur doit être créé dans AD DS avant la tentative d’ouverture de session.

> [!NOTE]
> Chaque ordinateur exécutant Windows comporte une base de données de comptes d’utilisateurs et de groupes locaux ; il s’agit de la base de données des comptes d’utilisateurs du Gestionnaire de comptes de sécurité (SAM, Security Accounts Manager). Lorsque vous créez un compte d’utilisateur dans la base de données du Gestionnaire de comptes de sécurité sur un ordinateur local, vous pouvez ouvrir une session sur ce dernier, mais pas sur un domaine. Les comptes d’utilisateurs de domaine sont créés dans la console MMC (Microsoft Management Console) Utilisateurs et ordinateurs Active Directory sur un contrôleur de domaine, et pas avec des utilisateurs et groupes locaux sur l’ordinateur local.

Une fois la première ouverture de session réussie avec les informations d’ouverture de session de domaine, les paramètres d’ouverture de session sont conservés sauf si l’ordinateur est supprimé du domaine ou si les paramètres d’ouverture de session sont modifiés manuellement.

Avant d’ouvrir une session sur le domaine, effectuez les tâches suivantes :

-   Créez des comptes d’utilisateurs dans Utilisateurs et ordinateurs Active Directory. Chaque utilisateur doit avoir un compte d’utilisateur des services de domaine Active Directory dans Utilisateurs et ordinateurs Active Directory. Pour plus d’informations, voir [Créer un compte d’utilisateur dans Utilisateurs et ordinateurs Active Directory](#BKMK_createUA).

-   Vérifiez que les adresses IP sont correctement configurées. Pour joindre un ordinateur au domaine, l’ordinateur doit avoir une adresse IP. Dans ce guide, les serveurs sont configurés avec des adresses IP statiques et les ordinateurs clients reçoivent des baux d’adresses IP du serveur DHCP. C’est pourquoi, le serveur DHCP doit être déployé avant que vous ne joigniez des clients au domaine. Pour plus d’informations, voir [Déploiement du serveur DHCP1](#BKMK_deployDHCP01).

-   Joignez l’ordinateur au domaine. Tout ordinateur fournissant des ressources réseau ou y accédant doit être joint au domaine. Pour plus d’informations, voir [Joindre des ordinateurs au domaine et ouvrir une session](#BKMK_joinlogserver) et [Joindre des ordinateurs clients au domaine et ouvrir une session](#BKMK_joinlogclients).

### <a name="bkmk_NetFndtn_Pln_DHCP-01"></a>Planification du déploiement du serveur DHCP1
Cette section décrit les principales étapes de planification qui doivent être suivies avant d’installer le rôle serveur DHCP sur DHCP1.

#### <a name="planning-dhcp-servers-and-dhcp-forwarding"></a>Planification des serveurs DHCP et de l’acheminement DHCP
Les messages DHCP étant des messages de diffusion, ils ne sont pas acheminés entre les sous-réseaux par les routeurs. Si vous avez plusieurs sous-réseaux et souhaitez proposer le service DHCP sur chacun d’eux, vous devez procéder comme suit :

-   Installez un serveur DHCP sur chaque sous-réseau.

-   Configurez les routeurs pour qu’ils acheminent les messages de diffusion DHCP dans les sous-réseaux et configurez plusieurs étendues sur le serveur DHCP, à raison d’une étendue par sous-réseau.

En règle générale, il est économiquement plus avantageux de configurer des routeurs pour qu’ils acheminent les messages de diffusion DHCP plutôt que de déployer un serveur DHCP sur chaque segment physique du réseau.

#### <a name="planning-ip-address-ranges"></a>Planification de plages d’adresses IP
Chaque sous-réseau doit avoir sa propre plage d’adresses IP unique. Ces plages sont représentées sur un serveur DHCP avec des étendues.

Une étendue est un groupement administratif des adresses IP des ordinateurs d’un sous-réseau qui utilisent le service DHCP. L’administrateur crée d’abord une étendue pour chaque sous-réseau physique, puis utilise l’étendue pour définir les paramètres utilisés par les clients.

Une étendue possède les propriétés suivantes :

-   une plage d’adresses IP où inclure ou exclure les adresses utilisées pour les offres de bail de service DHCP ;

-   un masque de sous-réseau, qui détermine le préfixe de sous-réseau correspondant à une adresse IP donnée ;

-   un nom affecté à l’étendue lors de sa création ;

-   des valeurs de durée de bail, qui sont affectées aux clients DHCP recevant des adresses IP allouées de manière dynamique ;

-   des options d’étendue DHCP configurées pour être affectées aux clients DHCP, telles que l’adresse IP d’un serveur DNS et l’adresse IP d’un routeur/d’une passerelle par défaut ;

-   des réservations, utilisées de manière optionnelle pour s’assurer qu’un client DHCP reçoit toujours la même adresse IP.

Avant de déployer vos serveurs, dressez la liste de vos sous-réseaux et des plages d’adresses IP à utiliser pour chaque sous-réseau.

#### <a name="planning-subnet-masks"></a>Planification des masques de sous-réseaux
Au sein d’une adresse IP, les ID de réseau et les ID d’hôte sont différenciés à l’aide d’un masque de sous-réseau. Chaque masque de sous-réseau est un nombre de 32 bits qui utilise des groupes de bits consécutifs composés uniquement de 1 pour identifier l’ID de réseau et composés uniquement de 0 pour identifier les parties de l’adresse IP relatives à l’ID d’hôte.

Par exemple, un masque de sous-réseau normalement utilisé avec l’adresse IP 131.107.16.200 correspond au nombre binaire de 32 bits suivant :

```
11111111 11111111 00000000 00000000
```

Ce numéro de masque de sous-réseau est 16 bits suivies de 16 bits de valeur zéro, indiquant que les sections réseau les ID ID et l’hôte de cette adresse IP sont les deux une longueur de 16 bits. Ce masque de sous-réseau est affiché en notation décimale séparée par des points sous la forme 255.255.0.0.

Le tableau ci-dessous indique les masques de sous-réseau des classes d’adresses Internet.

|Classe d’adresses|Bits du masque de sous-réseau|Masque de sous-réseau|
|-----------------|------------------------|---------------|
|Classe A|11111111 00000000 00000000 00000000|255.0.0.0|
|Classe B|11111111 11111111 00000000 00000000|255.255.0.0|
|Classe C|11111111 11111111 11111111 00000000|255.255.255.0|

Lorsque vous créez une étendue dans DHCP et que vous entrez la plage d’adresses IP de cette étendue, DHCP fournit ces valeurs de masque de sous-réseau par défaut. Les valeurs de masque de sous-réseau par défaut conviennent généralement pour la plupart des réseaux qui ne nécessitent aucune configuration particulière et où chaque segment de réseau IP correspond au même réseau physique.

Dans d’autres cas, vous pourrez utiliser des masques de sous-réseau personnalisés pour implémenter la mise en sous-réseau IP. Dans le cadre de la mise en sous-réseau IP, vous pouvez subdiviser la partie ID d’hôte par défaut d’une adresse IP pour spécifier des sous-réseaux, qui sont des sous-divisions de l’ID de réseau basé sur la classe.

En personnalisant la longueur du masque de sous-réseau, vous pouvez réduire le nombre de bits utilisés pour l’ID d’hôte.

Pour éviter les problèmes d’adressage et de routage, vous devez veiller à ce que tous les ordinateurs TCP/IP d’un segment réseau utilisent le même masque de sous-réseau et que chaque ordinateur ou périphérique possède une adresse IP unique.

#### <a name="planning-exclusion-ranges"></a>Planification des plages d’exclusion
Lorsque vous définissez une étendue sur un serveur DHCP, vous spécifiez une plage d’adresses IP qui inclut toutes les adresses IP que le serveur DHCP est autorisé à louer aux clients DHCP, tels que les ordinateurs et autres périphériques. Si, par la suite, vous configurez manuellement certains serveurs et autres périphériques avec des adresses IP statiques de la même plage d’adresses IP que le serveur DHCP utilise, vous risquez involontairement d’attribuer à un périphérique une adresse IP qui a déjà été attribuée à un autre périphérique par le serveur DHCP, et de générer ainsi un conflit d’adresse IP.

Vous pouvez éviter ce problème en créant une plage d’exclusion pour l’étendue DHCP. Une plage d’exclusion est une contiguës plage d’adresses IP au sein de la plage d’adresses IP de l’étendue que le serveur DHCP n’est pas autorisé à utiliser. Si vous créez une plage d’exclusion, le serveur DHCP ne peut attribuer aucune des adresses de cette plage. Vous pouvez alors attribuer manuellement ces adresses sans risque de générer un conflit d’adresse IP.

Vous pouvez exclure des adresses IP de la distribution par le serveur DHCP en créant une plage d’exclusion pour chaque étendue. Vous devez utiliser des exclusions pour tous les périphériques configurés avec une adresse IP statique. Dans les adresses exclues doivent figurer toutes les adresses IP que vous avez attribuées manuellement à d’autres serveurs DHCP, clients non DHCP, stations de travail sans disque ou clients de routage et d’accès à distance et clients PPP.

Il est recommandé de configurer votre plage d’exclusion avec des adresses supplémentaires afin de prendre en compte la croissance future du réseau. Le tableau suivant fournit un exemple de plage d’exclusion pour une étendue avec une plage d’adresses IP de 10.0.0.1 - 10.0.0.254 et un masque de sous-réseau 255.255.255.0.

|Éléments de configuration|Exemples de valeurs|
|-----------------------|------------------|
|Adresse IP de début de la plage d’exclusion|10.0.0.1|
|Adresse IP de fin de la plage d’exclusion|10.0.0.25|

#### <a name="planning-tcpip-static-configuration"></a>Planification de la configuration TCP/IP statique
Certains périphériques, comme les routeurs, les serveurs DHCP et les serveurs DNS doivent être configurés avec une adresse IP statique. Par ailleurs, vous voudrez peut-être garantir que des périphériques supplémentaires, comme des imprimantes, disposent toujours de la même adresse IP. Dressez la liste des périphériques que vous souhaitez configurer de manière statique pour chaque sous-réseau, puis planifiez la plage d’exclusion que vous voulez utiliser sur le serveur DHCP pour faire en sorte que le serveur DHCP ne loue pas l’adresse IP d’un périphérique configuré de manière statique. Une plage d’exclusion est une séquence limitée d’adresses IP au sein d’une étendue, qui est exclue des offres du service DHCP. Les plages d’exclusion garantissent qu’aucune adresse figurant dans ces plages n’est offerte par le serveur aux clients DHCP sur votre réseau.

Par exemple, si la plage d’adresses IP d’un sous-réseau est 192.168.0.1 à 192.168.0.254 et que vous souhaitez configurer dix périphériques avec une adresse IP statique, vous pouvez créer une plage d’exclusion pour l’étendue 192.168.0.*x* qui inclut dix adresses IP ou plus : 192.168.0.1 à 192.168.0.15.

Dans cet exemple, vous utilisez dix des adresses IP exclues pour configurer des serveurs et autres périphériques avec des adresses IP statiques. Il reste cinq adresses IP pour configurer de manière statique de nouveaux périphériques que vous voudrez peut-être ajouter plus tard. Avec cette plage d’exclusion, le serveur DHCP dispose d’un pool d’adresses allant de 192.168.0.16 à 192.168.0.254.

Exemples supplémentaires d’éléments de configuration pour les services AD DS et DNS sont fournis dans le tableau suivant.

|Éléments de configuration|Exemples de valeurs|
|-----------------------|------------------|
|Liaisons de connexion réseau|Ethernet|
|Paramètres du serveur DNS|DC1.corp.contoso.com|
|Adresse IP du serveur DNS préféré|10.0.0.2|
|Valeurs de la boîte de dialogue Ajouter un étendue<br /><br />1.  Nom de l'étendue<br />2.  Adresse IP de début<br />3.  Adresse IP de fin<br />4.  Masque de sous-réseau<br />5.  Passerelle par défaut (facultatif)<br />6.  Durée du bail|1.  Sous-réseau principal<br />2.  10.0.0.1<br />3.  10.0.0.254<br />4.  255.255.255.0<br />5.  10.0.0.1<br />6. 8 jours|
|Mode d’opération du serveur DHCP IPv6|Non activé|

## <a name="BKMK_deployment"></a>Déploiement de réseau de base
Le déploiement d’un réseau de base comprend les étapes suivantes :

1.  [Configuration de tous les serveurs](#BKMK_configuringAll)

2.  [Déploiement du serveur DC1](#BKMK_deployADDNS01)

3.  [Jonction des serveurs au domaine et ouverture de session](#BKMK_joinlogserver)

4.  [Déploiement du serveur DHCP1](#BKMK_deployDHCP01)

5.  [Joindre des ordinateurs clients au domaine et ouverture de session](#BKMK_joinlogclients)

6.  [Déploiement des fonctionnalités facultatives pour l’authentification d’accès réseau et les services Web](#BKMK_optionalfeatures)

> [!NOTE]
> -   Les commandes Windows PowerShell équivalentes sont fournies pour la plupart des procédures décrites dans ce guide. Avant d’exécuter ces applets de commande dans Windows PowerShell, remplacez les exemples de valeurs par les valeurs qui sont appropriées pour votre déploiement de réseau. Par ailleurs, veillez à entrer chaque applet de commande sur une ligne distincte dans Windows PowerShell. Dans ce guide, certaines applets de commande peuvent se présenter sur plusieurs lignes en raison des contraintes de mise en forme et des paramètres d’affichage définis dans votre navigateur ou toute autre application.
> -   Les procédures décrites dans ce guide ne donnent aucune instruction dans le cas où la boîte de dialogue **Contrôle de compte d’utilisateur** s’ouvre pour demander votre autorisation de continuer. Si cette boîte de dialogue s’ouvre pendant que vous réalisez les procédures de ce guide, et si elle s’ouvre en réponse à vos actions, cliquez sur **Continuer**.

### <a name="BKMK_configuringAll"></a>Configuration de tous les serveurs
Avant d’installer d’autres technologies, comme les services de domaine Active Directory ou DHCP, assurez-vous d’effectuer les tâches suivantes :

-   [Renommer l’ordinateur](#BKMK_rename)

-   [Configurer une adresse IP statique](#BKMK_ip)

Vous pouvez utiliser les sections suivantes pour effectuer ces tâches sur chaque serveur.

Pour effectuer ces procédures, il est nécessaire d’appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent.

#### <a name="BKMK_rename"></a>Renommer l’ordinateur
Vous pouvez utiliser la procédure dans cette section pour modifier le nom d’un ordinateur. Cela peut être utile si vous souhaitez modifier un nom d’ordinateur qui a été créé automatiquement par le système d’exploitation.

> [!NOTE]
> Pour effectuer cette procédure en utilisant Windows PowerShell, ouvrez PowerShell et tapez les applets de commande suivantes sur des lignes distinctes, puis appuyez sur ENTRÉE. Remplacez *NomOrdinateur* par le nom d’ordinateur de votre choix.
>
> `Rename-Computer`*ComputerName*
>
> `Restart-Computer`

###### <a name="to-rename-computers-running-windows-server-2016-windows-server-2012-r2-and-windows-server-2012"></a>Pour renommer des ordinateurs exécutant Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

1.  Dans le Gestionnaire de serveur, cliquez sur **Serveur local**. Les **Propriétés** de l’ordinateur sont affichées dans le volet d’informations.

2.  Dans **Propriétés**, dans **Nom de l’ordinateur**, cliquez sur le nom d’ordinateur actuel. La boîte de dialogue **Propriétés système** s’ouvre. Cliquez sur **Modifier**. La boîte de dialogue **Modification du nom ou du domaine de l’ordinateur** s’ouvre.

3.  Dans la boîte de dialogue **Modification du nom ou du domaine de l’ordinateur**, dans **Nom de l’ordinateur**, tapez le nouveau nom à attribuer à l’ordinateur. Par exemple, pour nommer l’ordinateur DC1, tapez **DC1**.

4.  Cliquez deux fois sur **OK** , puis cliquez sur **Fermer**. Si vous souhaitez redémarrer immédiatement l’ordinateur pour terminer le processus de modification du nom, cliquez sur **Redémarrer maintenant**. Sinon, cliquez sur **Redémarrer ultérieurement**.

> [!NOTE]
> Pour plus d’informations sur la façon de renommer des ordinateurs qui exécutent d’autres systèmes d’exploitation de Microsoft, consultez [annexe A - changement de nom des ordinateurs](#BKMK_A).

#### <a name="BKMK_ip"></a>Configurer une adresse IP statique
Vous pouvez utiliser les procédures dans cette rubrique pour configurer le Kit de développement Internet Protocol version 4 (IPv4) les propriétés d’une connexion réseau avec une adresse IP statique d’adresses pour les ordinateurs exécutant Windows Server 2016.

> [!NOTE]
> Pour effectuer cette procédure en utilisant Windows PowerShell, ouvrez PowerShell et tapez les applets de commande suivantes sur des lignes distinctes, puis appuyez sur ENTRÉE. Remplacez les noms d’interface et les adresses IP utilisés dans cet exemple par les valeurs de configuration souhaitées pour votre ordinateur.
>
> `New-NetIPAddress -IPAddress 10.0.0.2 -InterfaceAlias "Ethernet" -DefaultGateway 10.0.0.1 -AddressFamily IPv4 -PrefixLength 24`
>
> `Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 127.0.0.1`

###### <a name="to-configure-a-static-ip-address-on-computers-running-windows-server-2016-windows-server-2012-r2-and-windows-server-2012"></a>Pour configurer une adresse IP statique sur les ordinateurs exécutant Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

1.  Dans la barre des tâches, cliquez avec le bouton droit sur l’icône Réseau, puis cliquez sur **Ouvrir le Centre Réseau et partage**.

2.  Dans **Centre réseau et partage**, cliquez sur **modifier les paramètres de la carte**. Le dossier **Connexions réseau** s’ouvre et affiche les connexions réseau disponibles.

3.  Dans **Connexions réseau**, cliquez avec le bouton droit sur la connexion que vous voulez configurer, puis cliquez sur **Propriétés**. La boîte de dialogue **Propriétés** de la connexion réseau s’ouvre.

4.  Dans la boîte de dialogue **Propriétés** de la connexion réseau, dans **Cette connexion utilise les éléments suivants**, sélectionnez **Protocole Internet version 4 (TCP/IPv4)** , puis cliquez sur **Propriétés**. La boîte de dialogue **Propriétés de Protocole Internet version 4 (TCP/IPv4)** s’ouvre.

5.  Dans **Propriétés de Protocole Internet version 4 (TCP/IPv4)** , sous l’onglet **Général**, cliquez sur **Utiliser l’adresse IP suivante**. Dans **Adresse IP**, tapez l’adresse IP que vous voulez utiliser.

6.  Appuyez sur Tab pour placer le curseur dans **Masque de sous-réseau**. Une valeur par défaut pour le masque de sous-réseau est entrée automatiquement. Acceptez le masque de sous-réseau par défaut, ou tapez le masque de sous-réseau que vous voulez utiliser.

7.  Dans **Passerelle par défaut**, tapez l’adresse IP de votre passerelle par défaut.

    > [!NOTE]
    > Vous devez configurer **Passerelle par défaut** avec la même adresse IP que celle que vous avez utilisée sur l’interface de réseau local (LAN) de votre routeur. Par exemple, si vous avez un routeur qui est connecté à la fois à un réseau étendu (WAN) comme Internet et à votre réseau local (LAN), configurez l’interface de réseau local avec l’adresse IP que vous utiliserez pour **Passerelle par défaut**. Prenons comme autre exemple un routeur qui est connecté à un réseau local LAN A et à un réseau local LAN B utilisant respectivement les plages d’adresses 10.0.0.0/24 et 192.168.0.0/24. Vous devez configurer l’adresse IP du routeur du réseau LAN A avec une adresse de la plage d’adresses 10.0.0.0/24, telle que 10.0.0.1. En outre, dans l’étendue DHCP de cette plage d’adresses, configurez **Passerelle par défaut** avec l’adresse IP 10.0.0.1. Pour le réseau LAN B, configurez l’interface de son routeur avec une adresse de la plage d’adresses 192.168.0.0/24, telle que 192.168.0.1, puis configurez l’étendue du réseau LAN B 192.168.0.0/24 avec une valeur **Passerelle par défaut** de 192.168.0.1.

8.  Dans **Serveur DNS préféré**, tapez l’adresse IP de votre serveur DNS. Si vous envisagez d’utiliser l’ordinateur local comme serveur DNS préféré, tapez son adresse IP.

9. Dans **Serveur DNS auxiliaire**, tapez l’adresse IP de votre serveur DNS auxiliaire, le cas échéant. Si vous envisagez d’utiliser l’ordinateur local comme serveur DNS auxiliaire, tapez son adresse IP.

10. Cliquez sur **OK**, puis sur **Fermer**.

> [!NOTE]
> Pour plus d’informations sur la façon de configurer une adresse IP statique sur les ordinateurs qui exécutent d’autres systèmes d’exploitation de Microsoft, consultez [annexe B - configuration des adresses IP statiques](#BKMK_B).

#### <a name="BKMK_deployADDNS01"></a>Déploiement du serveur DC1
Pour déployer le serveur DC1, qui est l’ordinateur exécutant les Services de domaine Active Directory (AD DS) et DNS, vous devez effectuer ces étapes dans l’ordre suivant :

-   Effectuer la procédure indiquée dans la section [Configuration de tous les serveurs](#BKMK_configuringAll)

-   [Installer les services AD DS et DNS pour une nouvelle forêt](#BKMK_installAD-DNS)

-   [Créer un compte d’utilisateur dans utilisateurs et ordinateurs Active Directory](#BKMK_createUA)

-   [Appartenance d’affecter un groupe](#BKMK_assigngroup)

-   [Configurer une Zone de recherche inversée DNS](#BKMK_reverse)

**Privilèges d’administrateur**

Si vous installez un petit réseau dont vous êtes le seul administrateur, nous vous recommandons de créer un compte d’utilisateur pour vous-même, puis d’ajouter votre compte d’utilisateur en tant que membre des groupes Administrateurs de l’entreprise et Administrateurs du domaine. Vous pourrez ainsi plus facilement jouer le rôle d’administrateur pour toutes les ressources réseau. Nous vous recommandons également d’ouvrir une session avec ce compte uniquement lorsque vous avez besoin d’effectuer des tâches d’administration et de créer un autre compte d’utilisateur pour réaliser les tâches non informatiques.

Si vous avez une plus grande organisation comptant plusieurs administrateurs, reportez-vous à la documentation d’AD DS pour déterminer l’appartenance au groupe meilleures pour les employés de l’organisation.

**Différences entre les comptes d’utilisateur de domaine et les comptes d’utilisateur sur l’ordinateur local**

L’un des avantages d’une infrastructure reposant sur des domaines est qu’il n’est pas nécessaire de créer des comptes d’utilisateurs sur chaque ordinateur du domaine. Ceci vaut que l’ordinateur soit un ordinateur client ou un serveur.

Par conséquent, vous ne devez pas créer des comptes d’utilisateurs sur chaque ordinateur du domaine. Créez tous les comptes d’utilisateurs dans Utilisateurs et ordinateurs Active Directory et utilisez les procédures précédentes pour attribuer des membres aux groupes. Par défaut, tous les comptes d’utilisateurs sont membres du groupe Utilisateurs du domaine.

Tous les membres du groupe Utilisateurs du domaine peuvent ouvrir une session sur un ordinateur client qui a été joint au domaine.

Vous pouvez configurer les comptes d’utilisateurs de sorte qu’ils indiquent les jours et les heures auxquels l’utilisateur est autorisé à ouvrir une session sur l’ordinateur. Vous pouvez également désigner les ordinateurs que chaque utilisateur est autorisé à utiliser. Pour configurer ces paramètres, ouvrez Utilisateurs et ordinateurs Active Directory, recherchez le compte d’utilisateur à configurer, puis double-cliquez sur ce compte. Dans les **Propriétés** du compte d’utilisateur, cliquez sur l’onglet **Compte**, puis cliquez sur **Horaires d’accès** ou **Se connecter à**.

#### <a name="BKMK_installAD-DNS"></a>Installer les services AD DS et DNS pour une nouvelle forêt

Vous pouvez utiliser une des procédures suivantes pour installer les Services de domaine Active Directory (AD DS) et DNS et de créer un nouveau domaine dans une nouvelle forêt. 

La première procédure fournit des instructions sur l’exécution de ces actions à l’aide de Windows PowerShell, tandis que la seconde procédure vous montre comment installer les services AD DS et DNS à l’aide du Gestionnaire de serveur.

>[!IMPORTANT]
>Une fois que vous avez terminé d’effectuer les étapes de cette procédure, l’ordinateur est redémarré automatiquement.

**Installer les services AD DS et DNS à l’aide de Windows PowerShell**

Vous pouvez utiliser les commandes suivantes pour installer et configurer les services AD DS et DNS. Vous devez remplacer le nom de domaine dans cet exemple avec la valeur que vous souhaitez utiliser pour votre domaine.

>[!NOTE]
>Pour plus d’informations sur ces commandes Windows PowerShell, consultez les rubriques de référence suivantes.
>- [Install-WindowsFeature](https://docs.microsoft.com/powershell/module/servermanager/install-windowsfeature?view=win10-ps)
>- [Install-ADDSForest](https://docs.microsoft.com/powershell/module/addsdeployment/install-addsforest?view=win10-ps)

Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs**.

- Exécuter Windows PowerShell en tant qu’administrateur, tapez la commande suivante, puis appuyez sur ENTRÉE :  

`Install-WindowsFeature AD-Domain-Services -IncludeManagementTools`

Lorsque l’installation est terminée, le message suivant s’affiche dans Windows PowerShell.


    Success Restart Needed  Exit Code   Feature Result
    ------- --------------  ---------   --------------
    True    No              Success     {Active Directory Domain Services, Group P...


- Dans Windows PowerShell, tapez la commande suivante, en remplaçant le texte **corp.contoso.com** avec votre nom de domaine, puis appuyez sur ENTRÉE :

````
Install-ADDSForest -DomainName "corp.contoso.com"
````

- Pendant l’installation et la configuration, qui s’affiche en haut de la fenêtre Windows PowerShell, l’invite suivante s’affiche. Une fois qu’il s’affiche, tapez un mot de passe et appuyez sur ENTRÉE.

    **SafeModeAdministratorPassword:**

- Une fois que vous tapez un mot de passe et appuyez sur entrée, l’invite de confirmation suivant s’affiche. Tapez le mot de passe et appuyez sur ENTRÉE.

    **Confirmer SafeModeAdministratorPassword :**

- Lorsque l’invite suivante s’affiche, tapez la lettre **Y** puis appuyez sur ENTRÉE.


~~~
The target server will be configured as a domain controller and restarted when this operation is complete.
Do you want to continue with this operation?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"):
~~~

- Si vous le souhaitez, vous pouvez lire les messages d’avertissement qui sont affichent pendant l’installation normale, réussie des services AD DS et DNS. Ces messages sont normales et ne sont pas une indication d’échec d’installation.

- Une fois l’installation réussit, un message s’affiche indiquant que vous êtes sur le point d’être fermé la session sur l’ordinateur afin que l’ordinateur peut redémarrer. Si vous cliquez sur **fermer**, vous êtes connecté immédiatement l’ordinateur hors tension et l’ordinateur redémarre. Si vous ne cliquez pas sur **fermer**, l’ordinateur redémarre après une période par défaut.

- Une fois que le serveur est redémarré, vous pouvez vérifier la réussite de l’installation des Services de domaine Active Directory et DNS. Ouvrez Windows PowerShell, tapez la commande suivante et appuyez sur ENTRÉE.

````
Get-WindowsFeature
````

Les résultats de cette commande sont affichés dans Windows PowerShell et doivent être similaires à ceux dans l’image ci-dessous. Pour les technologies installés, les crochets à gauche du nom de la technologie contient le caractère **X**et la valeur de **état d’installation** est **installé**.

![Résultats de la commande Get-WindowsFeature](../media/Core-Network-Guide/server-roles-installed.jpg)

**Installer les services AD DS et DNS à l’aide du Gestionnaire de serveur**

1.  Sur le serveur DC1, dans le **Gestionnaire de serveur**, cliquez sur **Gérer**, puis sur **Ajouter des rôles et des fonctionnalités**. L’Assistant Ajout de rôles et de fonctionnalités s’ouvre.

2.  Dans **Avant de commencer**, cliquez sur **Suivant**.

    > [!NOTE]
    > La page **Avant de commencer** de l’Assistant Ajout de rôles et de fonctionnalités ne s’affiche pas si vous avez préalablement sélectionné **Ignorer cette page par défaut** dans l’Assistant.

3.  Dans **Sélectionner le type d’installation**, vérifiez que **Installation basée sur un rôle ou une fonctionnalité** est sélectionné, puis cliquez sur **Suivant**.

4.  Dans **Sélectionner le serveur de destination**, vérifiez que **Sélectionner un serveur du pool de serveurs** est sélectionné. Dans **Pool de serveurs**, vérifiez que l’ordinateur local est sélectionné. Cliquez sur **Suivant**.

5.  Dans **Sélectionner des rôles de serveurs**, dans **Rôles**, cliquez sur **Services de domaine Active Directory**. Dans **Ajouter les fonctionnalités requises pour les services de domaine Active Directory**, cliquez sur **Ajouter des fonctionnalités**. Cliquez sur **Suivant**.

6.  Dans **Sélectionner des fonctionnalités**, cliquez sur **Suivant**, puis dans **Services de domaine Active Directory**, vérifiez les informations fournies et cliquez sur **Suivant**.

7.  Dans **Confirmer les sélections pour l’installation**, cliquez sur **Installer**. La page de progression de l’installation indique l’état du processus d’installation. À la fin du processus, dans les détails du message, cliquez sur **Promouvoir ce serveur en contrôleur de domaine**. L’Assistant Configuration des services de domaine Active Directory s’ouvre.

8.  Dans **Configuration de déploiement**, sélectionnez **Ajouter une nouvelle forêt**. Dans **Nom de domaine racine**, tapez le nom de domaine complet (FQDN) du domaine. Par exemple, si votre nom de domaine complet est corp.contoso.com, tapez **corp.contoso.com**. Cliquez sur **Suivant**.

9. Dans **Options du contrôleur de domaine**, dans **Sélectionner le niveau fonctionnel de la nouvelle forêt et du domaine racine**, sélectionnez le niveau fonctionnel souhaité pour la forêt et le domaine. Dans **Spécifier les fonctionnalités de contrôleur de domaine**, vérifiez que **Serveur DNS (Domain Name System)** et **Catalogue global (GC)** sont sélectionnés. Dans **Mot de passe** et **Confirmer le mot de passe**, tapez un mot de passe pour le mode de restauration des services d’annuaire (DSRM). Cliquez sur **Suivant**.

10. Dans **Options DNS**, cliquez sur **Suivant**.

11. Dans **Options supplémentaires**, vérifiez le nom NetBIOS qui est attribué au domaine, et modifiez-le si nécessaire. Cliquez sur **Suivant**.

12. Dans **Chemins d’accès**, dans **Spécifier l’emplacement de la base de données AD DS, des fichiers journaux et de SYSVOL**, effectuez l’une des opérations suivantes :

    -   Acceptez la valeur par défaut.

    -   Tapez l’emplacement des dossiers que vous souhaitez utiliser pour **Dossier de la base de données**, **Dossier des fichiers journaux** et **Dossier SYSVOL**.

13. Cliquez sur **Suivant**.

14. Dans **Examiner les options**, vérifiez vos sélections.

15. Si vous voulez exporter les paramètres dans un script Windows PowerShell, cliquez sur **Afficher le script**. Le script s’ouvre dans le Bloc-notes. Vous pouvez ensuite l’enregistrer à l’emplacement de dossier de votre choix. Cliquez sur **Suivant**. Dans **Vérification de la configuration requise**, vos sélections sont validées. À la fin de la vérification, cliquez sur **Installer**. À l’invite de Windows, cliquez sur **Fermer**. Le serveur redémarre pour terminer l’installation des services AD DS et DNS.

16. Pour vérifier la réussite de l’installation, affichez la console du Gestionnaire de serveur après le redémarrage du serveur. Les services AD DS et DNS doit apparaître dans le volet gauche, comme les éléments en surbrillance dans l’image ci-dessous.

![Les services AD DS et DNS dans le Gestionnaire de serveur](../media/Core-Network-Guide/server-roles-installed-sm.jpg)

##### <a name="BKMK_createUA"></a>Créer un compte d’utilisateur dans utilisateurs et ordinateurs Active Directory
Vous pouvez utiliser cette procédure pour créer un compte d’utilisateur de domaine dans la console MMC (Microsoft Management Console) Utilisateurs et ordinateurs Active Directory.

Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs du domaine** ou à un groupe équivalent.

> [!NOTE]
> Pour effectuer cette procédure en utilisant Windows PowerShell, ouvrez PowerShell et tapez l’applet de commande suivante sur une seule ligne, puis appuyez sur ENTRÉE. Remplacez le nom de compte d’utilisateur utilisé dans cet exemple par le nom de votre choix.
>
> `New-ADUser -SamAccountName User1 -AccountPassword (read-host "Set user password" -assecurestring) -name "User1" -enabled $true -PasswordNeverExpires $true -ChangePasswordAtLogon $false`
>
> Après avoir appuyé sur ENTRÉE, tapez le mot de passe du compte d’utilisateur. Le compte ainsi créé appartient par défaut au groupe Utilisateurs du domaine.
>
> Si vous le souhaitez, vous pouvez ajouter ce nouveau compte d’utilisateur à d’autres groupes en utilisant l’applet de commande suivante. Dans cet exemple, User1 est ajouté aux deux groupes Administrateurs du domaine et Administrateurs de l’entreprise. Avant d’exécuter la commande ci-dessous, remplacez les exemples de nom de compte d’utilisateur, de nom de domaine et de groupes par les valeurs souhaitées.
>
> `Add-ADPrincipalGroupMembership -Identity "CN=User1,CN=Users,DC=corp,DC=contoso,DC=com" -MemberOf "CN=Enterprise Admins,CN=Users,DC=corp,DC=contoso,DC=com","CN=Domain Admins,CN=Users,DC=corp,DC=contoso,DC=com"`

###### <a name="to-create-a-user-account"></a>Pour créer un compte d’utilisateur

1.  Sur le serveur DC1, dans le Gestionnaire de serveur, cliquez sur **Outils**, puis sur **Utilisateurs et ordinateurs Active Directory**. La console MMC Utilisateurs et ordinateurs Active Directory s’ouvre. Si le nœud de votre domaine n’est pas encore sélectionné, cliquez dessus. Par exemple, si votre domaine est corp.contoso.com, cliquez sur **corp.contoso.com**.

2.  Dans le volet d’informations, cliquez avec le bouton droit sur le dossier auquel ajouter un compte d’utilisateur.

    **Où ?**

    -   Active Directory Users and Computers /*nœud de domaine*/*dossier*

3.  Pointez sur **Nouveau**, puis cliquez sur **Utilisateur**. Le **nouvel objet - utilisateur** boîte de dialogue s’ouvre.

4.  Dans **Prénom**, tapez le prénom de l’utilisateur.

5.  Dans **Initiales**, tapez les initiales de l’utilisateur.

6.  Dans **Nom**, tapez le nom de l’utilisateur.

7.  Modifiez le **Nom complet** pour ajouter des initiales ou inverser l’ordre du nom et du prénom.

8.  Dans **Nom d’ouverture de session de l’utilisateur**, tapez le nom d’ouverture de session de l’utilisateur. Cliquez sur **Suivant**.

9. Dans **Nouvel objet - Utilisateur**, dans **Mot de passe** et **Confirmer le mot de passe**, tapez le mot de passe de l’utilisateur, puis sélectionnez les options de mot de passe appropriées.

10. Cliquez sur **Suivant**, passez en revue les paramètres du nouveau compte d’utilisateur, puis cliquez sur **Terminer**.

##### <a name="BKMK_assigngroup"></a>Appartenance d’affecter un groupe
Vous pouvez utiliser cette procédure pour ajouter un utilisateur, un ordinateur ou un groupe à un groupe dans la console MMC (Microsoft Management Console) Utilisateurs et ordinateurs Active Directory.

Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs du domaine** ou à un groupe équivalent.

###### <a name="to-assign-group-membership"></a>Pour attribuer des membres à un groupe

1.  Sur le serveur DC1, dans le Gestionnaire de serveur, cliquez sur **Outils**, puis sur **Utilisateurs et ordinateurs Active Directory**. La console MMC Utilisateurs et ordinateurs Active Directory s’ouvre. Si le nœud de votre domaine n’est pas encore sélectionné, cliquez dessus. Par exemple, si votre domaine est corp.contoso.com, cliquez sur **corp.contoso.com**.

2.  Dans le volet d’informations, double-cliquez sur le dossier qui contient le groupe auquel ajouter un membre.

    Où ?

    -   **Active Directory Users and Computers**/*nœud de domaine*/*dossier qui contient le groupe*

3.  Dans le volet d’informations, cliquez avec le bouton droit sur l’utilisateur, l’ordinateur ou tout autre objet à ajouter au groupe, puis cliquez sur **Propriétés**. L’objet **propriétés** boîte de dialogue s’ouvre. Cliquez sur l’onglet **Membre de**.

4.  Sous l’onglet **Membre de**, cliquez sur **Ajouter**.

5.  Dans **Entrez les noms des objets à sélectionner**, tapez le nom du groupe auquel vous voulez ajouter l’objet, puis cliquez sur **OK**.

6.  Pour attribuer d’autres utilisateurs, groupes ou ordinateurs au groupe, répétez les étapes 4 et 5 de cette procédure.

##### <a name="BKMK_reverse"></a>Configurer une Zone de recherche inversée DNS
Vous pouvez utiliser cette procédure pour configurer une zone de recherche inversée dans DNS (Domain Name System).

Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs du domaine**.

> [!NOTE]
> -   Pour les moyennes et grandes entreprises, il est recommandé que vous allez configurer et utiliser le groupe DNSAdmins dans Active Directory Users and Computers. Pour plus d’informations, voir [Ressources techniques supplémentaires](#BKMK_resources).
> -   Pour effectuer cette procédure en utilisant Windows PowerShell, ouvrez PowerShell et tapez l’applet de commande suivante sur une seule ligne, puis appuyez sur ENTRÉE. Remplacez les noms de zone de recherche inversée DNS et de fichier de zone utilisés dans cet exemple par les noms de votre choix. Veillez à inverser l’ordre de l’ID réseau dans le nom de la zone de recherche inversée. Par exemple, si l’ID de réseau est 192.168.0, créer le nom de zone de recherche inversée **0.168.192.in-addr.arpa**.
>
> `Add-DnsServerPrimaryZone 0.0.10.in-addr.arpa -ZoneFile 0.0.10.in-addr.arpa.dns`

###### <a name="to-configure-a-dns-reverse-lookup-zone"></a>Pour configurer une zone de recherche inversée DNS

1.  Sur le serveur DC1, dans le Gestionnaire de serveur, cliquez sur **Outils**, puis sur **DNS**. La console MMC DNS s’ouvre.

2.  Dans DNS, le cas échéant, double-cliquez sur le nom du serveur pour développer son arborescence. Par exemple, si le serveur DNS s’appelle DC1, double-cliquez sur **DC1**.

3.  Sélectionnez **Zones de recherche inversée**, cliquez dessus avec le bouton droit **** , puis cliquez sur **Nouvelle zone**. L’Assistant Nouvelle zone s’ouvre.

4.  Sur la page **Bienvenue !** , cliquez sur **Suivant**.

5.  Dans **Type de zone**, sélectionnez **Zone principale**.

6.  Si votre serveur DNS est un contrôleur de domaine accessible en écriture, vérifiez que **Enregistrer la zone dans Active Directory** est sélectionné. Cliquez sur **Suivant**.

7.  Dans **Étendue de la zone de réplication de Active Directory**, sélectionnez **Vers tous les serveurs DNS exécutés sur des contrôleurs de domaine dans ce domaine**, sauf si vous souhaitez spécifiquement choisir une autre option. Cliquez sur **Suivant**.

8.  Dans la première page **Nom de la zone de recherche inversée**, sélectionnez **Zone de recherche inversée IPv4**. Cliquez sur **Suivant**.

9. Dans la seconde page **Nom de la zone de recherche inversée**, effectuez l’une des opérations suivantes :

    -   Dans **ID réseau**, tapez l’ID réseau de votre plage d’adresses IP. Par exemple, si la plage d’adresses IP est de 10.0.0.1 à 10.0.0.254, tapez **10.0.0**.

    -   Le nom de votre zone de recherche inversée IPv4 est automatiquement ajouté dans **Nom de la zone de recherche inversée**. Cliquez sur **Suivant**.

10. Dans **Mise à jour dynamique**, sélectionnez le type de mises à jour dynamiques à autoriser. Cliquez sur **Suivant**.

11. Dans **Fin de l’Assistant Nouvelle zone**, passez en revue vos sélections, puis cliquez sur **Terminer**.

#### <a name="BKMK_joinlogserver"></a>Jonction des serveurs au domaine et ouverture de session
Après avoir installé les Services de domaine Active Directory (AD DS) et créé un ou plusieurs comptes d’utilisateur disposant des autorisations pour joindre un ordinateur au domaine, vous pouvez joindre des serveurs de réseau principal au domaine, puis ouvrez une session sur les serveurs afin d’installer d’autres technologies, telles que DHCP Dynamic Host Configuration Protocol ().

Sur tous les serveurs que vous déployez, sauf pour le serveur qui exécute les services AD DS, procédez comme suit :

1.  Effectuez les procédures indiquées dans [Configuration de tous les serveurs](#BKMK_configuringAll).

2.  Utilisez les instructions indiquées dans les deux procédures suivantes pour joindre vos serveurs au domaine et ouvrir une session sur ces serveurs afin de réaliser des tâches de déploiement supplémentaires :

> [!NOTE]
> Pour effectuer cette procédure en utilisant Windows PowerShell, ouvrez PowerShell et tapez l’applet de commande suivante, puis appuyez sur ENTRÉE. Remplacez le nom de domaine utilisé dans cet exemple par le nom de votre choix.
>
> `Add-Computer -DomainName corp.contoso.com`
>
> Lorsque vous y êtes invité, tapez le nom d’utilisateur et le mot de passe d’un compte qui est autorisé à joindre un ordinateur au domaine. Pour redémarrer l’ordinateur, tapez la commande suivante, puis appuyez sur ENTRÉE.
>
> `Restart-Computer`

###### <a name="to-join-computers-running--windows-server-2016--windows-server-2012-r2--and--windows-server-2012--to-the-domain"></a>Pour joindre des ordinateurs exécutant Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012 au domaine

1.  Dans le Gestionnaire de serveur, cliquez sur **Serveur local**. Dans le volet d’informations, cliquez sur **WORKGROUP**. La boîte de dialogue **Propriétés système** s’ouvre.

2.  Dans la boîte de dialogue **Propriétés système**, cliquez sur **Modifier**. La boîte de dialogue **Modification du nom ou du domaine de l’ordinateur** s’ouvre.

3.  Dans **Nom de l’ordinateur**, dans **Membre de**, cliquez sur **Domaine**, puis tapez le nom du domaine auquel joindre l’ordinateur. Par exemple, si le nom de domaine est corp.contoso.com, tapez **corp.contoso.com**.

4.  Cliquez sur **OK**. La boîte de dialogue **Sécurité de Windows** s’ouvre.

5.  Dans **Modification du nom ou du domaine de l’ordinateur**, dans **Nom d’utilisateur**, tapez le nom d’utilisateur. Dans **Mot de passe**, tapez le mot de passe, puis cliquez sur **OK**. La boîte de dialogue **Modification du nom ou du domaine de l’ordinateur** s’ouvre et vous souhaite la bienvenue dans le domaine. Cliquez sur **OK**.

6.  La boîte de dialogue **Modification du nom ou du domaine de l’ordinateur** affiche un message indiquant que vous devez redémarrer l’ordinateur pour appliquer les modifications. Cliquez sur **OK**.

7.  Dans la boîte de dialogue **Propriétés système**, sous l’onglet **Nom de l’ordinateur**, cliquez sur **Fermer**. La boîte de dialogue **Microsoft Windows** s’ouvre et affiche un message indiquant de nouveau que vous devez redémarrer l’ordinateur pour appliquer les modifications. Cliquez sur **Redémarrer maintenant**.

> [!NOTE]
> Pour plus d’informations sur la façon de joindre des ordinateurs qui exécutent d’autres systèmes d’exploitation de Microsoft au domaine, consultez [annexe C - Ajout d’ordinateurs au domaine](#BKMK_C).

###### <a name="to-log-on-to-the-domain-using-computers-running-windows-server-2016"></a>Pour ouvrir une session le domaine à l’aide des ordinateurs exécutant Windows Server 2016

1.  Fermez la session sur l’ordinateur et redémarrez-le.

2.  Appuyez sur Ctrl+Alt+Suppr. L’écran d’ouverture de session s’affiche.

3.  Dans l’angle inférieur gauche, cliquez sur **autre utilisateur**.

4.  Dans **nom d’utilisateur**, tapez votre nom d’utilisateur.

5.  Dans **Mot de passe**, tapez votre mot de passe de domaine, puis cliquez sur la flèche ou appuyez sur ENTRÉE.

> [!NOTE]
> Pour plus d’informations sur la façon de se connecter au domaine à l’aide d’ordinateurs qui exécutent d’autres systèmes d’exploitation de Microsoft, consultez [annexe D - ouvrez une session sur le domaine](#BKMK_D).

#### <a name="BKMK_deployDHCP01"></a>Déploiement du serveur DHCP1
Avant de déployer ce composant sur le réseau de base, vous devez effectuer les actions suivantes :

-   Effectuer la procédure indiquée dans la section [Configuration de tous les serveurs](#BKMK_configuringAll)

-   Effectuer la procédure indiquée dans la section [Jonction des serveurs au domaine et ouverture d’une session](#BKMK_joinlogserver)

Pour déployer le serveur DHCP1, qui exécute le rôle serveur DHCP (Dynamic Host Configuration Protocol), vous devez effectuer les étapes suivantes dans l’ordre indiqué :

-   [Installer Dynamic Host Configuration Protocol (DHCP)](#BKMK_installDHCP)

-   [Créer et activer une nouvelle étendue DHCP](#BKMK_newscopeDHCP)

> [!NOTE]
> Pour effectuer ces procédures en utilisant Windows PowerShell, ouvrez PowerShell et tapez les applets de commande suivantes sur des lignes distinctes, puis appuyez sur ENTRÉE. Remplacez le nom de l’étendue, les adresses IP de début et de fin de plage, le masque de sous-réseau et autres valeurs utilisés dans cet exemple par les valeurs de votre choix.
>
> `Install-WindowsFeature DHCP -IncludeManagementTools`
>
> `Add-DhcpServerv4Scope -name "Corpnet" -StartRange 10.0.0.1 -EndRange 10.0.0.254 -SubnetMask 255.255.255.0 -State Active`
>
> `Add-DhcpServerv4ExclusionRange -ScopeID 10.0.0.0 -StartRange 10.0.0.1 -EndRange 10.0.0.15`
>
> `Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.0.1 -ScopeID 10.0.0.0 -ComputerName DHCP1.corp.contoso.com`
>
> `Add-DhcpServerv4Scope -name "Corpnet2" -StartRange 10.0.1.1 -EndRange 10.0.1.254 -SubnetMask 255.255.255.0 -State Active`
>
> `Add-DhcpServerv4ExclusionRange -ScopeID 10.0.1.0 -StartRange 10.0.1.1 -EndRange 10.0.1.15`
>
> `Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.1.1 -ScopeID 10.0.1.0 -ComputerName DHCP1.corp.contoso.com`
>
> `Set-DhcpServerv4OptionValue -DnsDomain corp.contoso.com -DnsServer 10.0.0.2`
>
> `Add-DhcpServerInDC -DnsName DHCP1.corp.contoso.com`

##### <a name="BKMK_installDHCP"></a>Installer Dynamic Host Configuration Protocol (DHCP)
Vous pouvez utiliser cette procédure pour installer et configurer le rôle Serveur DHCP à l’aide de l’Assistant Ajout de rôles et de fonctionnalités.

Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs du domaine** ou à un groupe équivalent.

###### <a name="to-install-dhcp"></a>Pour installer DHCP

1.  Sur le serveur DHCP1, dans le Gestionnaire de serveur, cliquez sur **Gérer**, puis sur **Ajouter des rôles et des fonctionnalités**. L’Assistant Ajout de rôles et de fonctionnalités s’ouvre.

2.  Dans **Avant de commencer**, cliquez sur **Suivant**.

    > [!NOTE]
    > La page **Avant de commencer** de l’Assistant Ajout de rôles et de fonctionnalités ne s’affiche pas si vous avez préalablement sélectionné **Ignorer cette page par défaut** dans l’Assistant.

3.  Dans **Sélectionner le type d’installation**, vérifiez que **Installation basée sur un rôle ou une fonctionnalité** est sélectionné, puis cliquez sur **Suivant**.

4.  Dans **Sélectionner le serveur de destination**, vérifiez que **Sélectionner un serveur du pool de serveurs** est sélectionné. Dans **Pool de serveurs**, vérifiez que l’ordinateur local est sélectionné. Cliquez sur **Suivant**.

5.  Dans **sélectionner des rôles de serveur**, dans **rôles**, sélectionnez **serveur DHCP**. Dans **Ajouter les fonctionnalités requises pour le serveur DHCP**, cliquez sur **Ajouter des fonctionnalités**. Cliquez sur **Suivant**.

6.  Dans **Sélectionner des fonctionnalités**, cliquez sur **Suivant**, puis dans **Serveur DHCP**, vérifiez les informations fournies et cliquez sur **Suivant**.

7.  Dans **Confirmer les sélections d’installation**, cliquez sur **Redémarrer automatiquement le serveur de destination, si nécessaire**. Lorsque vous êtes invité à confirmer ce choix, cliquez sur **Oui**, puis sur **Installer**. Le **progression de l’Installation** page affiche l’état pendant le processus d’installation. La fin du processus, le message « Configuration requise. Installation réussie sur *ComputerName*» s’affiche, où *ComputerName* est le nom de l’ordinateur sur lequel vous avez installé le serveur DHCP. Dans la fenêtre de message, cliquez sur **Terminer la configuration DHCP**. L’Assistant Configuration post-installation DHCP s’ouvre. Cliquez sur **Suivant**.

8.  Dans **Autorisation**, spécifiez les informations d’identification à utiliser pour autoriser le serveur DHCP dans les services de domaine Active Directory, puis cliquez sur **Valider**. Une fois l’autorisation terminée, cliquez sur **Fermer**.

##### <a name="BKMK_newscopeDHCP"></a>Créer et activer une nouvelle étendue DHCP
Vous pouvez utiliser cette procédure pour créer une étendue DHCP à l’aide de la console MMC (Microsoft Management Console) DHCP. Cette procédure permet d’activer une étendue et de définir une plage d’exclusion pour empêcher le serveur DHCP de louer les adresses IP statiques avec lesquelles vous avez configuré vos serveurs et autres périphériques nécessitant ce type d’adresse.

Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs DHCP** ou à un groupe équivalent.

###### <a name="to-create-and-activate-a-new-dhcp-scope"></a>Pour créer et activer une nouvelle étendue DHCP

1.  Sur le serveur DHCP1, dans le Gestionnaire de serveur, cliquez sur **Outils**, puis sur **DHCP**. La console MMC DHCP s’ouvre.

2.  Dans **DHCP**, développez le nom du serveur. Par exemple, si le nom du serveur DHCP s’appelle DHCP1.corp.contoso.com, cliquez sur la flèche vers le bas à côté **DHCP1.corp.contoso.com**.

3.  Sous le nom de serveur avec le bouton droit **IPv4**, puis cliquez sur **nouvelle étendue**. L’Assistant Nouvelle étendue s’ouvre.

4.  Sur la page **Bienvenue !** , cliquez sur **Suivant**.

5.  Dans **Nom de l’étendue**, dans **Nom**, tapez le nom de l’étendue. Par exemple, tapez **Sous-réseau 1**.

6.  Dans la zone **Description**, tapez une description de la nouvelle étendue, puis cliquez sur **Suivant**.

7.  Dans **Plage d’adresses IP**, procédez comme suit :

    1.  Dans **Adresse IP de début**, tapez la première adresse IP de la plage. Par exemple, tapez **10.0.0.1**.

    2.  Dans **Adresse IP de fin**, tapez la dernière adresse IP de la plage. Par exemple, tapez **10.0.0.254**. Les valeurs de **Longueur** et **Masque de sous-réseau** sont entrées automatiquement, en fonction de l’adresse IP entrée dans **Adresse IP de début**.

    3.  Si nécessaire, modifiez les valeurs de **Longueur** ou **Masque de sous-réseau** comme il convient pour votre modèle d’adressage.

    4.  Cliquez sur **Suivant**.

8.  Dans **Ajout d’exclusions**, procédez comme suit :

    1.  Dans **Adresse IP de début**, tapez la première adresse IP de la plage d’exclusion. Par exemple, tapez **10.0.0.1**.

    2.  Dans **Adresse IP de fin**, tapez la dernière adresse IP de la plage d’exclusion. Par exemple, tapez **10.0.0.15**.

9. Cliquez sur **Ajouter**, puis sur **Suivant**.

10. Dans **Durée du bail**, modifiez les valeurs par défaut de **Jours**, **Heures** et **Minutes** comme il convient pour votre réseau, puis cliquez sur **Suivant**.

11. Dans **Configuration des paramètres DHCP**, sélectionnez **Oui, je veux configurer ces options maintenant**, puis cliquez sur **Suivant**.

12. Dans **Routeur (passerelle par défaut)** , effectuez l’une des actions suivantes :

    -   Si votre réseau ne comporte pas de routeurs, cliquez sur **Suivant**.

    -   Dans **Adresse IP**, tapez l’adresse IP de votre routeur ou de votre passerelle par défaut. Par exemple, tapez **10.0.0.1**. Cliquez sur **Ajouter**, puis sur **Suivant**.

13. Dans **Nom de domaine et serveurs DNS**, procédez comme suit :

    1.  Dans **Domaine parent**, tapez le nom du domaine DNS que les clients utilisent pour la résolution de noms. Par exemple, tapez **corp.contoso.com**.

    2.  Dans **Nom du serveur**, tapez le nom de l’ordinateur DNS que les clients utilisent pour la résolution de noms. Par exemple, tapez **DC1**.

    3.  Cliquez sur **Résoudre**. L’adresse IP du serveur DNS est ajoutée dans **Adresse IP**. Cliquez sur **Ajouter**, attendez la fin de la validation de l’adresse IP du serveur DNS, puis cliquez sur **Suivant**.

14. Dans **Serveurs WINS**, puisque vous n’avez pas de serveurs WINS sur votre réseau, cliquez sur **Suivant**.

15. Dans **Activer l’étendue**, sélectionnez **Oui, je veux activer cette étendue maintenant**.

16. Cliquez sur **Suivant**, puis sur **Terminer**.

> [!IMPORTANT]
> Si vous souhaitez créer d’autres étendues pour des sous-réseaux supplémentaires, répétez cette procédure. Utilisez une plage d’adresses IP distincte pour chaque sous-réseau que vous prévoyez de déployer et assurez-vous que l’acheminement des messages DHCP est activé sur tous les routeurs d’accès aux autres sous-réseaux.

### <a name="BKMK_joinlogclients"></a>Joindre des ordinateurs clients au domaine et ouverture de session

> [!NOTE]
> Pour effectuer cette procédure en utilisant Windows PowerShell, ouvrez PowerShell et tapez l’applet de commande suivante, puis appuyez sur ENTRÉE. Remplacez le nom de domaine utilisé dans cet exemple par le nom de votre choix.
>
> `Add-Computer -DomainName corp.contoso.com`
>
> Lorsque vous y êtes invité, tapez le nom d’utilisateur et le mot de passe d’un compte qui est autorisé à joindre un ordinateur au domaine. Pour redémarrer l’ordinateur, tapez la commande suivante, puis appuyez sur ENTRÉE.
>
> `Restart-Computer`

##### <a name="to-join-computers-running-windows-10-to-the-domain"></a>Pour joindre des ordinateurs exécutant Windows 10 au domaine

1.  Ouvrez une session sur l’ordinateur avec le compte Administrateur local.

2.  Dans **effectuer la recherche web et Windows**, type **système**. Dans les résultats de recherche, cliquez sur **système (Panneau de configuration)** . La boîte de dialogue **Système** s’ouvre.

3.  Dans **système**, cliquez sur **paramètres système avancés**. La boîte de dialogue **Propriétés système** s’ouvre. Cliquez sur le **nom de l’ordinateur** onglet.

4.  Dans **nom de l’ordinateur**, cliquez sur **modification**. La boîte de dialogue **Modification du nom ou du domaine de l’ordinateur** s’ouvre.

5.  Dans **modification du nom ou du domaine d’ordinateur** , dans **membre**, cliquez sur **domaine**, puis tapez le nom du domaine à joindre. Par exemple, si le nom de domaine est corp.contoso.com, tapez **corp.contoso.com**.

6.  Cliquez sur **OK**. La boîte de dialogue **Sécurité de Windows** s’ouvre.

7.  Dans **Modification du nom ou du domaine de l’ordinateur**, dans **Nom d’utilisateur**, tapez le nom d’utilisateur. Dans **Mot de passe**, tapez le mot de passe, puis cliquez sur **OK**. La boîte de dialogue **Modification du nom ou du domaine de l’ordinateur** s’ouvre et vous souhaite la bienvenue dans le domaine. Cliquez sur **OK**.

8.  La boîte de dialogue **Modification du nom ou du domaine de l’ordinateur** affiche un message indiquant que vous devez redémarrer l’ordinateur pour appliquer les modifications. Cliquez sur **OK**.

9. Dans la boîte de dialogue **Propriétés système**, sous l’onglet **Nom de l’ordinateur**, cliquez sur **Fermer**. La boîte de dialogue **Microsoft Windows** s’ouvre et affiche un message indiquant de nouveau que vous devez redémarrer l’ordinateur pour appliquer les modifications. Cliquez sur **Redémarrer maintenant**.

##### <a name="to-join-computers-running-windows-81-to-the-domain"></a>Pour joindre des ordinateurs exécutant Windows 8.1 au domaine

1.  Ouvrez une session sur l’ordinateur avec le compte Administrateur local.

2.  Avec le bouton droit **Démarrer**, puis cliquez sur **système**. La boîte de dialogue **Système** s’ouvre.

3.  Dans **système**, cliquez sur **paramètres système avancés**. La boîte de dialogue **Propriétés système** s’ouvre. Cliquez sur le **nom de l’ordinateur** onglet.

4.  Dans **nom de l’ordinateur**, cliquez sur **modification**. La boîte de dialogue **Modification du nom ou du domaine de l’ordinateur** s’ouvre.

5.  Dans **modification du nom ou du domaine d’ordinateur** , dans **membre**, cliquez sur **domaine**, puis tapez le nom du domaine à joindre. Par exemple, si le nom de domaine est corp.contoso.com, tapez **corp.contoso.com**.

6.  Cliquez sur **OK**. La boîte de dialogue **Sécurité de Windows** s’ouvre.

7.  Dans **Modification du nom ou du domaine de l’ordinateur**, dans **Nom d’utilisateur**, tapez le nom d’utilisateur. Dans **Mot de passe**, tapez le mot de passe, puis cliquez sur **OK**. La boîte de dialogue **Modification du nom ou du domaine de l’ordinateur** s’ouvre et vous souhaite la bienvenue dans le domaine. Cliquez sur **OK**.

8.  La boîte de dialogue **Modification du nom ou du domaine de l’ordinateur** affiche un message indiquant que vous devez redémarrer l’ordinateur pour appliquer les modifications. Cliquez sur **OK**.

9. Dans la boîte de dialogue **Propriétés système**, sous l’onglet **Nom de l’ordinateur**, cliquez sur **Fermer**. La boîte de dialogue **Microsoft Windows** s’ouvre et affiche un message indiquant de nouveau que vous devez redémarrer l’ordinateur pour appliquer les modifications. Cliquez sur **Redémarrer maintenant**.

##### <a name="to-log-on-to-the-domain-using-computers-running-windows-10"></a>Pour ouvrir une session le domaine à l’aide des ordinateurs exécutant Windows 10

1.  Fermez la session sur l’ordinateur et redémarrez-le.

2.  Appuyez sur Ctrl+Alt+Suppr. L’écran d’ouverture de session s’affiche.

3.  Dans le coin inférieur gauche, cliquez sur **autre utilisateur**.

4.  Dans **Nom d’utilisateur**, tapez vos noms de domaine et d’utilisateur au format *domaine\utilisateur*. Par exemple, pour vous connecter au domaine corp.contoso.com avec un compte nommé **Utilisateur-01**, tapez **CORP\Utilisateur-01**.

5.  Dans **Mot de passe**, tapez votre mot de passe de domaine, puis cliquez sur la flèche ou appuyez sur ENTRÉE.

### <a name="BKMK_optionalfeatures"></a>Déploiement des fonctionnalités facultatives pour l’authentification d’accès réseau et les services Web
Si vous prévoyez de déployer des serveurs d’accès réseau, tels que les points d’accès sans fil ou des serveurs VPN, après l’installation de votre réseau de base, il est recommandé que vous déployez un serveur NPS et un serveur Web. Pour les déploiements de l’accès réseau, nous vous recommandons d’utiliser des méthodes d’authentification sécurisée basée sur les certificats. Vous pouvez utiliser le serveur NPS pour gérer les stratégies d’accès réseau et déployer des méthodes d’authentification sécurisée. Utilisez un serveur Web pour publier la liste de révocation de certificats émise par l’autorité de certification qui fournit les certificats d’authentification sécurisée.

> [!NOTE]
> Vous pouvez déployer des certificats de serveur et des fonctionnalités supplémentaires à l’aide des guides d’accompagnement du réseau de base. Pour plus d’informations, voir [Ressources techniques supplémentaires](#BKMK_resources).

L’illustration suivante montre la topologie de réseau de base Windows Server avec les serveurs NPS et Web ajoutés.

![Topologie de réseau de base Windows Server avec les serveurs NPS et Web ajoutés](../media/Core-Network-Guide/cng16_overview_2.jpg)

Les sections suivantes fournissent des informations sur l’ajout de serveurs NPS et Web à votre réseau.

-   [Déploiement de NPS1](#BKMK_deployNPS1)

-   [Déploiement de WEB1](#BKMK_IIS)

#### <a name="BKMK_deployNPS1"></a>Déploiement de NPS1
Le serveur NPS (Network Policy Server) est installé lors d’une étape préparatoire au déploiement d’autres technologies d’accès au réseau, comme les serveurs VPN (Virtual Private Network), les points d’accès sans fil et les commutateurs d’authentification 802.1X.

Serveur NPS (Network Policy Server) vous permet de configurer et gérer des stratégies de réseau avec les fonctionnalités suivantes de manière centralisée : Serveur distant de l’authentification Dial-In Service RADIUS (User) et proxy RADIUS.

Le serveur NPS est un composant facultatif d’un réseau de base, mais vous devez l’installer si l’une des conditions suivantes est vraie :

-   Vous envisagez d’étendre votre réseau pour inclure des serveurs d’accès à distance qui sont compatibles avec le protocole RADIUS, tel qu’un ordinateur exécutant Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008 et Le service Routage et accès à distance, la passerelle des Services Terminal Server ou passerelle des services Bureau à distance.


-   Vous envisagez de déployer l’authentification de 802. 1 X pour câblé ou d’accès sans fil.

Avant de déployer ce service de rôle, vous devez effectuer les étapes suivantes sur l’ordinateur que vous configurez comme un serveur NPS.

-   Effectuer la procédure indiquée dans la section [Configuration de tous les serveurs](#BKMK_configuringAll)

-   Effectuer la procédure indiquée dans la section [Jonction des serveurs au domaine et ouverture d’une session](#BKMK_joinlogserver)

Pour déployer le serveur NPS1, qui exécute le service de rôle NPS (Network Policy Server) du rôle serveur Services de stratégie et d’accès réseau, vous devez effectuer les tâches suivantes :

-   [Planification du déploiement du serveur NPS1](#bkmk_NetFndtn_Pln_NPS-01)

-   [Installer le serveur de stratégie réseau (NPS)](#BKMK_installNPS)

-   [Inscrire le serveur NPS dans le domaine par défaut](#BKMK_registerNPS)

> [!NOTE]
> Ce guide fournit des instructions pour le déploiement de NPS sur un serveur autonome ou machine virtuelle nommée serveur NPS1.  Un autre modèle de déploiement recommandé est l’installation du serveur NPS sur un contrôleur de domaine. Si vous préférez installer le serveur NPS sur un contrôleur de domaine au lieu de sur un serveur autonome, installez NPS sur DC1.

##### <a name="bkmk_NetFndtn_Pln_NPS-01"></a>Planification du déploiement du serveur NPS1
Si vous envisagez de déployer des serveurs d’accès réseau, comme des points d’accès sans fil ou des serveurs VPN, après avoir déployé le réseau de base, il est recommandé de déployer un serveur NPS.

Si vous utilisez un serveur NPS comme serveur RADIUS (Remote Authentication Dial-in User Service), le serveur NPS effectue l’authentification et l’autorisation pour les demandes de connexion par le biais de vos serveurs d’accès réseau. Le serveur NPS vous permet également de configurer et gérer de manière centralisée les stratégies réseau qui déterminent qui est autorisé à accéder au réseau, comment et quand.

Cette section décrit les principales étapes de planification qui doivent être suivies avant d’installer le serveur NPS.

- Planifiez la base de données des comptes d’utilisateurs. Par défaut, si vous joignez le serveur NPS à un domaine Active Directory, NPS effectue l’authentification et l’autorisation à l’aide de la base de données de comptes utilisateur AD DS. Dans certains cas, par exemple avec les réseaux de grand taille qui utilisent un serveur NPS en tant que proxy RADIUS pour acheminer les demandes de connexion vers d’autres serveurs RADIUS, vous voudrez peut-être installer le serveur NPS sur un ordinateur qui n’est pas membre du domaine.

- Planifiez la gestion de comptes RADIUS. Un serveur NPS vous permet d’enregistrer les données de gestion de comptes dans une base de données SQL Server ou dans un fichier texte sur l’ordinateur local. Si vous souhaitez utiliser la journalisation SQL Server, planifiez l’installation et la configuration de votre serveur exécutant SQL Server.

##### <a name="BKMK_installNPS"></a>Installer le serveur de stratégie réseau (NPS)
Vous pouvez utiliser cette procédure pour installer le serveur NPS (Network Policy Server) à l’aide de l’ajout de rôles et fonctionnalités Assistant. NPS est un service de rôle du rôle serveur Services de stratégie et d’accès réseau.

> [!NOTE]
> Par défaut, le serveur NPS écoute le trafic RADIUS sur les ports 1812, 1813, 1645 et 1646 sur toutes les cartes réseau installées. Si le pare-feu Windows avec fonctions avancées de sécurité est activé lorsque vous installez NPS, les exceptions de pare-feu pour ces ports sont automatiquement créées pendant le processus d’installation pour Internet Protocol version 6 \(IPv6\) et le trafic IPv4. Si vos serveurs d’accès réseau sont configurés pour envoyer le trafic RADIUS sur des ports autres que ces valeurs par défaut, supprimez les exceptions créées dans le pare-feu Windows avec fonctions avancées de sécurité pendant l’installation du serveur NPS et créer des exceptions pour les ports que vous n’utilisez pas pour Trafic RADIUS.

**Informations d’identification administratives**

Pour effectuer cette procédure, vous devez être membre du groupe **Administrateurs du domaine**.

> [!NOTE]
> Pour effectuer cette procédure en utilisant Windows PowerShell, ouvrez PowerShell et tapez l’applet de commande suivante, puis appuyez sur ENTRÉE.
>
> `Install-WindowsFeature NPAS -IncludeManagementTools`

###### <a name="to-install-nps"></a>Pour installer le serveur NPS

1.  Sur le serveur NPS1, dans le Gestionnaire de serveur, cliquez sur **Gérer**, puis sur **Ajouter des rôles et des fonctionnalités**. L’Assistant Ajout de rôles et de fonctionnalités s’ouvre.

2.  Dans **Avant de commencer**, cliquez sur **Suivant**.

    > [!NOTE]
    > La page **Avant de commencer** de l’Assistant Ajout de rôles et de fonctionnalités ne s’affiche pas si vous avez préalablement sélectionné **Ignorer cette page par défaut** dans l’Assistant.

3.  Dans **Sélectionner le type d’installation**, vérifiez que **Installation basée sur un rôle ou une fonctionnalité** est sélectionné, puis cliquez sur **Suivant**.

4.  Dans **Sélectionner le serveur de destination**, vérifiez que **Sélectionner un serveur du pool de serveurs** est sélectionné. Dans **Pool de serveurs**, vérifiez que l’ordinateur local est sélectionné. Cliquez sur **Suivant**.

5.  Dans **sélectionner des rôles de serveur**, dans **rôles**, sélectionnez **stratégie réseau et Services d’accès**. Une boîte de dialogue s’ouvre, vous demandant si elle doit ajouter des fonctionnalités qui sont nécessaires pour la stratégie de réseau et les Services d’accès. Cliquez sur **Ajouter les fonctionnalités**, puis sur **Suivant**.

6.  Dans **Sélectionner des fonctionnalités**, cliquez sur **Suivant**, puis dans **Services de stratégie et d’accès réseau**, vérifiez les informations fournies et cliquez sur **Suivant**.

7.  Dans **Sélectionner des services de rôle**, cliquez sur **Serveur NPS (Network Policy Server)** .  Dans **Ajouter les fonctionnalités requises pour le serveur NPS (Network Policy Server)** , cliquez sur **Ajouter des fonctionnalités**. Cliquez sur **Suivant**.

8.  Dans **Confirmer les sélections d’installation**, cliquez sur **Redémarrer automatiquement le serveur de destination, si nécessaire**. Lorsque vous êtes invité à confirmer ce choix, cliquez sur **Oui**, puis sur **Installer**. La page de progression de l’installation indique l’état du processus d’installation. La fin du processus, le message « Installation réussie sur *ComputerName*» s’affiche, où *ComputerName* est le nom de l’ordinateur sur lequel vous avez installé le serveur de stratégie réseau. Cliquez sur **Fermer**.

##### <a name="BKMK_registerNPS"></a>Inscrire le serveur NPS dans le domaine par défaut
Vous pouvez utiliser cette procédure pour inscrire un serveur NPS dans le domaine où le serveur est membre d’un domaine.

NPSs doit être inscrit dans Active Directory afin qu’ils aient l’autorisation de lire les propriétés de numérotation des comptes d’utilisateur pendant le processus d’autorisation. L’inscription d’un serveur NPS ajoute le serveur à la **serveurs RAS et IAS** groupe dans Active Directory.

**Informations d’identification administratives**

Pour effectuer cette procédure, vous devez être membre du groupe **Administrateurs du domaine**.

> [!NOTE]
> Pour effectuer cette procédure à l’aide des commandes du shell (Netsh) réseau dans Windows PowerShell, ouvrez PowerShell et tapez la commande suivante, puis appuyez sur ENTRÉE.
>
> `netsh nps add registeredserver domain=corp.contoso.com server=NPS1.corp.contoso.com`

###### <a name="to-register-an-nps-in-its-default-domain"></a>Pour inscrire un serveur NPS dans son domaine par défaut

1.  Sur le serveur NPS1, dans le Gestionnaire de serveur, cliquez sur Outils, puis sur **Serveur NPS (Network Policy Server)** . La console MMC Serveur NPS (Network Policy Server) s’ouvre.

2.  Cliquez avec le bouton droit sur **NPS (local)** , puis cliquez sur **Inscrire un serveur dans Active Directory**. La boîte de dialogue **Serveur NPS (Network Policy Server)** s’ouvre.

3.  Dans **Network Policy Server**, cliquez sur **OK**, puis cliquez sur **OK** à nouveau.

Pour plus d’informations sur le serveur de stratégie réseau, consultez [serveur NPS (Network Policy Server)](../technologies/nps/nps-top.md).

#### <a name="BKMK_IIS"></a>Déploiement de WEB1

Le rôle de serveur Web (IIS) dans Windows Server 2016 fournit une plate-forme sécurisée, facile à gérer, modulaire et extensible pour héberger des sites web, services et applications de manière fiable. Avec les Services Internet (IIS), vous pouvez partager des informations avec des utilisateurs sur Internet, un intranet ou un extranet. IIS est une plateforme web unifiée qui intègre IIS, ASP.NET, les services FTP, PHP et Windows Communication Foundation (WCF).

En plus de vous permettre de publier une liste de révocation pour l’accès par les ordinateurs membres du domaine, le rôle de serveur serveur Web (IIS) vous permet de configurer et gérer plusieurs sites web, les applications web et les sites FTP. IIS fournit également les avantages suivants :

-   Maximiser la sécurité Web grâce à un encombrement serveur réduit et l’isolation d’applications automatique.

-   Déployer et exécuter facilement des applications Web ASP.NET, Classic ASP et PHP sur le même serveur.

-   Mettre en place l’isolation d’applications en donnant aux processus de travail une identité unique et une configuration en mode bac à sable (sandbox) par défaut, réduisant ainsi les risques de sécurité.

-   Ajouter, supprimer et même remplacer facilement les composants IIS intégrés par des modules personnalisés, adaptés aux besoins des clients.

-   Accroître les performances de votre site Web grâce à la mise en cache dynamique intégrée et à une compression améliorée.

Pour déployer le serveur WEB1, qui exécute le rôle serveur Serveur Web (IIS), vous devez effectuer les tâches suivantes :

-   Effectuer la procédure indiquée dans la section [Configuration de tous les serveurs](#BKMK_configuringAll)

-   Effectuer la procédure indiquée dans la section [Jonction des serveurs au domaine et ouverture d’une session](#BKMK_joinlogserver)

-   [Installer le rôle de serveur serveur Web (IIS)](#BKMK_install_IIS)

##### <a name="BKMK_install_IIS"></a>Installer le rôle de serveur serveur Web (IIS)
Pour réaliser cette procédure, vous devez être membre du groupe **Administrateurs**.

> [!NOTE]
> Pour effectuer cette procédure en utilisant Windows PowerShell, ouvrez PowerShell et tapez l’applet de commande suivante, puis appuyez sur ENTRÉE.
>
> `Install-WindowsFeature Web-Server -IncludeManagementTools`

1.  Dans le **Gestionnaire de serveur**, cliquez sur **Gérer**, puis sur **Ajouter des rôles et des fonctionnalités**. L’Assistant Ajout de rôles et de fonctionnalités s’ouvre.

2.  Dans **Avant de commencer**, cliquez sur **Suivant**.

    > [!NOTE]
    > La page **Avant de commencer** de l’Assistant Ajout de rôles et de fonctionnalités ne s’affiche pas si vous avez préalablement sélectionné **Ignorer cette page par défaut** dans l’Assistant.

3.  Sur le **sélectionner le Type d’Installation** , cliquez sur **suivant**.

4.  Sur le **server de sélectionner la destination** , vérifiez que l’ordinateur local est sélectionné, puis cliquez sur **suivant**.

5.  Sur le **sélectionner des rôles de serveur** page, faites défiler jusqu'à et sélectionnez **serveur Web (IIS)** . Le **ajouter des fonctionnalités qui sont nécessaires pour le serveur Web (IIS)** boîte de dialogue s’ouvre. Cliquez sur **Ajouter les fonctionnalités**, puis sur **Suivant**.

6.  Cliquez sur **Suivant** plusieurs fois afin d’accepter tous les paramètres par défaut du serveur Web, puis cliquez sur **Installer**.

7.  Vérifiez que toutes les installations ont réussi, puis cliquez sur **Fermer**.

## <a name="BKMK_resources"></a>Ressources techniques supplémentaires
Pour plus d’informations sur les technologies présentées dans ce guide, consultez les ressources suivantes :

 Windows Server 2016, Windows Server 2012 R2 et ressources de bibliothèque technique Windows Server 2012

-   [Quelles sont les nouveautés dans les Services de domaine Active Directory (AD DS) dans Windows Server 2016](https://technet.microsoft.com/library/mt163897.aspx)

-   [Vue d’ensemble de Active Directory Domain Services](https://technet.microsoft.com/library/hh831484.aspx) à https://technet.microsoft.com/library/hh831484.aspx.

-   [Vue d’ensemble du système DNS (Name) de domaine](https://technet.microsoft.com/library/hh831667.aspx) à https://technet.microsoft.com/library/hh831667.aspx.

-   [Implémentation du rôle Administrateurs de DNS](https://technet.microsoft.com/library/cc756152(WS.10).aspx)

-   [Vue d’ensemble de Dynamic Host Configuration Protocol (DHCP)](https://technet.microsoft.com/library/hh831825.aspx) à https://technet.microsoft.com/library/hh831825.aspx.

-   [Vue d’ensemble de stratégie réseau et Services d’accès](https://technet.microsoft.com/library/hh831683.aspx) à https://technet.microsoft.com/library/hh831683.aspx.

-   [Vue d’ensemble du serveur (IIS) de Web](https://technet.microsoft.com/library/hh831725.aspx) à https://technet.microsoft.com/library/hh831725.aspx.

## <a name="BKMK_appendix"></a>Annexes A à E
Les sections suivantes contiennent des informations de configuration supplémentaires pour les ordinateurs qui exécutent des systèmes d’exploitation autres que Windows Server 2016, Windows 10, Windows Server 2012 et Windows 8. En outre, une feuille de préparation de réseau est fournie pour vous aider à votre déploiement.

1.  [Annexe A - changement de nom des ordinateurs](#BKMK_A)

2.  [Annexe B - configuration d’adresse IP statique des adresses](#BKMK_B)

3.  [Annexe C - Ajout d’ordinateurs au domaine](#BKMK_C)

4.  [Annexe D - ouvrez une session sur le domaine](#BKMK_D)

5.  [Annexe E - feuille de préparation de la planification de réseau de base](#BKMK_E)

## <a name="BKMK_A"></a>Annexe A - changement de nom des ordinateurs
Vous pouvez utiliser les procédures dans cette section pour fournir aux ordinateurs exécutant Windows Server 2008 R2, Windows 7, Windows Server 2008 et Windows Vista avec un autre nom d’ordinateur.

-   [Windows Server 2008 R2 et Windows 7](#bkmk_NetFndtn_Pln_rename_R2)

-   [Windows Server 2008 et Windows Vista](#bkmk_NetFndtn_Pln_Renam08)

### <a name="bkmk_NetFndtn_Pln_rename_R2"></a>Windows Server 2008 R2 et Windows 7
Pour effectuer ces procédures, il est nécessaire d’appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent.

##### <a name="to-rename-computers-running-windows-server-2008-r2-and-windows-7"></a>Pour renommer des ordinateurs exécutant Windows Server 2008 R2 et Windows 7

1.  Cliquez sur **Démarrer**, cliquez avec le bouton droit sur **Ordinateur**, puis cliquez sur **Propriétés**. La boîte de dialogue **Système** s’ouvre.

2.  Dans **Paramètres de nom d’ordinateur, de domaine et de groupe de travail**, cliquez sur **Modifier les paramètres**. La boîte de dialogue **Propriétés système** s’ouvre.

    > [!NOTE]
    > Sur les ordinateurs exécutant Windows 7, avant le **propriétés système** boîte de dialogue s’ouvre, le **contrôle de compte d’utilisateur** boîte de dialogue s’affiche, demandant l’autorisation de continuer. Cliquez sur **Continuer** pour continuer.

3.  Cliquez sur **Modifier**. La boîte de dialogue **Modification du nom ou du domaine de l’ordinateur** s’ouvre.

4.  Dans **Nom de l’ordinateur**, tapez le nom de votre ordinateur. Par exemple, pour nommer l’ordinateur DC1, tapez **DC1**.

5.  Cliquez deux fois sur **OK**, cliquez sur **Fermer**, puis sur **Redémarrer maintenant** pour redémarrer l’ordinateur.

### <a name="bkmk_NetFndtn_Pln_Renam08"></a>Windows Server 2008 et Windows Vista
Pour effectuer ces procédures, il est nécessaire d’appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent.

##### <a name="to-rename-computers-running-windows-server-2008-and-windows-vista"></a>Pour renommer des ordinateurs exécutant Windows Server 2008 et Windows Vista

1.  Cliquez sur **Démarrer**, cliquez avec le bouton droit sur **Ordinateur**, puis cliquez sur **Propriétés**. La boîte de dialogue **Système** s’ouvre.

2.  Dans **Paramètres de nom d’ordinateur, de domaine et de groupe de travail**, cliquez sur **Modifier les paramètres**. La boîte de dialogue **Propriétés système** s’ouvre.

    > [!NOTE]
    > Sur les ordinateurs exécutant Windows Vista, avant le **propriétés système** boîte de dialogue s’ouvre, le **contrôle de compte d’utilisateur** boîte de dialogue s’affiche, demandant l’autorisation de continuer. Cliquez sur **Continuer** pour continuer.

3.  Cliquez sur **Modifier**. La boîte de dialogue **Modification du nom ou du domaine de l’ordinateur** s’ouvre.

4.  Dans **Nom de l’ordinateur**, tapez le nom de votre ordinateur. Par exemple, pour nommer l’ordinateur DC1, tapez **DC1**.

5.  Cliquez deux fois sur **OK**, cliquez sur **Fermer**, puis sur **Redémarrer maintenant** pour redémarrer l’ordinateur.

## <a name="BKMK_B"></a>Annexe B - configuration d’adresse IP statique des adresses
Cette rubrique présente les procédures à suivre pour configurer des adresses IP statiques sur des ordinateurs exécutant les systèmes d’exploitation suivants :

-   [Windows Server 2008 R2](#bkmk_R2Cng_WS08R2IP)

-   [Windows Server 2008](#bkmk_NetFndtn_Pln_CfgStatic08)

### <a name="bkmk_R2Cng_WS08R2IP"></a>Windows Server 2008 R2
Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent.

##### <a name="to-configure-a-static-ip-address-on-a-computer-running-windows-server-2008-r2"></a>Pour configurer une adresse IP statique sur un ordinateur exécutant Windows Server 2008 R2

1.  Cliquez sur **Démarrer**, puis sur **Panneau de configuration**.

2.  Dans le **Panneau de configuration**, cliquez sur **Réseau et Internet**. La boîte de dialogue **Réseau et Internet** s’ouvre.

    Dans **Réseau et Internet**, cliquez sur **Centre Réseau et partage**. La boîte de dialogue **Centre Réseau et partage** s’ouvre.

3.  Dans **Centre Réseau et partage**, cliquez sur **Modifier les paramètres de la carte**. La boîte de dialogue **Connexions réseau** s’ouvre.

4.  Dans **Connexions réseau**, cliquez avec le bouton droit sur la connexion réseau que vous voulez configurer, puis cliquez sur **Propriétés**.

5.  Dans **Propriétés de la connexion au réseau local**, dans **Cette connexion utilise les éléments suivants**, sélectionnez **Protocole Internet version 4 (TCP/IPv4)** , puis cliquez sur **Propriétés**. La boîte de dialogue **Propriétés de Protocole Internet version 4 (TCP/IPv4)** s’ouvre.

6.  Dans **Propriétés de Protocole Internet version 4 (TCP/IPv4)** , sous l’onglet **Général**, cliquez sur **Utiliser l’adresse IP suivante**. Dans **Adresse IP**, tapez l’adresse IP que vous voulez utiliser.

7.  Appuyez sur Tab pour placer le curseur dans **Masque de sous-réseau**. Une valeur par défaut pour le masque de sous-réseau est entrée automatiquement. Acceptez le masque de sous-réseau par défaut, ou tapez le masque de sous-réseau que vous voulez utiliser.

8.  Dans **Passerelle par défaut**, tapez l’adresse IP de votre passerelle par défaut.

9. Dans **Serveur DNS préféré**, tapez l’adresse IP de votre serveur DNS. Si vous envisagez d’utiliser l’ordinateur local comme serveur DNS préféré, tapez son adresse IP.

10. Dans **Serveur DNS auxiliaire**, tapez l’adresse IP de votre serveur DNS auxiliaire, le cas échéant. Si vous envisagez d’utiliser l’ordinateur local comme serveur DNS auxiliaire, tapez son adresse IP.

11. Cliquez sur **OK**, puis sur **Fermer**.

### <a name="bkmk_NetFndtn_Pln_CfgStatic08"></a>Windows Server 2008
Pour effectuer ces procédures, il est nécessaire d’appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent.

##### <a name="to-configure-a-static-ip-address-on-a-computer-running-windows-server-2008"></a>Pour configurer une adresse IP statique sur un ordinateur exécutant Windows Server 2008

1.  Cliquez sur **Démarrer**, puis sur **Panneau de configuration**.

2.  Dans le **Panneau de configuration**, vérifiez que **Affichage classique** est sélectionné, puis double-cliquez sur **Centre Réseau et partage**.

3.  Dans le **Centre Réseau et partage**, dans **Tâches**, cliquez sur **Gérer les connexions réseau**.

4.  Dans **Connexions réseau**, cliquez avec le bouton droit sur la connexion réseau que vous voulez configurer, puis cliquez sur **Propriétés**.

5.  Dans **Propriétés de la connexion au réseau local**, dans **Cette connexion utilise les éléments suivants**, sélectionnez **Protocole Internet version 4 (TCP/IPv4)** , puis cliquez sur **Propriétés**. La boîte de dialogue **Propriétés de Protocole Internet version 4 (TCP/IPv4)** s’ouvre.

6.  Dans **Propriétés de Protocole Internet version 4 (TCP/IPv4)** , sous l’onglet **Général**, cliquez sur **Utiliser l’adresse IP suivante**. Dans **Adresse IP**, tapez l’adresse IP que vous voulez utiliser.

7.  Appuyez sur Tab pour placer le curseur dans **Masque de sous-réseau**. Une valeur par défaut pour le masque de sous-réseau est entrée automatiquement. Acceptez le masque de sous-réseau par défaut, ou tapez le masque de sous-réseau que vous voulez utiliser.

8.  Dans **Passerelle par défaut**, tapez l’adresse IP de votre passerelle par défaut.

9. Dans **Serveur DNS préféré**, tapez l’adresse IP de votre serveur DNS. Si vous envisagez d’utiliser l’ordinateur local comme serveur DNS préféré, tapez son adresse IP.

10. Dans **Serveur DNS auxiliaire**, tapez l’adresse IP de votre serveur DNS auxiliaire, le cas échéant. Si vous envisagez d’utiliser l’ordinateur local comme serveur DNS auxiliaire, tapez son adresse IP.

11. Cliquez sur **OK**, puis sur **Fermer**.

## <a name="BKMK_C"></a>Annexe C - Ajout d’ordinateurs au domaine
Vous pouvez utiliser ces procédures pour joindre des ordinateurs exécutant Windows Server 2008 R2, Windows 7, Windows Server 2008 et Windows Vista au domaine.

-   [Windows Server 2008 R2 et Windows 7](#BKMK_c1)

-   [Windows Server 2008 et Windows Vista](#BKMK_c2)

> [!IMPORTANT]
> Pour joindre un ordinateur à un domaine, vous devez avoir ouvert une session sur l’ordinateur avec le compte Administrateur local ou, si vous avez ouvert une session avec un compte d’utilisateur qui ne possède pas les informations d’identification d’administration de l’ordinateur local, vous devez fournir les informations d’identification du compte Administrateur local au cours du processus visant à joindre l’ordinateur au domaine. Par ailleurs, vous devez posséder un compte d’utilisateur dans le domaine auquel vous voulez joindre l’ordinateur. Au cours du processus visant à joindre l’ordinateur au domaine, vous serez invité à fournir vos informations d’identification de compte de domaine (nom d’utilisateur et mot de passe).

### <a name="BKMK_c1"></a>Windows Server 2008 R2 et Windows 7
Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Utilisateurs du domaine** ou à un groupe équivalent.

##### <a name="to-join-computers-running-windows-server-2008-r2-and-windows-7-to-the-domain"></a>Pour joindre des ordinateurs exécutant Windows Server 2008 R2 et Windows 7 au domaine

1.  Ouvrez une session sur l’ordinateur avec le compte Administrateur local.

2.  Cliquez sur **Démarrer**, cliquez avec le bouton droit sur **Ordinateur**, puis cliquez sur **Propriétés**. La boîte de dialogue **Système** s’ouvre.

3.  Dans **Paramètres de nom d’ordinateur, de domaine et de groupe de travail**, cliquez sur **Modifier les paramètres**. La boîte de dialogue **Propriétés système** s’ouvre.

    > [!NOTE]
    > Sur les ordinateurs exécutant Windows 7, avant le **propriétés système** boîte de dialogue s’ouvre, le **contrôle de compte d’utilisateur** boîte de dialogue s’affiche, demandant l’autorisation de continuer. Cliquez sur **Continuer** pour continuer.

4.  Cliquez sur **Modifier**. La boîte de dialogue **Modification du nom ou du domaine de l’ordinateur** s’ouvre.

5.  Dans **Nom de l’ordinateur**, dans **Membre de**, sélectionnez **Domaine**, puis tapez le nom du domaine auquel joindre l’ordinateur. Par exemple, si le nom de domaine est corp.contoso.com, tapez **corp.contoso.com**.

6.  Cliquez sur **OK**. La boîte de dialogue **Sécurité de Windows** s’ouvre.

7.  Dans **Modification du nom ou du domaine de l’ordinateur**, dans **Nom d’utilisateur**, tapez le nom d’utilisateur. Dans **Mot de passe**, tapez le mot de passe, puis cliquez sur **OK**. La boîte de dialogue **Modification du nom ou du domaine de l’ordinateur** s’ouvre et vous souhaite la bienvenue dans le domaine. Cliquez sur **OK**.

8.  La boîte de dialogue **Modification du nom ou du domaine de l’ordinateur** affiche un message indiquant que vous devez redémarrer l’ordinateur pour appliquer les modifications. Cliquez sur **OK**.

9. Dans la boîte de dialogue **Propriétés système**, sous l’onglet **Nom de l’ordinateur**, cliquez sur **Fermer**. La boîte de dialogue **Microsoft Windows** s’ouvre et affiche un message indiquant de nouveau que vous devez redémarrer l’ordinateur pour appliquer les modifications. Cliquez sur **Redémarrer maintenant**.

### <a name="BKMK_c2"></a>Windows Server 2008 et Windows Vista
Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Utilisateurs du domaine** ou à un groupe équivalent.

##### <a name="to-join-computers-running-windows-server-2008-and-windows-vista-to-the-domain"></a>Pour joindre des ordinateurs exécutant Windows Server 2008 et Windows Vista au domaine

1.  Ouvrez une session sur l’ordinateur avec le compte Administrateur local.

2.  Cliquez sur **Démarrer**, cliquez avec le bouton droit sur **Ordinateur**, puis cliquez sur **Propriétés**. La boîte de dialogue **Système** s’ouvre.

3.  Dans **Paramètres de nom d’ordinateur, de domaine et de groupe de travail**, cliquez sur **Modifier les paramètres**. La boîte de dialogue **Propriétés système** s’ouvre.

4.  Cliquez sur **Modifier**. La boîte de dialogue **Modification du nom ou du domaine de l’ordinateur** s’ouvre.

5.  Dans **Nom de l’ordinateur**, dans **Membre de**, sélectionnez **Domaine**, puis tapez le nom du domaine auquel joindre l’ordinateur. Par exemple, si le nom de domaine est corp.contoso.com, tapez **corp.contoso.com**.

6.  Cliquez sur **OK**. La boîte de dialogue **Sécurité de Windows** s’ouvre.

7.  Dans **Modification du nom ou du domaine de l’ordinateur**, dans **Nom d’utilisateur**, tapez le nom d’utilisateur. Dans **Mot de passe**, tapez le mot de passe, puis cliquez sur **OK**. La boîte de dialogue **Modification du nom ou du domaine de l’ordinateur** s’ouvre et vous souhaite la bienvenue dans le domaine. Cliquez sur **OK**.

8.  La boîte de dialogue **Modification du nom ou du domaine de l’ordinateur** affiche un message indiquant que vous devez redémarrer l’ordinateur pour appliquer les modifications. Cliquez sur **OK**.

9. Dans la boîte de dialogue **Propriétés système**, sous l’onglet **Nom de l’ordinateur**, cliquez sur **Fermer**. La boîte de dialogue **Microsoft Windows** s’ouvre et affiche un message indiquant de nouveau que vous devez redémarrer l’ordinateur pour appliquer les modifications. Cliquez sur **Redémarrer maintenant**.

## <a name="BKMK_D"></a>Annexe D - ouvrez une session sur le domaine
Vous pouvez utiliser ces procédures pour vous connecter au domaine sur des ordinateurs exécutant Windows Server 2008 R2, Windows 7, Windows Server 2008 et Windows Vista.

-   [Windows Server 2008 R2 et Windows 7](#BKMK_d1)

-   [Windows Server 2008 et Windows Vista](#BKMK_d2)

### <a name="BKMK_d1"></a>Windows Server 2008 R2 et Windows 7
Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Utilisateurs du domaine** ou à un groupe équivalent.

##### <a name="log-on-to-the-domain-using-computers-running-windows-server-2008-r2-and-windows-7"></a>Ouvrez une session le domaine à l’aide des ordinateurs exécutant Windows Server 2008 R2 et Windows 7

1.  Fermez la session sur l’ordinateur et redémarrez-le.

2.  Appuyez sur Ctrl+Alt+Suppr. L’écran d’ouverture de session s’affiche.

3.  Cliquez sur **Changer d’utilisateur**, puis sur **Autre utilisateur**.

4.  Dans **Nom d’utilisateur**, tapez vos noms de domaine et d’utilisateur au format *domaine\utilisateur*. Par exemple, pour vous connecter au domaine corp.contoso.com avec un compte nommé **Utilisateur-01**, tapez **CORP\Utilisateur-01**.

5.  Dans **Mot de passe**, tapez votre mot de passe de domaine, puis cliquez sur la flèche ou appuyez sur ENTRÉE.

### <a name="BKMK_d2"></a>Windows Server 2008 et Windows Vista
Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Utilisateurs du domaine** ou à un groupe équivalent.

##### <a name="log-on-to-the-domain-using-computers-running-windows-server-2008-and-windows-vista"></a>Ouvrez une session le domaine à l’aide des ordinateurs exécutant Windows Server 2008 et Windows Vista

1.  Fermez la session sur l’ordinateur et redémarrez-le.

2.  Appuyez sur Ctrl+Alt+Suppr. L’écran d’ouverture de session s’affiche.

3.  Cliquez sur **Changer d’utilisateur**, puis sur **Autre utilisateur**.

4.  Dans **Nom d’utilisateur**, tapez vos noms de domaine et d’utilisateur au format *domaine\utilisateur*. Par exemple, pour vous connecter au domaine corp.contoso.com avec un compte nommé **Utilisateur-01**, tapez **CORP\Utilisateur-01**.

5.  Dans **Mot de passe**, tapez votre mot de passe de domaine, puis cliquez sur la flèche ou appuyez sur ENTRÉE.

## <a name="BKMK_E"></a>Annexe E - feuille de préparation de la planification de réseau de base
Vous pouvez utiliser la feuille de préparation de la planification de réseau proposée dans cette annexe pour collecter les informations nécessaires à l’installation d’un réseau de base. Cette annexe comporte des tableaux répertoriant les éléments de configuration propres à chaque ordinateur serveur pour lesquels vous devez fournir des informations ou des valeurs spécifiques au cours du processus d’installation ou de configuration. Des exemples de valeurs sont fournis pour chaque élément de configuration.

Des espaces sont prévus dans chaque tableau pour que vous puissiez entrer les valeurs utilisées pour votre déploiement. Si vous enregistrez des valeurs relatives à la sécurité dans ces tableaux, assurez-vous de stocker ces informations en lieu sûr.

Les liens suivants permettent d’accéder aux différentes sections de cette annexe qui présentent les éléments de configuration et les exemples de valeurs qui sont associés aux procédures de déploiement décrites dans ce guide.

1.  [Installation de DNS et Active Directory Domain Services](#BKMK_FndtnPrep_InstallAD)

    -   [Configuration d’une Zone de recherche inversée DNS](#BKMK_FndtnPrep_DNSRevrsLook)

2.  [L’installation de DHCP](#BKMK_FndtnPrep_InstallDHCP)

    -   [Création d’une plage d’exclusion dans DHCP](#BKMK_FndtnPrep_DHCP_Exclusn)

    -   [Création d’une nouvelle étendue DHCP](#bkmk_NetFndtn_Pln_DHCP_NewScope)

3.  [L’installation du serveur de stratégie réseau (facultatif)](#BKMK_FndtnPrep_InstallNPS)

### <a name="BKMK_FndtnPrep_InstallAD"></a>Installation de DNS et Active Directory Domain Services
Les tableaux de cette section répertorient les éléments de configuration pour la préinstallation et l’installation des Services de domaine Active Directory (AD DS) et DNS.

##### <a name="pre-installation-configuration-items-for-ad-ds-and-dns"></a>Éléments de configuration de pré-installation pour AD DS et DNS
Les tableaux suivants répertorient les éléments de configuration pour la préinstallation comme décrit dans la procédure [Configuration de tous les serveurs](#BKMK_configuringAll) :

-   [Configurer une adresse IP statique](#BKMK_ip)

|Éléments de configuration|Exemples de valeurs|Valeurs|
|-----------------------|------------------|----------|
|Adresse IP|10.0.0.2||
|Masque de sous-réseau|255.255.255.0||
|Passerelle par défaut|10.0.0.1||
|Serveur DNS préféré|127.0.0.1||
|Serveur DNS auxiliaire|10.0.0.15||

-   [Renommer l’ordinateur](#BKMK_rename)

|Élément de configuration|Exemple de valeur|Value|
|----------------------|-----------------|---------|
|Nom de l'ordinateur|DC1||

##### <a name="ad-ds-and-dns-installation-configuration-items"></a>Éléments de configuration d’installation AD DS et DNS
Éléments de configuration pour le déploiement d’un réseau de base Windows Server comme décrit dans la procédure [Installer les services de domaine Active Directory et DNS dans une nouvelle forêt](#BKMK_installAD-DNS) :

|Éléments de configuration|Exemples de valeurs|Valeurs|
|-----------------------|------------------|----------|
|Nom DNS complet|corp.contoso.com||
|Niveau fonctionnel de forêt|Windows Server 2003||
|Emplacement du dossier de la base de données des services de domaine Active Directory|E:\Configuration\\<br /><br />Ou acceptez la valeur par défaut.||
|Emplacement du dossier des fichiers journaux des services de domaine Active Directory|E:\Configuration\\<br /><br />Ou acceptez la valeur par défaut.||
|Emplacement du dossier SYSVOL des services de domaine Active Directory|E:\Configuration\\<br /><br />Ou acceptez la valeur par défaut.||
|Mot de passe administrateur de restauration des services d’annuaire|J*p2leO4$F||
|Nom du fichier de réponses (facultatif)|AD DS_AnswerFile||

#### <a name="BKMK_FndtnPrep_DNSRevrsLook"></a>Configuration d’une Zone de recherche inversée DNS

|Éléments de configuration|Exemples de valeurs|Valeurs|
|-----------------------|------------------|----------|
|Type de zone|-Zone principale<br />-Zone secondaire<br />-Zone de stub||
|Type de zone<br /><br />**Store de la zone dans Active Directory**|-Sélectionné<br />-Non sélectionné||
|Étendue de réplication de la zone Active Directory|-Pour tous les serveurs DNS dans cette forêt<br />-Pour tous les serveurs DNS dans ce domaine<br />-Pour tous les contrôleurs de domaine dans ce domaine<br />-Pour tous les contrôleurs de domaine spécifiés dans l’étendue de cette partition d’annuaire||
|Nom de la zone de recherche inversée<br /><br />(Type IP)|: Zone de recherche inversée IPv4<br />: Zone de recherche inversée IPv6||
|Nom de la zone de recherche inversée<br /><br />(ID réseau)|10.0.0||

### <a name="BKMK_FndtnPrep_InstallDHCP"></a>L’installation de DHCP
Les tableaux de cette section répertorient les éléments de configuration pour la préinstallation et l’installation de DHCP.

##### <a name="pre-installation-configuration-items-for-dhcp"></a>Éléments de configuration pour la préinstallation de DHCP
Les tableaux suivants répertorient les éléments de configuration pour la préinstallation comme décrit dans la procédure [Configuration de tous les serveurs](#BKMK_configuringAll) :

-   [Configurer une adresse IP statique](#BKMK_ip)

|Éléments de configuration|Exemples de valeurs|Valeurs|
|-----------------------|------------------|----------|
|Adresse IP|10.0.0.3||
|Masque de sous-réseau|255.255.255.0||
|Passerelle par défaut|10.0.0.1||
|Serveur DNS préféré|10.0.0.2||
|Serveur DNS auxiliaire|10.0.0.15||

-   [Renommer l’ordinateur](#BKMK_rename)

|Élément de configuration|Exemple de valeur|Value|
|----------------------|-----------------|---------|
|Nom de l'ordinateur|DHCP1||

##### <a name="dhcp-installation-configuration-items"></a>Éléments de configuration pour l’installation de DHCP
Éléments de configuration pour le déploiement d’un réseau de base Windows Server comme décrit dans la procédure [Installer DHCP (Dynamic Host Configuration Protocol)](#BKMK_installDHCP) :

|Éléments de configuration|Exemples de valeurs|Valeurs|
|-----------------------|------------------|----------|
|Liaisons de connexion réseau|Ethernet||
|Paramètres du serveur DNS|DC1||
|Adresse IP du serveur DNS préféré|10.0.0.2||
|Adresse IP du serveur DNS auxiliaire|10.0.0.15||
|Nom de l’étendue|Corp1||
|Adresse IP de début|10.0.0.1||
|Adresse IP de fin|10.0.0.254||
|Masque de sous-réseau|255.255.255.0||
|Passerelle par défaut (facultatif)|10.0.0.1||
|Durée du bail|8 jours||
|Mode d’opération du serveur DHCP IPv6|Non activé||

#### <a name="BKMK_FndtnPrep_DHCP_Exclusn"></a>Création d’une plage d’exclusion dans DHCP
Éléments de configuration pour créer une plage d’exclusion lors de la création d’une étendue dans DHCP.

|Éléments de configuration|Exemples de valeurs|Valeurs|
|-----------------------|------------------|----------|
|Nom de l’étendue|Corp1||
|Description de l’étendue|Sous-réseau 1 du siège social||
|Adresse IP de début de la plage d’exclusion|10.0.0.1||
|Adresse IP de fin de la plage d’exclusion|10.0.0.15||

#### <a name="bkmk_NetFndtn_Pln_DHCP_NewScope"></a>Création d’une nouvelle étendue DHCP
Éléments de configuration pour le déploiement d’un réseau de base Windows Server comme décrit dans la procédure [Créer et activer une nouvelle étendue DHCP](#BKMK_newscopeDHCP) :

|Éléments de configuration|Exemples de valeurs|Valeurs|
|-----------------------|------------------|----------|
|Nom de la nouvelle étendue|Corp2||
|Description de l’étendue|Sous-réseau local du siège 2||
|(Plage d’adresses IP)<br /><br />Adresse IP de début|10.0.1.1||
|(Plage d’adresses IP)<br /><br />Adresse IP de fin|10.0.1.254||
|Durée|8||
|Masque de sous-réseau|255.255.255.0||
|Adresse IP de début de la plage d’exclusion|10.0.1.1||
|Adresse IP de fin de la plage d’exclusion|10.0.1.15||
|Durée du bail<br /><br />Days<br /><br />Heures<br /><br />Minutes|-   8<br />-   0<br />-   0||
|Routeur (passerelle par défaut)<br /><br />Adresse IP|10.0.1.1||
|Domaine parent DNS|corp.contoso.com||
|Serveur DNS<br /><br />Adresse IP|10.0.0.2||

### <a name="BKMK_FndtnPrep_InstallNPS"></a>L’installation du serveur de stratégie réseau (facultatif)
Les tableaux de cette section répertorient les éléments de configuration pour la préinstallation et l’installation d’un serveur NPS.

##### <a name="pre-installation-configuration-items"></a>Éléments de configuration pour la préinstallation
Les trois tableaux suivants répertorient les éléments de configuration pour la préinstallation comme décrit dans la procédure [Configuration de tous les serveurs](#BKMK_configuringAll) :

-   [Configurer une adresse IP statique](#BKMK_ip)

|Éléments de configuration|Exemples de valeurs|Valeurs|
|-----------------------|------------------|----------|
|Adresse IP|10.0.0.4||
|Masque de sous-réseau|255.255.255.0||
|Passerelle par défaut|10.0.0.1||
|Serveur DNS préféré|10.0.0.2||
|Serveur DNS auxiliaire|10.0.0.15||

-   [Renommer l’ordinateur](#BKMK_rename)

|Élément de configuration|Exemple de valeur|Value|
|----------------------|-----------------|---------|
|Nom de l'ordinateur|NPS1||

##### <a name="network-policy-server-installation-configuration-items"></a>Éléments de configuration pour l’installation d’un serveur NPS
Éléments de configuration pour les procédures de déploiement de serveur NPS Windows Server Core réseau [installer serveur NPS (Network Policy)](#BKMK_installNPS) et [inscrire le serveur NPS dans le domaine par défaut](#BKMK_registerNPS).

-   Aucun élément de configuration supplémentaire n’est requis pour installer et inscrire un serveur NPS.

