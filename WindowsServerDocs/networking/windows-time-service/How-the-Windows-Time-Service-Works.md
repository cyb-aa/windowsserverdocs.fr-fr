---
ms.assetid: d1953097-63ea-4a0e-b860-2f3b7c175c41
title: Fonctionnement du service de temps Windows
description: ''
author: eross-msft
ms.author: lizross
manager: dougkim
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: d8532dedb6473a34591a1f160a94a785cc4ba367
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315172"
---
# <a name="how-the-windows-time-service-works"></a>Fonctionnement du service de temps Windows

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 ou version ultérieure

**Dans cette section**  
  
-   [Architecture du service de temps Windows](#windows-time-service-architecture)  
  
-   [Protocole de temps du service de temps Windows](#windows-time-service-time-protocols)  
  
-   [Processus et interactions du service de temps Windows](#windows-time-service-processes-and-interactions)  
  
-   [Ports réseau utilisés par le service de temps Windows](#network-ports-used-by-windows-time-service)  
  
> [!NOTE]  
> Cette rubrique explique uniquement le fonctionnement du service de temps Windows (W32Time). Pour savoir comment configurer le service de temps Windows, consultez [Configuration de systèmes de haute précision](configuring-systems-for-high-accuracy.md).
  
> [!NOTE]  
> Dans Windows Server 2003 et Microsoft Windows 2000 Server, le service d’annuaire se nomme Services d’annuaire Active Directory. Dans Windows Server 2008 et les versions ultérieures, le service d’annuaire se nomme Active Directory Domain Services (AD DS). Le reste de cette rubrique fait référence aux services AD DS, mais les informations sont également applicables à Active Directory.  
  
Bien que le service de temps Windows ne soit pas une implémentation exacte du protocole NTP (Network Time Protocol), il utilise la suite complexe d’algorithmes définie dans les spécifications NTP pour faire en sorte que les horloges des ordinateurs d’un réseau soient aussi précises que possible. Idéalement, toutes les horloges d’ordinateurs d’un domaine AD DS doivent être synchronisées avec l’heure d’un ordinateur de référence. De nombreux facteurs peuvent affecter la synchronisation de l’heure sur un réseau. Voici les facteurs qui sont souvent préjudiciables à la précision de la synchronisation dans AD DS :  
  
-   Conditions réseau  
  
-   Précision de l’horloge matérielle de l’ordinateur  
  
-   Quantité de ressources processeur et réseau disponibles pour le service de temps Windows  
  
> [!IMPORTANT]  
> Avant Windows Server 2016, le service W32Time n’était pas conçu pour répondre aux besoins des applications sensibles au facteur temps.  Cependant, les mises à jour apportées à Windows Server 2016 vous permettent désormais d’implémenter une solution pour une précision de 1 ms dans votre domaine.  Pour plus d’informations, consultez [Précision de l’heure dans Windows Server 2016](accurate-time.md) et [Limites de prise en charge pour configurer le service de temps Windows pour les environnements de haute précision](support-boundary.md).  
  
Par défaut, les ordinateurs qui synchronisent leur heure avec moins fréquence ou qui ne sont pas joints à un domaine sont configurés pour se synchroniser avec time.windows.com.  Par conséquent, il est impossible de garantir la précision de l’heure sur les ordinateurs qui ont des connexions réseau intermittentes ou inexistantes.  
  
Une forêt AD DS a une hiérarchie de synchronisation d’heure prédéterminée. Le service de temps Windows synchronise l’heure entre les ordinateurs de la hiérarchie, au sommet de laquelle se trouvent les horloges de référence les plus précises. Si plusieurs sources de temps sont configurées sur un ordinateur, le service de temps Windows utilise les algorithmes NTP pour sélectionner la meilleure source de temps parmi celles configurées en tenant compte de la capacité de l’ordinateur à se synchroniser avec celles-ci. Le service de temps Windows ne prend pas en charge la synchronisation réseau à partir d’homologues de diffusion ou de multidiffusion. Pour plus d’informations sur ces fonctionnalités NTP, consultez la RFC 1305 dans la base de données IETF RFC.  
  
L’objectif pour chaque ordinateur exécutant le service de temps Windows est de maintenir la précision de l’heure. Sachant que les ordinateurs membres d’un domaine jouent par défaut le rôle de client temps, dans la plupart des cas, il n’est pas nécessaire de configurer le service de temps Windows. Cependant, le service de temps Windows peut être configuré pour demander l’heure à une source de temps de référence désignée et peut aussi fournir l’heure aux clients.
  
Le degré de précision de l’heure d’un ordinateur s’appelle une strate. La source de temps la plus précise sur un réseau (par exemple une horloge matérielle) occupe la strate la plus basse, à savoir la strate 1. Cette source de temps précise s’appelle une horloge de référence. Un serveur NTP qui obtient son heure directement auprès d’une horloge de référence occupe la strate juste au-dessus de celle de l’horloge de référence. Les ressources qui obtiennent l’heure auprès du serveur NTP sont distantes de l’horloge de référence de deux niveaux. Par conséquent, elles occupent une strate située à deux niveaux au-dessus de la source de temps la plus précise, et ainsi de suite. Plus le numéro de strate d’un ordinateur est élevé, plus la précision de son heure système est susceptible de diminuer. Par conséquent, le niveau de strate d’un ordinateur est une indication de sa proximité vis-à-vis de la source de temps la plus précise.  
  
Quand le gestionnaire W32Time reçoit des échantillons d’heure, il utilise des algorithmes spéciaux NTP pour déterminer ceux qu’il est préférable d’utiliser. Le service de temps utilise par ailleurs un autre jeu d’algorithmes pour identifier la source de temps la plus précise parmi celles configurées. Une fois que le service de temps a identifié le meilleur échantillon d’heure, sur la base des critères précédents, il ajuste la fréquence d’horloge locale pour la faire converger vers l’heure exacte. Si la différence d’heure entre l’horloge locale et l’échantillon d’heure précis sélectionné (ce qui s’appelle aussi une asymétrie temporelle) est trop importante pour être corrigée par l’ajustement de la fréquence d’horloge locale, le service de temps met l’horloge locale à la bonne heure. Cet ajustement de la fréquence d’horloge ou le réglage direct de l’heure est ce que l’on appelle la « discipline d’horloge ».  
  
## <a name="windows-time-service-architecture"></a>Architecture du service de temps Windows  
Le service de temps Windows se compose des éléments suivants :  
  
-   Gestionnaire de contrôle des services  
  
-   Gestionnaire du service de temps Windows  
  
-   Discipline d’horloge  
  
-   Fournisseurs de temps  
  
Le schéma suivant illustre l’architecture du service de temps Windows.  
  
**Architecture du service de temps Windows**  
  
![Horloge Windows](../media/Windows-Time-Service/How-the-Windows-Time-Service-Works/trnt_sec_arcc.gif)
  
Le gestionnaire de contrôle des services est chargé de démarrer et d’arrêter le service de temps Windows. Le gestionnaire du service de temps Windows est chargé de lancer l’action des fournisseurs de temps NTP intégrés au système d’exploitation. Le gestionnaire du service de temps Windows contrôle toutes les fonctions du service de temps Windows et assure le regroupement de tous les échantillons d’heure. En plus de fournir des informations sur l’état actuel du système, notamment la source de temps actuelle ou la date de la dernière mise à jour de l’horloge système, le gestionnaire du service de temps Windows est aussi chargé de créer des événements dans le journal des événements.  
  
Le processus de synchronisation de l’heure comprend les étapes suivantes :  
  
-   Les fournisseurs en entrée demandent des échantillons d’heure aux sources de temps NTP configurées et les réceptionnent.  
  
-   Ces échantillons d’heure sont transmis au gestionnaire du service de temps Windows, qui collecte tous les échantillons et les transmet au sous-composant Discipline d’horloge.  
  
-   Le sous-composant Discipline d’horloge applique les algorithmes NTP, ce qui se traduit par la sélection du meilleur l’échantillon d’heure.  
  
-   Le sous-composant Discipline d’horloge ajuste l’heure de l’horloge système avec la plus grande précision possible en ajustant la fréquence d’horloge ou en réglant directement l’heure.  
  
Si un ordinateur a été désigné serveur de temps, il peut envoyer l’heure à un ordinateur qui demande une synchronisation de l’heure à n’importe quel stade du processus.  
  
## <a name="windows-time-service-time-protocols"></a>Protocoles de temps du service de temps Windows  

Les protocoles de temps déterminent le niveau de synchronisation entre les horloges de deux ordinateurs. Un protocole de temps est chargé d’identifier les meilleures informations d’heure disponibles et de faire converger les horloges de sorte que les différents systèmes restent à la même heure.  
  
Le service de temps Windows utilise le protocole NTP (Network Time Protocol) pour synchroniser l’heure sur un réseau. NTP est un protocole de temps Internet qui comprend les algorithmes de discipline nécessaires à la synchronisation des horloges. Ce protocole de temps est plus précis que SNTP (Simple Network Time Protocol) qui est utilisé dans certaines versions de Windows. Cependant, W32Time continue de prendre en charge SNTP pour permettre une compatibilité descendante avec les ordinateurs qui exécutent des services de temps SNTP, comme Windows 2000.  
  
### <a name="network-time-protocol"></a>Protocole NTP  
Le protocole NTP (Network Time Protocol) est le protocole de synchronisation d’heure que le service de temps Windows utilise par défaut dans le système d’exploitation. NTP est un protocole de temps tolérant aux pannes et hautement scalable qui est le plus souvent utilisé pour synchroniser les horloges d’ordinateurs à partir d’une référence de temps désignée.  
  
La synchronisation d’heure NTP se produit au cours d’une période donnée et implique le transfert de paquets NTP sur un réseau. Les paquets NTP contiennent des horodatages constitués d’un échantillon d’heure du client et du serveur participant à la synchronisation de l’heure.  
  
NTP se fie à une horloge de référence pour définir l’heure la plus précise possible et synchronise toutes les horloges d’un réseau avec cette horloge de référence. L’heure fournie par NTP est basée sur le standard universel UTC (temps universel coordonné). L’heure UTC étant indépendante des fuseaux horaires, NTP peut être utilisé partout dans le monde, quels que soient les paramètres de fuseau horaire.  
  
#### <a name="ntp-algorithms"></a>Algorithmes NTP  
Pour identifier le meilleur échantillon d’heure, le service de temps Windows s’appuie sur les deux algorithmes NTP que sont l’algorithme de filtrage d’horloge et l’algorithme de sélection d’horloge. L’algorithme de filtrage d’horloge a pour fonction d’analyser les échantillons de temps reçus des sources de temps interrogées et d’identifier les meilleurs échantillons d’heure en provenance de chaque source. L’algorithme de sélection d’horloge identifie ensuite le serveur de temps le plus précis sur le réseau. Ces informations sont ensuite transmises à l’algorithme de discipline d’horloge, qui se sert des informations collectées pour corriger l’horloge locale de l’ordinateur, tout en compensant les erreurs dues à la latence du réseau et à l’imprécision de l’horloge de l’ordinateur.  
  
Les algorithmes NTP sont les plus précis dans les conditions de charges réseau et serveur légères à modérées. Comme pour tout algorithme qui prend en compte le temps de transit réseau, il peut arriver que les algorithmes NTP fonctionnent mal dans des conditions d’encombrement réseau extrême. Pour plus d’informations sur les algorithmes NTP, consultez la RFC 1305 dans la base de données IETF RFC.  
  
#### <a name="ntp-time-provider"></a>Fournisseur de temps NTP  
Le service de temps Windows est un package de synchronisation d’heure complet qui peut prendre en charge une grande variété d’appareils et de protocoles de temps. Pour permettre cette prise en charge, le service utilise des fournisseurs de temps enfichables. Un fournisseur de temps est chargé d’obtenir des horodatages précis (auprès du réseau ou du matériel) ou de fournir ces horodatages à d’autres ordinateurs via le réseau.  
  
Le fournisseur NTP est le fournisseur de temps standard intégré au système d’exploitation. Il est conforme aux standards spécifiés par la version 3 du protocole NTP pour un client et un serveur et peut interagir avec les clients et les serveurs SNTP pour assurer une compatibilité descendante avec Windows 2000 et d’autres clients SNTP. Le fournisseur NTP du service de temps Windows se compose des éléments suivants :  
  
-   **Fournisseur NtpServer en sortie.** Il s’agit d’un serveur de temps qui répond aux demandes d’heure clientes sur le réseau.  
  
-   **Fournisseur NtpClient en entrée.** Il s’agit d’un client de temps qui obtient les informations d’heure auprès d’une autre source (soit un appareil, soit un serveur NTP) qui peut ensuite retourner les échantillons d’heure utiles à la synchronisation de l’horloge locale.  
  
Bien que les opérations réelles de ces deux fournisseurs soient étroitement liées, elles sont indépendantes pour le service de temps. Depuis Windows 2000 Server, du moment qu’un ordinateur Windows est connecté à un réseau, il est configuré en tant que client NTP. De même, les ordinateurs exécutant le service de temps Windows tentent par défaut de synchroniser l’heure avec uniquement un contrôleur de domaine ou une source de temps spécifiée manuellement. Il s’agit des fournisseurs de temps préférés, car ce sont des sources de temps automatiquement disponibles et sécurisées.  
  
#### <a name="ntp-security"></a>Sécurité NTP  

Dans une forêt AD DS, le service de temps Windows s’appuie sur des fonctionnalités de sécurité de domaine standard pour imposer l’authentification des données de temps. La sécurité des paquets NTP échangés entre un ordinateur membre de domaine et un contrôleur de domaine local qui joue le rôle de serveur de temps est basée sur l’authentification par clé partagée. Le service de temps Windows utilise la clé de session Kerberos de l’ordinateur pour créer des signatures authentifiées sur les paquets NTP qui transitent par le réseau. Les paquets NTP ne sont pas transmis dans le canal sécurisé du service Net Logon. Au lieu de cela, quand un ordinateur demande l’heure à un contrôleur de domaine situé dans la hiérarchie de domaines, le service de temps Windows exige une authentification de l’heure. Le contrôleur de domaine retourne alors les informations demandées sous la forme d’une valeur de 64 bits qui a été authentifiée avec la clé de session à partir du service Net Logon. Si le paquet NTP retourné n’est pas signé avec la clé de session de l’ordinateur ou s’il l’est de manière incorrecte, l’heure est rejetée. Tous ces échecs d’authentification sont consignés dans le journal des événements. C’est ainsi que le service de temps Windows assure la sécurité des données NTP au sein d’une forêt AD DS.  
  
En général, les clients du service de temps Windows obtiennent automatiquement une heure précise pour la synchronisation auprès des contrôleurs de domaine d’un même domaine. Dans une forêt, les contrôleurs de domaine d’un domaine enfant synchronisent l’heure avec les contrôleurs de domaine de leurs domaines parents. Quand un serveur de temps retourne un paquet NTP authentifié à un client qui demande l’heure, le paquet est signé au moyen d’une clé de session Kerberos définie par un compte de confiance interdomaine. Ce compte est créé au moment où un nouveau domaine AD DS est joint à une forêt, et le service Net Logon gère la clé de session. De cette façon, le contrôleur de domaine qui est configuré comme étant fiable dans le domaine racine de la forêt devient la source de temps authentifiée de tous les contrôleurs de domaine des domaines parent et enfants, et indirectement de tous les ordinateurs situés dans l’arborescence de domaines.  
  
Le service de temps Windows peut être configuré pour fonctionner entre différentes forêts, mais il est important de noter que cette configuration n’est pas sécurisée. Par exemple, un serveur NTP peut être disponible dans une forêt différente. Cependant, comme cet ordinateur se trouve dans autre forêt différente, il n’existe pas de clé de session Kerberos permettant de signer et authentifier les paquets NTP. Pour obtenir une synchronisation d’heure précise à partir d’un ordinateur situé dans une forêt différente, le client a besoin d’un accès réseau à cet ordinateur et le service de temps doit être configuré pour utiliser une source de temps spécifique située dans l’autre forêt. Si un client est configuré manuellement pour accéder à l’heure d’un serveur NTP situé en dehors de sa propre hiérarchie de domaines, les paquets NTP échangés entre le client et le serveur de temps ne sont pas authentifiés et donc pas sécurisés. Même avec l’implémentation des approbations de forêt, le service de temps Windows n’est pas sécurisé entre les forêts. Bien que le canal sécurisé Net Logon soit le mécanisme d’authentification du service de temps Windows, l’authentification entre les forêts n’est pas prise en charge.  
  
#### <a name="hardware-devices-that-are-supported-by-the-windows-time-service"></a>Appareils pris en charge par le service de temps Windows  
Les horloges matérielles que sont les horloges GPS ou radio sont souvent utilisées comme horloges de référence de haute précision. Par défaut, le fournisseur de temps NTP du service de temps Windows ne prend pas en charge la connexion directe d’un appareil à un ordinateur, même s’il est possible de créer un fournisseur de temps logiciel indépendant qui prend en charge ce type de connexion. Ce type de fournisseur, en liaison avec le service de temps Windows, peut fournir une référence de temps fiable et stable.  
  
Les appareils, tels qu’une horloge au césium ou un récepteur GPS (Global Positioning System), fournissent une heure précise en suivant un standard pour obtenir une définition d’heure précise. Si les horloges au césium sont extrêmement stables et ne sont pas affectées par des facteurs comme la température, la pression ou l’humidité, elles sont aussi très coûteuses. Un récepteur GPS est beaucoup moins onéreux à exploiter et constitue également une horloge de référence précise. Les récepteurs GPS obtiennent leur heure auprès de satellites qui eux-mêmes obtiennent leur heure auprès d’une horloge au césium. Sans passer par un fournisseur de temps indépendant, les serveurs de temps Windows peuvent obtenir leur heure en se connectant à un serveur NTP externe, lui-même connecté à un appareil par téléphone ou Internet. Des organismes comme l’observatoire naval des États-Unis disposent de serveurs NTP qui sont connectés à des horloges de référence extrêmement fiables.  
  
De nombreux récepteurs GPS et autres appareils de temps peuvent fonctionner comme serveurs NTP sur un réseau. Vous pouvez configurer votre forêt AD DS de sorte qu’elle synchroniser l’heure avec ces appareils externes à condition qu’ils jouent aussi le rôle de serveurs NTP sur votre réseau. Pour ce faire, configurez le contrôleur de domaine faisant office d’émulateur de contrôleur de domaine principal (PDC) dans la racine de votre forêt de sorte qu’il se synchronise avec le serveur NTP fourni par l’appareil GPS. Pour cela, consultez [Configurer le service de temps Windows sur l’émulateur PDC du domaine racine de forêt](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29).  
  
### <a name="simple-network-time-protocol"></a>Protocole SNTP  
Le protocole SNTP (simple Network Time Protocol) est un protocole de temps simplifié qui s’adresse aux serveurs et clients qui n’exigent pas le degré de précision offert par NTP. SNTP, version plus rudimentaire de NTP, est le principal protocole de temps utilisé dans Windows 2000. Comme le format des paquets réseau SNTP et NTP est identique, les deux protocoles sont interopérables. La principale différence entre les deux est que le protocole SNTP ne dispose pas des systèmes de filtrage complexes et de gestion d’erreurs proposés par NTP. Pour plus d’informations sur le protocole SNTP, consultez la RFC 1769 dans la base de données IETF RFC.  
  
### <a name="time-protocol-interoperability"></a>Interopérabilité des protocoles de temps  
Le service de temps Windows peut fonctionner dans un environnement mixte d’ordinateurs exécutant Windows 2000, Windows XP et Windows Server 2003, car le protocole SNTP utilisé dans Windows 2000 est interopérable avec le protocole NTP de Windows XP et Windows Server 2003.  
  
Le service de temps de Windows NT Server 4.0, appelé TimeServ, synchronise l’heure sur un réseau Windows NT 4.0. TimeServ est une fonctionnalité complémentaire disponible dans le *Kit de ressources Microsoft Windows NT 4.0* dont le niveau de fiabilité de la synchronisation d’heure ne répond pas aux exigences de Windows Server 2003.  
  
Le service de temps Windows peut interagir avec les ordinateurs exécutant Windows NT 4.0, car ils peuvent synchroniser l’heure avec des ordinateurs exécutant Windows 2000 ou Windows Server 2003. En revanche, un ordinateur exécutant Windows 2000 ou Windows Server 2003 ne découvre pas automatiquement les serveurs de temps Windows NT4.0. Par exemple, si votre domaine est configuré pour synchroniser l’heure selon la méthode de synchronisation basée sur la hiérarchie de domaines et que vous souhaitez que les ordinateurs de la hiérarchie de domaines synchronisent l’heure avec un contrôleur de domaine Windows NT 4.0, vous devez configurer ces ordinateurs manuellement pour qu’ils se synchronisent avec les contrôleurs de domaine Windows NT 4.0.  
  
Windows NT 4.0 utilise un mécanisme de synchronisation d’heure plus simple que celui du service de temps Windows. Par conséquent, pour assurer une synchronisation d’heure précise sur votre réseau, il est recommandé de mettre à niveau les contrôleurs de domaine Windows NT 4.0 vers Windows 2000 ou Windows Server 2003.  
  
## <a name="windows-time-service-processes-and-interactions"></a>Processus et interactions du service de temps Windows  

Le service de temps Windows est conçu pour synchroniser les horloges des ordinateurs d’un réseau. Le processus de synchronisation d’heure de réseau, également appelé « convergence horaire », intervient sur un réseau chaque fois que les ordinateurs accèdent à l’heure d’un serveur de temps plus précis. La convergence horaire implique un processus par lequel un serveur de référence fournit l’heure du moment aux ordinateurs clients sous forme de paquets NTP. Les informations contenues dans un paquet indiquent si l’heure actuelle de l’ordinateur doit être ajustée de façon à être synchronisée avec le serveur plus précis.  
  
Dans le cadre du processus de convergence, les membres d’un domaine essaient de synchroniser l’heure avec un contrôleur de domaine situé dans le même domaine. Si l’ordinateur est un contrôleur de domaine, il tente de se synchroniser avec un contrôleur de domaine qui fait davantage autorité.  
  
Les ordinateurs qui exécutent Windows XP Édition familiale ou qui ne sont pas joints à un domaine ne tentent pas de se synchroniser avec la hiérarchie de domaines, mais ils sont configurés par défaut pour obtenir l’heure auprès de time.windows.com.  
  
Pour qu’un ordinateur exécutant Windows Server 2003 puisse faire autorité, il doit être configuré comme étant une source de temps fiable. Par défaut, le premier contrôleur de domaine à être installé sur un domaine Windows Server 2003 est automatiquement configuré comme étant une source de temps fiable. S’agissant de l’ordinateur de référence du domaine, il doit être configuré pour se synchroniser avec une source de temps externe et non avec la hiérarchie de domaines. De plus, tous les autres membres de domaine Windows Server 2003 sont configurés par défaut pour se synchroniser avec la hiérarchie de domaines.  
  
Une fois que vous avez établi un réseau Windows Server 2003, vous pouvez configurer le service de temps Windows de façon à utiliser l’une des options de synchronisation suivantes :  
  
-   Synchronisation basée sur la hiérarchie de domaines  
  
-   Source de synchronisation spécifiée manuellement  
  
-   Tous les mécanismes de synchronisation disponibles  
  
-   Aucune synchronisation  
  
Chacun de ces types de synchronisation est décrit dans la section suivante.  
  
### <a name="domain-hierarchy-based-synchronization"></a>Synchronisation basée sur la hiérarchie de domaines  

La synchronisation basée sur une hiérarchie de domaines recherche une source fiable avec laquelle synchroniser l’heure dans la hiérarchie de domaines AD DS. Le service de temps Windows détermine la précision de chaque serveur de temps en fonction de la hiérarchie de domaines. Dans une forêt Windows Server 2003, l’ordinateur qui détient le rôle de maître d’opérations de l’émulateur de contrôleur de domaine principal (PDC), situé dans le domaine racine de la forêt, est considéré comme la meilleure source de temps, à moins qu’une autre source de temps fiable ait été configurée. Le schéma suivant illustre un chemin de synchronisation d’heure entre les ordinateurs d’une hiérarchie de domaines.  
  
**Synchronisation de l’heure dans une hiérarchie AD DS**  
![Temps Windows](../media/Windows-Time-Service/How-the-Windows-Time-Service-Works/trnt_ntw_adhc.gif)
  
#### <a name="reliable-time-source-configuration"></a>Configuration d’une source de temps fiable  
Un ordinateur configuré en tant que source de temps fiable est identifié comme étant la racine du service de temps. Celle-ci correspond au serveur de référence du domaine et est généralement configurée pour récupérer l’heure auprès d’un serveur NTP externe ou d’un appareil. Un serveur de temps peut être configuré comme source de temps fiable pour optimiser la façon dont l’heure est transférée dans la hiérarchie de domaines. Si un contrôleur de domaine est configuré comme étant une source de temps fiable, le service Net Logon présente ce contrôleur de domaine comme étant une source de temps fiable au moment de se connecter au réseau. Quand d’autres contrôleurs de domaine recherchent une source de temps avec laquelle se synchroniser, ils choisissent d’abord une source fiable s’il en existe une.  
  
#### <a name="time-source-selection"></a>Sélection de la source de temps  
Le processus de sélection de la source de temps peut créer deux problèmes sur un réseau :  
  
-   Cycles de synchronisation supplémentaires.  
  
-   Volume accru du trafic réseau.  
  
Un cycle dans le réseau de synchronisation se produit quand l’heure reste cohérente entre un groupe de contrôleurs de domaine et que la même heure est partagée entre eux en continu sans resynchronisation avec une autre source de temps fiable. L’algorithme de sélection de la source de temps du service de temps Windows est conçu pour éviter ces types de problèmes.  
  
Un ordinateur utilise l’une des méthodes suivantes pour identifier une source de temps avec laquelle se synchroniser :  
  
-   Si l’ordinateur n’est pas membre d’un domaine, il doit être configuré pour se synchroniser avec une source de temps spécifiée.  
  
-   Si l’ordinateur est un serveur ou une station de travail membre au sein d’un domaine, par défaut, il suit la hiérarchie AD DS et synchronise son heure avec un contrôleur de domaine de son domaine local qui exécute actuellement  le service de temps Windows.  
  
Si l’ordinateur est un contrôleur de domaine, il effectue jusqu’à six requêtes pour trouver un autre contrôleur de domaine avec lequel se synchroniser. Chaque requête vise à identifier une source de temps présentant certains attributs, par exemple un type de contrôleur de domaine, un emplacement particulier et s’il s’agit ou non d’une source de temps fiable. La source de temps doit aussi respecter les contraintes suivantes :  
  
-   Une source de temps fiable ne peut se synchroniser qu’avec un contrôleur de domaine du domaine parent.  
  
-   Un émulateur PDC peut se synchroniser avec une source de temps fiable de son propre domaine ou avec un contrôleur de domaine du domaine parent.  
  
Si le contrôleur de domaine n’est pas en mesure de se synchroniser avec le type de contrôleur de domaine qu’il interroge, la requête n’est pas exécutée. Avant d’effectuer la requête, le contrôleur de domaine sait auprès de quel type d’ordinateur il peut obtenir l’heure. Par exemple, un émulateur PDC local ne tente pas d’interroger les numéros trois ou six, car un contrôleur de domaine ne tente pas de se synchroniser avec lui-même.  
  
Le tableau suivant liste les requêtes qu’effectue un contrôleur de domaine pour trouver une source de temps ainsi que l’ordre d’exécution des requêtes.  
  
**Requêtes de source de temps du contrôleur de domaine**  
  
|Numéro de requête|Contrôleur de domaine|Emplacement|Fiabilité de la source de temps|  
|----------------|---------------------|------------|------------------------------|  
|1|Contrôleur de domaine parent|Sur site|Préfère une source de temps fiable, mais peut se synchroniser avec une source de temps non fiable si c’est la seule à être disponible.|  
|2|Contrôleur de domaine local|Sur site|Se synchronise uniquement avec une source de temps fiable.|  
|3|Émulateur PDC local|Sur site|Non applicable.<br /><br />Un contrôleur de domaine ne tente pas de se synchroniser avec lui-même.|  
|4|Contrôleur de domaine parent|Hors site|Préfère une source de temps fiable, mais peut se synchroniser avec une source de temps non fiable si c’est la seule à être disponible.|  
|5|Contrôleur de domaine local|Hors site|Se synchronise uniquement avec une source de temps fiable.|  
|6|Émulateur PDC local|Hors site|Non applicable.<br /><br />Un contrôleur de domaine ne tente pas de se synchroniser avec lui-même.| 
  
**Remarque**  
  
-   Un ordinateur ne se synchronise jamais avec lui-même. Si l’ordinateur qui tente la synchronisation est l’émulateur PDC local, il ne tente pas les requêtes 3 ou 6.  
  
Chaque requête retourne la liste des contrôleurs de domaine qui peuvent être utilisés comme source de temps. Le service de temps Windows attribue à chaque contrôleur de domaine interrogé un score basé sur la fiabilité et l’emplacement du contrôleur de domaine. Le tableau suivant liste les scores attribués par le service de temps Windows à chaque type de contrôleur de domaine.  
  
**Détermination du score**  
  
|État du contrôleur de domaine|Score|  
|----------------------------|---------|  
|Contrôleur de domaine situé sur le même site|8|  
|Contrôleur de domaine marqué comme source de temps fiable|4|  
|Contrôleur de domaine situé dans le domaine parent|2|  
|Contrôleur de domaine correspondant à un émulateur PDC|1|  
  
Une fois que le service de temps Windows a déterminé qu’il a identifié le contrôleur de domaine ayant le meilleur score possible, aucune autre requête n’est effectuée. Les scores attribués par le service de temps sont cumulatifs, ce qui signifie qu’un émulateur PDC situé sur le même site reçoit un score de neuf.  
  
Si la racine du service de temps n’est pas configurée pour se synchroniser avec une source externe, l’horloge matérielle interne de l’ordinateur régit l’heure.  
  
### <a name="manually-specified-synchronization"></a>Synchronisation spécifiée manuellement  
La synchronisation spécifiée manuellement vous permet de désigner un homologue unique ou une liste d’homologues auprès desquels un ordinateur obtient l’heure. Si l’ordinateur n’est pas membre d’un domaine, il doit être configuré manuellement pour se synchroniser avec une source de temps spécifiée. Un ordinateur membre d’un domaine est configuré par défaut pour se synchroniser à partir de la hiérarchie de domaines. La synchronisation spécifiée manuellement est particulièrement utile pour la racine de la forêt du domaine ou pour les ordinateurs qui ne sont pas joints à un domaine. Le fait de spécifier manuellement qu’un serveur NTP externe se synchronise avec l’ordinateur de référence de votre domaine fournit une heure fiable. Cependant, configurer l’ordinateur de référence pour que votre domaine se synchronise avec une horloge matérielle est en fait une meilleure solution et est le gage de l’heure la plus précise et la plus sûre pour votre domaine.  
  
Les sources de temps spécifiées manuellement ne sont pas authentifiées, à moins qu’un fournisseur de temps spécifique soit écrit pour elles, et sont donc vulnérables aux attaquants. De même, si un ordinateur se synchronise avec une source spécifiée manuellement plutôt que son contrôleur de domaine d’authentification, les deux ordinateurs risquent de se désynchroniser, provoquant ainsi l’échec de l’authentification Kerberos. Cela peut entraîner l’échec d’autres actions nécessitant une authentification réseau, comme l’impression ou le partage de fichiers. Si seule la racine de la forêt est configurée pour se synchroniser avec une source externe, tous les autres ordinateurs de la forêt restent synchronisés entre eux, ce qui contrarie les attaques par relecture.  
  
### <a name="all-available-synchronization-mechanisms"></a>Tous les mécanismes de synchronisation disponibles  

L’option « all available synchronization mechanisms » (tous les mécanismes de synchronisation disponibles) est la méthode de synchronisation la plus intéressante pour les utilisateurs d’un réseau. Cette méthode permet une synchronisation avec la hiérarchie de domaines et peut aussi fournir une autre source de temps si la hiérarchie de domaines devient indisponible, selon la configuration. Si le client ne parvient pas à synchroniser l’heure avec la hiérarchie de domaines, la source de temps revient automatiquement à la source de temps spécifiée par le paramètre **NtpServer**. Cette méthode de synchronisation est la plus encline à fournir une heure précise aux clients.  

### <a name="stopping-time-synchronization"></a>Arrêt de la synchronisation de l’heure  
Dans certaines situations, vous souhaiterez peut-être empêcher un ordinateur de synchroniser son heure. Par exemple, si un ordinateur tente de se synchroniser auprès d’une source de temps basée sur Internet ou sur un autre site en empruntant un réseau étendu (WAN) au moyen d’une connexion d’accès à distance, cela peut occasionner des frais téléphoniques coûteux. Si vous désactivez la synchronisation sur cet ordinateur, vous l’empêcherez d’accéder à une source de temps via une connexion d’accès à distance.  
  
Vous pouvez aussi désactiver la synchronisation pour empêcher la génération d’erreurs dans le journal des événements. Chaque fois qu’un ordinateur tente de se synchroniser avec une source de temps qui n’est pas disponible, il génère une erreur dans le journal des événements. Si une source de temps est déconnectée du réseau pour des raisons de maintenance planifiée et que vous n’envisagez pas de reconfigurer le client afin qu’il se synchronise auprès d’une autre source, vous pouvez désactiver la synchronisation sur le client pour l’empêcher de tenter une synchronisation pendant que le serveur de temps est indisponible.  
  
Il est utile de désactiver la synchronisation sur l’ordinateur désigné comme racine du réseau de synchronisation. Cela indique que l’ordinateur racine fait confiance à son horloge locale. Si la racine de la hiérarchie de synchronisation n’est pas définie sur **NoSync** et qu’elle ne peut pas se synchroniser avec une autre source de temps, les clients n’acceptent pas le paquet envoyé par cet ordinateur, car la confiance ne peut pas être accordée à son heure.
  
Les seuls serveurs de temps à être considérés comme fiables par les clients, même s’ils n’ont pas été synchronisés avec une autre source de temps, sont ceux qui ont été identifiés par le client comme étant des serveurs de temps fiables.  
  
### <a name="disabling-the-windows-time-service"></a>Désactivation du service de temps Windows  
Le service de temps Windows (W32Time) peut être complètement désactivé. Si vous choisissez d’implémenter un produit de synchronisation d’heure tiers qui utilise NTP, vous devez désactiver le service de temps Windows. En effet, tous les serveurs NTP ont besoin d’accéder au port UDP (User Datagram Protocol) 123, et tant que le service de temps Windows s’exécute sur le système d’exploitation Windows Server 2003, le port 123 reste réservé au service de temps Windows.  
  
## <a name="network-ports-used-by-windows-time-service"></a>Ports réseau utilisés par le service de temps Windows  
Le service de temps Windows communique sur un réseau pour identifier les sources de temps fiables, obtenir les informations d’heure et les fournir à d’autres ordinateurs. Il établit cette communication selon les normes RFC NTP et SNTP.  
  
**Affectations de port pour le service de temps Windows**  
  
|Nom du service|UDP|TCP|  
|----------------|-------|-------|  
|NTP|123|NON APPLICABLE|  
|SNTP|123|NON APPLICABLE|  
  
## <a name="see-also"></a>Voir aussi  
[Informations techniques de référence sur le service de temps Windows](windows-time-service-tech-ref.md)
[Paramètres et outils du service de temps Windows](Windows-Time-Service-Tools-and-Settings.md)
[Article 902229 de la Base de connaissances Microsoft](https://go.microsoft.com/fwlink/?LinkId=186066)