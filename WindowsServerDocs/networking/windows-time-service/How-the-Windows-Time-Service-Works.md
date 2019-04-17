---
ms.assetid: d1953097-63ea-4a0e-b860-2f3b7c175c41
title: Comment fonctionne le Service de temps Windows
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: c9ab52229c2a16d6ae6b0b2733c52e53a25cfa44
ms.sourcegitcommit: fb4e2ace2e0a29e0f6b028f1cb945cab6aa6ee2c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/05/2018
---
# <a name="how-the-windows-time-service-works"></a>Comment fonctionne le Service de temps Windows

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

**Dans cette section**  
  
-   [Architecture de Service de temps Windows](#w2k3tr_times_how_rrfo)  
  
-   [Protocoles de temps de Service de temps Windows](#w2k3tr_times_how_ekoc)  
  
-   [Processus de Service et les Interactions de temps Windows](#w2k3tr_times_how_izcr)  
  
-   [Ports réseau utilisés par le Service de temps Windows](#w2k3tr_times_how_ydum)  
  
> [!NOTE]  
> Cette rubrique explique comment le service de temps Windows (W32Time) fonctionne uniquement. Pour plus d’informations sur la configuration du service de temps Windows, consultez la liste des rubriques dans la section [où trouver Windows Time Service Configuration Information](https://technet.microsoft.com/library/cc773061.aspx).  
  
> [!NOTE]  
> Dans Windows Server 2003 et Microsoft Windows 2000 Server, le service d’annuaire est nommé service d’annuaire Active Directory. Dans Windows Server 2008 et versions ultérieures, le service d’annuaire est nommé Services de domaine Active Directory (AD DS). Le reste de cette rubrique fait référence aux services AD DS, mais les informations sont également applicables à Active Directory.  
  
Bien que le service de temps Windows n’est pas une implémentation exacte du protocole NTP (Network Time), il utilise la suite complexe d’algorithmes qui est définie dans les spécifications NTP pour vous assurer que les horloges sur les ordinateurs sur un réseau sont aussi précis que possible. Dans l’idéal, toutes les horloges d’ordinateur dans un domaine AD DS sont synchronisés avec l’heure d’un ordinateur faisant autorité. De nombreux facteurs peuvent affecter la synchronisation de l’heure sur un réseau. Souvent, les facteurs suivants affectent la précision de la synchronisation dans AD DS:  
  
-   Conditions du réseau  
  
-   La précision de l’horloge du matériel de l’ordinateur  
  
-   La quantité de ressources du processeur et de réseau disponibles pour le service de temps Windows  
  
> [!IMPORTANT]  
> Avant Windows Server 2016, le service W32Time n’est pas conçu pour répondre aux besoins des applications sensibles à la fois.  Toutefois, les mises à jour vers Windows Server 2016 permettent désormais d’implémenter une solution pour 1 MS précision dans votre domaine.  Voir [Windows 2016 précis temps](accurate-time.md) et [limite de prise en charge pour configurer le service de temps Windows pour les environnements de grande précision](https://go.microsoft.com/fwlink/?LinkID=179459) pour plus d’informations.  
  
Les ordinateurs qui ne sont pas joints à un domaine ou de synchronisent leur temps moins fréquemment sont configurés par défaut, à synchroniser avec time.windows.com. Par conséquent, il est impossible de garantir la précision du temps sur les ordinateurs qui ont intermittente ou aucune connexion réseau.  
  
Une forêt AD DS a une hiérarchie de synchronisation de temps prédéfini. Le service de temps Windows synchronise l’heure entre des ordinateurs au sein de la hiérarchie, avec les horloges de référence plus précises en haut. Si plus d’une source de temps est configurée sur un ordinateur, temps Windows utilise les algorithmes NTP pour sélectionner la meilleure source de temps à partir des sources configurés basés sur la capacité de l’ordinateur à synchroniser avec cette source de temps. Le service de temps Windows ne gère pas la synchronisation de réseau de diffusion ou multidiffusion homologues. Pour plus d’informations sur ces fonctionnalités NTP, voir la norme RFC 1305 dans la base de données de IETF RFC.  
  
Chaque ordinateur qui exécute le service de temps Windows utilise le service pour mettre à jour de l’heure la plus précise. Les ordinateurs qui sont membres d’un domaine agissent comme un client de l’heure par défaut, par conséquent, dans la plupart des cas, il n’est pas nécessaire de configurer le Service de temps Windows. Toutefois, le Service de temps Windows peut être configuré pour la durée de la requête à partir d’une source de temps de référence désigné et peut également fournir le temps aux clients.
  
Le degré auquel les temps d’un ordinateur est précise est appelé une couche. La source de temps plus précise sur un réseau (par exemple, une horloge matérielle) occupe le niveau le plus bas niveau, ou couche une. Cette source de temps précise est appelée une horloge de référence. Un serveur NTP qui acquiert sa durée directement à partir d’une horloge de référence occupe une couche trouve un niveau supérieur à celui de l’horloge de référence. Ressources acquérir le temps à partir du serveur NTP sont deux opérations pour l’horloge de référence et par conséquent occupent une couche deux est supérieure à la source de temps plus précise et ainsi de suite. Comme le nombre de niveau d’un ordinateur augmente, l’heure sur l’horloge système peut devenir moins précises. Par conséquent, le niveau de la couche de n’importe quel ordinateur est un indicateur de comment cet ordinateur est synchronisé avec la source de temps plus précise.  
  
Lorsque le Gestionnaire de W32Time reçoit des exemples, il utilise les algorithmes spéciales dans NTP pour déterminer parmi les échantillons de temps est le plus approprié pour une utilisation. Le service de temps utilise également un autre ensemble d’algorithmes pour déterminer lequel des sources de temps configurée est la plus précise. Lorsque le service de temps a déterminé les exemples de temps est préférable, selon les critères ci-dessus, il ajuste la vitesse d’horloge locale pour lui permettre de faire converger vers l’heure correcte. Si la différence entre l’horloge locale et l’heure précise sélectionné (également appelé l’heure du décalage) est trop importante pour corriger en réglant la fréquence d’horloge local, le service de temps définit l’horloge locale sur l’heure correcte. Cet ajustement de la fréquence d’horloge ou changement d’heure horloge direct est appelé discipline de l’horloge.  
  
## <a name="w2k3tr_times_how_rrfo"></a>Architecture de Service de temps Windows  
Le service de temps Windows se compose des éléments suivants:  
  
-   Gestionnaire de contrôle de service  
  
-   Gestionnaire de Service de temps Windows  
  
-   Discipline de l’horloge  
  
-   Fournisseurs de temps  
  
La figure suivante illustre l’architecture du service de temps Windows.  
  
**Architecture de Service de temps Windows**  
  
![Temps Windows](media/How-the-Windows-Time-Service-Works/trnt_sec_arcc.gif)  
  
Le Gestionnaire de contrôle de Service est responsable pour démarrer et arrêter le service de temps Windows. Le Gestionnaire de Service de temps Windows est chargé de lancer l’action des fournisseurs de temps NTP inclus avec le système d’exploitation. Le Gestionnaire de Service de temps Windows contrôle toutes les fonctions du service de temps Windows et la fusion de tous les échantillons de temps. En plus de fournir des informations sur l’état actuel du système, telles que la source de temps en cours ou le dernier temps de l’horloge système a été mis à jour, le Gestionnaire de Service de temps Windows est également responsable de la création d’événements dans le journal des événements.  
  
Le processus de synchronisation de temps implique les étapes suivantes:  
  
-   Fournisseurs d’entrée demandent et recevoir des échantillons de temps à partir des sources de temps NTP configurés.  
  
-   Ces exemples sont ensuite transmis à la fois Gestionnaire de services Windows, qui collecte tous les exemples et les transmet au sous-composant discipline de l’horloge.  
  
-   Le sous-composant discipline horloge s’applique les algorithmes NTP, ce qui entraîne la sélection de l’échantillon de temps meilleures.  
  
-   Le sous-composant discipline horloge ajuste l’heure de l’horloge système à l’heure la plus précise en ajustant la fréquence d’horloge ou modifier directement le temps.  
  
Si un ordinateur a été désigné comme un serveur de temps, il peut envoyer la fois une session sur n’importe quel ordinateur demande de synchronisation de l’heure à tout moment dans ce processus.  
  
## <a name="w2k3tr_times_how_ekoc"></a>Protocoles de temps de Service de temps Windows  

Protocoles de temps déterminent comment étroitement deux ordinateurs horloges sont synchronisées. Un protocole de temps est chargé de déterminer les meilleures informations temps disponible et converger les horloges pour garantir un temps de cohérence sur des systèmes distincts.  
  
Le service de temps Windows utilise le protocole NTP (Network Time) pour synchroniser l’heure sur un réseau. NTP est un protocole de temps Internet qui inclut les algorithmes discipline nécessaires pour la synchronisation des horloges. NTP est un protocole de temps plus précis que le temps protocole SNTP (Simple Network) qui est utilisé dans certaines versions de Windows; Toutefois W32Time continue de prendre en charge SNTP pour activer la compatibilité descendante avec les ordinateurs exécutant les services de temps SNTP base, tels que Windows 2000.  
  
### <a name="network-time-protocol"></a>Network Time Protocol  
Protocole NTP (Network Time Protocol) est la valeur par défaut utilisé par le service de temps Windows dans le système d’exploitation de protocole de synchronisation de temps. NTP est un protocole de temps à tolérance de pannes, évolutive et le protocole plus souvent utilisé pour la synchronisation d’horloge de l’ordinateur à l’aide d’une référence de temps désigné.  
  
Synchronisation de temps NTP a lieu sur une période de temps et implique le transfert de paquets NTP sur un réseau. Paquets NTP contiennent des horodatages qui incluent un échantillon de temps à partir du client et du serveur participant de synchronisation de l’heure.  
  
NTP s’appuie sur une horloge de référence pour définir l’heure la plus précise à utiliser et synchronise les horloges sur un réseau à cette horloge de référence. NTP utilise le temps universel coordonné (UTC) en tant que la norme universelle pour l’heure actuelle. UTC est indépendante des fuseaux horaires et permet NTP à utiliser n’importe où dans le monde, quels que soient les paramètres de fuseau horaire.  
  
#### <a name="ntp-algorithms"></a>Algorithmes NTP  
NTP inclut deux algorithmes, un algorithme de filtrage d’horloge et un algorithme de sélection de l’horloge, pour aider le service de temps Windows dans la détermination de l’échantillon de temps meilleures. L’algorithme de filtrage d’horloge est conçu pour passer en revue les exemples qui sont reçues à partir de sources de temps interrogé et déterminent les meilleures échantillons de temps à partir de chaque source. L’algorithme de sélection de l’horloge détermine ensuite le serveur de temps plus précis sur le réseau. Cette information est alors transmise à l’algorithme de discipline horloge, qui utilise les informations collectées pour corriger l’horloge locale de l’ordinateur, tout en récupérant des erreurs en raison d’imprécision de l’horloge de latence et l’ordinateur réseau.  
  
Les algorithmes NTP sont plus précises dans des conditions de lumière modérée des charges de réseau et serveur. Comme avec n’importe quel algorithme qui tient compte de la durée de transit de réseau, les algorithmes NTP peuvent médiocres dans des conditions de congestion du réseau extrêmes. Pour plus d’informations sur les algorithmes NTP, consultez 1305 RFC dans la base de données de IETF RFC.  
  
#### <a name="ntp-time-provider"></a>Fournisseur de temps NTP  
Le service de temps Windows est un package de synchronisation de temps complet qui prend en charge une variété de périphériques matériels et les protocoles de temps. Pour activer cette prise en charge, le service utilise les fournisseurs de temps enfichables. Un fournisseur de temps est responsable pour l’obtention précise que les horodatages (à partir du réseau ou du matériel) ou pour fournir ces horodatages à d’autres ordinateurs sur le réseau.  
  
Le fournisseur NTP est le fournisseur de temps standard inclus avec le système d’exploitation. Le fournisseur NTP respectant les normes spécifiés par NTP version 3 pour un client et un serveur et peut interagir avec les clients SNTP et les serveurs pour la compatibilité descendante avec les clients Windows 2000 et autres SNTP. Le fournisseur NTP dans le service de temps Windows se compose de deux parties suivantes:  
  
-   **Fournisseur de sortie NtpServer.** Il s’agit d’un serveur de temps qui répond aux demandes client sur le réseau.  
  
-   **NtpClient fournisseur en entrée.** Il s’agit d’un client de temps qui obtienne des informations de temps à partir d’une autre source, un périphérique matériel ou un serveur NTP et peut retourner des exemples qui sont utiles pour la synchronisation de l’horloge locale.  
  
Bien que les opérations de ces deux fournisseurs sont étroitement liées, ils apparaissent indépendants pour le service de temps. À partir de Windows 2000 Server, lorsqu’un ordinateur Windows est connecté à un réseau, il est configuré comme un client NTP. En outre, les ordinateurs exécutant Windows Time service uniquement une tentative pour synchroniser l’heure avec un contrôleur de domaine ou d’une source de temps spécifiée manuellement par défaut. Il s’agit de fournisseurs par défaut, car elles sont automatiquement disponibles et sécurisées des sources de temps.  
  
#### <a name="ntp-security"></a>Sécurité NTP  

Au sein d’une forêt AD DS, le service de temps Windows s’appuie sur les fonctionnalités de sécurité de domaine standard pour appliquer l’authentification des données de temps. La sécurité des paquets NTP qui sont échangées entre un ordinateur membre du domaine et un contrôleur de domaine local qui agit comme un serveur de temps est basée sur l’authentification par clé partagée. Le service de temps Windows utilise la clé de session Kerberos de l’ordinateur pour créer des signatures authentifiés sur les paquets NTP qui sont envoyés sur le réseau. Paquets NTP ne sont pas transmises à l’intérieur d’un canal sécurisé l’ouverture de session réseau. Au lieu de cela, lorsqu’un ordinateur demande l’heure à partir d’un contrôleur de domaine dans la hiérarchie de domaine, le service de temps Windows requiert que l’heure soit authentifié. Le contrôleur de domaine renvoie ensuite les informations requises sous la forme d’une valeur de 64 bits qui a été authentifiée avec la clé de session à partir du service d’ouverture de session réseau. Si le paquet NTP renvoyé n’est pas signé avec la clé de session de l’ordinateur ou est correctement connecté, l’heure est rejetée. Toutes ces échecs d’authentification sont consignées dans le journal des événements. De cette façon, le service de temps Windows fournit la sécurité des données NTP dans une forêt AD DS.  
  
En règle générale, les clients de temps Windows obtenir automatiquement une heure précise pour la synchronisation à partir de contrôleurs de domaine dans le même domaine. Dans une forêt, les contrôleurs de domaine d’un domaine enfant synchronisent le temps avec des contrôleurs de domaine dans leurs domaines parents. Lorsqu’un serveur de temps retourne un paquet NTP authentifié à un client qui demande le temps, le paquet est connecté au moyen d’une clé de session Kerberos définie par un compte d’approbation entre domaines. Le compte d’approbation entre domaines est créé lorsqu’une forêt joint à un domaine AD DS et le service Net Logon gère la clé de session. De cette façon, le contrôleur de domaine qui est configuré en tant que fiable dans le domaine racine de forêt devient la source de temps authentifié pour tous les contrôleurs de domaine dans des domaines parent et enfant et indirectement pour tous les ordinateurs que qui se trouvent dans l’arborescence de domaine.  
  
Le service de temps Windows peut être configuré pour fonctionner entre forêts, mais il est important de noter que cette configuration n’est pas sécurisée. Par exemple, un serveur NTP peut être disponible dans une autre forêt. Toutefois, étant donné que cet ordinateur est dans une autre forêt, il n’existe aucune clé de session Kerberos avec laquelle se connecter et d’authentifier les paquets NTP. Pour obtenir la synchronisation de l’heure précise à partir d’un ordinateur dans une forêt différente, le client nécessite un accès réseau à l’ordinateur et le service de temps doit être configuré pour utiliser une source de temps spécifique située dans l’autre forêt. Si un client est configuré manuellement à l’heure de l’accès à partir d’un serveur NTP en dehors de sa propre hiérarchie de domaine, les paquets NTP envoyées entre le client et le serveur de temps ne sont pas authentifiés et par conséquent, ne sont pas sécurisés. Même avec l’implémentation des approbations de forêt, le service de temps Windows n’est pas sécurisé entre les forêts. Bien que le canal sécurisé ouverture de session réseau est le mécanisme d’authentification pour le service de temps Windows, l’authentification entre forêts n’est pas pris en charge.  
  
#### <a name="hardware-devices-that-are-supported-by-the-windows-time-service"></a>Périphériques matériels sont pris en charge par le Service de temps Windows  
En fonction du matériel des horloges comme GPS ou les horloges de cases d’option sont souvent utilisés en tant que périphériques d’horloge de référence extrêmement précises. Par défaut, le fournisseur de temps NTP service horloge Windows ne prend pas en charge la connexion directe d’un périphérique matériel à un ordinateur, même s’il est possible de créer un fournisseur de logiciels de temps indépendants qui prend en charge ce type de connexion. Ce type de fournisseur, conjointement avec le service de temps Windows, peut fournir une référence temporelle fiable et stable.  
  
Périphériques matériels, comme une horloge cesium ou un récepteur système GPS (Global Positioning), fournissent une heure actuelle en suivant une norme pour obtenir une définition précise de temps. Horloges CESIUM sont très stables et ne sont pas affectés par des facteurs tels que la température, la pression ou humidité, mais sont également très coûteux. Un récepteur GPS est beaucoup moins coûteux et est également une horloge référence précises. Récepteurs GPS obtiennent leur temps de satellites qui obtiennent l’heure à partir d’une horloge cesium. Sans l’utilisation d’un fournisseur indépendant de temps, les serveurs de temps Windows peuvent obtenir leur temps en vous connectant à un serveur NTP externe, qui est connecté à un périphérique matériel au moyen d’un téléphone ou via Internet. Les organisations telles que l’United States Naval Observatory fournissent des serveurs NTP qui sont connectés à des horloges référence extrêmement fiable.  
  
Nombreux récepteurs GPS et autres périphériques temps peuvent fonctionner en tant que serveurs NTP sur un réseau. Vous pouvez configurer votre forêt AD DS pour synchroniser l’heure à partir de ces périphériques externes uniquement si elles sont également agissant en tant que serveurs NTP sur votre réseau. Pour ce faire, configurez le contrôleur de domaine fonctionne en tant que contrôleur de domaine principal (émulateur) dans votre racine de forêt à synchroniser avec le serveur NTP fourni par le périphérique GPS. Pour ce faire, voir [configurer le service de temps Windows sur l’émulateur de contrôleur de domaine principal dans le domaine racine de forêt](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29).  
  
### <a name="simple-network-time-protocol"></a>Simple Network Time Protocol  
Le temps protocole SNTP (Simple Network) est un protocole de temps simplifiée prévu pour les serveurs et les clients qui ne nécessitent pas le degré de précision NTP fournit. SNTP, une version plus rudimentaire de NTP, est le protocole de temps principal qui est utilisé dans Windows 2000. Étant donné que les formats de paquet réseau de SNTP et NTP sont identiques, les deux protocoles sont interopérables. La principale différence entre les deux est que SNTP ne dispose pas de la gestion des erreurs et les systèmes de filtrage complexes qui fournit NTP. Pour plus d’informations sur la Simple Network Time Protocol, voir 1769 RFC dans la base de données de IETF RFC.  
  
### <a name="time-protocol-interoperability"></a>Interopérabilité de protocole de temps  
Le service de temps Windows peut fonctionner dans un environnement mixte d’ordinateurs exécutant Windows 2000, Windows XP et Windows Server 2003, car le protocole SNTP utilisé dans Windows 2000 peut interagir avec le protocole NTP dans Windows XP et Windows Server 2003.  
  
Le service de temps dans Windows NT Server 4.0, appelé TimeServ, synchronise l’heure sur un réseau de Windows NT 4.0. TimeServ est une fonctionnalité de module complémentaire disponible en tant que partie de la *Kit de ressources Microsoft Windows NT 4.0* et ne fournit pas le degré de fiabilité de la synchronisation horaire requis par Windows Server 2003.  
  
Le service de temps Windows peut interagir avec les ordinateurs exécutant Windows NT 4.0, car ils peuvent synchroniser l’heure avec les ordinateurs exécutant Windows 2000 ou Windows Server 2003; Toutefois, un ordinateur exécutant Windows 2000 ou Windows Server 2003 ne découvre pas automatiquement les serveurs de temps Windows NT 4.0. Par exemple, si votre domaine est configuré pour synchroniser l’heure à l’aide du domaine basé sur une hiérarchie de méthode de synchronisation et que vous souhaitez que les ordinateurs dans la hiérarchie de domaine pour synchroniser l’heure avec un contrôleur de domaine Windows NT 4.0, vous devez configurer ces ordinateurs manuellement pour se synchroniser avec les contrôleurs de domaine Windows NT 4.0.  
  
Windows NT 4.0 utilise un mécanisme plus simple pour la synchronisation de temps que le service de temps Windows utilise. Par conséquent, pour garantir la synchronisation de l’heure précise sur votre réseau, il est recommandé de mettre à niveau des contrôleurs de domaine Windows NT 4.0 pour Windows 2000 ou Windows Server 2003.  
  
## <a name="w2k3tr_times_how_izcr"></a>Processus de Service et les Interactions de temps Windows  

Le service de temps Windows est conçu pour synchroniser les horloges des ordinateurs sur un réseau. Le processus de synchronisation de l’heure réseau, également appelé convergence de temps, se produit sur un réseau en tant que chaque heure d’accède à l’ordinateur à partir d’un serveur de temps plus précis. Convergence de temps implique un processus par lequel un serveur faisant autorité fournit l’heure actuelle sur les ordinateurs clients sous la forme de paquets NTP. Les informations fournies dans un paquet indiquent si un ajustement doit être apportées à l’heure actuelle de l’horloge de l’ordinateur afin qu’il est synchronisé avec le serveur plus précis.  
  
Dans le cadre du processus de convergence de temps, essayez de membres du domaine synchroniser l’heure avec n’importe quel contrôleur de domaine situé dans le même domaine. Si l’ordinateur est un contrôleur de domaine, il tente de se synchroniser avec un contrôleur de domaine plus faisant autorité.  
  
Ordinateurs exécutant Windows XP Édition familiale ou les ordinateurs qui ne sont pas joints à un domaine n’essayez pas de se synchroniser avec la hiérarchie de domaine, mais sont configurés par défaut pour obtenir des temps de time.windows.com.  
  
Pour établir un ordinateur exécutant Windows Server 2003 comme faisant autorité, l’ordinateur doit être configuré pour être une source de temps fiable. Par défaut, le premier contrôleur de domaine est installé sur un domaine Windows Server 2003 est automatiquement configuré pour être une source de temps fiable. Dans la mesure où il est l’ordinateur de référence pour le domaine, il doit être configuré pour être synchronisé avec une source de temps externe plutôt qu’avec la hiérarchie de domaine. Également par défaut, tous les autres membres de domaine Windows Server 2003 sont configurés pour être synchronisé avec la hiérarchie de domaine.  
  
Après avoir établi un réseau de Windows Server 2003, vous pouvez configurer le service de temps Windows pour utiliser l’une des options suivantes pour la synchronisation:  
  
-   Synchronisation basée sur une hiérarchie de domaine  
  
-   Une source de synchronisation manuelle  
  
-   Tous les mécanismes de synchronisation disponibles  
  
-   Aucune synchronisation.  
  
Chacun de ces types de synchronisation est décrite dans la section suivante.  
  
### <a name="domain-hierarchy-based-synchronization"></a>Synchronisation basée sur une hiérarchie de domaine  

Synchronisation est basée sur une hiérarchie de domaine utilise la hiérarchie de domaine AD DS pour rechercher une source fiable avec lequel synchroniser l’heure. En fonction de la hiérarchie de domaine, le service de temps Windows détermine la précision de chaque serveur de temps. Dans une forêt Windows Server 2003, l’ordinateur qui héberge le principal rôle contrôleur de domaine (PDC) émulateur operations maître, situé dans le domaine racine de forêt, occupe le poste de meilleure source de temps, sauf si une autre source de temps fiable a été configurée. La figure suivante illustre un chemin d’accès de la synchronisation horaire entre les ordinateurs dans une hiérarchie de domaine.  
  
**Synchronisation de l’heure dans une hiérarchie d’AD DS**  
  
![Temps Windows](media/How-the-Windows-Time-Service-Works/trnt_ntw_adhc.gif)  
  
#### <a name="reliable-time-source-configuration"></a>Configuration de Source de temps fiable  
Un ordinateur qui est configuré pour être une source de temps fiable est identifié comme la racine du service de temps. La racine du service de temps est le serveur faisant autorité pour le domaine et est généralement configurée pour récupérer l’heure à partir d’un serveur NTP externe ou un périphérique matériel. Un serveur de temps peut être configuré en tant que source de temps fiable pour optimiser la façon dont le temps est transféré tout au long de la hiérarchie de domaine. Si un contrôleur de domaine est configuré pour être une source de temps fiable, service Net Logon annonce ce contrôleur de domaine en tant que source de temps fiable lorsqu’il ouvre une session sur le réseau. Lorsque d’autres contrôleurs de domaine recherchent une source de temps pour se synchroniser avec, ils choisissent une source fiable tout d’abord s’il est disponible.  
  
#### <a name="time-source-selection"></a>Sélection de Source de temps  
Le processus de sélection de source de temps peut créer deux problèmes sur un réseau:  
  
-   Cycles de synchronisation supplémentaire.  
  
-   Augmentation du trafic réseau.  
  
Un cycle dans le réseau de synchronisation se produit lorsque durée reste cohérente entre un groupe de contrôleurs de domaine et le même temps est partagé entre eux en permanence sans une resynchronisation avec une autre source de temps fiable. Algorithme de sélection de source de temps du service de temps Windows est conçu pour vous protéger contre ces types de problèmes.  
  
Un ordinateur utilise l’une des méthodes suivantes pour identifier une source de temps pour se synchroniser avec:  
  
-   Si l’ordinateur n’est pas membre d’un domaine, il doit être configuré pour être synchronisé avec une source de temps spécifié.  
  
-   Si l’ordinateur est un serveur membre ou une station de travail au sein d’un domaine, par défaut, il suit la hiérarchie de domaine Active Directory et synchronise l’heure avec un contrôleur de domaine dans son domaine local qui est en cours d’exécution du service de temps Windows.  
  
Si l’ordinateur est un contrôleur de domaine, il effectue jusqu'à six requêtes pour localiser le synchroniser avec un autre contrôleur de domaine. Chaque requête est conçue pour identifier une source de temps avec certains attributs, par exemple, un type de contrôleur de domaine, un emplacement particulier, et indique si elle est une source de temps fiable. La source de temps doit également respecter les contraintes suivantes:  
  
-   Source de temps fiable peut synchroniser uniquement un contrôleur de domaine dans le domaine parent.  
  
-   Un émulateur PDC peut synchroniser avec une source de temps fiable dans son propre domaine ou n’importe quel contrôleur de domaine dans le domaine parent.  
  
Si le contrôleur de domaine n’est pas en mesure de se synchroniser avec le type de contrôleur de domaine qu’il effectue une requête, la requête n’est pas établie. Le contrôleur de domaine connaît le type d’ordinateur, il peut obtenir l’heure à partir d’avant d’effectuer la requête. Par exemple, un émulateur de contrôleur principal de domaine local n’essaie pas de numéros de requête trois ou six car un contrôleur de domaine ne tente pas de se synchroniser avec lui-même.  
  
Le tableau suivant répertorie les requêtes pour trouver une source de temps et l’ordre dans lequel les requêtes sont effectuées par un contrôleur de domaine.  
  
**Requêtes de la Source de temps de contrôleur de domaine**  
  
|Nombre de requêtes|Contrôleur de domaine|Emplacement|Fiabilité de la Source de temps|  
|----------------|---------------------|------------|------------------------------|  
|1|Contrôleur de domaine parent|Dans le site|Préfère une source de temps, mais il peut se synchroniser avec une source non fiable si c’est tout ce qui est disponible.|  
|2|Contrôleur de domaine local|Dans le site|Synchronise uniquement avec une source de temps fiable.|  
|3|Émulateur de contrôleur de domaine principal local|Dans le site|Ne s’applique pas.<br /><br />Un contrôleur de domaine ne tente pas de se synchroniser avec lui-même.|  
|4|Contrôleur de domaine parent|Hors site|Préfère une source de temps, mais il peut se synchroniser avec une source non fiable si c’est tout ce qui est disponible.|  
|5|Contrôleur de domaine local|Hors site|Synchronise uniquement avec une source de temps fiable.|  
|6|Émulateur de contrôleur de domaine principal local|Hors site|Ne s’applique pas.<br /><br />Un contrôleur de domaine ne tente pas de se synchroniser avec lui-même.|  
  
**Remarque**  
  
-   Un ordinateur se synchronise jamais avec lui-même. Si l’ordinateur tente de synchronisation est l’émulateur de contrôleur principal de domaine local, il n’essaie pas de requêtes 3 ou 6.  
  
Chaque requête renvoie une liste de contrôleurs de domaine qui peut être utilisé comme source de temps. Temps Windows affecte chaque contrôleur de domaine qui est interrogé score en fonction de la fiabilité et l’emplacement du contrôleur de domaine. Le tableau suivant répertorie les scores attribués par de temps Windows pour chaque type de contrôleur de domaine.  
  
**Détermination de score**  
  
|État du contrôleur de domaine|Score|  
|----------------------------|---------|  
|Contrôleur de domaine situé dans le même site|8|  
|Contrôleur de domaine est marqué comme source de temps fiable|4|  
|Contrôleur de domaine situé dans le domaine parent|2|  
|Contrôleur de domaine qui est un émulateur de contrôleur de domaine principal|1|  
  
Lorsque le service de temps Windows détermine qu’il a déterminé le contrôleur de domaine avec le meilleur score possible, aucuns plus de requêtes ne sont effectuées. Les scores attribués par le service de temps sont cumulatives, ce qui signifie qu’un émulateur de contrôleur de domaine principal situé dans le même site reçoit un score de neuf.  
  
Si la racine du service de temps n’est pas configurée pour être synchronisé avec une source externe, l’horloge de matériel interne de l’ordinateur régit l’heure.  
  
### <a name="manually-specified-synchronization"></a>Synchronisation manuelle  
Synchronisation manuelle vous permet de désigner un homologue ou une liste d’homologues à partir de laquelle un ordinateur obtient le temps. Si l’ordinateur n’est pas membre d’un domaine, il doit être configuré manuellement pour être synchronisé avec une source de temps spécifié. Un ordinateur qui est que membre d’un domaine est configuré par défaut à synchroniser à partir de la hiérarchie de domaine, spécifié manuellement la synchronisation est particulièrement utile pour la racine de la forêt du domaine ou pour les ordinateurs qui ne sont pas joints à un domaine. Fournit des temps fiable spécifier manuellement un serveur NTP externe pour la synchronisation avec l’ordinateur de référence pour votre domaine. Toutefois, la configuration de l’ordinateur de référence pour votre domaine pour une synchronisation avec une horloge matérielle est en fait une meilleure solution pour fournir l’heure plus précise, sécurisé à votre domaine.  
  
Sources de temps spécifié manuellement ne sont pas authentifiés, sauf si un fournisseur de temps spécifique est écrit pour elles, et ils sont donc vulnérables aux pirates. En outre, si un ordinateur se synchronise avec une source spécifié manuellement, plutôt que son contrôleur de domaine authentificateur, les deux ordinateurs peut-être pas synchronisées, provoquant l’échec de l’authentification Kerberos. Cela peut provoquer des autres actions nécessitant une authentification de réseau à échouer, telles que l’impression ou le partage de fichiers. Si seule la racine de forêt est configurée pour être synchronisé avec une source externe, tous les autres ordinateurs de la forêt sont synchronisés entre eux, ce qui rend les attaques par relecture difficile.  
  
### <a name="all-available-synchronization-mechanisms"></a>Tous les mécanismes de synchronisation disponibles  

L’option «tous les mécanismes de synchronisation disponibles» est la meilleure méthode de synchronisation pour les utilisateurs sur un réseau. Cette méthode permet la synchronisation avec la hiérarchie de domaine et qu’il peut également fournir une autre source de temps si la hiérarchie de domaine devient indisponible, selon la configuration. Si le client ne peut pas synchroniser l’heure avec la hiérarchie de domaine, la source de temps automatiquement revient à la source de temps spécifiée par le **NtpServer** paramètre. Cette méthode de synchronisation est la plus probable fournir une heure précise aux clients.  

### <a name="stopping-time-synchronization"></a>Synchronisation horaire de l’arrêt  
Il existe certaines situations dans lesquelles vous souhaiterez arrêter un ordinateur de synchroniser l’heure. Par exemple, si un ordinateur tente de synchroniser à partir d’une source de temps sur Internet ou d’un autre site sur un réseau étendu au moyen d’une connexion d’accès à distance, il peut occasionner des frais téléphoniques coûteux. Lorsque vous désactivez la synchronisation sur cet ordinateur, vous empêchez l’ordinateur de tenter d’accéder à une source de temps via une connexion d’accès à distance.  
  
Vous pouvez également désactiver la synchronisation pour éviter la génération d’erreurs dans le journal des événements. Chaque fois qu’un ordinateur tente de se synchroniser avec une source de temps qui n’est pas disponible, il génère une erreur dans le journal des événements. Si une source de temps est effectuée sur le réseau pour une maintenance planifiée et que vous ne souhaitez pas reconfigurer le client à synchroniser à partir d’une autre source, vous pouvez désactiver la synchronisation sur le client afin d’éviter une tentative de synchronisation pendant que le serveur de temps n’est pas disponible.  
  
Il est utile désactiver la synchronisation sur l’ordinateur qui est désigné comme la racine du réseau de synchronisation. Cela indique que l’ordinateur racine approuve son horloge local. Si la racine de la hiérarchie de synchronisation n’est pas définie sur **NoSync** et s’il est impossible de synchroniser avec une autre source de temps, les clients n’acceptent pas le paquet cet ordinateur envoie sa durée ne pouvant pas être approuvée.  
  
Le seul moment où les serveurs qui sont approuvés par les clients, même si elles n’ont pas été synchronisés avec une autre source de temps sont ceux qui ont été identifiés par le client en tant que serveurs de temps fiable.  
  
### <a name="disabling-the-windows-time-service"></a>La désactivation du Service de temps Windows  
Le service de temps Windows (W32Time) peut être complètement désactivé. Si vous choisissez d’implémenter un produit de synchronisation de temps tiers qui utilise le protocole NTP, vous devez désactiver le service de temps Windows. Il s’agit, car tous les serveurs NTP ont besoin d’accéder au port de protocole UDP (User Datagram) 123 et tant que le service de temps Windows est en cours d’exécution sur le système d’exploitation Windows Server 2003, le port 123 reste réservé par Windows.  
  
## <a name="w2k3tr_times_how_ydum"></a>Ports réseau utilisés par le Service de temps Windows  
Le service de temps Windows communique sur un réseau pour identifier les sources de temps fiable, obtenir des informations de temps et fournissent des informations de temps à d’autres ordinateurs. Il effectue cette communication tel que défini par le protocole NTP et SNTP RFC.  
  
**Affectations de port pour le Service de temps Windows**  
  
|Nom du service|UDP|TCP|  
|----------------|-------|-------|  
|NTP|123|NON APPLICABLE|  
|SNTP|123|NON APPLICABLE|  
  
## <a name="see-also"></a>Voir aussi  
[Référence technique du Service de temps Windows](https://technet.microsoft.com/library/cc773061.aspx)  
[Paramètres et outils de Service de temps Windows](Windows-Time-Service-Tools-and-Settings.md)  
[Article 902229 de la Base de connaissances Microsoft](https://go.microsoft.com/fwlink/?LinkId=186066)  
  


