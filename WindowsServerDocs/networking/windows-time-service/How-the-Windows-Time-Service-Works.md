---
ms.assetid: d1953097-63ea-4a0e-b860-2f3b7c175c41
title: Fonctionnement du service de temps Windows
description: ''
author: shortpatti
ms.author: pashort
manager: dougkim
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 2bf4a887218cd51e9c10954a75bbc1ba2112647f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405151"
---
# <a name="how-the-windows-time-service-works"></a>Fonctionnement du service de temps Windows

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 ou version ultérieure

**Dans cette section**  
  
-   [Architecture du service de temps Windows](#windows-time-service-architecture)  
  
-   [Protocoles de Temps de service de temps Windows](#windows-time-service-time-protocols)  
  
-   [Processus et interactions du service de temps Windows](#windows-time-service-processes-and-interactions)  
  
-   [Ports réseau utilisés par le service de temps Windows](#network-ports-used-by-windows-time-service)  
  
> [!NOTE]  
> Cette rubrique décrit le fonctionnement du service de temps Windows (W32Time). Pour plus d’informations sur la configuration du service de temps Windows, consultez [Configuration des systèmes pour une haute précision](configuring-systems-for-high-accuracy.md).
  
> [!NOTE]  
> Dans Windows Server 2003 et Microsoft Windows 2000 Server, le service d’annuaire est nommé Active Directory Service d’annuaire. Dans Windows Server 2008 et versions ultérieures, le service d’annuaire est nommé Active Directory Domain Services (AD DS). Le reste de cette rubrique fait référence aux services AD DS, mais les informations sont également applicables à Active Directory.  
  
Bien que le service de temps Windows ne soit pas une implémentation exacte du protocole NTP (Network Time Protocol), il utilise la suite complexe d’algorithmes définie dans les spécifications NTP pour s’assurer que les horloges sur les ordinateurs d’un réseau sont aussi précises que possible. Idéalement, toutes les horloges des ordinateurs d’un domaine AD DS sont synchronisées avec l’heure d’un ordinateur faisant autorité. De nombreux facteurs peuvent affecter la synchronisation de l’heure sur un réseau. Les facteurs suivants affectent souvent la précision de la synchronisation dans AD DS :  
  
-   Conditions réseau  
  
-   La précision de l’horloge matérielle de l’ordinateur ;  
  
-   La quantité de ressources processeur et réseau disponibles pour le service de temps Windows  
  
> [!IMPORTANT]  
> Avant Windows Server 2016, le service W32Time n’a pas été conçu pour répondre aux besoins des applications qui respectent le temps.  Toutefois, les mises à jour de Windows Server 2016 vous permettent désormais d’implémenter une solution pour une précision de 1 ms dans votre domaine.  Pour plus d’informations, consultez limite du temps et de l’assistance de [windows 2016](accurate-time.md) [pour configurer le service de temps Windows pour les environnements à haute précision](support-boundary.md) .  
  
Les ordinateurs qui synchronisent leur temps moins fréquemment ou qui ne sont pas joints à un domaine sont configurés par défaut pour se synchroniser avec time.windows.com.  Par conséquent, il est impossible de garantir l’exactitude du temps sur les ordinateurs qui ont des connexions réseau intermittentes ou inexistantes.  
  
Une forêt AD DS a une hiérarchie de synchronisation de l’heure prédéterminée. Le service de temps Windows synchronise l’heure entre les ordinateurs de la hiérarchie, avec les horloges de référence les plus précises en haut. Si plus d’une source de temps est configurée sur un ordinateur, l’heure Windows utilise des algorithmes NTP pour sélectionner la source de temps la plus appropriée à partir des sources configurées en fonction de la capacité de l’ordinateur à se synchroniser avec cette source de temps. Le service de temps Windows ne prend pas en charge la synchronisation réseau des homologues de diffusion ou de multidiffusion. Pour plus d’informations sur ces fonctionnalités NTP, consultez RFC 1305 dans la base de données RFC de l’IETF.  
  
Chaque ordinateur exécutant le service de temps Windows utilise le service pour maintenir l’heure la plus précise. Par défaut, les ordinateurs qui sont membres d’un domaine jouent le rôle de client de temps, par conséquent, dans la plupart des cas, il n’est pas nécessaire de configurer le service de temps Windows. Toutefois, le service de temps Windows peut être configuré pour demander du temps à partir d’une source de temps de référence désignée et peut également fournir du temps aux clients.
  
Le degré de précision de l’heure d’un ordinateur est appelé un strate. La source de temps la plus précise sur un réseau (par exemple, une horloge matérielle) occupe le niveau de couche le plus bas, ou en strate un. Cette source de temps précise est appelée « horloge de référence ». Un serveur NTP qui acquiert son temps directement à partir d’une horloge de référence occupe une strate d’un niveau supérieur à celui de l’horloge de référence. Les ressources qui acquièrent du temps à partir du serveur NTP sont en deux étapes à partir de l’horloge de référence et, par conséquent, occupent une strate qui est supérieure à la source de temps la plus précise, et ainsi de suite. À mesure que le nombre de couches d’un ordinateur augmente, le temps de son horloge système peut devenir moins précis. Par conséquent, le niveau de couche de n’importe quel ordinateur est un indicateur de la façon dont l’ordinateur est synchronisé avec la source de temps la plus précise.  
  
Lorsque le gestionnaire W32Time reçoit des exemples d’heure, il utilise des algorithmes spéciaux dans NTP pour déterminer lequel des exemples de temps est le plus approprié pour une utilisation. Le service de temps utilise également un autre ensemble d’algorithmes pour déterminer quelles sources de temps configurées sont les plus précises. Lorsque le service de temps a déterminé l’exemple de temps le plus approprié, en fonction des critères ci-dessus, il ajuste la fréquence d’horloge locale pour lui permettre de converger vers l’heure correcte. Si la différence de temps entre l’horloge locale et l’échantillon de temps précis sélectionné (également appelé décalage horaire) est trop grande pour être corrigée en ajustant le taux d’horloge local, le service de temps définit l’horloge locale sur l’heure correcte. Ce réglage du taux d’horloge ou du changement de temps d’horloge direct est appelé « discipline d’horloge ».  
  
## <a name="windows-time-service-architecture"></a>Architecture du service de temps Windows  
Le service de temps Windows est constitué des composants suivants :  
  
-   Gestionnaire de contrôle des services  
  
-   Service Manager de temps Windows  
  
-   Discipline de l’horloge  
  
-   Fournisseurs de temps  
  
La figure suivante illustre l’architecture du service de temps Windows.  
  
**Architecture du service de temps Windows**  
  
![Horloge Windows](../media/Windows-Time-Service/How-the-Windows-Time-Service-Works/trnt_sec_arcc.gif)
  
Le gestionnaire de contrôle des services est responsable du démarrage et de l’arrêt du service de temps Windows. Le Service Manager de temps Windows est chargé de lancer l’action des fournisseurs de temps NTP inclus dans le système d’exploitation. Le Service Manager de temps Windows contrôle toutes les fonctions du service de temps Windows et la fusion de tous les échantillons d’heure. En plus de fournir des informations sur l’état actuel du système, telles que la source de temps actuelle ou la dernière mise à jour de l’horloge système, le Service Manager de temps Windows est également chargé de créer des événements dans le journal des événements.  
  
Le processus de synchronisation de l’heure implique les étapes suivantes :  
  
-   Les fournisseurs d’entrée demandent des échantillons d’heure de réception à partir de sources de temps NTP configurées.  
  
-   Ces exemples de temps sont ensuite passés à la Service Manager de temps Windows, qui collecte tous les échantillons et les transmet au sous-composant de discipline d’horloge.  
  
-   Le sous-composant discipline de l’horloge applique les algorithmes NTP qui résultent de la sélection de l’exemple le plus opportun.  
  
-   Le sous-composant discipline de l’horloge ajuste l’heure de l’horloge système à l’heure la plus précise en ajustant la fréquence d’horloge ou en modifiant directement l’heure.  
  
Si un ordinateur a été désigné comme serveur de temps, il peut envoyer l’heure à un ordinateur demandant la synchronisation de l’heure à n’importe quel stade de ce processus.  
  
## <a name="windows-time-service-time-protocols"></a>Protocoles de Temps de service de temps Windows  

Les protocoles de temps déterminent la synchronisation des horloges de deux ordinateurs. Un protocole de temps est chargé de déterminer les meilleures informations disponibles en matière de temps et de converger les horloges pour s’assurer qu’une durée cohérente est conservée sur des systèmes distincts.  
  
Le service de temps Windows utilise le protocole NTP (Network Time Protocol) pour permettre la synchronisation de l’heure sur un réseau. NTP est un protocole Internet qui comprend les algorithmes de discipline nécessaires à la synchronisation des horloges. NTP est un protocole de temps plus précis que le protocole SNTP (simple Network Time Protocol) utilisé dans certaines versions de Windows. Toutefois, W32Time continue à prendre en charge SNTP pour permettre la compatibilité descendante avec les ordinateurs qui exécutent des services de temps basés sur SNTP, tels que Windows 2000.  
  
### <a name="network-time-protocol"></a>Protocole de temps réseau  
Le protocole NTP (Network Time Protocol) est le protocole de synchronisation d’heure par défaut utilisé par le service de temps Windows dans le système d’exploitation. NTP est un protocole de temps hautement évolutif et tolérant aux pannes. il s’agit du protocole utilisé le plus souvent pour synchroniser les horloges des ordinateurs à l’aide d’une référence horaire désignée.  
  
La synchronisation de l’heure NTP a lieu sur une période donnée et implique le transfert de paquets NTP sur un réseau. Les paquets NTP contiennent des horodatages qui incluent un exemple de temps provenant du client et du serveur participant à la synchronisation horaire.  
  
NTP s’appuie sur une horloge de référence pour définir l’heure la plus précise à utiliser et synchronise toutes les horloges sur un réseau avec cette horloge de référence. NTP utilise le temps universel coordonné (UTC, Universal Time Coordinated) comme norme universelle pour l’heure actuelle. L’heure UTC est indépendante des fuseaux horaires et permet l’utilisation de NTP partout dans le monde, quels que soient les paramètres de fuseau horaire.  
  
#### <a name="ntp-algorithms"></a>Algorithmes NTP  
NTP comprend deux algorithmes, un algorithme de filtrage de l’horloge et un algorithme de sélection de l’horloge, pour aider le service de temps Windows à déterminer le meilleur exemple de temps. L’algorithme de filtrage de l’horloge est conçu pour passer en revue les exemples de temps reçus des sources de temps interrogées et déterminer les meilleurs exemples de temps à partir de chaque source. L’algorithme de sélection de l’horloge détermine ensuite le serveur de temps le plus précis sur le réseau. Ces informations sont ensuite transmises à l’algorithme de discipline d’horloge, qui utilise les informations collectées pour corriger l’horloge locale de l’ordinateur, tout en compensant les erreurs dues à la latence du réseau et à l’inexactitude de l’horloge de l’ordinateur.  
  
Les algorithmes NTP sont plus précis dans des conditions de charge de réseau et de réseau légères à modérer. Comme pour tout algorithme qui prend en compte le temps de transit réseau, les algorithmes NTP peuvent fonctionner mal dans des conditions d’encombrement réseau extrême. Pour plus d’informations sur les algorithmes NTP, consultez RFC 1305 dans la base de données RFC de l’IETF.  
  
#### <a name="ntp-time-provider"></a>Fournisseur de temps NTP  
Le service de temps Windows est un package de synchronisation à temps complet qui peut prendre en charge un large éventail de périphériques matériels et de protocoles de temps. Pour activer cette prise en charge, le service utilise des fournisseurs de temps enfichables. Un fournisseur de temps est chargé d’obtenir des horodatages précis (à partir du réseau ou du matériel) ou de fournir ces horodatages à d’autres ordinateurs sur le réseau.  
  
Le fournisseur NTP est le fournisseur de temps standard inclus dans le système d’exploitation. Le fournisseur NTP suit les normes spécifiées par NTP version 3 pour un client et un serveur, et peut interagir avec les clients et les serveurs SNTP pour assurer la compatibilité descendante avec Windows 2000 et d’autres clients SNTP. Le fournisseur NTP du service de temps Windows se compose des deux parties suivantes :  
  
-   **Fournisseur de sortie NtpServer.** Il s’agit d’un serveur de temps qui répond aux demandes de temps du client sur le réseau.  
  
-   **Fournisseur d’entrée NtpClient.** Il s’agit d’un client de temps qui obtient des informations de temps à partir d’une autre source, soit un périphérique matériel, soit un serveur NTP, et peut retourner des exemples de temps qui sont utiles pour la synchronisation de l’horloge locale.  
  
Bien que les opérations réelles de ces deux fournisseurs soient étroitement liées, elles apparaissent indépendamment du service de temps. À compter de Windows 2000 Server, lorsqu’un ordinateur Windows est connecté à un réseau, il est configuré en tant que client NTP. En outre, les ordinateurs qui exécutent le service de temps Windows essaient uniquement de synchroniser l’heure avec un contrôleur de domaine ou une source de temps spécifiée manuellement par défaut. Il s’agit des fournisseurs de temps préférés, car ils sont automatiquement disponibles et sont des sources de temps sécurisées.  
  
#### <a name="ntp-security"></a>Sécurité NTP  

Au sein d’une forêt AD DS, le service de temps Windows s’appuie sur les fonctionnalités de sécurité de domaine standard pour appliquer l’authentification des données de temps. La sécurité des paquets NTP envoyés entre un ordinateur membre du domaine et un contrôleur de domaine local qui agit en tant que serveur de temps est basée sur l’authentification par clé partagée. Le service de temps Windows utilise la clé de session Kerberos de l’ordinateur pour créer des signatures authentifiées sur les paquets NTP envoyés sur le réseau. Les paquets NTP ne sont pas transmis dans le canal sécurisé Net Logon. Au lieu de cela, lorsqu’un ordinateur demande l’heure à partir d’un contrôleur de domaine dans la hiérarchie de domaine, le service de temps Windows requiert l’authentification de l’heure. Le contrôleur de domaine retourne ensuite les informations requises sous la forme d’une valeur 64 bits qui a été authentifiée à l’aide de la clé de session du service accès réseau. Si le paquet NTP retourné n’est pas signé avec la clé de session de l’ordinateur ou s’il est signé de manière incorrecte, l’heure est rejetée. Tous ces échecs d’authentification sont consignés dans le journal des événements. De cette façon, le service de temps Windows fournit la sécurité pour les données NTP dans une forêt AD DS.  
  
En règle générale, les clients de temps Windows obtiennent automatiquement un temps précis pour la synchronisation à partir de contrôleurs de domaine dans le même domaine. Dans une forêt, les contrôleurs de domaine d’un domaine enfant synchronisent l’heure avec les contrôleurs de domaine dans leurs domaines parents. Lorsqu’un serveur de temps renvoie un paquet NTP authentifié à un client qui demande l’heure, le paquet est signé au moyen d’une clé de session Kerberos définie par un compte de confiance interdomaine. Le compte de confiance interdomaine est créé lorsqu’un nouveau domaine de AD DS rejoint une forêt et que le service accès réseau gère la clé de session. De cette façon, le contrôleur de domaine qui est configuré comme fiable dans le domaine racine de forêt devient la source de temps authentifiée pour tous les contrôleurs de domaine dans les domaines parents et enfants, et indirectement pour tous les ordinateurs situés dans l’arborescence de domaine.  
  
Le service de temps Windows peut être configuré pour fonctionner entre les forêts, mais il est important de noter que cette configuration n’est pas sécurisée. Par exemple, un serveur NTP peut être disponible dans une forêt différente. Toutefois, étant donné que cet ordinateur se trouve dans une autre forêt, il n’existe pas de clé de session Kerberos avec laquelle signer et authentifier les paquets NTP. Pour obtenir une synchronisation de l’heure précise à partir d’un ordinateur situé dans une autre forêt, le client a besoin d’un accès réseau à cet ordinateur et le service de temps doit être configuré pour utiliser une source de temps spécifique située dans l’autre forêt. Si un client est configuré manuellement pour accéder au temps à partir d’un serveur NTP en dehors de sa propre hiérarchie de domaine, les paquets NTP envoyés entre le client et le serveur de temps ne sont pas authentifiés et ne sont donc pas sécurisés. Même avec l’implémentation des approbations de forêt, le service de temps Windows n’est pas sécurisé entre les forêts. Bien que le canal sécurisé Net Logon soit le mécanisme d’authentification pour le service de temps Windows, l’authentification sur les forêts n’est pas prise en charge.  
  
#### <a name="hardware-devices-that-are-supported-by-the-windows-time-service"></a>Périphériques matériels pris en charge par le service de temps Windows  
Les horloges matérielles, telles que les horloges GPS ou radio, sont souvent utilisées comme périphériques de référence très précis. Par défaut, le fournisseur de temps NTP du service de temps Windows ne prend pas en charge la connexion directe d’un périphérique matériel à un ordinateur, bien qu’il soit possible de créer un fournisseur de temps indépendant basé sur le logiciel qui prend en charge ce type de connexion. Ce type de fournisseur, conjointement avec le service de temps Windows, peut fournir une référence temporelle fiable et stable.  
  
Les périphériques matériels, tels qu’un cesium Clock ou un récepteur Global Positioning System (GPS), fournissent une heure actuelle précise en suivant une norme pour obtenir une définition précise du temps. Les horloges cesium sont extrêmement stables et ne sont pas affectées par des facteurs tels que la température, la pression ou l’humidité, mais elles sont également très coûteuses. Un récepteur GPS est beaucoup moins onéreux à fonctionner et est également une horloge de référence précise. Les destinataires GPS obtiennent leur temps à partir de satellites qui obtiennent leur temps à partir d’une horloge cesium. Sans l’utilisation d’un fournisseur de temps indépendant, les serveurs de temps Windows peuvent obtenir leur temps en se connectant à un serveur NTP externe, qui est connecté à un périphérique matériel par le biais d’un téléphone ou d’Internet. Les organisations telles que le États-Unis observatoire naval fournissent des serveurs NTP connectés à des horloges de référence extrêmement fiables.  
  
De nombreux récepteurs GPS et d’autres appareils de temps peuvent fonctionner comme serveurs NTP sur un réseau. Vous pouvez configurer votre forêt AD DS pour synchroniser l’heure à partir de ces périphériques matériels externes uniquement s’ils font également office de serveurs NTP sur votre réseau. Pour ce faire, configurez le contrôleur de domaine fonctionnant en tant qu’émulateur de contrôleur de domaine principal (PDC) dans la racine de votre forêt pour la synchronisation avec le serveur NTP fourni par le périphérique GPS. Pour ce faire, consultez [configurer le service de temps Windows sur l’émulateur de contrôleur de domaine principal dans le domaine racine de forêt](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29).  
  
### <a name="simple-network-time-protocol"></a>Protocole de temps réseau simple  
Le protocole SNTP (simple Network Time Protocol) est un protocole de temps simplifié qui est destiné aux serveurs et aux clients qui ne nécessitent pas le degré de précision fourni par NTP. SNTP, une version plus rudimentaire de NTP, est le protocole principal utilisé dans Windows 2000. Étant donné que les formats de paquets réseau de SNTP et NTP sont identiques, les deux protocoles sont interopérables. La principale différence entre les deux est que le protocole SNTP ne dispose pas des systèmes de gestion des erreurs et de filtrage complexes fournis par NTP. Pour plus d’informations sur le protocole simple Network Time, consultez RFC 1769 dans la base de données RFC de l’IETF.  
  
### <a name="time-protocol-interoperability"></a>Interopérabilité des protocoles de temps  
Le service de temps Windows peut fonctionner dans un environnement mixte d’ordinateurs exécutant Windows 2000, Windows XP et Windows Server 2003, car le protocole SNTP utilisé dans Windows 2000 est interopérable avec le protocole NTP dans Windows XP et Windows Server 2003.  
  
Le service de temps de Windows NT Server 4,0, appelé TimeServ, synchronise le temps sur un réseau Windows NT 4,0. TimeServ est une fonctionnalité complémentaire disponible dans le *Kit de ressources Microsoft Windows NT 4,0* et ne fournit pas le degré de fiabilité de la synchronisation horaire requis par Windows Server 2003.  
  
Le service de temps Windows peut interagir avec des ordinateurs exécutant Windows NT 4,0, car ils peuvent synchroniser l’heure avec les ordinateurs exécutant Windows 2000 ou Windows Server 2003. Toutefois, un ordinateur exécutant Windows 2000 ou Windows Server 2003 ne découvre pas automatiquement les serveurs de temps Windows NT 4,0. Par exemple, si votre domaine est configuré pour synchroniser l’heure à l’aide de la méthode de synchronisation basée sur la hiérarchie de domaine et que vous souhaitez que les ordinateurs de la hiérarchie de domaine synchronisent l’heure avec un contrôleur de domaine Windows NT 4,0, vous devez les configurer. les ordinateurs manuellement pour se synchroniser avec les contrôleurs de domaine Windows NT 4,0.  
  
Windows NT 4,0 utilise un mécanisme plus simple pour la synchronisation de l’heure que celui utilisé par le service de temps Windows. Par conséquent, pour garantir une synchronisation précise de l’heure sur votre réseau, il est recommandé de mettre à niveau tous les contrôleurs de domaine Windows NT 4,0 vers Windows 2000 ou Windows Server 2003.  
  
## <a name="windows-time-service-processes-and-interactions"></a>Processus et interactions du service de temps Windows  

Le service de temps Windows est conçu pour synchroniser les horloges des ordinateurs sur un réseau. Le processus de synchronisation de l’heure du réseau, également appelé « convergence temporelle », intervient sur un réseau, car chaque ordinateur accède au temps à partir d’un serveur de temps plus précis. La convergence temporelle implique un processus par lequel un serveur faisant autorité fournit l’heure actuelle aux ordinateurs clients sous la forme de paquets NTP. Les informations fournies dans un paquet indiquent si un ajustement doit être effectué à l’heure actuelle de l’ordinateur pour qu’il soit synchronisé avec le serveur le plus précis.  
  
Dans le cadre du processus de convergence, les membres du domaine essaient de synchroniser l’heure avec n’importe quel contrôleur de domaine situé dans le même domaine. Si l’ordinateur est un contrôleur de domaine, il tente de se synchroniser avec un contrôleur de domaine plus faisant autorité.  
  
Les ordinateurs qui exécutent Windows XP Édition personnelle ou ceux qui ne sont pas joints à un domaine n’essaient pas de se synchroniser avec la hiérarchie de domaine, mais ils sont configurés par défaut pour obtenir le temps à partir de time.windows.com.  
  
Pour établir un ordinateur exécutant Windows Server 2003 comme faisant autorité, l’ordinateur doit être configuré pour être une source de temps fiable. Par défaut, le premier contrôleur de domaine installé sur un domaine Windows Server 2003 est automatiquement configuré pour être une source de temps fiable. Étant donné qu’il s’agit de l’ordinateur faisant autorité pour le domaine, il doit être configuré pour se synchroniser avec une source de temps externe plutôt qu’avec la hiérarchie de domaine. De même, par défaut, tous les autres membres de domaine Windows Server 2003 sont configurés pour se synchroniser avec la hiérarchie de domaine.  
  
Une fois que vous avez établi un réseau Windows Server 2003, vous pouvez configurer le service de temps Windows de façon à utiliser l’une des options suivantes pour la synchronisation :  
  
-   Synchronisation basée sur la hiérarchie de domaine  
  
-   Source de synchronisation spécifiée manuellement  
  
-   Tous les mécanismes de synchronisation disponibles  
  
-   Aucune synchronisation.  
  
Chacun de ces types de synchronisation est abordé dans la section suivante.  
  
### <a name="domain-hierarchy-based-synchronization"></a>Synchronisation basée sur la hiérarchie de domaine  

La synchronisation basée sur une hiérarchie de domaine utilise la hiérarchie de domaine AD DS pour rechercher une source fiable avec laquelle synchroniser l’heure. En fonction de la hiérarchie de domaine, le service de temps Windows détermine la précision de chaque serveur de temps. Dans une forêt Windows Server 2003, l’ordinateur qui détient le rôle de maître d’opérations de l’émulateur du contrôleur de domaine principal (PDC), situé dans le domaine racine de la forêt, contient la position de la source de temps la plus appropriée, sauf si une autre source de temps fiable a été configurée. L’illustration suivante montre un chemin de synchronisation de l’heure entre les ordinateurs d’une hiérarchie de domaines.  
  
**Synchronisation de l’heure dans une hiérarchie de AD DS**  
![Windows temps @ no__t-1
  
#### <a name="reliable-time-source-configuration"></a>Configuration de la source de temps fiable  
Un ordinateur configuré pour être une source de temps fiable est identifié comme la racine du service de temps. La racine du service de temps est le serveur faisant autorité pour le domaine et est généralement configuré pour récupérer l’heure à partir d’un serveur NTP externe ou d’un périphérique matériel. Un serveur de temps peut être configuré en tant que source de temps fiable pour optimiser la façon dont le temps est transféré dans l’ensemble de la hiérarchie du domaine. Si un contrôleur de domaine est configuré pour être une source de temps fiable, le service Net Logon annonce que ce contrôleur de domaine en tant que source de temps fiable lorsqu’il se connecte au réseau. Lorsque d’autres contrôleurs de domaine recherchent une source de temps avec laquelle se synchroniser, ils choisissent d’abord une source fiable, si celle-ci est disponible.  
  
#### <a name="time-source-selection"></a>Sélection de la source de temps  
Le processus de sélection de la source de temps peut créer deux problèmes sur un réseau :  
  
-   Cycles de synchronisation supplémentaires.  
  
-   Augmentation du volume du trafic réseau.  
  
Un cycle dans le réseau de synchronisation se produit lorsque l’heure reste cohérente entre un groupe de contrôleurs de domaine et que la même heure est partagée entre eux en continu sans resynchronisation avec une autre source de temps fiable. L’algorithme de sélection de la source de temps du service de temps Windows est conçu pour protéger contre ces types de problèmes.  
  
Un ordinateur utilise l’une des méthodes suivantes pour identifier une source de temps à synchroniser avec :  
  
-   Si l’ordinateur n’est pas membre d’un domaine, il doit être configuré pour se synchroniser avec une source de temps spécifiée.  
  
-   Si l’ordinateur est un serveur membre ou une station de travail au sein d’un domaine, par défaut, il suit la hiérarchie AD DS et synchronise son temps avec un contrôleur de domaine dans son domaine local qui exécute actuellement le service de temps Windows.  
  
Si l’ordinateur est un contrôleur de domaine, il effectue jusqu’à six requêtes pour trouver un autre contrôleur de domaine avec lequel se synchroniser. Chaque requête est conçue pour identifier une source de temps avec certains attributs, tels qu’un type de contrôleur de domaine, un emplacement particulier et s’il s’agit d’une source de temps fiable. La source de temps doit également respecter les contraintes suivantes :  
  
-   Une source de temps fiable peut uniquement se synchroniser avec un contrôleur de domaine dans le domaine parent.  
  
-   Un émulateur de contrôleur de domaine principal peut se synchroniser avec une source de temps fiable dans son propre domaine ou dans n’importe quel contrôleur de domaine du domaine parent.  
  
Si le contrôleur de domaine n’est pas en mesure de se synchroniser avec le type de contrôleur de domaine qu’il interroge, la requête n’est pas effectuée. Le contrôleur de domaine sait à quel type d’ordinateur il peut obtenir l’heure avant de procéder à la requête. Par exemple, un émulateur de contrôleur de domaine principal local ne tente pas d’interroger les numéros trois ou six, car un contrôleur de domaine ne tente pas de se synchroniser avec lui-même.  
  
Le tableau suivant répertorie les requêtes qu’un contrôleur de domaine effectue pour rechercher une source de temps et l’ordre dans lequel les requêtes sont effectuées.  
  
**Requêtes de source de temps du contrôleur de domaine**  
  
|Numéro de requête|Contrôleur de domaine|Location|Fiabilité de la source de temps|  
|----------------|---------------------|------------|------------------------------|  
|1|Contrôleur de domaine parent|Sur site|Préfère une source de temps fiable, mais elle peut se synchroniser avec une source de temps non fiable si c’est tout ce qui est disponible.|  
|2|Contrôleur de domaine local|Sur site|Se synchronise uniquement avec une source de temps fiable.|  
|3|Émulateur de contrôleur de domaine principal local|Sur site|Ne s’applique pas.<br /><br />Un contrôleur de domaine ne tente pas de se synchroniser avec lui-même.|  
|4|Contrôleur de domaine parent|Hors site|Préfère une source de temps fiable, mais elle peut se synchroniser avec une source de temps non fiable si c’est tout ce qui est disponible.|  
|5|Contrôleur de domaine local|Hors site|Se synchronise uniquement avec une source de temps fiable.|  
|6\.|Émulateur de contrôleur de domaine principal local|Hors site|Ne s’applique pas.<br /><br />Un contrôleur de domaine ne tente pas de se synchroniser avec lui-même.| 
  
**Remarque**  
  
-   Un ordinateur ne se synchronise jamais avec lui-même. Si l’ordinateur qui tente la synchronisation est l’émulateur de contrôleur de domaine principal local, il n’essaie pas les requêtes 3 ou 6.  
  
Chaque requête retourne une liste de contrôleurs de domaine qui peuvent être utilisés comme source de temps. L’heure Windows affecte à chaque contrôleur de domaine un score basé sur la fiabilité et l’emplacement du contrôleur de domaine. Le tableau suivant répertorie les scores attribués par le temps Windows à chaque type de contrôleur de domaine.  
  
**Détermination du score**  
  
|État du contrôleur de domaine|Score|  
|----------------------------|---------|  
|Contrôleur de domaine situé dans le même site|8|  
|Contrôleur de domaine marqué comme source de temps fiable|4|  
|Contrôleur de domaine situé dans le domaine parent|2|  
|Contrôleur de domaine qui est un émulateur de contrôleur de domaine principal|1|  
  
Lorsque le service de temps Windows détermine qu’il a identifié le contrôleur de domaine avec le meilleur score possible, aucune autre requête n’est effectuée. Les scores attribués par le service de temps sont cumulatifs, ce qui signifie qu’un émulateur de contrôleur de domaine principal situé sur le même site reçoit un score de neuf.  
  
Si la racine du service de temps n’est pas configurée pour se synchroniser avec une source externe, l’horloge matérielle interne de l’ordinateur régit le temps.  
  
### <a name="manually-specified-synchronization"></a>Synchronisation spécifiée manuellement  
La synchronisation spécifiée manuellement vous permet de désigner un pair ou une liste d’homologues à partir desquels un ordinateur obtient du temps. Si l’ordinateur n’est pas membre d’un domaine, il doit être configuré manuellement pour se synchroniser avec une source de temps spécifiée. Un ordinateur membre d’un domaine est configuré par défaut pour être synchronisé à partir de la hiérarchie du domaine. la synchronisation spécifiée manuellement est particulièrement utile pour la racine de la forêt du domaine ou pour les ordinateurs qui ne sont pas joints à un domaine. La spécification manuelle d’un serveur NTP externe à synchroniser avec l’ordinateur faisant autorité pour votre domaine fournit un temps fiable. Toutefois, la configuration de l’ordinateur faisant autorité pour votre domaine en vue d’une synchronisation avec une horloge matérielle est en fait une meilleure solution pour fournir le temps le plus précis et sécurisé à votre domaine.  
  
Les sources de temps spécifiées manuellement ne sont pas authentifiées, sauf si un fournisseur de temps spécifique y est écrit et qu’elles sont donc vulnérables aux attaquants. En outre, si un ordinateur se synchronise avec une source spécifiée manuellement plutôt que son contrôleur de domaine d’authentification, les deux ordinateurs peuvent ne pas être synchronisés, provoquant ainsi l’échec de l’authentification Kerberos. Cela peut entraîner l’échec d’autres actions nécessitant l’authentification réseau, telles que l’impression ou le partage de fichiers. Si seule la racine de la forêt est configurée pour se synchroniser avec une source externe, tous les autres ordinateurs de la forêt restent synchronisés les uns avec les autres, ce qui complique les attaques par relecture.  
  
### <a name="all-available-synchronization-mechanisms"></a>Tous les mécanismes de synchronisation disponibles  

L’option « tous les mécanismes de synchronisation disponibles » est la méthode de synchronisation la plus précieuse pour les utilisateurs sur un réseau. Cette méthode permet la synchronisation avec la hiérarchie de domaine et peut également fournir une autre source de temps si la hiérarchie de domaine devient indisponible, en fonction de la configuration. Si le client ne parvient pas à synchroniser l’heure avec la hiérarchie de domaine, la source de temps revient automatiquement à la source de temps spécifiée par le paramètre **NtpServer** . Cette méthode de synchronisation est plus susceptible de fournir un délai précis aux clients.  

### <a name="stopping-time-synchronization"></a>Synchronisation de l’heure d’arrêt  
Dans certaines situations, vous souhaiterez peut-être empêcher un ordinateur de synchroniser son temps. Par exemple, si un ordinateur tente de se synchroniser à partir d’une source de temps sur Internet ou à partir d’un autre site via un réseau étendu au moyen d’une connexion d’accès à distance, il peut entraîner des frais téléphoniques coûteux. Lorsque vous désactivez la synchronisation sur cet ordinateur, vous empêchez l’ordinateur de tenter d’accéder à une source de temps via une connexion d’accès à distance.  
  
Vous pouvez également désactiver la synchronisation pour empêcher la génération d’erreurs dans le journal des événements. Chaque fois qu’un ordinateur tente de se synchroniser avec une source de temps qui n’est pas disponible, il génère une erreur dans le journal des événements. Si une source de temps est déconnectée du réseau pour la maintenance planifiée et que vous n’envisagez pas de reconfigurer le client pour la synchronisation à partir d’une autre source, vous pouvez désactiver la synchronisation sur le client pour l’empêcher de tenter une synchronisation pendant que le serveur de temps n’est pas disponible.  
  
Il est utile de désactiver la synchronisation sur l’ordinateur désigné comme racine du réseau de synchronisation. Cela indique que l’ordinateur racine approuve son horloge locale. Si la racine de la hiérarchie de synchronisation n’est pas définie sur **nosync** et si elle ne peut pas se synchroniser avec une autre source de temps, les clients n’acceptent pas le paquet envoyé par cet ordinateur car son heure ne peut pas être approuvée.
  
Les seuls serveurs de temps qui sont approuvés par les clients, même s’ils n’ont pas été synchronisés avec une autre source de temps, sont ceux qui ont été identifiés par le client comme des serveurs de temps fiables.  
  
### <a name="disabling-the-windows-time-service"></a>Désactivation du service de temps Windows  
Le service de temps Windows (W32Time) peut être complètement désactivé. Si vous choisissez d’implémenter un produit de synchronisation de temps tiers qui utilise NTP, vous devez désactiver le service de temps Windows. Cela est dû au fait que tous les serveurs NTP doivent accéder au port UDP (User Datagram Protocol) 123, et tant que le service de temps Windows est en cours d’exécution sur le système d’exploitation Windows Server 2003, le port 123 reste réservé par l’heure de Windows.  
  
## <a name="network-ports-used-by-windows-time-service"></a>Ports réseau utilisés par le service de temps Windows  
Le service de temps Windows communique sur un réseau pour identifier les sources de temps fiables, obtenir des informations sur l’heure et fournir des informations de temps à d’autres ordinateurs. Il effectue cette communication comme défini par les RFC NTP et SNTP.  
  
**Affectations de port pour le service de temps Windows**  
  
|Nom du service|UDP|TCP|  
|----------------|-------|-------|  
|NTP|123|N/A|  
|SNTP|123|N/A|  
  
## <a name="see-also"></a>Voir aussi  
[Référence technique du service de temps windows](windows-time-service-tech-ref.md)
[Outils et paramètres du service de temps Windows](Windows-Time-Service-Tools-and-Settings.md)
[article 902229 de la base de connaissances Microsoft](https://go.microsoft.com/fwlink/?LinkId=186066)