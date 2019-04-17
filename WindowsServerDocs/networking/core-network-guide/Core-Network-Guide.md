---
title: Guide du réseau de base
description: Ce guide fournit des instructions sur la façon de planifier et déployer les principaux composants requis pour un réseau pleinement fonctionnel et un nouveau domaine ActiveDirectory dans une nouvelle forêt avec Windows Server2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: b3cd60f7-d380-4712-9a78-0a8f551e1121
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 90390a7b975bc358fb96715d23fe542bc261220e
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="core-network-guide"></a>Guide du réseau de base

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Ce guide fournit des instructions sur la façon de planifier et déployer les principaux composants requis pour un réseau pleinement fonctionnel et un nouveau domaine ActiveDirectory dans une nouvelle forêt.

> [!NOTE]
> Ce guide est disponible en téléchargement au format MicrosoftWord à partir de la Galerie TechNet. Pour plus d’informations, voir [Guide de réseau de base pour Windows Server2016](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683).

Ce guide contient les sections suivantes.

- [À propos de ce guide](#BKMK_about)

- [Vue d’ensemble du réseau de base](#BKMK_overview)

- [Planification de réseau de base](#BKMK_planning)

- [Déploiement de réseau de base](#BKMK_deployment)

- [Ressources techniques supplémentaires](#BKMK_resources)

- [Annexes A à E](#BKMK_appendix)

## <a name="BKMK_about"></a>À propos de ce guide
Ce guide est conçu pour les administrateurs réseau et système qui installent un nouveau réseau ou qui souhaitent créer un réseau basé sur le domaine pour remplacer un réseau constitué de groupes de travail. Le scénario de déploiement fourni dans ce guide est particulièrement utile si vous prévoyez la nécessité d’ajouter des services et des fonctionnalités à votre réseau à l’avenir.

Il est recommandé que vous passez en revue la conception et des guides de déploiement pour chacune des technologies utilisées dans ce scénario de déploiement pour vous aider à déterminer si ce guide fournit les services et la configuration dont vous avez besoin.

Un réseau de base est une collection de matériels de réseau, périphériques et logiciels qui fournit les services centraux répondant pour les technologies de l’information de votre organisation (IT) aux besoins.

Un réseau de base Windows Server vous offre de nombreux avantages, notamment les suivantes.

-   Protocoles centraux pour la connectivité réseau entre les ordinateurs et autres appareils compatibles TCP/IP Transmission Control Protocol/Internet Protocol (). TCP/IP est une suite de protocoles standard pour la connexion des ordinateurs et de création de réseaux. TCP/IP est un logiciel de protocole réseau fourni avec les systèmes d’exploitation MicrosoftWindows qui implémente et prend en charge la suite de protocoles TCP/IP.

-   Dynamic Host Configuration Protocol (DHCP) automatique attribution d’adresses IP aux ordinateurs et autres périphériques qui sont configurés en tant que clients DHCP. Configuration manuelle d’adresses IP sur tous les ordinateurs sur votre réseau prend du temps et est moins souple que dynamiquement aux ordinateurs et autres périphériques avec les configurations d’adresses IP à l’aide d’un serveur DHCP.

-   Service de résolution de nom du système DNS (Domain Name). DNS permet aux utilisateurs, ordinateurs, applications et services trouver les adresses IP des ordinateurs et périphériques sur le réseau à l’aide du nom de domaine complet de l’ordinateur ou le périphérique.

-   Une forêt, qui est un ou plusieurs domaines ActiveDirectory qui partagent les mêmes définitions de classes et d’attributs (schéma), informations de site et de réplication (configuration) et des fonctionnalités de recherche de la forêt (catalogue global).

-   Un domaine racine de forêt, qui est le premier domaine créé dans une nouvelle forêt. Les groupes Administrateurs de l’entreprise et administrateurs du schéma, qui sont des groupes d’administration de forêt, sont trouvent dans le domaine racine de forêt. En outre, un domaine racine de forêt, comme avec d’autres domaines, est une collection d’objets ordinateur, utilisateur et groupe qui sont définies par l’administrateur dans les Services de domaine ActiveDirectory (ADDS). Ces objets partagent une stratégies de sécurité et de la base de données Active courants. Ils peuvent également partager des relations de sécurité avec d’autres domaines si vous ajoutez les domaines que votre organisation se développe. Également, le service d’annuaire stocke les données d’annuaire et permet à des ordinateurs autorisés, applications et les utilisateurs à accéder aux données.

-   Une base de données de compte utilisateur et ordinateur. Le service d’annuaire fournit une base de données de comptes utilisateur centralisée qui permet de créer des comptes d’utilisateur et ordinateur pour les personnes et les ordinateurs qui sont autorisés à se connecter à votre réseau et le réseau de l’accès aux ressources, telles que les applications, les bases de données, les fichiers partagés et les dossiers et les imprimantes.

Un réseau de base vous permet également de faire évoluer votre réseau de votre organisation se développe et modifier des besoins. Par exemple, avec un réseau de base, vous pouvez ajouter des domaines, sous-réseaux IP, services d’accès à distance, services sans fil et autres fonctionnalités et rôles serveur fournis par Windows Server2016.

### <a name="network-hardware-requirements"></a>Configuration matérielle réseau
Pour déployer correctement un réseau de base, vous devez déployer le matériel réseau, notamment les suivantes:

-   Ethernet, Fast Ethernet ou Gigabyte Ethernet câblage

-   Un concentrateur, couche 2 ou 3 commutateur, routeur ou autre appareil qui exécute la fonction de relais du trafic réseau entre les ordinateurs et périphériques.

-   Ordinateurs qui répondent à la configuration matérielle minimale requise pour leurs systèmes d’exploitation client et serveur respectifs.

## <a name="what-this-guide-does-not-provide"></a>Ce que ce guide ne fournit pas
Ce guide ne fournit pas d’instructions pour déployer les éléments suivants:

-   Matériel réseau, comme le câblage, les routeurs, les commutateurs et les concentrateurs

-   Ressources réseau supplémentaires, telles que des imprimantes et des serveurs de fichiers

-   Connectivité Internet

-   Accès à distance

-   Accès sans fil

-   Déploiement d’ordinateurs clients

> [!NOTE]
> Ordinateurs exécutant des systèmes d’exploitation Windows client sont configurés par défaut pour recevoir des baux d’adresses IP à partir du serveur DHCP. Par conséquent, aucune supplémentaires DHCP ou le protocole Internet version4 (IPv4) configuration des ordinateurs clients est requise.

## <a name="technology-overviews"></a>Vues d’ensemble de la technologie
Les sections suivantes fournissent de brèves vues d’ensemble des technologies requises qui sont déployés pour créer un réseau de base.

### <a name="active-directory-domain-services"></a>Services de domaine ActiveDirectory
Un répertoire est une structure hiérarchique qui stocke des informations sur les objets sur le réseau, tels que les utilisateurs et ordinateurs. Un service d’annuaire, telles que les services ADDS, fournit les méthodes pour stocker les données d’annuaire et rendre ces données disponibles pour les administrateurs et utilisateurs du réseau. Par exemple, les services ADDS stocke des informations sur les comptes d’utilisateur, y compris les noms, les adresses de messagerie, les mots de passe et les numéros de téléphone et permet aux autres utilisateurs autorisés sur le même réseau accéder à ces informations.

### <a name="dns"></a>DNS
DNS est un protocole de résolution de nom pour les réseaux TCP/IP, par exemple Internet ou un réseau d’entreprise. Un serveur DNS héberge les informations qui permettent aux ordinateurs clients et les services de résoudre reconnu, les noms DNS alphanumériques pour les adresses IP que les ordinateurs utilisent pour communiquer entre eux.

### <a name="dhcp"></a>DHCP
DHCP est une norme IP permettant de simplifier la gestion de configuration IP de l’hôte. La norme permet l’utilisation de serveurs DHCP comme moyen de gérer l’allocation dynamique d’adresses IP et d’autres informations de configuration pour les clients DHCP sur votre réseau.

DHCP vous permet d’utiliser un serveur DHCP pour attribuer dynamiquement une adresse IP à un ordinateur ou un autre périphérique, tel qu’une imprimante, sur votre réseau local. Chaque ordinateur sur un réseau TCP/IP doit avoir une adresse IP unique, car l’adresse IP et son masque de sous-réseau associé d’identifier l’ordinateur hôte et le sous-réseau auquel l’ordinateur est attaché. À l’aide de DHCP, vous pouvez vous assurer que tous les ordinateurs qui sont configurés en tant que clients DHCP reçoivent une adresse IP qui convient à leur emplacement réseau et le sous-réseau, et à l’aide des options DHCP, telles que la passerelle par défaut et les serveurs DNS, vous pouvez fournir automatiquement aux clients DHCP avec les informations dont ils ont besoin pour fonctionner correctement sur votre réseau.

Pour les réseaux basés sur IP, DHCP réduit la complexité et la quantité de travail administratif impliqué dans la reconfiguration des ordinateurs.

### <a name="tcpip"></a>TCP/IP
TCP/IP dans Windows Server2016 est la suivante:

-   Logiciel réseau reposant sur les protocoles réseau standard.

-   Un protocole réseau d’entreprise routable qui prend en charge la connexion de votre ordinateur basé sur Windows à un réseau local (LAN) et les environnements de réseau (étendu WAN).

-   Technologies de base et des utilitaires pour connecter votre ordinateur Windows avec des systèmes hétérogènes pour le partage d’informations.

-   Une base pour pouvoir accéder aux services Internet globaux, tels que les serveurs World Wide Web et le protocole FTP (File Transfer).

-   Une infrastructure client/serveur interplateforme, évolutive et robuste.

TCP/IP propose des utilitaires TCP/IP de base qui permettent à des ordinateurs Windows à se connecter et partager des informations avec d’autres Microsoft et les systèmes non Microsoft, y compris:

-    Windows Server2016

-   Windows10

-    Windows Server2012R2

-   Windows8.1

-    Windows Server2012

-   Windows8

-    Windows Server2008R2

-    Windows7

-    Windows Server2008

-   WindowsVista

-   Hôtes Internet

-   Systèmes Apple Macintosh

-   Gros systèmes IBM

-   Systèmes UNIX et Linux

-   Systèmes Open VMS

-   Imprimantes réseau

-   Tablettes et téléphones cellulaires avec les réseaux câblés Ethernet ou de la technologie sans fil 802.11 activé

## <a name="BKMK_overview"></a>Vue d’ensemble du réseau de base
L’illustration suivante montre la topologie de réseau de base Windows Server.

![Topologie de réseau de base Windows Server](../media/Core-Network-Guide/cng16_overview.jpg)

> [!NOTE]
> Ce guide inclut également des instructions pour l’ajout de serveurs facultatifs de serveur NPS (Network Policy) et le serveur Web (IIS) pour la topologie de votre réseau afin de fournir la base pour les solutions d’accès réseau sécurisé, telles que 802. 1 X câblé et sans fil déploiements que vous pouvez implémenter à l’aide des guides d’accompagnement du réseau de base. Pour plus d’informations, voir [déploiement des fonctionnalités facultatives pour l’authentification d’accès réseau et les services Web](#BKMK_optionalfeatures).

### <a name="core-network-components"></a>Composants de réseau de base
Voici les composants d’un réseau de base.

##### <a name="router"></a>Routeur
Ce guide de déploiement fournit des instructions pour le déploiement d’un réseau de base avec deux sous-réseaux séparés par un routeur sur lequel l’acheminement DHCP est activé. Vous pouvez, toutefois, déployer un commutateur de couche 2, un commutateur de couche 3 ou un concentrateur, selon vos besoins et les ressources. Si vous déployez un commutateur, le commutateur doit être capable d’acheminement DHCP ou vous devez placer un serveur DHCP sur chaque sous-réseau. Si vous déployez un concentrateur, vous déployez un sous-réseau unique et ne devez pas d’acheminement DHCP ni une seconde étendue sur votre serveur DHCP.

##### <a name="static-tcpip-configurations"></a>Configurations TCP/IP statiques
Les serveurs de ce déploiement sont configurés avec des adresses IPv4 statiques. Les ordinateurs clients sont configurés par défaut pour recevoir des baux d’adresses IP à partir du serveur DHCP.

##### <a name="active-directory-domain-services-global-catalog-and-dns-server-dc1"></a>Catalogue global des Services de domaine ActiveDirectory et serveur DNS DC1
Les Services de domaine ActiveDirectory (ADDS) et le système DNS (Domain Name) sont installés sur ce serveur, appelé DC1, qui fournit le répertoire et à tous les ordinateurs et périphériques sur le réseau, les services de résolution de nom.

##### <a name="dhcp-server-dhcp1"></a>Serveur DHCP DHCP1
Le serveur DHCP, appelé DHCP1, est configuré avec une étendue qui fournit des baux d’adresses IP (Internet Protocol) aux ordinateurs sur le sous-réseau local. Le serveur DHCP peut également être configuré avec des étendues supplémentaires pour fournir des baux d’adresses IP aux ordinateurs sur d’autres sous-réseaux si l’acheminement DHCP est configuré sur les routeurs.

##### <a name="client-computers"></a>Ordinateurs clients
Ordinateurs exécutant des systèmes d’exploitation Windows client sont configurés par défaut en tant que clients DHCP, lequel obtiennent automatiquement les adresses IP et les options DHCP à partir du serveur DHCP.

## <a name="BKMK_planning"></a>Planification de réseau de base
Avant de déployer un réseau de base, vous devez planifier les éléments suivants.

-   [Planification des sous-réseaux](#bkmk_NetFndtn_Pln_Subnt)

-   [Planification de la configuration de base de tous les serveurs](#bkmk_NetFndtn_Pln_AllSrvrs)

-   [Planification du déploiement de DC1](#bkmk_NetFndtn_Pln_AD-DNS-01)

-   [Planification de l’accès de domaine](#bkmk_NetFndtn_Pln_DomAccess)

-   [Planification du déploiement du serveur DHCP1](#bkmk_NetFndtn_Pln_DHCP-01)

Les sections suivantes fournissent plus de détails sur chacun de ces éléments.

> [!NOTE]
> Pour obtenir une assistance à la planification de votre déploiement, voir également [annexe E - feuille de préparation de planification de base réseau](#BKMK_E).

### <a name="bkmk_NetFndtn_Pln_Subnt"></a>Planification des sous-réseaux
Dans un réseau TCP/IP Transmission Control Protocol/Internet Protocol (), les routeurs sont utilisés pour interconnecter le matériel et les logiciels utilisés sur des segments réseau physique différent appelés sous-réseaux. Les routeurs sont également utilisés pour transférer des paquets IP entre ces sous-réseaux. Déterminer la disposition physique de votre réseau, y compris le nombre de routeurs et sous-réseaux que vous avez besoin, avant de poursuivre les instructions fournies dans ce guide.

En outre, pour configurer les serveurs sur votre réseau avec des adresses IP statiques, vous devez déterminer la plage d’adresses IP que vous souhaitez utiliser pour le sous-réseau où se trouvent vos serveurs de réseau de base. Dans ce guide, les plages d’adresses IP privées 10.0.0.1 - 10.0.0.254 et 10.0.1.1 - 10.0.1.254 sont utilisés comme exemples, mais vous pouvez utiliser n’importe quel plage d’adresses IP privées que vous préférez.

> [!IMPORTANT]
> Après avoir sélectionné les plages d’adresses IP que vous souhaitez utiliser pour chaque sous-réseau, veillez à configurer vos routeurs avec une adresse IP à partir de la même plage d’adresses IP que celle utilisée sur le sous-réseau sur lequel le routeur est installé. Par exemple, si votre routeur est configuré par défaut avec l’adresse IP 192.168.1.1, mais que vous installez le service de routeur sur un sous-réseau avec une plage d’adresses IP de 10.0.0.0/24, vous devez reconfigurer le routeur pour utiliser une adresse IP à partir de la plage d’adresses IP 10.0.0.0/24.

Adresse IP privée plages sont spécifiées par Internet demander des commentaires RFC1918reconnues:

-   10.0.0.0 - 10.255.255.255

-   172.16.0.0 - 172.31.255.255

-   192.168.0.0 - 192.168.255.255

Lorsque vous utilisez les plages d’adresses IP privées comme spécifié dans le document RFC1918, vous ne pouvez pas vous connecter directement à Internet à l’aide d’une adresse IP privée, car les demandes dirigées vers ou à partir de ces adresses sont automatiquement ignorées par les routeurs de fournisseur de services Internet. Pour ajouter une connexion Internet à votre réseau de base ultérieurement, vous devez contrat avec un fournisseur de services Internet pour obtenir une adresse IP publique.

> [!IMPORTANT]
> Lorsque vous utilisez des adresses IP privées, vous devez utiliser un type de proxy ou réseau serveur serveur address translation (NAT) pour convertir les plages d’adresses IP privées sur votre réseau local pour une adresse IP publique qui peut être routée sur Internet. La plupart des routeurs fournissant des services NAT, afin de la sélection d’un routeur NAT ne devrait pas poser.

Pour plus d’informations, voir [planification du déploiement du serveur DHCP1](#bkmk_NetFndtn_Pln_DHCP-01).

### <a name="bkmk_NetFndtn_Pln_AllSrvrs"></a>Planification de la configuration de base de tous les serveurs
Pour chaque serveur dans le réseau de base, vous devez renommer l’ordinateur et affecter et configurer une adresse IPv4 statique et autres propriétés TCP/IP pour l’ordinateur.

#### <a name="planning-naming-conventions-for-computers-and-devices"></a>Planification des conventions d’affectation de noms pour les ordinateurs et périphériques
Par souci de cohérence sur votre réseau, il est judicieux d’utiliser des noms cohérents pour les serveurs, imprimantes et autres périphériques. Noms de l’ordinateur peuvent être utilisés pour aider les utilisateurs et les administrateurs à identifier l’objectif et l’emplacement du serveur, imprimante ou tout autre périphérique. Par exemple, si vous disposez de trois serveurs DNS, un à San Francisco, un à Los Angeles et Orlando, vous pouvez utiliser la convention d’affectation de noms *fonction server*-*emplacement*-*nombre*:

-   DNS-DEN-01. Ce nom représente le serveur DNS de Denver (Colorado). Si les serveurs DNS supplémentaires sont ajoutés à Denver, la valeur numérique dans le nom peut être incrémentée, comme dans le DNS-DEN-02, DNS-DEN-03.

-   SPAS-DNS-01. Ce nom représente le serveur DNS dans South Pasadena (Californie).

-   ORL-DNS-01. Ce nom représente le serveur DNS dans Orlando (Floride).

Pour ce guide, la convention d’affectation de noms de serveur est très simple et se compose de la fonction de serveur principal et un nombre. Par exemple, le contrôleur de domaine nommé DC1 et le serveur DHCP est nommé DHCP1.

Il est recommandé de choisir une convention d’affectation de noms avant d’installer votre réseau de base à l’aide de ce guide.

#### <a name="planning-static-ip-addresses"></a>Planification des adresses IP statiques
Avant de configurer chaque ordinateur avec une adresse IP statique, vous devez planifier vos sous-réseaux et les plages d’adresses IP. En outre, vous devez déterminer les adresses IP de vos serveurs DNS. Si vous prévoyez d’installer un routeur qui fournit l’accès à d’autres réseaux, tels que des sous-réseaux supplémentaires ou Internet, vous devez connaître l’adresse IP du routeur, également appelé passerelle par défaut, pour la configuration d’adresse IP statique.

Le tableau suivant fournit des exemples de valeurs pour la configuration d’adresse IP statique.

|Éléments de configuration|Exemples de valeurs|
|-----------------------|------------------|
|Adresse IP|10.0.0.2|
|Masque de sous-réseau|255.255.255.0|
|Passerelle par défaut (adresse IP du routeur)|10.0.0.1|
|Serveur DNS préféré|10.0.0.2|

> [!NOTE]
> Si vous prévoyez de déployer plusieurs serveurs DNS, vous pouvez également planifier l’adresse IP du serveur DNS autre.

### <a name="bkmk_NetFndtn_Pln_AD-DNS-01"></a>Planification du déploiement de DC1
Voici les étapes de planification clés avant d’installer les Services de domaine ActiveDirectory (ADDS) et DNS sur DC1.

#### <a name="planning-the-name-of-the-forest-root-domain"></a>Planification du nom de domaine racine de forêt
La première étape du processus de conception d’ADDS consiste à déterminer le nombre de forêts votre organisation a besoin. Une forêt est le conteneur ADDS de niveau supérieur et se compose d’un ou plusieurs domaines qui partagent un schéma commun et un catalogue global. Une organisation peut avoir plusieurs forêts, mais la plupart des organisations, un modèle de forêt unique est le modèle par défaut et le plus simple à administrer.

Lorsque vous créez le premier contrôleur de domaine dans votre organisation, vous créez le premier domaine (également appelé le domaine racine de forêt) et la première forêt. Toutefois, avant d’entreprendre cette action à l’aide de ce guide, vous devez déterminer le meilleur nom de domaine pour votre organisation. Dans la plupart des cas, le nom d’organisation est utilisé comme nom de domaine et dans de nombreux cas, ce nom de domaine est enregistré. Si vous prévoyez de déployer externes basés sur Internet pour fournir des informations et des services pour vos clients ou partenaires, choisissez un nom de domaine qui n’est pas déjà en cours d’utilisation et puis inscrire le nom de domaine afin que votre organisation possède les serveurs Web.

#### <a name="planning-the-forest-functional-level"></a>Planification du niveau fonctionnel de forêt
Lors de l’installation des services ADDS, vous devez choisir le niveau fonctionnel de forêt que vous souhaitez utiliser. La fonctionnalité des domaines et forêts, introduite dans Windows Server2003 ActiveDirectory, fournit un moyen d’activer les fonctionnalités ActiveDirectory dans votre environnement réseau domaine ou forêt échelle. Différents niveaux de fonctionnalité du domaine et de la fonctionnalité de la forêt sont disponibles, selon votre environnement.

La fonctionnalité de forêt Active les fonctionnalités dans tous les domaines de votre forêt. Les niveaux fonctionnels de forêt suivants sont disponibles:

-    Windows Server2008. Ce niveau fonctionnel de forêt prend en charge uniquement les contrôleurs de domaine qui exécutent Windows Server2008 et versions ultérieures du système d’exploitation Windows Server.

-    Windows Server2008R2. Ce niveau fonctionnel de forêt prend en charge les contrôleurs de domaine Windows Server2008R2 et les contrôleurs de domaine qui exécutent des versions ultérieures du système d’exploitation Windows Server.

-    Windows Server2012. Ce niveau fonctionnel de forêt prend en charge les contrôleurs de domaine Windows Server2012 et les contrôleurs de domaine qui exécutent des versions ultérieures du système d’exploitation Windows Server.

-    Windows Server2012R2. Ce niveau fonctionnel de forêt prend en charge les contrôleurs de domaine Windows Server2012R2 et les contrôleurs de domaine qui exécutent des versions ultérieures du système d’exploitation Windows Server.

-    Windows Server2016. Ce niveau fonctionnel de forêt prend en charge uniquement les contrôleurs de domaine Windows Server2016 et les contrôleurs de domaine qui exécutent des versions ultérieures du système d’exploitation Windows Server.

Si vous déployez un nouveau domaine dans une nouvelle forêt et tous vos contrôleurs de domaine exécutent Windows Server2016, il est recommandé de configurer les services ADDS avec le niveau fonctionnel de forêt Windows Server2016lors de l’installation des services ADDS.

> [!IMPORTANT]
> Une fois le niveau fonctionnel de forêt est déclenché, les contrôleurs de domaine qui exécutent des systèmes d’exploitation antérieurs ne peuvent pas être inclus dans la forêt. Par exemple, si vous augmentez le niveau fonctionnel de la forêt à Windows Server2016, contrôleurs de domaine exécutant Windows Server2012R2 ou Windows Server2008 ne peut pas être ajoutés à la forêt.

Exemples d’éléments de configuration pour les services ADDS sont fournies dans le tableau suivant.

|Éléments de configuration:|Exemples de valeurs:|
|------------------------|-------------------|
|Nom DNS complet|Exemples:<br /><br />-corp.contoso.com<br />-example.com|
|Niveau fonctionnel de forêt|-Windows Server2008 <br />-Windows Server2008R2 <br />-Windows Server2012 <br />-Windows Server2012R2 <br />-Windows Server2016|
|Emplacement de dossier de base de données des Services de domaine ActiveDirectory|E:\Configuration\\<br /><br />Ou acceptez l’emplacement par défaut.|
|Emplacement du dossier des fichiers journaux de Services de domaine ActiveDirectory|E:\Configuration\\<br /><br />Ou acceptez l’emplacement par défaut.|
|Emplacement du dossier SYSVOL de Services de domaine ActiveDirectory|E:\Configuration\\<br /><br />Ou acceptez l’emplacement par défaut|
|Mot de passe administrateur en Mode de restauration Active|**J\ * p2leO4$ F**|
|Nom de fichier de réponses (facultatif)|**AD DS_AnswerFile**|

#### <a name="planning-dns-zones"></a>Planification des zones DNS
Sur les serveurs DNS principales, intégrées à ActiveDirectory, une zone de recherche directe est créée par défaut lors de l’installation du rôle de serveur DNS. Une zone de recherche directe permet aux ordinateurs et périphériques de rechercher un autre ordinateur ou l’appareil adresse IP en fonction de son nom DNS. En plus de la zone de recherche directe, il est recommandé que vous créez une zone de recherche inversée DNS. Avec un serveur DNS inversée requête, un ordinateur ou un périphérique peut découvrir le nom d’un autre ordinateur ou appareil à l’aide de son adresse IP. Déploiement d’une zone de recherche inversée permet généralement d’améliorer les performances DNS et augmente considérablement le succès des requêtes DNS.

Lorsque vous créez une zone de recherche inversée, le domaine in-addr.arpa, qui est défini dans les normes DNS et réservé dans l’espace de noms DNS Internet pour fournir un moyen fiable et pratique d’effectuer des requêtes inversées, est configuré dans le système DNS. Pour créer l’espace de noms inversé, des sous-domaines au sein du domaine in-addr.arpa sont formés, utilisant le classement inversé des nombres dans la notation décimale séparée par des adresses IP.

Le domaine in-addr.arpa s’applique à tous les réseaux TCP/IP basés sur Internet Protocol version4 (IPv4) d’adressage. L’Assistant Nouvelle Zone suppose automatiquement que vous utilisez ce domaine lorsque vous créez une nouvelle zone de recherche inversée.

Lorsque vous exécutez l’Assistant Nouvelle Zone, les sélections suivantes sont recommandées:

|Éléments de configuration|Exemples de valeurs|
|-----------------------|------------------|
|Type de zone|**Zone principale**, et **enregistrer la zone dans ActiveDirectory** est sélectionné|
|Étendue de la réplication de Zone ActiveDirectory|**Tous les serveurs DNS dans ce domaine**|
|Page de l’Assistant nom première Zone de recherche inversée|**Zone de recherche inversée IPv4**|
|Page de l’Assistant nom seconde Zone de recherche inversée|ID réseau = 10.0.0.|
|Mises à jour dynamiques|**Autoriser uniquement les mises à jour dynamiques sécurisées**|

### <a name="bkmk_NetFndtn_Pln_DomAccess"></a>Planification de l’accès de domaine
Pour vous connecter au domaine, l’ordinateur doit être un ordinateur membre du domaine et le compte d’utilisateur doit être créé dans ADDS avant la tentative d’ouverture de session.

> [!NOTE]
> Les ordinateurs individuels qui exécutent Windows ont local utilisateurs et groupes compte d’utilisateur de base de données qui est appelée à la base de données de comptes utilisateur Gestionnaire de comptes de sécurité (SAM). Lorsque vous créez un compte d’utilisateur sur l’ordinateur local dans la base de données SAM, vous pouvez vous connecter à l’ordinateur local, mais vous ne pouvez pas vous connecter à un domaine. Comptes d’utilisateur de domaine sont créés avec les utilisateurs ActiveDirectory et les ordinateurs MicrosoftManagement Console (MMC) sur un contrôleur de domaine, pas avec les utilisateurs locaux et les groupes sur l’ordinateur local.

Après la première ouverture de session réussie avec les informations d’identification d’ouverture de session de domaine, les paramètres d’ouverture de session sont conservées, sauf si l’ordinateur est supprimé du domaine ou les paramètres d’ouverture de session sont modifiés manuellement.

Avant de vous connecter au domaine:

-   Créer des comptes d’utilisateur dans ActiveDirectory Users and Computers. Chaque utilisateur doit avoir un compte d’utilisateur dans ActiveDirectory utilisateurs et ordinateurs ActiveDirectory Domain Services. Pour plus d’informations, voir [créer un compte d’utilisateur dans ActiveDirectory Users and Computers](#BKMK_createUA).

-   Vérifiez la configuration d’adresse IP correcte. Pour joindre un ordinateur au domaine, l’ordinateur doit disposer d’une adresse IP. Dans ce guide, les serveurs sont configurés avec des adresses IP statiques et les ordinateurs clients reçoivent des baux d’adresses IP à partir du serveur DHCP. Pour cette raison, le serveur DHCP doit être déployé avant de joindre les clients au domaine. Pour plus d’informations, voir [DHCP1 déploiement](#BKMK_deployDHCP01).

-   Joindre l’ordinateur au domaine. N’importe quel ordinateur qui fournit ou accède aux ressources réseau doit être joint au domaine. Pour plus d’informations, voir [joindre des ordinateurs au domaine et ouvrir une session](#BKMK_joinlogserver) et [joindre des ordinateurs clients au domaine et ouvrir une session](#BKMK_joinlogclients).

### <a name="bkmk_NetFndtn_Pln_DHCP-01"></a>Planification du déploiement du serveur DHCP1
Voici les étapes de planification clés avant d’installer le rôle de serveur DHCP sur DHCP1.

#### <a name="planning-dhcp-servers-and-dhcp-forwarding"></a>Planification des serveurs DHCP et l’acheminement DHCP
Les messages DHCP étant des messages de diffusion, ils ne sont pas acheminés entre sous-réseaux par les routeurs. Si vous avez plusieurs sous-réseaux et que vous souhaitez fournir un service DHCP pour chaque sous-réseau, vous devez effectuer l’une des opérations suivantes:

-   Installer un serveur DHCP sur chaque sous-réseau

-   Configurez les routeurs pour transférer des messages de diffusion DHCP entre les sous-réseaux et configurez plusieurs étendues sur le serveur DHCP, une étendue par sous-réseau.

Dans la plupart des cas, la configuration de routeurs pour transférer des messages de diffusion DHCP est plus efficace que le déploiement d’un serveur DHCP sur chaque segment physique du réseau.

#### <a name="planning-ip-address-ranges"></a>Planification des plages d’adresses IP
Chaque sous-réseau doit avoir sa propre plage d’adresses IP unique. Ces plages sont représentées sur un serveur DHCP avec des étendues.

Une étendue est un groupement administratif des adresses IP des ordinateurs sur un sous-réseau qui utilisent le service DHCP. L’administrateur crée d’abord une étendue pour chaque sous-réseau physique, puis utilise l’étendue pour définir les paramètres utilisés par les clients.

Une étendue possède les propriétés suivantes:

-   Une plage d’adresses IP à partir duquel inclure ou exclure les adresses utilisées pour les offres de bail de service DHCP.

-   Un masque de sous-réseau, qui détermine le préfixe de sous-réseau pour une adresse IP donnée.

-   Nom d’étendue attribué lors de sa création.

-   Valeurs de durée de bail, qui sont affectées aux clients DHCP recevant des adresses IP allouées dynamiquement.

-   Des options d’étendue DHCP configurées pour être affectées aux clients DHCP, telles que l’adresse IP du serveur DNS et l’adresse IP de passerelle routeur/par défaut.

-   Réservations sont utilisées si vous le souhaitez pour vous assurer qu’un client DHCP reçoit toujours la même adresse IP.

Avant de déployer vos serveurs, liste de vos sous-réseaux et la plage d’adresses IP à utiliser pour chaque sous-réseau.

#### <a name="planning-subnet-masks"></a>Planification des masques de sous-réseau
ID de réseau et ID d’hôte au sein d’une adresse IP sont distingués à l’aide d’un masque de sous-réseau. Chaque masque de sous-réseau est un nombre 32bits qui utilise des groupes de bits consécutifs de tous les zéros (1) pour identifier le réseau ID et tous les zéros (0) pour identifier les parties d’ID hôte d’une adresse IP.

Par exemple, le masque de sous-réseau normalement utilisé avec l’adresse IP 131.107.16.200 est le nombre binaire de 32bits suivant:

```
11111111 11111111 00000000 00000000
```

Ce numéro de masque de sous-réseau est 16bits de suivi de 16bits de valeur zéro, indiquant que les sections réseau les ID ID et l’hôte de cette adresse IP sont les deux une longueur de 16bits. En règle générale, ce masque de sous-réseau est affiché en notation décimale comme 255.255.0.0.

Le tableau suivant affiche les masques de sous-réseau des classes d’adresses Internet.

|Classe d’adresses|Bits pour le masque de sous-réseau|Masque de sous-réseau|
|-----------------|------------------------|---------------|
|Classe A|11111111 00000000 00000000 00000000|255.0.0.0|
|Classe B|11111111 11111111 00000000 00000000|255.255.0.0|
|Classe C|11111111 11111111 11111111 00000000|255.255.255.0|

Lorsque vous créez une étendue dans DHCP et que vous entrez la plage d’adresses IP pour l’étendue, DHCP fournit ces valeurs de masque de sous-réseau par défaut. En règle générale, les valeurs de masque de sous-réseau par défaut sont acceptables pour la plupart des réseaux avec aucune configuration particulière et où chaque segment de réseau IP correspond à un seul réseau physique.

Dans certains cas, vous pouvez utiliser des masques de sous-réseau personnalisés pour implémenter la mise en sous-réseau IP. Mise en sous-réseau IP, vous pouvez subdiviser la partie ID d’hôte par défaut d’une adresse IP pour spécifier les sous-réseaux, qui sont des sous-divisions de l’ID de réseau basée sur la classe d’origine.

En personnalisant la longueur du masque de sous-réseau, vous pouvez réduire le nombre de bits utilisés pour l’ID d’hôte réel.

Pour éviter les problèmes d’adressage et de routage, vous devez vous assurer que tous les ordinateurs TCP/IP sur un segment réseau utilisent le même masque de sous-réseau et que chaque ordinateur ou périphérique possède une adresse IP unique.

#### <a name="planning-exclusion-ranges"></a>Planification des plages d’exclusion
Lorsque vous créez une étendue sur un serveur DHCP, vous spécifiez une plage d’adresses IP qui inclut toutes les adresses IP que le serveur DHCP est autorisé à louer aux clients DHCP, tels que les ordinateurs et autres périphériques. Si vous passez et configurez manuellement certains serveurs et autres périphériques avec statique des adresses IP à partir de la même plage d’adresses IP qui utilise le serveur DHCP, vous risquez de provoquer un conflit d’adresse IP, où vous et le serveur DHCP ont tous deux affectés la même adresse IP à différents appareils.

Pour résoudre ce problème, vous pouvez créer une plage d’exclusion pour l’étendue DHCP. Une plage d’exclusion est une contigu plage d’adresses IP dans la plage d’adresses IP de l’étendue que le serveur DHCP n’est pas autorisé à utiliser. Si vous créez une plage d’exclusion, le serveur DHCP n’affecte pas les adresses de cette plage, ce qui vous permet d’attribuer manuellement ces adresses sans avoir à créer un conflit d’adresse IP.

Vous pouvez exclure des adresses IP de la distribution par le serveur DHCP en créant une plage d’exclusion pour chaque étendue. Vous devez utiliser des exclusions pour tous les périphériques qui sont configurés avec une adresse IP statique. Les adresses exclues doivent figurer toutes les adresses IP que vous avez attribuées manuellement à d’autres serveurs, les clients non DHCP, stations de travail sans disque ou routage et accès à distance et clients PPP.

Il est recommandé de configurer votre plage d’exclusion avec des adresses supplémentaires pour prendre en compte la croissance future du réseau. Le tableau suivant fournit une plage d’exclusion exemple pour une étendue avec une plage d’adresses IP de 10.0.0.1 - 10.0.0.254 et un masque de sous-réseau de 255.255.255.0.

|Éléments de configuration|Exemples de valeurs|
|-----------------------|------------------|
|Adresse IP de début de la plage d’exclusion|10.0.0.1|
|Adresse IP de fin de la plage d’exclusion|10.0.0.25|

#### <a name="planning-tcpip-static-configuration"></a>Planification de la configuration TCP/IP statique
Certains appareils, tels que les routeurs, les serveurs DHCP et les serveurs DNS, doivent être configurés avec une adresse IP statique. En outre, vous devrez peut-être des périphériques supplémentaires, telles que des imprimantes, que vous souhaitez vous assurer de toujours avoir la même adresse IP. Répertorier les périphériques que vous souhaitez configurer statiquement pour chaque sous-réseau, puis planifiez la plage d’exclusion que vous souhaitez utiliser sur le serveur DHCP pour vous assurer que le serveur DHCP ne loue pas l’adresse IP d’un périphérique configuré de manière statique. Une plage d’exclusion est une séquence limitée d’adresses IP dans une étendue, exclue des offres de service DHCP. Plages d’exclusion s’assurer que toutes les adresses de ces plages ne sont pas proposés par le serveur aux clients DHCP sur votre réseau.

Par exemple, si la plage d’adresses IP pour un sous-réseau est 192.168.0.1 par le biais de 192.168.0.254 et dix périphériques que vous souhaitez configurer avec une adresse IP statique, vous pouvez créer une plage d’exclusion pour le 192.168.0. *x* étendue qui inclut dix ou plusieurs adresses IP: 192.168.0.1 via 192.168.0.15.

Dans cet exemple, vous utilisez dix des adresses IP exclues pour configurer des serveurs et autres périphériques avec des adresses IP statiques et des adresses IP supplémentaires cinq restent disponibles pour la configuration statique de nouveaux périphériques que vous souhaitez ajouter à l’avenir. Avec cette plage d’exclusion, le serveur DHCP est laissé avec un pool d’adresses de 192.168.0.16 via 192.168.0.254.

Vous trouverez des exemples supplémentaires d’éléments de configuration pour les services ADDS et DNS dans le tableau suivant.

|Éléments de configuration|Exemples de valeurs|
|-----------------------|------------------|
|Liaisons de connexion réseau|Ethernet|
|Paramètres du serveur DNS|DC1.corp.contoso.com|
|Adresse IP du serveur DNS préféré|10.0.0.2|
|Ajouter des valeurs de la boîte de dialogue étendue<br /><br />1. Nom de l’étendue de<br />2. Adresse IP de début<br />3. Adresse IP de fin<br />4. Masque de sous-réseau<br />5. (facultative) de la passerelle par défaut<br />6. Durée du bail|1. Sous-réseau principal<br />2.  10.0.0.1<br />3.  10.0.0.254<br />4.  255.255.255.0<br />5.  10.0.0.1<br />6. 8jours|
|Mode d’opération du serveur DHCP IPv6|Non activé|

## <a name="BKMK_deployment"></a>Déploiement de réseau de base
Pour déployer un réseau de base, les étapes de base sont les suivantes:

1.  [La configuration de tous les serveurs](#BKMK_configuringAll)

2.  [Déploiement du serveur DC1](#BKMK_deployADDNS01)

3.  [Jonction des serveurs au domaine et ouverture de session](#BKMK_joinlogserver)

4.  [Déploiement du serveur DHCP1](#BKMK_deployDHCP01)

5.  [Joindre des ordinateurs clients au domaine et ouverture de session](#BKMK_joinlogclients)

6.  [Déploiement des fonctionnalités facultatives pour l’authentification d’accès réseau et les services Web](#BKMK_optionalfeatures)

> [!NOTE]
> -   Les commandes Windows PowerShell équivalentes sont fournies pour la plupart des procédures décrites dans ce guide. Avant d’exécuter ces applets de commande dans Windows PowerShell, remplacez les exemples de valeurs avec les valeurs appropriées pour votre déploiement de réseau. En outre, vous devez entrer chaque applet de commande sur une seule ligne dans Windows PowerShell. Dans ce guide, certaines applets de commande peut apparaître sur plusieurs lignes en raison de contraintes et l’affichage du document de mise en forme par votre navigateur ou une autre application.
> -   Les procédures décrites dans ce guide n’incluent pas les instructions pour les cas dans lesquels le **contrôle de compte d’utilisateur** boîte de dialogue s’ouvre pour demander votre autorisation pour continuer. Si cette boîte de dialogue s’ouvre pendant que vous effectuez les procédures décrites dans ce guide et si la boîte de dialogue a été ouverte en réponse à vos actions, cliquez sur **continuer**.

### <a name="BKMK_configuringAll"></a>La configuration de tous les serveurs
Avant d’installer d’autres technologies, telles que les Services de domaine ActiveDirectory ou DHCP, il est important de configurer les éléments suivants.

-   [Renommer l’ordinateur](#BKMK_rename)

-   [Configurer une adresse IP statique](#BKMK_ip)

Vous pouvez utiliser les sections suivantes pour effectuer ces actions pour chaque serveur.

L’appartenance au groupe **administrateurs**, ou équivalente, est la condition minimale requise pour effectuer ces procédures.

#### <a name="BKMK_rename"></a>Renommer l’ordinateur
Vous pouvez utiliser la procédure de cette section pour modifier le nom d’un ordinateur. Modification du nom de l’ordinateur peut être utile dans lequel le système d’exploitation a été créé automatiquement un nom d’ordinateur que vous ne souhaitez pas utiliser.

> [!NOTE]
> Pour effectuer cette procédure en utilisant Windows PowerShell, ouvrez PowerShell et tapez les applets de commande suivantes sur des lignes distinctes, puis appuyez sur ENTRÉE. Vous devez aussi remplacer *Nom_Ordinateur* avec le nom que vous souhaitez utiliser.
>
> `Rename-Computer`*Nom_Ordinateur*
>
> `Restart-Computer`

###### <a name="to-rename-computers-running-windows-server-2016-windows-server-2012-r2-and-windows-server-2012"></a>Pour renommer des ordinateurs exécutant Windows Server2016, Windows Server2012R2 et Windows Server2012

1.  Dans le Gestionnaire de serveur, cliquez sur **serveur Local**. L’ordinateur **propriétés** sont affichés dans le volet d’informations.

2.  Dans **propriétés**, dans **nom de l’ordinateur**, cliquez sur le nom d’ordinateur existant. Le **propriétés système** boîte de dialogue s’ouvre. Cliquez sur **modification**. Le **modification du nom ou du domaine ordinateur** boîte de dialogue s’ouvre.

3.  Dans le **modification du nom ou du domaine ordinateur** la boîte de dialogue dans **nom de l’ordinateur**, tapez un nouveau nom pour votre ordinateur. Par exemple, si vous souhaitez nommer l’ordinateur DC1, tapez **DC1**.

4.  Cliquez sur **OK** deux fois, puis cliquez sur **fermer**. Si vous souhaitez redémarrer immédiatement l’ordinateur pour terminer la modification du nom, cliquez sur **redémarrer maintenant**. Dans le cas contraire, cliquez sur **redémarrer ultérieurement**.

> [!NOTE]
> Pour plus d’informations sur la façon de renommer des ordinateurs qui exécutent d’autres systèmes d’exploitation de Microsoft, consultez [annexe A - changement de nom des ordinateurs](#BKMK_A).

#### <a name="BKMK_ip"></a>Configurer une adresse IP statique
Vous pouvez utiliser les procédures décrites dans cette rubrique pour configurer Internet Protocol version4 (IPv4) les propriétés d’une connexion réseau avec une adresse IP statique d’adresses pour les ordinateurs exécutant Windows Server2016.

> [!NOTE]
> Pour effectuer cette procédure en utilisant Windows PowerShell, ouvrez PowerShell et tapez les applets de commande suivantes sur des lignes distinctes, puis appuyez sur ENTRÉE. Vous devez également remplacer des noms d’interface et les adresses IP dans cet exemple avec les valeurs que vous souhaitez utiliser pour configurer votre ordinateur.
>
> `New-NetIPAddress -IPAddress 10.0.0.2 -InterfaceAlias "Ethernet" -DefaultGateway 10.0.0.1 -AddressFamily IPv4 -PrefixLength 24`
>
> `Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 127.0.0.1`

###### <a name="to-configure-a-static-ip-address-on-computers-running-windows-server-2016-windows-server-2012-r2-and-windows-server-2012"></a>Pour configurer une adresse IP statique sur les ordinateurs exécutant Windows Server2016, Windows Server2012R2 et Windows Server2012

1.  Dans la barre des tâches, cliquez sur l’icône de réseau, puis cliquez sur **ouvrez Centre réseau et partage**.

2.  Dans **Centre réseau et partage**, cliquez sur **modifier les paramètres de carte**. Le **connexions réseau** dossier s’ouvre et affiche les connexions réseau disponibles.

3.  Dans **connexions réseau**, avec le bouton droit de la connexion que vous souhaitez configurer, puis cliquez sur **propriétés**. La connexion réseau **propriétés** boîte de dialogue s’ouvre.

4.  La connexion réseau **propriétés** la boîte de dialogue dans **cette connexion utilise les éléments suivants**, sélectionnez **Internet Protocol Version4 (TCP/IPv4)**, puis cliquez sur **propriétés**. Le **propriétés de protocole Internet Version4 (TCP/IPv4)** boîte de dialogue s’ouvre.

5.  Dans **propriétés de protocole Internet Version4 (TCP/IPv4)**, dans le **général**, cliquez sur **utiliser l’adresse IP suivante**. Dans **adresse IP**, tapez l’adresse IP que vous souhaitez utiliser.

6.  Appuyez sur tab pour placer le curseur dans **masque de sous-réseau**. Une valeur par défaut pour le masque de sous-réseau est entrée automatiquement. Acceptez le masque de sous-réseau par défaut, ou tapez le masque de sous-réseau que vous souhaitez utiliser.

7.  Dans **passerelle par défaut**, tapez l’adresse IP de votre passerelle par défaut.

    > [!NOTE]
    > Vous devez configurer **passerelle par défaut** avec la même adresse IP que vous utilisez sur l’interface de réseau local (LAN) de votre routeur. Par exemple, si vous avez un routeur qui est connecté à un réseau étendu (WAN) tels que l’Internet et à votre réseau local, configurez l’interface LAN avec la même adresse IP que vous utiliserez le **passerelle par défaut**. Dans un autre exemple, si vous disposez d’un routeur qui est connecté à deux réseaux locaux, où LAN A utilise l’adresse plage 10.0.0.0/24 et LAN B utilise l’adresse plage 192.168.0.0/24, configurer l’adresse IP du routeur LAN A avec une adresse de la plage d’adresses, telles que 10.0.0.1. En outre, dans l’étendue DHCP pour cette plage d’adresses, configurez **passerelle par défaut** avec l’adresse IP 10.0.0.1. Pour le réseau LAN B, configurez l’interface de routeur LAN B avec une adresse de la plage d’adresses, telles que 192.168.0.1, puis configurez le LAN B étendue 192.168.0.0/24 avec un **passerelle par défaut** valeur 192.168.0.1.

8.  Dans **serveur DNS préféré**, tapez l’adresse IP de votre serveur DNS. Si vous prévoyez d’utiliser l’ordinateur local comme serveur DNS préféré, tapez l’adresse IP de l’ordinateur local.

9. Dans **serveur DNS auxiliaire**, tapez l’adresse IP de votre serveur DNS auxiliaire, le cas échéant. Si vous prévoyez d’utiliser l’ordinateur local en tant que serveur DNS auxiliaire, tapez l’adresse IP de l’ordinateur local.

10. Cliquez sur **OK**, puis cliquez sur **fermer**.

> [!NOTE]
> Pour plus d’informations sur la façon de configurer une adresse IP statique sur les ordinateurs qui exécutent d’autres systèmes d’exploitation de Microsoft, consultez [annexe B - configuration des adresses IP statiques](#BKMK_B).

#### <a name="BKMK_deployADDNS01"></a>Déploiement du serveur DC1
Pour déployer le serveur DC1, qui exécute les Services de domaine ActiveDirectory (ADDS) et DNS, vous devez effectuer les étapes dans l’ordre suivant:

-   Effectuez les étapes dans la section [la configuration de tous les serveurs](#BKMK_configuringAll).

-   [Installer les services ADDS et DNS pour une nouvelle forêt](#BKMK_installAD-DNS)

-   [Créer un compte d’utilisateur dans utilisateurs et ordinateurs ActiveDirectory](#BKMK_createUA)

-   [Appartenance au groupe attribuer](#BKMK_assigngroup)

-   [Configurer une Zone de recherche inversée DNS](#BKMK_reverse)

**Privilèges d’administration**

Si vous installez un petit réseau et que vous êtes le seul administrateur pour le réseau, il est recommandé que vous créez un compte d’utilisateur pour vous-même et puis ajoutez votre compte d’utilisateur en tant que membre des administrateurs de l’entreprise et Admins du domaine. Cela rend plus facile à agir en tant que l’administrateur pour toutes les ressources réseau. Il est également recommandé que vous ouvrez une session avec ce compte uniquement lorsque vous avez besoin effectuer des tâches d’administration, et que vous créez un compte d’utilisateur distinct pour l’exécution non informatiques à des tâches.

Si vous disposez d’une grande entreprise avec plusieurs administrateurs, reportez-vous à la documentation des services ADDS pour déterminer l’appartenance au groupe meilleures pour les employés de l’organisation.

**Différences entre les comptes d’utilisateur de domaine et les comptes d’utilisateur sur l’ordinateur local**

Un des avantages d’une infrastructure basée sur le domaine est que vous n’avez pas besoin de créer des comptes d’utilisateur sur chaque ordinateur dans le domaine. Cela est vrai si l’ordinateur est un ordinateur client ou un serveur.

Pour cette raison, vous ne devez pas créer les comptes d’utilisateurs sur chaque ordinateur dans le domaine. Créer tous les comptes d’utilisateur dans ActiveDirectory Users and Computers et utilisez les procédures précédentes pour attribuer l’appartenance au groupe. Par défaut, tous les comptes d’utilisateurs sont membres du groupe utilisateurs du domaine.

Tous les membres du groupe utilisateurs du domaine peuvent se connecter à n’importe quel ordinateur client après que l’avoir joint au domaine.

Vous pouvez configurer des comptes d’utilisateur pour désigner les jours et les heures auxquels l’utilisateur est autorisé à ouvrir une session sur l’ordinateur. Vous pouvez également désigner les ordinateurs de chaque utilisateur est autorisé à utiliser. Pour configurer ces paramètres, ouvrez ActiveDirectory Users and Computers, recherchez le compte d’utilisateur que vous souhaitez configurer et double-cliquez sur le compte. Dans le compte d’utilisateur **propriétés**, cliquez sur le **compte** onglet, puis cliquez sur une **des heures d’ouverture de session** ou **se connecter à**.

#### <a name="BKMK_installAD-DNS"></a>Installer les services ADDS et DNS pour une nouvelle forêt

Vous pouvez utiliser une des procédures suivantes pour installer les Services de domaine ActiveDirectory (ADDS) et DNS et pour créer un nouveau domaine dans une nouvelle forêt. 

La première procédure fournit des instructions sur l’exécution de ces actions à l’aide de Windows PowerShell, alors que la seconde procédure vous montre comment installer les services ADDS et DNS à l’aide du Gestionnaire de serveur.

>[!IMPORTANT]
>Après avoir terminé les étapes de cette procédure, l’ordinateur est redémarré automatiquement.

**Installer les services ADDS et DNS à l’aide de Windows PowerShell**

Vous pouvez utiliser les commandes suivantes pour installer et configurer les services ADDS et DNS. Vous devez remplacer le nom de domaine dans cet exemple par la valeur que vous souhaitez utiliser pour votre domaine.

>[!NOTE]
>Pour plus d’informations sur ces commandes Windows PowerShell, consultez les rubriques de référence.
>- [Install-WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/server-manager/install-windowsfeature)
>- [Install-ADDSForest](https://technet.microsoft.com/itpro/powershell/windows/adds/deployment/install-addsforest)

L’appartenance au groupe **administrateurs** est la condition minimale requise pour effectuer cette procédure.

- Exécutez Windows PowerShell en tant qu’administrateur, tapez la commande suivante et appuyez sur ENTRÉE:  

`Install-WindowsFeature AD-Domain-Services -IncludeManagementTools`

Lorsque l’installation est terminée avec succès, le message suivant s’affiche dans Windows PowerShell.

    
    Success Restart Needed  Exit Code   Feature Result
    ------- --------------  ---------   --------------
    True    No              Success     {Active Directory Domain Services, Group P...
    

- Dans Windows PowerShell, tapez la commande suivante, en remplaçant le texte **corp.contoso.com** avec votre nom de domaine et appuyez sur ENTRÉE:

````
Install-ADDSForest -DomainName "corp.contoso.com"
````

- Pendant l’installation et la configuration, qui est visible en haut de la fenêtre Windows PowerShell, le message suivant s’affiche. Une fois qu’il s’affiche, tapez un mot de passe et appuyez sur ENTRÉE.

    **SafeModeAdministratorPassword:**

- Une fois que vous tapez un mot de passe et appuyez sur entrée, l’invite de confirmation suivant s’affiche. Tapez le mot de passe et appuyez sur ENTRÉE.

    **Vérifiez SafeModeAdministratorPassword:**

- Lorsque le message suivant s’affiche, tapez la lettre **Y** et appuyez sur ENTRÉE.

    
    Le serveur cible être configuré comme contrôleur de domaine et redémarré lorsque cette opération est terminée.
    Vous souhaitez poursuivre l’opération?
    [Y] Oui [T] Oui pour tous les [N] aucune [L] non pour tous les [S] ne Suspend [?] Aide (valeur par défaut est «Y»):
    
- Si vous le souhaitez, vous pouvez lire les messages d’avertissement qui sont affichent lors de l’installation normale, réussie des services ADDS et DNS. Ces messages sont normales et ne sont pas une indication de l’échec d’installation.

- Une fois l’installation réussit, un message s’affiche indiquant que vous êtes sur le point d’être déconnectés de l’ordinateur afin que l’ordinateur peut redémarrer. Si vous cliquez sur **fermer**, vous êtes déconnecté immédiatement de l’ordinateur et l’ordinateur redémarre. Si vous ne cliquez pas sur **fermer**, l’ordinateur redémarre après une période par défaut.

- Une fois que le serveur est redémarré, vous pouvez vérifier la réussite de l’installation des Services de domaine ActiveDirectory et DNS. Ouvrez Windows PowerShell, tapez la commande suivante et appuyez sur ENTRÉE.

````
Get-WindowsFeature
````

Les résultats de cette commande sont affichés dans Windows PowerShell et doivent être similaires aux résultats dans l’image ci-dessous. Pour les technologies installés, les crochets à gauche du nom de la technologie contient le caractère **X**et la valeur de **état d’installation** est **installé**.

![Résultats de la commande Get-WindowsFeature](../media/Core-Network-Guide/server-roles-installed.jpg)

**Installer les services ADDS et DNS à l’aide du Gestionnaire de serveur**

1.  Sur DC1, dans **le Gestionnaire de serveur**, cliquez sur **gérer**, puis cliquez sur **Ajout de rôles et fonctionnalités**. L’Assistant Ajouter des rôles et fonctionnalités s’ouvre.

2.  Dans **avant de commencer**, cliquez sur **suivant**.

    > [!NOTE]
    > Le **avant de commencer** page de l’ajout de rôles et fonctionnalités Assistant ne s’affiche pas si vous avez sélectionné précédemment **ignorer cette page par défaut** lorsque l’ajout de rôles et fonctionnalités Assistant a été exécuté.

3.  Dans **sélectionner le Type d’Installation**, assurez-vous que **installation basée sur un rôle ou une fonctionnalité** est sélectionné, puis cliquez sur **suivant**.

4.  Dans **serveur de destination sélectionnez**, assurez-vous que **sélectionner un serveur du pool de serveurs** est sélectionné. Dans **Pool de serveurs**, vérifiez que l’ordinateur local est sélectionné. Cliquez sur **suivant**.

5.  Dans **sélectionner des rôles de serveur**, dans **rôles**, cliquez sur **les Services de domaine ActiveDirectory**. Dans **ajouter des fonctionnalités qui sont requises pour les Services de domaine ActiveDirectory**, cliquez sur **ajouter des fonctionnalités**. Cliquez sur **suivant**.

6.  Dans **sélectionner des fonctionnalités**, cliquez sur **suivant**et dans **les Services de domaine ActiveDirectory**, passez en revue les informations qui sont fournies, puis cliquez sur **suivant**.

7.  Dans **confirmer les sélections d’installation**, cliquez sur **installer**. La page de progression Installation affiche l’état pendant le processus d’installation. Lorsque le processus terminé, dans les détails du message, cliquez sur **promouvoir ce serveur en contrôleur de domaine**. L’Assistant Configuration des Services de domaine ActiveDirectory s’ouvre.

8.  Dans **Configuration de déploiement**, sélectionnez **ajouter une nouvelle forêt**. Dans **nom de domaine racine**, tapez le nom de domaine complet (FQDN) pour votre domaine. Par exemple, si votre nom de domaine complet est corp.contoso.com, tapez **corp.contoso.com**. Cliquez sur **suivant**.

9. Dans **Options du contrôleur de domaine**, dans **sélectionner le niveau fonctionnel de la nouvelle forêt et le domaine racine**, sélectionnez le niveau fonctionnel de forêt et le niveau fonctionnel du domaine que vous souhaitez utiliser. Dans **spécifier des fonctionnalités de contrôleur de domaine**, assurez-vous que **serveur du système DNS (Domain Name)** et **catalogue Global (GC)** sont sélectionnés. Dans **mot de passe** et **confirmer le mot de passe**, tapez le mot de passe en Mode de restauration des Services annuaire (DSRM) que vous souhaitez utiliser. Cliquez sur **suivant**.

10. Dans **Options DNS**, cliquez sur **suivant**.

11. Dans **Options supplémentaires**, vérifiez le nom NetBIOS qui est attribué au domaine et modifiez-le si nécessaire. Cliquez sur **suivant**.

12. Dans **chemins d’accès**, dans **spécifier l’emplacement de la base de données ADDS, les fichiers journaux et SYSVOL**, effectuez l’une des opérations suivantes:

    -   Acceptez les valeurs par défaut.

    -   Tapez les emplacements de dossier que vous souhaitez utiliser pour **dossier de base de données**, **dossier des fichiers journaux**, et **dossier SYSVOL**.

13. Cliquez sur **suivant**.

14. Dans **passer en revue les Options**, passez en revue vos sélections.

15. Si vous souhaitez exporter les paramètres vers un script Windows PowerShell, cliquez sur **afficher le script**. Le script s’ouvre dans le bloc-notes, et vous pouvez l’enregistrer à l’emplacement du dossier que vous souhaitez. Cliquez sur **suivant**. Dans **vérification des conditions requises**, vos sélections sont validées. Lorsque la vérification est terminée, cliquez sur **installer**. Lorsque vous y êtes invité par Windows, cliquez sur **fermer**. Le serveur redémarre pour terminer l’installation des services ADDS et DNS.

16. Pour vérifier la réussite de l’installation, affichez la console du Gestionnaire de serveur après le redémarrage du serveur. Les services ADDS et DNS doit apparaître dans le volet gauche, comme les éléments en surbrillance dans l’image ci-dessous.

![Les services ADDS et DNS dans le Gestionnaire de serveur](../media/Core-Network-Guide/server-roles-installed-sm.jpg)

##### <a name="BKMK_createUA"></a>Créer un compte d’utilisateur dans utilisateurs et ordinateurs ActiveDirectory
Vous pouvez utiliser cette procédure pour créer un nouveau compte d’utilisateur de domaine dans ActiveDirectory utilisateurs et ordinateurs MicrosoftManagement Console (MMC).

L’appartenance au groupe **Admins du domaine**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.

> [!NOTE]
> Pour effectuer cette procédure en utilisant Windows PowerShell, ouvrez PowerShell et tapez l’applet de commande suivante sur une seule ligne, puis appuyez sur ENTRÉE. Vous devez aussi remplacer le nom de compte d’utilisateur dans cet exemple avec la valeur que vous souhaitez utiliser.
>
> `New-ADUser -SamAccountName User1 -AccountPassword (read-host "Set user password" -assecurestring) -name "User1" -enabled $true -PasswordNeverExpires $true -ChangePasswordAtLogon $false`
>
> Une fois que vous appuyez sur entrée, tapez le mot de passe pour le compte d’utilisateur. Le compte est créé et, par défaut, est accordé l’appartenance au groupe utilisateurs du domaine.
>
> Avec l’applet de commande suivante, vous pouvez affecter des appartenances aux groupes supplémentaires pour le nouveau compte d’utilisateur. L’exemple suivant ajoute User1 aux groupes Admins du domaine et administrateurs de l’entreprise. Vérifiez avant d’exécuter cette commande que vous modifiez le nom de compte d’utilisateur, nom de domaine et groupes à vos besoins spécifiques.
>
> `Add-ADPrincipalGroupMembership -Identity "CN=User1,CN=Users,DC=corp,DC=contoso,DC=com" -MemberOf "CN=Enterprise Admins,CN=Users,DC=corp,DC=contoso,DC=com","CN=Domain Admins,CN=Users,DC=corp,DC=contoso,DC=com"`

###### <a name="to-create-a-user-account"></a>Pour créer un compte d’utilisateur

1.  Sur DC1, dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **ActiveDirectory Users and Computers**. L’ActiveDirectory MMC Utilisateurs et ordinateurs s’ouvre. Si elle n’est pas déjà sélectionnée, cliquez sur le nœud de votre domaine. Par exemple, si votre domaine corp.contoso.com, cliquez sur **corp.contoso.com**.

2.  Dans le volet d’informations, cliquez sur le dossier dans lequel vous souhaitez ajouter un compte d’utilisateur.

    **Où?**

    -   Les utilisateurs et ordinateurs Active /*nœud de domaine*/*dossier*

3.  Pointez sur **New**, puis cliquez sur **utilisateur**. Le **nouvel objet - utilisateur** boîte de dialogue s’ouvre.

4.  Dans **prénom**, tapez le prénom de l’utilisateur.

5.  Dans **initiales**, tapez les initiales de l’utilisateur.

6.  Dans **nom**, tapez le nom de l’utilisateur.

7.  Modifier **nom complet** pour ajouter des initiales ou inverser l’ordre du nom et prénom.

8.  Dans **nom d’utilisateur d’ouverture de session**, tapez le nom d’ouverture de session utilisateur. Cliquez sur **suivant**.

9. Dans **nouvel objet - utilisateur**, dans **mot de passe** et **confirmer le mot de passe**, tapez le mot de passe de l’utilisateur, puis sélectionnez les options de mot de passe approprié.

10. Cliquez sur **suivant**, passez en revue les nouveaux paramètres de compte d’utilisateur, puis cliquez sur **Terminer**.

##### <a name="BKMK_assigngroup"></a>Appartenance au groupe attribuer
Vous pouvez utiliser cette procédure pour ajouter un utilisateur, un ordinateur ou un groupe à un groupe dans ActiveDirectory utilisateurs et ordinateurs MicrosoftManagement Console (MMC).

L’appartenance au groupe **Admins du domaine**, ou équivalent est le minimum requis pour effectuer cette procédure.

###### <a name="to-assign-group-membership"></a>Pour attribuer l’appartenance au groupe

1.  Sur DC1, dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **ActiveDirectory Users and Computers**. L’ActiveDirectory MMC Utilisateurs et ordinateurs s’ouvre. Si elle n’est pas déjà sélectionnée, cliquez sur le nœud de votre domaine. Par exemple, si votre domaine corp.contoso.com, cliquez sur **corp.contoso.com**.

2.  Dans le volet d’informations, double-cliquez sur le dossier qui contient le groupe auquel vous souhaitez ajouter un membre.

    Où?

    -   **Les utilisateurs et ordinateurs Active**/*nœud de domaine*/*dossier qui contient le groupe*

3.  Dans le volet d’informations, cliquez sur l’objet que vous souhaitez ajouter à un groupe, tel qu’un utilisateur ou un ordinateur, puis cliquez sur **propriétés**. L’objet **propriétés** boîte de dialogue s’ouvre. Cliquez sur le **membre** onglet.

4.  Sur le **membre**, cliquez sur **ajouter**.

5.  Dans **Entrez les noms des objets à sélectionner**, tapez le nom du groupe auquel vous souhaitez ajouter l’objet, puis cliquez sur **OK**.

6.  Pour attribuer l’appartenance au groupe à d’autres utilisateurs, groupes ou des ordinateurs, répétez les étapes4 et 5 de cette procédure.

##### <a name="BKMK_reverse"></a>Configurer une Zone de recherche inversée DNS
Vous pouvez utiliser cette procédure pour configurer une zone de recherche inversée dans système DNS (Domain Name).

L’appartenance au groupe **Admins du domaine** est la condition minimale requise pour effectuer cette procédure.

> [!NOTE]
> -   Pour les moyennes et grandes organisations, il est recommandé de configurer et utiliser le groupe DNSAdmins dans ActiveDirectory Users and Computers. Pour plus d’informations, voir [ressources techniques supplémentaires](#BKMK_resources)
> -   Pour effectuer cette procédure en utilisant Windows PowerShell, ouvrez PowerShell et tapez l’applet de commande suivante sur une seule ligne, puis appuyez sur ENTRÉE. Vous devez aussi remplacer les recherche inversée zone et fichier noms DNS dans cet exemple avec les valeurs que vous souhaitez utiliser. Assurez-vous que vous inversez l’ID de réseau pour le nom de la zone de recherche inversée. Par exemple, si l’ID de réseau est 192.168.0, créez le nom de zone de recherche inversée **0.168.192.in-addr.arpa**.
>
> `Add-DnsServerPrimaryZone 0.0.10.in-addr.arpa -ZoneFile 0.0.10.in-addr.arpa.dns`

###### <a name="to-configure-a-dns-reverse-lookup-zone"></a>Pour configurer une zone de recherche inversée DNS

1.  Sur DC1, dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **DNS**. La console MMC DNS s’ouvre.

2.  Dans DNS, si ce n’est pas déjà fait, double-cliquez sur le nom du serveur pour développer l’arborescence. Par exemple, si le nom du serveur DNS s’appelle DC1, double-cliquez sur **DC1**.

3.  Sélectionnez **Zones de recherche inversée**, avec le bouton droit **Zones de recherche inversée**, puis cliquez sur **nouvelle Zone**. L’Assistant Nouvelle Zone s’ouvre.

4.  Dans **Bienvenue dans l’Assistant Nouvelle Zone**, cliquez sur **suivant**.

5.  Dans **Type de Zone**, sélectionnez **zone principale**.

6.  Si votre serveur DNS est un contrôleur de domaine accessible en écriture, vérifiez que **enregistrer la zone dans ActiveDirectory** est sélectionné. Cliquez sur **suivant**.

7.  Dans **étendue de la réplication ActiveDirectory Zone**, sélectionnez **à tous les serveurs DNS en cours d’exécution sur les contrôleurs de domaine dans ce domaine**, sauf si vous avez une raison spécifique pour choisir une autre option. Cliquez sur **suivant**.

8.  Dans la première **nom de Zone de recherche inversée** page, sélectionnez **Zone de recherche inversée IPv4**. Cliquez sur **suivant**.

9. Dans la seconde **nom de Zone de recherche inversée** page, effectuez l’une des opérations suivantes:

    -   Dans **ID réseau**, tapez l’ID réseau de votre plage d’adresses IP. Par exemple, si votre plage d’adresses IP est 10.0.0.1 par le biais de 10.0.0.254, tapez **10.0.0**.

    -   Dans **nom de zone de recherche inversée**, votre nom de zone de recherche inversée IPv4 est automatiquement ajouté. Cliquez sur **suivant**.

10. Dans **mise à jour dynamique**, sélectionnez le type de mises à jour dynamiques que vous voulez autoriser. Cliquez sur **suivant**.

11. Dans **fin de l’Assistant Nouvelle Zone**, passez en revue vos choix, puis cliquez sur **Terminer**.

#### <a name="BKMK_joinlogserver"></a>Jonction des serveurs au domaine et ouverture de session
Après avoir installé les Services de domaine ActiveDirectory (ADDS) et créé un ou plusieurs comptes d’utilisateur disposant d’autorisations pour joindre un ordinateur au domaine, vous pouvez joindre des serveurs de réseau de base au domaine et ouvrez une session sur les serveurs afin d’installer d’autres technologies, telles que DHCP Dynamic Host Configuration Protocol ().

Sur tous les serveurs que vous déployez, à l’exception du serveur exécutant les services ADDS, procédez comme suit:

1.  Effectuez les procédures indiquées dans [la configuration de tous les serveurs](#BKMK_configuringAll).

2.  Utilisez les instructions dans les deux procédures suivantes pour joindre vos serveurs au domaine et ouvrir une session sur les serveurs pour effectuer des tâches de déploiement supplémentaires:

> [!NOTE]
> Pour effectuer cette procédure en utilisant Windows PowerShell, ouvrez PowerShell et tapez l’applet de commande suivante et appuyez sur ENTRÉE. Vous devez aussi remplacer le nom de domaine avec le nom que vous souhaitez utiliser.
>
> `Add-Computer -DomainName corp.contoso.com`
>
> Lorsque vous êtes invité à le faire, tapez le nom d’utilisateur et mot de passe d’un compte qui a l’autorisation de joindre un ordinateur au domaine. Pour redémarrer l’ordinateur, tapez la commande suivante et appuyez sur ENTRÉE.
>
> `Restart-Computer`

###### <a name="to-join-computers-running--windows-server-2016--windows-server-2012-r2--and--windows-server-2012--to-the-domain"></a>Pour joindre des ordinateurs exécutant Windows Server2016, Windows Server2012R2 et Windows Server2012 au domaine

1.  Dans le Gestionnaire de serveur, cliquez sur **serveur Local**. Dans le volet d’informations, cliquez sur **groupe de travail**. Le **propriétés système** boîte de dialogue s’ouvre.

2.  Dans le **propriétés système** boîte de dialogue, cliquez sur **modification**. Le **modification du nom ou du domaine ordinateur** boîte de dialogue s’ouvre.

3.  Dans **nom de l’ordinateur**, dans **membre**, cliquez sur **domaine**, puis tapez le nom du domaine que vous voulez joindre. Par exemple, si le nom de domaine est corp.contoso.com, tapez **corp.contoso.com**.

4.  Cliquez sur **OK**. Le **sécurité Windows** boîte de dialogue s’ouvre.

5.  Dans **modification du nom ou du domaine ordinateur**, dans **nom d’utilisateur**, tapez le nom d’utilisateur et dans **mot de passe**, tapez le mot de passe, puis cliquez sur **OK**. Le **modification du nom ou du domaine ordinateur** boîte de dialogue s’ouvre, pour vous accueillir dans le domaine. Cliquez sur **OK**.

6.  Le **modification du nom ou du domaine ordinateur** boîte de dialogue affiche un message indiquant que vous devez redémarrer l’ordinateur pour appliquer les modifications. Cliquez sur **OK**.

7.  Sur le **propriétés système** boîte de dialogue le **nom de l’ordinateur**, cliquez sur **fermer**. Le **MicrosoftWindows** boîte de dialogue s’ouvre et affiche un message indiquant de nouveau que vous devez redémarrer l’ordinateur pour appliquer les modifications. Cliquez sur **redémarrer maintenant**.

> [!NOTE]
> Pour plus d’informations sur la façon de joindre des ordinateurs qui exécutent d’autres systèmes d’exploitation de Microsoft au domaine, voir [annexe C - joindre des ordinateurs au domaine](#BKMK_C).

###### <a name="to-log-on-to-the-domain-using-computers-running-windows-server-2016"></a>Pour ouvrir une session sur le domaine à l’aide des ordinateurs exécutant Windows Server2016

1.  Fermez la session, ou redémarrer l’ordinateur.

2.  Appuyez sur CTRL + ALT + SUPPR. L’écran d’ouverture de session s’affiche.

3.  Dans l’angle inférieur gauche, cliquez sur **autre utilisateur**.

4.  Dans **nom d’utilisateur**, tapez votre nom d’utilisateur.

5.  Dans **mot de passe**, tapez votre mot de passe de domaine, puis cliquez sur la flèche, ou appuyez sur ENTRÉE.

> [!NOTE]
> Pour plus d’informations sur la façon de se connecter au domaine à partir d’ordinateurs exécutant d’autres systèmes d’exploitation de Microsoft, consultez [annexe D - ouvrez une session sur le domaine](#BKMK_D).

#### <a name="BKMK_deployDHCP01"></a>Déploiement du serveur DHCP1
Avant de déployer ce composant sur le réseau de base, vous devez procédez comme suit:

-   Effectuez les étapes dans la section [la configuration de tous les serveurs](#BKMK_configuringAll).

-   Effectuez les étapes dans la section [joindre des ordinateurs au domaine et ouvrir une session](#BKMK_joinlogserver).

Pour déployer le serveur DHCP1, qui est l’ordinateur exécutant le rôle de serveur Dynamic Host Configuration Protocol (DHCP), vous devez effectuer les étapes dans l’ordre suivant:

-   [Installer Dynamic Host Configuration Protocol (DHCP)](#BKMK_installDHCP)

-   [Créer et activer une étendue DHCP](#BKMK_newscopeDHCP)

> [!NOTE]
> Pour effectuer ces procédures à l’aide de Windows PowerShell, ouvrez PowerShell et tapez les applets de commande suivantes sur des lignes distinctes, puis appuyez sur ENTRÉE. Vous devez également remplacer le nom de l’étendue, adresses IP de début et fin de plage, le masque de sous-réseau et autres valeurs dans cet exemple par les valeurs que vous souhaitez utiliser.
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
Vous pouvez utiliser cette procédure pour installer et configurer le rôle de serveur DHCP à l’aide de l’ajout de rôles et fonctionnalités Assistant.

L’appartenance au groupe **Admins du domaine**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.

###### <a name="to-install-dhcp"></a>Pour installer DHCP

1.  Sur DHCP1, dans le Gestionnaire de serveur, cliquez sur **gérer**, puis cliquez sur **Ajout de rôles et fonctionnalités**. L’Assistant Ajouter des rôles et fonctionnalités s’ouvre.

2.  Dans **avant de commencer**, cliquez sur **suivant**.

    > [!NOTE]
    > Le **avant de commencer** page de l’ajout de rôles et fonctionnalités Assistant ne s’affiche pas si vous avez sélectionné précédemment **ignorer cette page par défaut** lorsque l’ajout de rôles et fonctionnalités Assistant a été exécuté.

3.  Dans **sélectionner le Type d’Installation**, assurez-vous que **installation basée sur un rôle ou une fonctionnalité** est sélectionné, puis cliquez sur **suivant**.

4.  Dans **serveur de destination sélectionnez**, assurez-vous que **sélectionner un serveur du pool de serveurs** est sélectionné. Dans **Pool de serveurs**, vérifiez que l’ordinateur local est sélectionné. Cliquez sur **suivant**.

5.  Dans **sélectionner des rôles de serveur**, dans **rôles**, sélectionnez **serveur DHCP**. Dans **ajouter des fonctionnalités qui sont requises pour le serveur DHCP**, cliquez sur **ajouter des fonctionnalités**. Cliquez sur **suivant**.

6.  Dans **sélectionner des fonctionnalités**, cliquez sur **suivant**et dans **serveur DHCP**, passez en revue les informations qui sont fournies, puis cliquez sur **suivant**.

7.  Dans **confirmer les sélections d’installation**, cliquez sur **redémarrer automatiquement le serveur de destination si nécessaire**. Lorsque vous êtes invité à confirmer ce choix, cliquez sur **Oui**, puis cliquez sur **installer**. Le **progression de l’Installation** page affiche l’état pendant le processus d’installation. Lorsque le processus terminé, le message «Configuration requise. Installation réussie sur *Nom_Ordinateur*» s’affiche, où *Nom_Ordinateur* est le nom de l’ordinateur sur lequel vous avez installé le serveur DHCP. Dans la fenêtre de message, cliquez sur **terminer la configuration DHCP**. L’Assistant de configuration DHCP après Post-Install s’ouvre. Cliquez sur **suivant**.

8.  Dans **autorisation**, spécifiez les informations d’identification que vous souhaitez utiliser pour autoriser le serveur DHCP dans les Services de domaine ActiveDirectory, puis cliquez sur **valider**. Une fois l’autorisation terminée, cliquez sur **fermer**.

##### <a name="BKMK_newscopeDHCP"></a>Créer et activer une étendue DHCP
Vous pouvez utiliser cette procédure pour créer une nouvelle étendue DHCP à l’aide de la Console MMC (MicrosoftManagement Console) DHCP. Lorsque vous effectuez la procédure, l’étendue est activée et la plage d’exclusion que vous créez empêche le serveur DHCP de louer les adresses IP que vous utilisez pour configurer de manière statique vos serveurs et autres périphériques qui requièrent une adresse IP statique.

L’appartenance au groupe **Administrateurs DHCP**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.

###### <a name="to-create-and-activate-a-new-dhcp-scope"></a>Pour créer et activer une étendue DHCP

1.  Sur DHCP1, dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **DHCP**. La console MMC DHCP s’ouvre.

2.  Dans **DHCP**, développez le nom du serveur. Par exemple, si le nom du serveur DHCP DHCP1.corp.contoso.com, cliquez sur la flèche vers le bas à côté **DHCP1.corp.contoso.com**.

3.  Sous le nom de serveur avec le bouton droit **IPv4**, puis cliquez sur **nouvelle étendue**. L’Assistant Nouvelle étendue s’ouvre.

4.  Dans **Bienvenue dans l’Assistant Nouvelle étendue**, cliquez sur **suivant**.

5.  Dans **nom de l’étendue**, dans **nom**, tapez un nom pour l’étendue. Par exemple, tapez **sous-réseau 1**.

6.  Dans **Description**, tapez une description de la nouvelle étendue, puis cliquez sur **suivant**.

7.  Dans **plage d’adresses IP**, procédez comme suit:

    1.  Dans **adresse IP de début**, tapez l’adresse IP qui est la première adresse IP dans la plage. Par exemple, tapez **10.0.0.1**.

    2.  Dans **adresse IP de fin**, tapez l’adresse IP qui est la dernière adresse IP dans la plage. Par exemple, tapez **10.0.0.254**. Les valeurs pour **longueur** et **masque de sous-réseau** sont entrées automatiquement, en fonction de l’adresse IP que vous avez entré pour **adresse IP de début**.

    3.  Si nécessaire, modifiez les valeurs dans **longueur** ou **masque de sous-réseau**, selon les besoins de votre schéma d’adressage.

    4.  Cliquez sur **suivant**.

8.  Dans **Ajout d’Exclusions**, procédez comme suit:

    1.  Dans **adresse IP de début**, tapez l’adresse IP qui est la première adresse IP dans la plage d’exclusion. Par exemple, tapez **10.0.0.1**.

    2.  Dans **adresse IP de fin**, tapez l’adresse IP qui est l’adresse IP dernière adresse dans la plage d’exclusion, par exemple, tapez **10.0.0.15**.

9. Cliquez sur **ajouter**, puis cliquez sur **suivant**.

10. Dans **durée du bail**, modifiez les valeurs par défaut **jours**, **heures**, et **Minutes**, de manière appropriée pour votre réseau, puis cliquez sur **suivant**.

11. Dans **configurer les Options DHCP**, sélectionnez **Oui, je veux configurer ces options maintenant**, puis cliquez sur **suivant**.

12. Dans **routeur (passerelle par défaut)**, effectuez l’une des opérations suivantes:

    -   Si vous n’avez pas de routeurs sur votre réseau, cliquez sur **suivant**.

    -   Dans **adresse IP**, tapez l’adresse IP de votre routeur ou passerelle par défaut. Par exemple, tapez **10.0.0.1**. Cliquez sur **ajouter**, puis cliquez sur **suivant**.

13. Dans **nom de domaine et serveurs DNS**, procédez comme suit:

    1.  Dans **domaine Parent**, tapez le nom de domaine DNS que les clients utilisent pour la résolution de noms. Par exemple, tapez **corp.contoso.com**.

    2.  Dans **nom du serveur**, tapez le nom de l’ordinateur DNS que les clients utilisent pour la résolution de noms. Par exemple, tapez **DC1**.

    3.  Cliquez sur **résoudre**. L’adresse IP du serveur DNS est ajouté dans **adresse IP**. Cliquez sur **ajouter**, attendez la fin de la validation adresse IP du serveur DNS terminer, puis cliquez sur **suivant**.

14. Dans **serveurs WINS**, car vous n’avez pas de serveurs WINS sur votre réseau, cliquez sur **suivant**.

15. Dans **activer l’étendue**, sélectionnez **Oui, je veux activer cette étendue maintenant**.

16. Cliquez sur **suivant**, puis cliquez sur **Terminer**.

> [!IMPORTANT]
> Pour créer de nouvelles étendues pour des sous-réseaux supplémentaires, répétez cette procédure. Utilisez une plage d’adresses IP différente pour chaque sous-réseau que vous envisagez de déployer et assurez-vous que le transfert des messages DHCP est activé sur tous les routeurs d’accès à d’autres sous-réseaux.

### <a name="BKMK_joinlogclients"></a>Joindre des ordinateurs clients au domaine et ouverture de session

> [!NOTE]
> Pour effectuer cette procédure en utilisant Windows PowerShell, ouvrez PowerShell et tapez l’applet de commande suivante et appuyez sur ENTRÉE. Vous devez aussi remplacer le nom de domaine avec le nom que vous souhaitez utiliser.
>
> `Add-Computer -DomainName corp.contoso.com`
>
> Lorsque vous êtes invité à le faire, tapez le nom d’utilisateur et mot de passe d’un compte qui a l’autorisation de joindre un ordinateur au domaine. Pour redémarrer l’ordinateur, tapez la commande suivante et appuyez sur ENTRÉE.
>
> `Restart-Computer`

##### <a name="to-join-computers-running-windows-10-to-the-domain"></a>Pour joindre des ordinateurs exécutant Windows10 au domaine

1.  Ouvrez une session sur l’ordinateur avec le compte administrateur local.

2.  Dans **recherches sur le web et Windows**, type **système**. Dans les résultats de recherche, cliquez sur **système (le panneau de configuration)**. Le **système** boîte de dialogue s’ouvre.

3.  Dans **système**, cliquez sur **paramètres système avancés**. Le **propriétés système** boîte de dialogue s’ouvre. Cliquez sur le **nom de l’ordinateur** onglet.

4.  Dans **nom de l’ordinateur**, cliquez sur **modification**. Le **modification du nom ou du domaine ordinateur** boîte de dialogue s’ouvre.

5.  Dans **modification du nom ou du domaine ordinateur**, dans **membre**, cliquez sur **domaine**, puis tapez le nom du domaine auquel joindre l’ordinateur. Par exemple, si le nom de domaine est corp.contoso.com, tapez **corp.contoso.com**.

6.  Cliquez sur **OK**. Le **sécurité Windows** boîte de dialogue s’ouvre.

7.  Dans **modification du nom ou du domaine ordinateur**, dans **nom d’utilisateur**, tapez le nom d’utilisateur et dans **mot de passe**, tapez le mot de passe, puis cliquez sur **OK**. Le **modification du nom ou du domaine ordinateur** boîte de dialogue s’ouvre, pour vous accueillir dans le domaine. Cliquez sur **OK**.

8.  Le **modification du nom ou du domaine ordinateur** boîte de dialogue affiche un message indiquant que vous devez redémarrer l’ordinateur pour appliquer les modifications. Cliquez sur **OK**.

9. Sur le **propriétés système** boîte de dialogue le **nom de l’ordinateur**, cliquez sur **fermer**. Le **MicrosoftWindows** boîte de dialogue s’ouvre et affiche un message indiquant de nouveau que vous devez redémarrer l’ordinateur pour appliquer les modifications. Cliquez sur **redémarrer maintenant**.

##### <a name="to-join-computers-running-windows-81-to-the-domain"></a>Pour joindre des ordinateurs exécutant Windows8.1 au domaine

1.  Ouvrez une session sur l’ordinateur avec le compte administrateur local.

2.  Avec le bouton droit **Démarrer**, puis cliquez sur **système**. Le **système** boîte de dialogue s’ouvre.

3.  Dans **système**, cliquez sur **paramètres système avancés**. Le **propriétés système** boîte de dialogue s’ouvre. Cliquez sur le **nom de l’ordinateur** onglet.

4.  Dans **nom de l’ordinateur**, cliquez sur **modification**. Le **modification du nom ou du domaine ordinateur** boîte de dialogue s’ouvre.

5.  Dans **modification du nom ou du domaine ordinateur**, dans **membre**, cliquez sur **domaine**, puis tapez le nom du domaine auquel joindre l’ordinateur. Par exemple, si le nom de domaine est corp.contoso.com, tapez **corp.contoso.com**.

6.  Cliquez sur **OK**. Le **sécurité Windows** boîte de dialogue s’ouvre.

7.  Dans **modification du nom ou du domaine ordinateur**, dans **nom d’utilisateur**, tapez le nom d’utilisateur et dans **mot de passe**, tapez le mot de passe, puis cliquez sur **OK**. Le **modification du nom ou du domaine ordinateur** boîte de dialogue s’ouvre, pour vous accueillir dans le domaine. Cliquez sur **OK**.

8.  Le **modification du nom ou du domaine ordinateur** boîte de dialogue affiche un message indiquant que vous devez redémarrer l’ordinateur pour appliquer les modifications. Cliquez sur **OK**.

9. Sur le **propriétés système** boîte de dialogue le **nom de l’ordinateur**, cliquez sur **fermer**. Le **MicrosoftWindows** boîte de dialogue s’ouvre et affiche un message indiquant de nouveau que vous devez redémarrer l’ordinateur pour appliquer les modifications. Cliquez sur **redémarrer maintenant**.

##### <a name="to-log-on-to-the-domain-using-computers-running-windows-10"></a>Pour ouvrir une session sur le domaine à l’aide des ordinateurs exécutant Windows10

1.  Fermez la session, ou redémarrer l’ordinateur.

2.  Appuyez sur CTRL + ALT + SUPPR. L’écran d’ouverture de session s’affiche.

3.  Dans le coin inférieur gauche, cliquez sur **autre utilisateur**.

4.  Dans **nom d’utilisateur**, tapez votre nom de domaine et d’utilisateur au format *domaine\utilisateur*. Par exemple, pour vous connecter au domaine corp.contoso.com avec un compte nommé **utilisateur-01**, type **corp\utilisateur-01**.

5.  Dans **mot de passe**, tapez votre mot de passe de domaine, puis cliquez sur la flèche, ou appuyez sur ENTRÉE.

### <a name="BKMK_optionalfeatures"></a>Déploiement des fonctionnalités facultatives pour l’authentification d’accès réseau et les services Web
Si vous envisagez de déployer des serveurs d’accès réseau, tels que les points d’accès sans fil ou des serveurs VPN, après l’installation de votre réseau de base, il est recommandé que vous déployez un serveur NPS et un serveur Web. Pour les déploiements d’accès réseau, l’utilisation des méthodes d’authentification par certificat est recommandée. Vous pouvez utiliser le serveur NPS pour gérer les stratégies d’accès réseau et déployer des méthodes d’authentification sécurisée. Vous pouvez utiliser un serveur Web pour publier la liste de révocation de certificats (CRL) de votre autorité de certification (CA) qui fournit les certificats d’authentification sécurisée.

> [!NOTE]
> Vous pouvez déployer des certificats de serveur et des fonctionnalités supplémentaires à l’aide des Guides d’accompagnement du réseau principal. Pour plus d’informations, voir [ressources techniques supplémentaires](#BKMK_resources).

L’illustration suivante montre la topologie de réseau de base Windows Server avec les serveurs NPS et Web ajoutés.

![Topologie de réseau de base Windows Server avec les serveurs NPS et Web ajoutés](../media/Core-Network-Guide/cng16_overview_2.jpg)

Les sections suivantes fournissent des informations sur l’ajout de serveurs NPS et Web à votre réseau.

-   [Déploiement de NPS1](#BKMK_deployNPS1)

-   [Déploiement de WEB1](#BKMK_IIS)

#### <a name="BKMK_deployNPS1"></a>Déploiement de NPS1
Le serveur du serveur NPS (Network Policy) est installé en tant qu’une étape préparatoire au déploiement d’autres technologies d’accès réseau, tels que les serveurs de réseau privé virtuel (VPN), les points d’accès sans fil et les commutateurs d’authentification 802. 1 X.

Serveur NPS (Network Policy) vous permet de configurer et gérer des stratégies de réseau avec les fonctionnalités suivantes de manière centralisée: server Authentication Dial-In utilisateur RADIUS (Remote Service) et proxy RADIUS.

NPS est un composant facultatif d’un réseau de base, mais vous devez l’installer si une des opérations suivantes est vraie:

-   Vous envisagez d’étendre votre réseau pour inclure des serveurs d’accès à distance qui sont compatibles avec le protocole RADIUS, tel qu’un ordinateur exécutant Windows Server2016, Windows Server2012R2, Windows Server2012, Windows Server2008R2 ou Windows Server2008 et service Routage et accès à distance, passerelle des Services Terminal Server ou passerelle des services Bureau à distance.


-   Vous prévoyez de déployer l’authentification de 802. 1 X pour câblé ou un accès sans fil.

Avant de déployer ce service de rôle, vous devez effectuer les étapes suivantes sur l’ordinateur que vous configurez comme un serveur NPS.

-   Effectuez les étapes dans la section [la configuration de tous les serveurs](#BKMK_configuringAll).

-   Effectuez les étapes dans la section [joindre des ordinateurs au domaine et ouvrir une session](#BKMK_joinlogserver)

Pour déployer le serveur NPS1, qui est l’ordinateur exécutant le service de rôle serveur NPS (Network Policy) du rôle de serveur de stratégie réseau et Services d’accès, vous devez effectuer cette étape:

-   [Planification du déploiement de NPS1](#bkmk_NetFndtn_Pln_NPS-01)

-   [Installer le serveur de stratégie réseau (NPS)](#BKMK_installNPS)

-   [Inscrire le serveur NPS dans le domaine par défaut](#BKMK_registerNPS)

> [!NOTE]
> Ce guide fournit des instructions pour déployer NPS sur un serveur autonome ou ordinateur virtuel nommé NPS1.  Un autre modèle de déploiement recommandée est l’installation du serveur NPS sur un contrôleur de domaine. Si vous souhaitez installer le serveur NPS sur un contrôleur de domaine plutôt que sur un serveur autonome, installez NPS sur DC1.

##### <a name="bkmk_NetFndtn_Pln_NPS-01"></a>Planification du déploiement de NPS1
Si vous envisagez de déployer des serveurs d’accès réseau, tels que les points d’accès sans fil ou des serveurs VPN, après le déploiement de votre réseau de base, il est recommandé de déployer le serveur NPS.

Lorsque vous utilisez NPS en tant qu’un serveur d’authentification Dial-In utilisateur RADIUS (Remote Service), NPS effectue l’authentification et autorisation pour les demandes de connexion par le biais de vos serveurs d’accès réseau. NPS vous permet également de configurer et gérer des stratégies réseau de manière centralisée qui déterminent qui peut accéder au réseau et comment ils peuvent accéder au réseau lorsqu’ils peuvent accéder au réseau.

Voici les étapes de planification clés avant d’installer le serveur NPS.

- Planifier la base de données de comptes d’utilisateur. Par défaut, si vous joignez le serveur exécutant NPS à un domaine ActiveDirectory, NPS effectue l’authentification et autorisation à l’aide de la base de données de comptes utilisateur ADDS. Dans certains cas, comme avec les réseaux de grande taille utilisent NPS en tant que proxy RADIUS pour transférer les demandes de connexion à d’autres serveurs RADIUS, vous devrez installer le serveur NPS sur un ordinateur membre du domaine.

- Planifier la gestion de comptes RADIUS. NPS permet d’enregistrer les données de gestion pour une base de données SQLServer ou dans un fichier texte sur l’ordinateur local. Si vous souhaitez utiliser la journalisation SQLServer, planifiez l’installation et la configuration de votre serveur exécutant SQLServer.

##### <a name="BKMK_installNPS"></a>Installer le serveur de stratégie réseau (NPS)
Vous pouvez utiliser cette procédure pour installer le serveur NPS (Network Policy) à l’aide de l’ajout de rôles et fonctionnalités Assistant. NPS est un service de rôle du rôle de serveur de stratégie réseau et Services d’accès.

> [!NOTE]
> Par défaut, le serveur NPS écoute le trafic RADIUS sur les ports 1812, 1813, 1645 et 1646 sur toutes les cartes réseau installées. Si le pare-feu Windows avec fonctions avancées de sécurité est activé lorsque vous installez le serveur NPS, les exceptions de pare-feu pour ces ports sont créées automatiquement pendant le processus d’installation pour Internet Protocol version6 \(IPv6\) et le trafic IPv4. Si vos serveurs d’accès réseau sont configurés pour envoyer le trafic RADIUS sur des ports autres que ces paramètres par défaut, supprimez les exceptions créées dans le pare-feu Windows avec fonctions avancées de sécurité lors de l’installation du serveur NPS et créer des exceptions pour les ports que vous n’utilisez pas pour le trafic RADIUS.

**Informations d’identification d’administration**

Pour effectuer cette procédure, vous devez être un membre de la **Admins du domaine** groupe.

> [!NOTE]
> Pour effectuer cette procédure en utilisant Windows PowerShell, ouvrez PowerShell et tapez la commande suivante, puis appuyez sur ENTRÉE.
>
> `Install-WindowsFeature NPAS -IncludeManagementTools`

###### <a name="to-install-nps"></a>Pour installer le serveur NPS

1.  Sur le serveur NPS1, dans le Gestionnaire de serveur, cliquez sur **gérer**, puis cliquez sur **Ajout de rôles et fonctionnalités**. L’Assistant Ajouter des rôles et fonctionnalités s’ouvre.

2.  Dans **avant de commencer**, cliquez sur **suivant**.

    > [!NOTE]
    > Le **avant de commencer** page de l’ajout de rôles et fonctionnalités Assistant ne s’affiche pas si vous avez sélectionné précédemment **ignorer cette page par défaut** lorsque l’ajout de rôles et fonctionnalités Assistant a été exécuté.

3.  Dans **sélectionner le Type d’Installation**, assurez-vous que **installation basée sur un rôle ou une fonctionnalité** est sélectionné, puis cliquez sur **suivant**.

4.  Dans **serveur de destination sélectionnez**, assurez-vous que **sélectionner un serveur du pool de serveurs** est sélectionné. Dans **Pool de serveurs**, vérifiez que l’ordinateur local est sélectionné. Cliquez sur **suivant**.

5.  Dans **sélectionner des rôles de serveur**, dans **rôles**, sélectionnez **stratégie réseau et Services d’accès**. Une boîte de dialogue s’ouvre et vous demande si elle doit ajouter des fonctionnalités qui sont nécessaires pour la stratégie réseau et Services d’accès. Cliquez sur **ajouter des fonctionnalités**, puis cliquez sur **suivant**.

6.  Dans **sélectionner des fonctionnalités**, cliquez sur **suivant**et dans **stratégie réseau et Services d’accès**, passez en revue les informations qui sont fournies, puis cliquez sur **suivant**.

7.  Dans **sélectionner les services de rôle**, cliquez sur **serveur NPS**.  Dans **ajouter des fonctionnalités qui sont nécessaires pour le serveur NPS**, cliquez sur **ajouter des fonctionnalités**. Cliquez sur **suivant**.

8.  Dans **confirmer les sélections d’installation**, cliquez sur **redémarrer automatiquement le serveur de destination si nécessaire**. Lorsque vous êtes invité à confirmer ce choix, cliquez sur **Oui**, puis cliquez sur **installer**. La page de progression Installation affiche l’état pendant le processus d’installation. Lorsque le processus terminé, le message «Installation réussie sur *Nom_Ordinateur*» s’affiche, où *Nom_Ordinateur* est le nom de l’ordinateur sur lequel vous avez installé le serveur de stratégie réseau. Cliquez sur **fermer **.

##### <a name="BKMK_registerNPS"></a>Inscrire le serveur NPS dans le domaine par défaut
Vous pouvez utiliser cette procédure pour inscrire un serveur NPS dans le domaine où le serveur est membre d’un domaine.

Les serveurs NPS doivent être enregistrés dans ActiveDirectory afin qu’ils sont autorisés à lire les propriétés de numérotation des comptes d’utilisateur au cours du processus d’autorisation. L’inscription d’un serveur NPS ajoute le serveur à le **serveurs RAS et IAS** groupe dans ActiveDirectory.

**Informations d’identification d’administration**

Pour effectuer cette procédure, vous devez être un membre de la **Admins du domaine** groupe.

> [!NOTE]
> Pour effectuer cette procédure à l’aide des commandes de shell (Netsh) réseau dans Windows PowerShell, ouvrez PowerShell et tapez la commande suivante, puis appuyez sur ENTRÉE.
>
> `netsh nps add registeredserver domain=corp.contoso.com server=NPS1.corp.contoso.com`

###### <a name="to-register-an-nps-server-in-its-default-domain"></a>Pour inscrire un serveur NPS dans son domaine par défaut

1.  Sur le serveur NPS1, dans le Gestionnaire de serveur, cliquez sur Outils, puis cliquez sur **serveur NPS**. La console MMC serveur de stratégie réseau s’ouvre.

2.  Avec le bouton droit **NPS (Local)**, puis cliquez sur **inscrire un serveur dans ActiveDirectory**. Le **serveur NPS** boîte de dialogue s’ouvre.

3.  Dans **serveur NPS**, cliquez sur **OK**, puis cliquez sur **OK** à nouveau.

Pour plus d’informations sur le serveur de stratégie réseau, voir [serveur NPS (Network Policy)](../technologies/nps/nps-top.md).

#### <a name="BKMK_IIS"></a>Déploiement de WEB1

Le rôle de serveur Web (IIS) dans Windows Server2016 fournit une plateforme sécurisée, facile à gérer, modulaire et extensible héberger de manière fiable des sites Web, services et applications. Avec les Services Internet (IIS), vous pouvez partager des informations avec des utilisateurs sur Internet, un intranet ou un extranet. IIS est une plateforme web unifiée qui intègre IIS, ASP.NET, services FTP, PHP et WindowsCommunicationFoundation (WCF).

En plus de qui vous permet de publier une liste de révocation pour l’accès par des ordinateurs membres du domaine, le rôle serveur serveur Web (IIS) vous permet de configurer et gérer plusieurs sites Web, applications web et sites FTP. IIS fournit également les avantages suivants:

-   Optimiser la sécurité web grâce à une isolation des applications d’impression et automatique foot serveur réduite.

-   Facilement déployer et exécuter ASP.NET, classic ASP et PHP applications web sur le même serveur.

-   Isolation des applications en offrant des processus de travail une identité unique et la configuration de bac à sable par défaut, réduisant ainsi les risques de sécurité.

-   Facilement ajouter, supprimer et même remplacer les composants IIS intégrés par des modules personnalisés, adaptés pour les besoins des clients.

-   Accélérer votre site Web par le biais de la mise en cache dynamique intégrée et une compression améliorée.

Pour déployer le serveur WEB1, qui est l’ordinateur qui exécute le rôle serveur serveur Web (IIS), vous devez procédez comme suit:

-   Effectuez les étapes dans la section [la configuration de tous les serveurs](#BKMK_configuringAll).

-   Effectuez les étapes dans la section [joindre des ordinateurs au domaine et ouvrir une session](#BKMK_joinlogserver)

-   [Installer le rôle serveur serveur Web (IIS)](#BKMK_install_IIS)

##### <a name="BKMK_install_IIS"></a>Installer le rôle serveur serveur Web (IIS)
Pour effectuer cette procédure, vous devez être un membre de la **administrateurs** groupe.

> [!NOTE]
> Pour effectuer cette procédure en utilisant Windows PowerShell, ouvrez PowerShell et tapez la commande suivante, puis appuyez sur ENTRÉE.
>
> `Install-WindowsFeature Web-Server -IncludeManagementTools`

1.  Dans **le Gestionnaire de serveur**, cliquez sur **gérer**, puis cliquez sur **Ajout de rôles et fonctionnalités**. L’Assistant Ajouter des rôles et fonctionnalités s’ouvre.

2.  Dans **avant de commencer**, cliquez sur **suivant**.

    > [!NOTE]
    > Le **avant de commencer** page de l’ajout de rôles et fonctionnalités Assistant ne s’affiche pas si vous avez sélectionné précédemment **ignorer cette page par défaut** lorsque l’ajout de rôles et fonctionnalités Assistant a été exécuté.

3.  Sur le **sélectionner le Type d’Installation**, cliquez sur **suivant**.

4.  Sur le **serveur de destination sélectionnez**, vérifiez que l’ordinateur local est sélectionné, puis cliquez sur **suivant**.

5.  Sur le **sélectionner des rôles de serveur** page, faites défiler jusqu'à et sélectionnez **serveur Web (IIS)**. Le **ajouter des fonctionnalités qui sont nécessaires pour le serveur Web (IIS)** boîte de dialogue s’ouvre. Cliquez sur **ajouter des fonctionnalités**, puis cliquez sur **suivant**.

6.  Cliquez sur **suivant** jusqu'à ce que vous avez accepté la valeur par défaut tous les paramètres du serveur web, puis cliquez sur **installer**.

7.  Vérifiez que toutes les installations ont réussi, puis cliquez sur **fermer**.

## <a name="BKMK_resources"></a>Ressources techniques supplémentaires
Pour plus d’informations sur les technologies de ce guide, consultez les ressources suivantes:

 Ressources de bibliothèque technique Windows Server2012, Windows Server2012R2 et Windows Server2016

-   [Nouveautés des Services de domaine ActiveDirectory (ADDS) dans Windows Server2016](https://technet.microsoft.com/en-us/library/mt163897.aspx)

-   [ActiveDirectory Vue d’ensemble des Services de domaine](https://technet.microsoft.com/library/hh831484.aspx) à https://technet.microsoft.com/library/hh831484.aspx.

-   [Vue d’ensemble du système DNS (Name) de domaine](https://technet.microsoft.com/library/hh831667.aspx) à https://technet.microsoft.com/library/hh831667.aspx.

-   [Mise en œuvre le rôle Administrateurs DNS](https://technet.microsoft.com/library/cc756152(WS.10).aspx)

-   [Vue d’ensemble de Dynamic Host Configuration Protocol (DHCP)](https://technet.microsoft.com/library/hh831825.aspx) à https://technet.microsoft.com/library/hh831825.aspx.

-   [Vue d’ensemble de la stratégie réseau et Services d’accès](https://technet.microsoft.com/library/hh831683.aspx) à https://technet.microsoft.com/library/hh831683.aspx.

-   [Vue d’ensemble du serveur (IIS) de Web](https://technet.microsoft.com/library/hh831725.aspx) à https://technet.microsoft.com/library/hh831725.aspx.

## <a name="BKMK_appendix"></a>Annexes A à E
Les sections suivantes contiennent des informations de configuration supplémentaires pour les ordinateurs qui exécutent des systèmes d’exploitation autres que Windows Server2016, Windows10, Windows Server2012 et Windows8. En outre, une feuille de préparation de réseau est fournie pour vous aider à votre déploiement.

1.  [Annexe A - changement de nom des ordinateurs](#BKMK_A)

2.  [Annexe B - configuration d’adresse IP statique adresses](#BKMK_B)

3.  [Annexe C - joindre des ordinateurs au domaine](#BKMK_C)

4.  [Annexe D: ouvrez une session sur le domaine](#BKMK_D)

5.  [Annexe E - feuille de préparation de planification de réseau de base](#BKMK_E)

## <a name="BKMK_A"></a>Annexe A - changement de nom des ordinateurs
Vous pouvez utiliser les procédures de cette section pour fournir aux ordinateurs exécutant Windows Server2008R2, Windows7, Windows Server2008 et WindowsVista avec un nom d’ordinateur différent.

-   [Windows Server2008R2 et Windows7](#bkmk_NetFndtn_Pln_rename_R2)

-   [Windows Server2008 et WindowsVista](#bkmk_NetFndtn_Pln_Renam08)

### <a name="bkmk_NetFndtn_Pln_rename_R2"></a>Windows Server2008R2 et Windows7
L’appartenance au groupe **administrateurs**, ou équivalente, est la condition minimale requise pour effectuer ces procédures.

##### <a name="to-rename-computers-running-windows-server-2008-r2-and-windows-7"></a>Pour renommer des ordinateurs exécutant Windows Server2008R2 et Windows7

1.  Cliquez sur **Démarrer**, avec le bouton droit **ordinateur**, puis cliquez sur **propriétés**. Le **système** boîte de dialogue s’ouvre.

2.  Dans **paramètres de groupe de travail, le domaine et nom de l’ordinateur**, cliquez sur **modifier les paramètres**. Le **propriétés système** boîte de dialogue s’ouvre.

    > [!NOTE]
    > Sur les ordinateurs exécutant Windows7, avant le **propriétés système** boîte de dialogue s’ouvre, la **contrôle de compte d’utilisateur** boîte de dialogue s’ouvre, demandant l’autorisation de continuer. Cliquez sur **continuer** pour continuer.

3.  Cliquez sur **modification**. Le **modification du nom ou du domaine ordinateur** boîte de dialogue s’ouvre.

4.  Dans **nom de l’ordinateur**, tapez le nom de votre ordinateur. Par exemple, si vous souhaitez nommer l’ordinateur DC1, tapez **DC1**.

5.  Cliquez sur **OK** deux fois, cliquez sur **fermer**, puis cliquez sur **redémarrer maintenant** à redémarrer l’ordinateur.

### <a name="bkmk_NetFndtn_Pln_Renam08"></a>Windows Server2008 et WindowsVista
L’appartenance au groupe **administrateurs**, ou équivalente, est la condition minimale requise pour effectuer ces procédures.

##### <a name="to-rename-computers-running-windows-server-2008-and-windows-vista"></a>Pour renommer des ordinateurs exécutant Windows Server2008 et WindowsVista

1.  Cliquez sur **Démarrer**, avec le bouton droit **ordinateur**, puis cliquez sur **propriétés**. Le **système** boîte de dialogue s’ouvre.

2.  Dans **paramètres de groupe de travail, le domaine et nom de l’ordinateur**, cliquez sur **modifier les paramètres**. Le **propriétés système** boîte de dialogue s’ouvre.

    > [!NOTE]
    > Sur les ordinateurs exécutant WindowsVista, avant le **propriétés système** boîte de dialogue s’ouvre, la **contrôle de compte d’utilisateur** boîte de dialogue s’ouvre, demandant l’autorisation de continuer. Cliquez sur **continuer** pour continuer.

3.  Cliquez sur **modification**. Le **modification du nom ou du domaine ordinateur** boîte de dialogue s’ouvre.

4.  Dans **nom de l’ordinateur**, tapez le nom de votre ordinateur. Par exemple, si vous souhaitez nommer l’ordinateur DC1, tapez **DC1**.

5.  Cliquez sur **OK** deux fois, cliquez sur **fermer**, puis cliquez sur **redémarrer maintenant** à redémarrer l’ordinateur.

## <a name="BKMK_B"></a>Annexe B - configuration d’adresse IP statique adresses
Cette rubrique fournit des procédures pour la configuration des adresses IP statiques sur des ordinateurs exécutant les systèmes d’exploitation suivants:

-   [Windows Server2008R2](#bkmk_R2Cng_WS08R2IP)

-   [Windows Server2008](#bkmk_NetFndtn_Pln_CfgStatic08)

### <a name="bkmk_R2Cng_WS08R2IP"></a>Windows Server2008R2
L’appartenance au groupe **administrateurs**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.

##### <a name="to-configure-a-static-ip-address-on-a-computer-running-windows-server-2008-r2"></a>Pour configurer une adresse IP statique sur un ordinateur exécutant Windows Server2008R2

1.  Cliquez sur **Démarrer**, puis cliquez sur **le panneau de configuration**.

2.  Dans **le panneau de configuration**, cliquez sur **réseau et Internet**. **Réseau et Internet** s’ouvre.

    Dans **réseau et Internet**, cliquez sur **Centre réseau et partage**. **Centre réseau et partage** s’ouvre.

3.  Dans **Centre réseau et partage**, cliquez sur **modifier les paramètres de carte**. **Connexions réseau** s’ouvre.

4.  Dans **connexions réseau**, avec le bouton droit de la connexion réseau que vous souhaitez configurer, puis cliquez sur **propriétés**.

5.  Dans **propriétés de connexion de réseau Local**, dans **cette connexion utilise les éléments suivants**, sélectionnez **Internet Protocol Version4 (TCP/IPv4)**, puis cliquez sur **propriétés**. Le **propriétés de protocole Internet Version4 (TCP/IPv4)** boîte de dialogue s’ouvre.

6.  Dans **propriétés de protocole Internet Version4 (TCP/IPv4)**, dans le **général**, cliquez sur **utiliser l’adresse IP suivante**. Dans **adresse IP**, tapez l’adresse IP que vous souhaitez utiliser.

7.  Appuyez sur tab pour placer le curseur dans **masque de sous-réseau**. Une valeur par défaut pour le masque de sous-réseau est entrée automatiquement. Acceptez le masque de sous-réseau par défaut, ou tapez le masque de sous-réseau que vous souhaitez utiliser.

8.  Dans **passerelle par défaut**, tapez l’adresse IP de votre passerelle par défaut.

9. Dans **serveur DNS préféré**, tapez l’adresse IP de votre serveur DNS. Si vous prévoyez d’utiliser l’ordinateur local comme serveur DNS préféré, tapez l’adresse IP de l’ordinateur local.

10. Dans **serveur DNS auxiliaire**, tapez l’adresse IP de votre serveur DNS auxiliaire, le cas échéant. Si vous prévoyez d’utiliser l’ordinateur local en tant que serveur DNS auxiliaire, tapez l’adresse IP de l’ordinateur local.

11. Cliquez sur **OK**, puis cliquez sur **fermer**.

### <a name="bkmk_NetFndtn_Pln_CfgStatic08"></a>Windows Server2008
L’appartenance au groupe **administrateurs**, ou équivalente, est la condition minimale requise pour effectuer ces procédures.

##### <a name="to-configure-a-static-ip-address-on-a-computer-running-windows-server-2008"></a>Pour configurer une adresse IP statique sur un ordinateur exécutant Windows Server2008

1.  Cliquez sur **Démarrer**, puis cliquez sur **le panneau de configuration**.

2.  Dans **le panneau de configuration**, vérifiez que **affichage classique** est sélectionné, puis double-cliquez sur **Centre réseau et partage**.

3.  Dans **Centre réseau et partage**, dans **tâches**, cliquez sur **gérer les connexions réseau**.

4.  Dans **connexions réseau**, avec le bouton droit de la connexion réseau que vous souhaitez configurer, puis cliquez sur **propriétés**.

5.  Dans **propriétés de connexion de réseau Local**, dans **cette connexion utilise les éléments suivants**, sélectionnez **Internet Protocol Version4 (TCP/IPv4)**, puis cliquez sur **propriétés**. Le **propriétés de protocole Internet Version4 (TCP/IPv4)** boîte de dialogue s’ouvre.

6.  Dans **propriétés de protocole Internet Version4 (TCP/IPv4)**, dans le **général**, cliquez sur **utiliser l’adresse IP suivante**. Dans **adresse IP**, tapez l’adresse IP que vous souhaitez utiliser.

7.  Appuyez sur tab pour placer le curseur dans **masque de sous-réseau**. Une valeur par défaut pour le masque de sous-réseau est entrée automatiquement. Acceptez le masque de sous-réseau par défaut, ou tapez le masque de sous-réseau que vous souhaitez utiliser.

8.  Dans **passerelle par défaut**, tapez l’adresse IP de votre passerelle par défaut.

9. Dans **serveur DNS préféré**, tapez l’adresse IP de votre serveur DNS. Si vous prévoyez d’utiliser l’ordinateur local comme serveur DNS préféré, tapez l’adresse IP de l’ordinateur local.

10. Dans **serveur DNS auxiliaire**, tapez l’adresse IP de votre serveur DNS auxiliaire, le cas échéant. Si vous prévoyez d’utiliser l’ordinateur local en tant que serveur DNS auxiliaire, tapez l’adresse IP de l’ordinateur local.

11. Cliquez sur **OK**, puis cliquez sur **fermer**.

## <a name="BKMK_C"></a>Annexe C - joindre des ordinateurs au domaine
Vous pouvez utiliser ces procédures pour joindre des ordinateurs exécutant Windows Server2008R2, Windows7, Windows Server2008 et WindowsVista au domaine.

-   [Windows Server2008R2 et Windows7](#BKMK_c1)

-   [Windows Server2008 et WindowsVista](#BKMK_c2)

> [!IMPORTANT]
> Pour joindre un ordinateur à un domaine, vous devez être connecté à l’ordinateur avec le compte administrateur local ou, si vous êtes connecté à l’ordinateur avec un compte d’utilisateur qui ne dispose pas d’informations d’identification d’administration de l’ordinateur local, vous devez fournir les informations d’identification pour le compte administrateur local au cours du processus visant à joindre l’ordinateur au domaine. En outre, vous devez disposer un compte d’utilisateur dans le domaine auquel vous souhaitez joindre l’ordinateur. Pendant le processus visant à joindre l’ordinateur au domaine, vous serez invité vos informations d’identification du compte de domaine (nom d’utilisateur et mot de passe).

### <a name="BKMK_c1"></a>Windows Server2008R2 et Windows7
L’appartenance au groupe **utilisateurs du domaine**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.

##### <a name="to-join-computers-running-windows-server-2008-r2-and-windows-7-to-the-domain"></a>Pour joindre des ordinateurs exécutant Windows Server2008R2 et Windows7 au domaine

1.  Ouvrez une session sur l’ordinateur avec le compte administrateur local.

2.  Cliquez sur **Démarrer**, avec le bouton droit **ordinateur**, puis cliquez sur **propriétés**. Le **système** boîte de dialogue s’ouvre.

3.  Dans **paramètres de groupe de travail, le domaine et nom de l’ordinateur**, cliquez sur **modifier les paramètres**. Le **propriétés système** boîte de dialogue s’ouvre.

    > [!NOTE]
    > Sur les ordinateurs exécutant Windows7, avant le **propriétés système** boîte de dialogue s’ouvre, la **contrôle de compte d’utilisateur** boîte de dialogue s’ouvre, demandant l’autorisation de continuer. Cliquez sur **continuer** pour continuer.

4.  Cliquez sur **modification**. Le **modification du nom ou du domaine ordinateur** boîte de dialogue s’ouvre.

5.  Dans **nom de l’ordinateur**, dans **membre**, sélectionnez **domaine**, puis tapez le nom du domaine auquel joindre l’ordinateur. Par exemple, si le nom de domaine est corp.contoso.com, tapez **corp.contoso.com**.

6.  Cliquez sur **OK**. Le **sécurité Windows** boîte de dialogue s’ouvre.

7.  Dans **modification du nom ou du domaine ordinateur**, dans **nom d’utilisateur**, tapez le nom d’utilisateur et dans **mot de passe**, tapez le mot de passe, puis cliquez sur **OK**. Le **modification du nom ou du domaine ordinateur** boîte de dialogue s’ouvre, pour vous accueillir dans le domaine. Cliquez sur **OK**.

8.  Le **modification du nom ou du domaine ordinateur** boîte de dialogue affiche un message indiquant que vous devez redémarrer l’ordinateur pour appliquer les modifications. Cliquez sur **OK**.

9. Sur le **propriétés système** boîte de dialogue le **nom de l’ordinateur**, cliquez sur **fermer**. Le **MicrosoftWindows** boîte de dialogue s’ouvre et affiche un message indiquant de nouveau que vous devez redémarrer l’ordinateur pour appliquer les modifications. Cliquez sur **redémarrer maintenant**.

### <a name="BKMK_c2"></a>Windows Server2008 et WindowsVista
L’appartenance au groupe **utilisateurs du domaine**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.

##### <a name="to-join-computers-running-windows-server-2008-and-windows-vista-to-the-domain"></a>Pour joindre des ordinateurs exécutant Windows Server2008 et WindowsVista au domaine

1.  Ouvrez une session sur l’ordinateur avec le compte administrateur local.

2.  Cliquez sur **Démarrer**, avec le bouton droit **ordinateur**, puis cliquez sur **propriétés**. Le **système** boîte de dialogue s’ouvre.

3.  Dans **paramètres de groupe de travail, le domaine et nom de l’ordinateur**, cliquez sur **modifier les paramètres**. Le **propriétés système** boîte de dialogue s’ouvre.

4.  Cliquez sur **modification**. Le **modification du nom ou du domaine ordinateur** boîte de dialogue s’ouvre.

5.  Dans **nom de l’ordinateur**, dans **membre**, sélectionnez **domaine**, puis tapez le nom du domaine auquel joindre l’ordinateur. Par exemple, si le nom de domaine est corp.contoso.com, tapez **corp.contoso.com**.

6.  Cliquez sur **OK**. Le **sécurité Windows** boîte de dialogue s’ouvre.

7.  Dans **modification du nom ou du domaine ordinateur**, dans **nom d’utilisateur**, tapez le nom d’utilisateur et dans **mot de passe**, tapez le mot de passe, puis cliquez sur **OK**. Le **modification du nom ou du domaine ordinateur** boîte de dialogue s’ouvre, pour vous accueillir dans le domaine. Cliquez sur **OK**.

8.  Le **modification du nom ou du domaine ordinateur** boîte de dialogue affiche un message indiquant que vous devez redémarrer l’ordinateur pour appliquer les modifications. Cliquez sur **OK**.

9. Sur le **propriétés système** boîte de dialogue le **nom de l’ordinateur**, cliquez sur **fermer**. Le **MicrosoftWindows** boîte de dialogue s’ouvre et affiche un message indiquant de nouveau que vous devez redémarrer l’ordinateur pour appliquer les modifications. Cliquez sur **redémarrer maintenant**.

## <a name="BKMK_D"></a>Annexe D: ouvrez une session sur le domaine
Vous pouvez utiliser ces procédures pour ouvrir une session sur le domaine à l’aide des ordinateurs exécutant Windows Server2008R2, Windows7, Windows Server2008 et WindowsVista.

-   [Windows Server2008R2 et Windows7](#BKMK_d1)

-   [Windows Server2008 et WindowsVista](#BKMK_d2)

### <a name="BKMK_d1"></a>Windows Server2008R2 et Windows7
L’appartenance au groupe **utilisateurs du domaine**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.

##### <a name="log-on-to-the-domain-using-computers-running-windows-server-2008-r2-and-windows-7"></a>Ouvrez une session sur le domaine à l’aide des ordinateurs exécutant Windows Server2008R2 et Windows7

1.  Fermez la session, ou redémarrer l’ordinateur.

2.  Appuyez sur CTRL + ALT + SUPPR. L’écran d’ouverture de session s’affiche.

3.  Cliquez sur **changer d’utilisateur**, puis cliquez sur **autre utilisateur**.

4.  Dans **nom d’utilisateur**, tapez votre nom de domaine et d’utilisateur au format *domaine\utilisateur*. Par exemple, pour vous connecter au domaine corp.contoso.com avec un compte nommé **utilisateur-01**, type **corp\utilisateur-01**.

5.  Dans **mot de passe**, tapez votre mot de passe de domaine, puis cliquez sur la flèche, ou appuyez sur ENTRÉE.

### <a name="BKMK_d2"></a>Windows Server2008 et WindowsVista
L’appartenance au groupe **utilisateurs du domaine**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.

##### <a name="log-on-to-the-domain-using-computers-running-windows-server-2008-and-windows-vista"></a>Ouvrez une session sur le domaine à l’aide des ordinateurs exécutant Windows Server2008 et WindowsVista

1.  Fermez la session, ou redémarrer l’ordinateur.

2.  Appuyez sur CTRL + ALT + SUPPR. L’écran d’ouverture de session s’affiche.

3.  Cliquez sur **changer d’utilisateur**, puis cliquez sur **autre utilisateur**.

4.  Dans **nom d’utilisateur**, tapez votre nom de domaine et d’utilisateur au format *domaine\utilisateur*. Par exemple, pour vous connecter au domaine corp.contoso.com avec un compte nommé **utilisateur-01**, type **corp\utilisateur-01**.

5.  Dans **mot de passe**, tapez votre mot de passe de domaine, puis cliquez sur la flèche, ou appuyez sur ENTRÉE.

## <a name="BKMK_E"></a>Annexe E - feuille de préparation de planification de réseau de base
Vous pouvez utiliser cette feuille de préparation de planification de réseau pour collecter les informations nécessaires pour installer un réseau de base. Cette rubrique fournit des tables qui contiennent les éléments de configuration individuels pour chaque ordinateur du serveur pour lequel vous devez fournir des informations ou des valeurs spécifiques au cours du processus d’installation ou de configuration. Exemples de valeurs sont fournis pour chaque élément de configuration.

Pour la planification et à des fins de suivi, les espaces sont prévus dans chaque table pour entrer les valeurs utilisées pour votre déploiement. Si vous vous connectez des valeurs relatives à la sécurité dans ces tableaux, vous devez stocker les informations dans un emplacement sécurisé.

Les liens suivants permettent aux sections de cette rubrique qui fournissent les éléments de configuration et des exemples de valeurs qui sont associés aux procédures de déploiement décrites dans ce guide.

1.  [L’installation de DNS et des Services de domaine ActiveDirectory](#BKMK_FndtnPrep_InstallAD)

    -   [La configuration d’une Zone de recherche inversée DNS](#BKMK_FndtnPrep_DNSRevrsLook)

2.  [L’installation de DHCP](#BKMK_FndtnPrep_InstallDHCP)

    -   [Création d’une plage d’exclusion dans DHCP](#BKMK_FndtnPrep_DHCP_Exclusn)

    -   [Création d’une nouvelle étendue DHCP](#bkmk_NetFndtn_Pln_DHCP_NewScope)

3.  [L’installation du serveur de stratégie réseau (facultatif)](#BKMK_FndtnPrep_InstallNPS)

### <a name="BKMK_FndtnPrep_InstallAD"></a>L’installation de DNS et des Services de domaine ActiveDirectory
Les tableaux de cette section répertorient les éléments de configuration pour la préinstallation et l’installation des Services de domaine ActiveDirectory (ADDS) et DNS.

##### <a name="pre-installation-configuration-items-for-ad-ds-and-dns"></a>Éléments de configuration de préinstallation pour ADDS et DNS
Les tableaux ci-après les éléments de configuration de pré-installation liste comme décrit dans [la configuration de tous les serveurs](#BKMK_configuringAll):

-   [Configurer une adresse IP statique](#BKMK_ip)

|Éléments de configuration|Exemples de valeurs|Valeurs|
|-----------------------|------------------|----------|
|Adresse IP|10.0.0.2||
|Masque de sous-réseau|255.255.255.0||
|Passerelle par défaut|10.0.0.1||
|Serveur DNS préféré|127.0.0.1||
|Serveur DNS auxiliaire|10.0.0.15||

-   [Renommer l’ordinateur](#BKMK_rename)

|Élément de configuration|Exemple de valeur|Valeur|
|----------------------|-----------------|---------|
|Nom de l’ordinateur|DC1||

##### <a name="ad-ds-and-dns-installation-configuration-items"></a>Éléments de configuration d’installation ADDS et DNS
Éléments de configuration pour la procédure de déploiement de réseau de base Windows Server [installer les services ADDS et DNS dans une nouvelle forêt](#BKMK_installAD-DNS):

|Éléments de configuration|Exemples de valeurs|Valeurs|
|-----------------------|------------------|----------|
|Nom DNS complet|corp.contoso.com||
|Niveau fonctionnel de forêt|Windows Server2003||
|Emplacement du dossier de base de données Services de domaine ActiveDirectory|E:\Configuration\\<br /><br />Ou acceptez l’emplacement par défaut.||
|Emplacement du dossier de fichiers du journal Services de domaine ActiveDirectory|E:\Configuration\\<br /><br />Ou acceptez l’emplacement par défaut.||
|Emplacement du dossier SYSVOL de Services de domaine ActiveDirectory|E:\Configuration\\<br /><br />Ou acceptez l’emplacement par défaut||
|Mot de passe administrateur de restauration Active|J * p2leO4$ F||
|Nom de fichier de réponses (facultatif)|AD DS_AnswerFile||

#### <a name="BKMK_FndtnPrep_DNSRevrsLook"></a>La configuration d’une Zone de recherche inversée DNS

|Éléments de configuration|Exemples de valeurs|Valeurs|
|-----------------------|------------------|----------|
|Type de zone:|-Zone principale<br />-Zone secondaire<br />-Zone de stub||
|Type de zone<br /><br />**Enregistrer la zone dans ActiveDirectory**|-Sélectionné<br />-N’est pas sélectionné.||
|Étendue de la réplication de zone ActiveDirectory|-Pour tous les serveurs DNS de cette forêt<br />-Pour tous les serveurs DNS dans ce domaine<br />-Pour tous les contrôleurs de domaine dans ce domaine<br />-Pour tous les contrôleurs de domaine spécifiés dans l’étendue de cette partition d’annuaire||
|Nom de zone de recherche inversée<br /><br />(Type IP)|: Zone de recherche inversée IPv4<br />: Zone de recherche inversée IPv6||
|Nom de zone de recherche inversée<br /><br />(ID de réseau)|10.0.0||

### <a name="BKMK_FndtnPrep_InstallDHCP"></a>L’installation de DHCP
Les tableaux de cette section répertorient les éléments de configuration pour la préinstallation et l’installation de DHCP.

##### <a name="pre-installation-configuration-items-for-dhcp"></a>Éléments de configuration de pré-installation pour DHCP
Les tableaux ci-après les éléments de configuration de pré-installation liste comme décrit dans [la configuration de tous les serveurs](#BKMK_configuringAll):

-   [Configurer une adresse IP statique](#BKMK_ip)

|Éléments de configuration|Exemples de valeurs|Valeurs|
|-----------------------|------------------|----------|
|Adresse IP|10.0.0.3||
|Masque de sous-réseau|255.255.255.0||
|Passerelle par défaut|10.0.0.1||
|Serveur DNS préféré|10.0.0.2||
|Serveur DNS auxiliaire|10.0.0.15||

-   [Renommer l’ordinateur](#BKMK_rename)

|Élément de configuration|Exemple de valeur|Valeur|
|----------------------|-----------------|---------|
|Nom de l’ordinateur|DHCP1||

##### <a name="dhcp-installation-configuration-items"></a>Éléments de configuration DHCP installation
Éléments de configuration pour la procédure de déploiement de réseau de base Windows Server [installer DHCP Dynamic Host Configuration Protocol ()](#BKMK_installDHCP):

|Éléments de configuration|Exemples de valeurs|Valeurs|
|-----------------------|------------------|----------|
|Liaisons de connexion réseau|Ethernet||
|Paramètres du serveur DNS|DC1||
|Adresse IP du serveur DNS préféré|10.0.0.2||
|Adresse IP de serveur DNS auxiliaire|10.0.0.15||
|Nom de l’étendue|Corp1||
|Adresse IP de début|10.0.0.1||
|Adresse IP de fin|10.0.0.254||
|Masque de sous-réseau|255.255.255.0||
|Passerelle par défaut (facultatif)|10.0.0.1||
|Durée du bail|8jours||
|Mode d’opération du serveur DHCP IPv6|Non activé||

#### <a name="BKMK_FndtnPrep_DHCP_Exclusn"></a>Création d’une plage d’exclusion dans DHCP
Éléments de configuration pour créer une plage d’exclusion lors de la création d’une étendue dans DHCP.

|Éléments de configuration|Exemples de valeurs|Valeurs|
|-----------------------|------------------|----------|
|Nom de l’étendue|Corp1||
|Description de l’étendue|Sous-réseau 1 du siège social||
|Adresse IP de début de plage d’exclusion|10.0.0.1||
|Adresse IP de fin de plage d’exclusion|10.0.0.15||

#### <a name="bkmk_NetFndtn_Pln_DHCP_NewScope"></a>Création d’une nouvelle étendue DHCP
Éléments de configuration pour la procédure de déploiement de réseau de base Windows Server [créer et activer une nouvelle étendue DHCP](#BKMK_newscopeDHCP):

|Éléments de configuration|Exemples de valeurs|Valeurs|
|-----------------------|------------------|----------|
|Nouveau nom de l’étendue|Corp2||
|Description de l’étendue|Sous-réseau du siège social 2||
|(Plage d’adresses IP)<br /><br />Adresse IP de début|10.0.1.1||
|(Plage d’adresses IP)<br /><br />Adresse IP de fin|10.0.1.254||
|Longueur|8||
|Masque de sous-réseau|255.255.255.0||
|(Plage d’exclusion) Adresse IP de début|10.0.1.1||
|Adresse IP de fin de plage d’exclusion|10.0.1.15||
|Durée du bail<br /><br />Jours<br /><br />Heures<br /><br />Minutes|-   8<br />-   0<br />-   0||
|Routeur (passerelle par défaut)<br /><br />Adresse IP|10.0.1.1||
|Domaine parent DNS|corp.contoso.com||
|Serveur DNS<br /><br />Adresse IP|10.0.0.2||

### <a name="BKMK_FndtnPrep_InstallNPS"></a>L’installation du serveur de stratégie réseau (facultatif)
Les tableaux de cette section répertorient les éléments de configuration pour la préinstallation et l’installation du serveur NPS.

##### <a name="pre-installation-configuration-items"></a>Éléments de configuration de pré-installation
Les trois tableaux suivants répertorient les éléments de configuration de pré-installation comme décrit dans [la configuration de tous les serveurs](#BKMK_configuringAll):

-   [Configurer une adresse IP statique](#BKMK_ip)

|Éléments de configuration|Exemples de valeurs|Valeurs|
|-----------------------|------------------|----------|
|Adresse IP|10.0.0.4||
|Masque de sous-réseau|255.255.255.0||
|Passerelle par défaut|10.0.0.1||
|Serveur DNS préféré|10.0.0.2||
|Serveur DNS auxiliaire|10.0.0.15||

-   [Renommer l’ordinateur](#BKMK_rename)

|Élément de configuration|Exemple de valeur|Valeur|
|----------------------|-----------------|---------|
|Nom de l’ordinateur|SERVEUR NPS1||

##### <a name="network-policy-server-installation-configuration-items"></a>Éléments de configuration d’installation du serveur de stratégie de réseau
Éléments de configuration pour les procédures de déploiement de WindowsServerCore réseau NPS [installer serveur NPS (Network Policy)](#BKMK_installNPS) et [inscrire le serveur NPS dans le domaine par défaut](#BKMK_registerNPS).

-   Aucun des éléments de configuration supplémentaires ne sont requis pour installer et inscrire un serveur NPS.

