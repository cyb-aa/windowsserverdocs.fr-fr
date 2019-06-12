---
ms.assetid: d1953097-63ea-4a0e-b860-2f3b7c175c41
title: Fonctionnement du service de temps Windows
description: ''
author: shortpatti
ms.author: pashort
manager: dougkim
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 9e4131c28a18a50f3312e5e0201a0ed9529d4555
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812399"
---
# <a name="how-the-windows-time-service-works"></a>Fonctionnement du service de temps Windows

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 ou version ultérieure

**Dans cette section**  
  
-   [Architecture du Service de temps Windows](#windows-time-service-architecture)  
  
-   [Protocoles de temps de Service de temps Windows](#windows-time-service-time-protocols)  
  
-   [Processus de Service et Interactions de temps Windows](#windows-time-service-processes-and-interactions)  
  
-   [Ports réseau utilisés par le Service de temps Windows](#network-ports-used-by-windows-time-service)  
  
> [!NOTE]  
> Cette rubrique explique comment le service de temps de Windows (W32Time) fonctionne uniquement. Pour plus d’informations sur la configuration du service de temps de Windows, consultez [configuration des systèmes de haute précision](configuring-systems-for-high-accuracy.md).
  
> [!NOTE]  
> Dans Windows Server 2003 et Microsoft Windows 2000 Server, le service d’annuaire est nommé service d’annuaire Active Directory. Dans Windows Server 2008 et versions ultérieures, le service d’annuaire est nommé Services de domaine Active Directory (AD DS). Le reste de cette rubrique fait référence aux services AD DS, mais les informations sont également applicables à Active Directory.  
  
Bien que le service de temps de Windows n’est pas une implémentation exacte du protocole NTP (Network Time), il utilise la suite d’algorithmes complexe qui est définie dans les spécifications NTP pour vous assurer que les horloges des ordinateurs à travers un réseau soient aussi précis que possible. Dans l’idéal, toutes les horloges d’ordinateur dans un domaine AD DS sont synchronisés avec l’heure d’un ordinateur faisant autorité. De nombreux facteurs peuvent affecter la synchronisation date / heure sur un réseau. Souvent, les facteurs suivants affectent la précision de la synchronisation dans AD DS :  
  
-   Conditions du réseau  
  
-   La précision de l’horloge de matériel  
  
-   La quantité de ressources processeur et réseau disponibles pour le service de temps de Windows  
  
> [!IMPORTANT]  
> Avant Windows Server 2016, le service W32Time n’est pas conçu pour répondre aux besoins de l’application de la contrainte de temps.  Toutefois, les mises à jour vers Windows Server 2016 maintenant vous permettent d’implémenter une solution de 1 MS précision dans votre domaine.  Consultez [Windows 2016 précise temps](accurate-time.md) et [limite de prise en charge pour configurer le service de temps de Windows pour les environnements de haute-précision](support-boundary.md) pour plus d’informations.  
  
Les ordinateurs qui ne sont pas joints à un domaine ou de synchronisent leur temps moins fréquemment sont configurés par défaut, pour se synchroniser avec time.windows.com.  Par conséquent, il est impossible de garantir la précision du temps sur les ordinateurs qui ont intermittents ou aucune connexion réseau.  
  
Une forêt AD DS a une hiérarchie de synchronisation de temps prédéterminé. Le service de temps de Windows synchronise l’heure entre les ordinateurs au sein de la hiérarchie, avec les horloges de référence plus précises en haut. Si plus d’une source de temps est configurée sur un ordinateur, les temps de Windows utilise les algorithmes NTP pour sélectionner la meilleure source de temps à partir des sources configurés selon la capacité de l’ordinateur de synchronisation avec cette source de temps. Le service de temps de Windows ne prend pas en charge la synchronisation de réseau de diffusion ou multidiffusion homologues. Pour plus d’informations sur ces fonctionnalités NTP, consultez 1305 RFC dans la base de données IETF RFC.  
  
Chaque ordinateur qui exécute le service de temps de Windows utilise le service pour mettre à jour de l’heure la plus précise. Les ordinateurs qui sont membres d’un domaine agissent comme un client de temps par défaut, par conséquent, dans la plupart des cas il n’est pas nécessaire de configurer le Service de temps Windows. Toutefois, le Service de temps Windows peut être configuré pour l’heure de la demande à partir d’une source de temps de référence désigné et peut également fournir le temps aux clients.
  
Le degré auquel les temps d’un ordinateur sont exacte est appelé une couche. La source de temps plus précise sur un réseau (par exemple, une horloge de matériel) occupe le niveau le plus bas niveau, ou par couche une. Cette source de temps précise est appelée une horloge de référence. Un serveur NTP qui acquiert son heure directement à partir d’une horloge de référence occupe une couche qui se trouve un niveau supérieur à celui de l’horloge de référence. Les ressources qui acquièrent le temps à partir du serveur NTP sont deux étapes en dehors de l’horloge de référence et par conséquent occupent une couche qui est de deux supérieure à la source de temps plus précise et ainsi de suite. En tant que nombre de niveau d’un ordinateur augmente, l’heure de son horloge système peut devenir moins précis. Par conséquent, le niveau de la couche de n’importe quel ordinateur est un indicateur de comment cet ordinateur est synchronisé avec la source de temps plus précise.  
  
Lorsque le Gestionnaire de W32Time reçoit des échantillons de temps, elle utilise des algorithmes spéciales dans NTP pour déterminer lequel des échantillons de temps est la plus appropriée pour une utilisation. Le service de temps utilise également un autre ensemble d’algorithmes pour déterminer lequel des sources de temps configuré est le plus précis. Lorsque le service de temps a déterminé les exemples de temps est préférable, en fonction des critères ci-dessus, il ajuste la vitesse d’horloge locale pour lui permettre de converger vers l’heure correcte. Si la différence de temps entre l’horloge locale et l’exemple de temps précise sélectionné (également appelé l’heure décalage) est trop grande pour corriger en réglant la fréquence d’horloge locale, le service de temps définit l’horloge locale sur l’heure correcte. Cet ajustement de la fréquence d’horloge ou le changement d’heure horloge direct est connu en tant que discipline d’horloge.  
  
## <a name="windows-time-service-architecture"></a>Architecture du service de temps Windows  
Le service de temps de Windows est constitué des composants suivants :  
  
-   Gestionnaire de contrôle des services  
  
-   Gestionnaire de Service de temps Windows  
  
-   Discipline de l’horloge  
  
-   Fournisseurs de temps  
  
La figure suivante illustre l’architecture du service de temps de Windows.  
  
**Architecture du Service de temps Windows**  
  
![Horloge Windows](../media/Windows-Time-Service/How-the-Windows-Time-Service-Works/trnt_sec_arcc.gif)
  
Le Gestionnaire de contrôle de Service est responsable du démarrage et l’arrêt du service de temps de Windows. Le Gestionnaire de Service de temps Windows est chargé de l’initialisation de l’action des fournisseurs de temps NTP inclus avec le système d’exploitation. Le Gestionnaire de Service de temps Windows contrôle toutes les fonctions du service de temps de Windows et la fusion de tous les échantillons de temps. En plus de fournir des informations sur l’état actuel du système, telles que la source de temps actuelle ou la dernière heure de l’horloge système a été mis à jour, le Gestionnaire de Service de temps Windows est également responsable de la création d’événements dans le journal des événements.  
  
Le processus de synchronisation de temps implique les étapes suivantes :  
  
-   Fournisseurs d’entrée demander et recevoir des échantillons de temps à partir de sources de temps NTP configurés.  
  
-   Ces exemples sont ensuite passées à la Windows Time Service Manager collecte tous les exemples et les transmet au sous-composant de discipline d’horloge.  
  
-   Le sous-composant de discipline horloge s’applique les algorithmes NTP, ce qui entraîne la sélection de l’échantillon de temps meilleures.  
  
-   Le sous-composant de discipline horloge ajuste l’heure de l’horloge système à l’heure la plus précise en ajustant la fréquence d’horloge ou modification directement de l’heure.  
  
Si un ordinateur a été désigné comme un serveur de temps, il peut envoyer la fois une session sur n’importe quel ordinateur demande de synchronisation de l’heure à tout moment dans ce processus.  
  
## <a name="windows-time-service-time-protocols"></a>Protocoles de temps de Service de temps Windows  

Protocoles de temps déterminent quelle précision deux ordinateurs horloges sont synchronisées. Un protocole de temps est chargé de déterminer les meilleures informations de temps disponible et converger les horloges pour vous assurer que les temps de cohérence est maintenue sur des systèmes séparés.  
  
Le service de temps de Windows utilise le protocole NTP (Network Time) pour synchroniser l’heure sur un réseau. NTP est un protocole de temps Internet qui inclut les algorithmes de discipline nécessaires pour la synchronisation des horloges. NTP est un protocole de temps plus précis que le temps protocole SNTP (Simple Network) qui est utilisé dans certaines versions de Windows ; Toutefois, W32Time continue à prendre en charge SNTP pour permettre la compatibilité descendante avec les ordinateurs qui exécutent les services de temps basées sur SNTP, tels que Windows 2000.  
  
### <a name="network-time-protocol"></a>Network Time Protocol  
Protocole NTP (Network Time) est la valeur par défaut utilisé par le service de temps de Windows dans le système d’exploitation de protocole de synchronisation de temps. NTP est un protocole de temps à tolérance de panne et hautement évolutif et le protocole plus souvent utilisé pour la synchronisation des horloges de l’ordinateur à l’aide d’une référence de temps désigné.  
  
Synchronisation de temps NTP a lieu sur une période de temps et implique le transfert de paquets NTP sur un réseau. Paquets NTP contiennent des horodatages qui incluent un exemple de temps à partir du client et le serveur participant dans synchronisation date / heure.  
  
NTP repose sur une horloge de référence pour définir l’heure la plus précise à utiliser et synchronise toutes les horloges d’un réseau de cette horloge de référence. NTP utilise le temps universel coordonné (UTC) en tant que la norme universelle pour l’heure actuelle. Heure UTC est indépendante des fuseaux horaires et permet NTP à utiliser n’importe où dans le monde, quels que soient les paramètres de fuseau horaire.  
  
#### <a name="ntp-algorithms"></a>Algorithmes NTP  
NTP inclut deux algorithmes, un algorithme de filtrage d’horloge et un algorithme de sélection de l’horloge, afin de faciliter le service de temps de Windows pour déterminer le meilleur exemple de temps. L’algorithme de filtrage d’horloge est conçu pour passer en revue les exemples qui sont reçus à partir de sources de temps demandé et déterminent les meilleures échantillons de temps à partir de chaque source. L’algorithme de sélection de l’horloge détermine ensuite le serveur de temps plus précis sur le réseau. Ces informations sont ensuite passées à l’algorithme de discipline d’horloge, qui utilise les informations collectées pour corriger l’horloge locale de l’ordinateur, lors de la compensation des erreurs en raison de l’imprécision de l’horloge de latence et l’ordinateur réseau.  
  
Les algorithmes NTP sont plus précises dans des conditions de lumière à modérer les charges réseau et serveur. Comme avec n’importe quel algorithme qui tient compte de la durée de transit réseau, les algorithmes NTP peuvent mal s’effectuer dans des conditions de congestion du réseau extrêmes. Pour plus d’informations sur les algorithmes NTP, consultez 1305 RFC dans la base de données IETF RFC.  
  
#### <a name="ntp-time-provider"></a>Fournisseur de temps NTP  
Le service de temps de Windows est un package de synchronisation de temps de traitement qui peut prendre en charge les divers périphériques matériels et les protocoles de temps. Pour activer cette prise en charge, le service utilise des fournisseurs de temps enfichables. Un fournisseur de temps est chargé pour l’obtention précise que les horodatages (à partir du réseau ou du matériel) ou pour fournir ces horodatages à d’autres ordinateurs sur le réseau.  
  
Le fournisseur NTP est le fournisseur de l’heure d’hiver inclus avec le système d’exploitation. Le fournisseur NTP suit les normes spécifiés par NTP version 3 pour un client et le serveur et peut interagir avec les clients SNTP et serveurs pour la compatibilité descendante avec Windows 2000 et d’autres clients SNTP. Le fournisseur NTP dans le service de temps de Windows se compose de deux éléments suivants :  
  
-   **Fournisseur de sortie NtpServer.** Il s’agit d’un serveur de temps qui répond aux demandes client sur le réseau.  
  
-   **Fournisseur d’entrées NtpClient.** Il s’agit d’un client de temps qui obtient des informations de temps à partir d’une autre source, un périphérique matériel ou un serveur NTP et peut retourner des exemples qui sont utiles pour la synchronisation de l’horloge locale.  
  
Bien que les opérations réelles de ces deux fournisseurs sont étroitement liées, ils apparaissent indépendants pour le service de temps. À partir de Windows 2000 Server, lorsqu’un ordinateur Windows est connecté à un réseau, il est configuré comme un client NTP. En outre, les ordinateurs exécutant Windows lors du service uniquement tente de synchroniser l’heure avec un contrôleur de domaine ou d’une source de temps manuelle par défaut. Il s’agit de fournisseurs par défaut, car elles sont automatiquement disponibles et sécurisées des sources de temps.  
  
#### <a name="ntp-security"></a>Sécurité NTP  

Dans une forêt AD DS, le service de temps de Windows s’appuie sur les fonctionnalités de sécurité de domaine standard pour appliquer l’authentification des données de temps. La sécurité des paquets NTP envoyés entre un ordinateur membre du domaine et un contrôleur de domaine local qui agit comme un serveur de temps est basée sur l’authentification par clé partagée. Le service de temps de Windows utilise la clé de session Kerberos de l’ordinateur pour créer des signatures authentifiés sur les paquets NTP qui sont envoyées sur le réseau. Paquets NTP ne sont pas transmis dans le canal sécurisé d’ouverture de session réseau. Au lieu de cela, lorsqu’un ordinateur demande l’heure à partir d’un contrôleur de domaine dans la hiérarchie de domaine, le service de temps de Windows nécessite que l’heure soit authentifié. Le contrôleur de domaine retourne ensuite les informations requises sous la forme d’une valeur 64 bits qui a été authentifiée avec la clé de session à partir du service Net Logon. Si le paquet NTP retourné n’est pas signé avec la clé de session de l’ordinateur ou est incorrectement signé, l’heure est rejetée. Tous les échecs de ce type d’authentification sont enregistrés dans le journal des événements. De cette façon, le service de temps de Windows fournit la sécurité pour les données NTP dans une forêt AD DS.  
  
En règle générale, les clients Windows obtiennent automatiquement une heure précise pour la synchronisation à partir de contrôleurs de domaine dans le même domaine. Dans une forêt, les contrôleurs de domaine d’un domaine enfant synchronisent l’heure avec les contrôleurs de domaine dans leurs domaines parents. Lorsqu’un serveur de temps renvoie un paquet NTP authentifié à un client qui demande le temps, le paquet est signé au moyen d’une clé de session Kerberos définie par un compte d’approbation entre domaines. Le compte d’approbation entre domaines est créé lorsqu’un nouveau domaine AD DS joint à une forêt, et le service Net Logon gère la clé de session. De cette façon, le contrôleur de domaine qui est configuré en tant que fiable dans le domaine racine de forêt devient la source de temps authentifié pour tous les contrôleurs de domaine dans des domaines parent et enfant et indirectement pour tous les ordinateurs situés dans l’arborescence de domaine.  
  
Le service de temps de Windows peut être configuré pour fonctionner entre forêts, mais il est important de noter que cette configuration n’est pas sécurisée. Par exemple, un serveur NTP peut être disponible dans une forêt différente. Toutefois, étant donné que cet ordinateur est dans une forêt différente, il n’est aucune clé de session Kerberos avec lequel signer et authentifier les paquets NTP. Pour obtenir la synchronisation date / heure précises à partir d’un ordinateur dans une forêt différente, le client nécessite un accès réseau à cet ordinateur et le service de temps doit être configuré pour utiliser une source de temps spécifique située dans l’autre forêt. Si un client est configuré manuellement au moment de l’accès à partir d’un serveur NTP en dehors de sa propre hiérarchie de domaine, les paquets NTP envoyés entre le client et le serveur de temps ne sont pas authentifiés et par conséquent, ne sont pas sécurisés. Même avec l’implémentation d’approbations de forêt, le service de temps de Windows n’est pas sécurisé entre les forêts. Bien que le canal sécurisé d’ouverture de session réseau est le mécanisme d’authentification pour le service de temps de Windows, l’authentification entre les forêts n’est pas pris en charge.  
  
#### <a name="hardware-devices-that-are-supported-by-the-windows-time-service"></a>Périphériques matériels qui sont pris en charge par le Service de temps Windows  
En fonction du matériel d’horloges comme un GPS ou des horloges de cases d’option sont souvent utilisés en tant qu’appareils de l’horloge de référence très précis. Par défaut, le fournisseur de temps NTP de temps de Windows service ne prend pas en charge la connexion directe d’un périphérique matériel pour un ordinateur, bien qu’il soit possible de créer un fournisseur de temps d’indépendant basé sur logiciel qui prend en charge ce type de connexion. Ce type de fournisseur, conjointement avec le service de temps de Windows, peut fournir une référence de temps fiable et stable.  
  
Périphériques matériels, comme une horloge cesium ou un récepteur système GPS (Global Positioning), fournissent un temps actuel précis en suivant une norme pour obtenir une définition précise de temps. Les horloges CESIUM sont extrêmement stables et ne sont pas affectées par des facteurs tels que la température, pression ou l’humidité, mais sont également très coûteuses. Un récepteur GPS est beaucoup moins coûteux de fonctionner et est également une horloge précise de référence. Récepteurs GPS obtiennent leur temps à partir de satellites qui obtiennent leur temps à partir d’une horloge cesium. Sans l’utilisation d’un fournisseur indépendant de temps, les serveurs de temps Windows peuvent acquérir leur temps en vous connectant à un serveur NTP externe, qui est connecté à un périphérique matériel au moyen d’un téléphone ou via Internet. Les organisations telles que l’United States Naval Observatory fournir des serveurs NTP qui sont connectés à des horloges de référence extrêmement fiable.  
  
Nombreux destinataires GPS et autres appareils de temps peuvent fonctionner en tant que serveurs NTP sur un réseau. Vous pouvez configurer votre forêt AD DS pour synchroniser l’heure à partir de ces périphériques matériels externes uniquement si elles sont également agissant en tant que serveurs NTP sur votre réseau. Pour ce faire, configurez le contrôleur de domaine fonctionne en tant que contrôleur de domaine principal (émulateur) dans votre racine de la forêt à synchroniser avec le serveur NTP fourni par l’appareil GPS. Pour ce faire, consultez [configurer le service de temps de Windows sur l’émulateur de contrôleur de domaine principal dans le domaine racine de forêt](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29).  
  
### <a name="simple-network-time-protocol"></a>Simple Network Time Protocol  
Le temps protocole SNTP (Simple Network) est un protocole de temps simplifiée qui est destiné à des serveurs et les clients qui ne nécessitent pas le degré de précision NTP fournit. SNTP, une version plus rudimentaire de NTP, est le protocole de temps principal qui est utilisé dans Windows 2000. Étant donné que les formats de paquet réseau de SNTP et NTP sont identiques, les deux protocoles sont interopérables. La principale différence entre les deux est que SNTP ne dispose pas de la gestion des erreurs et des systèmes de filtrage complexes NTP fournit. Pour plus d’informations sur la Simple Network Time Protocol, consultez 1769 RFC dans la base de données IETF RFC.  
  
### <a name="time-protocol-interoperability"></a>Interopérabilité des protocoles de temps  
Le service de temps de Windows peut fonctionner dans un environnement mixte d’ordinateurs exécutant Windows 2000, Windows XP et Windows Server 2003, étant donné que le protocole SNTP utilisé dans Windows 2000 est interopérable avec le protocole NTP dans Windows XP et Windows Server 2003.  
  
Le service de temps dans Windows NT Server 4.0, appelé TimeServ, synchronise l’heure sur un réseau de Windows NT 4.0. TimeServ est une fonctionnalité complémentaire disponible en tant que partie de la *Kit de ressources Microsoft Windows NT 4.0* et ne fournit pas le degré de fiabilité de la synchronisation date / heure qui est requis par Windows Server 2003.  
  
Le service de temps de Windows peut interagir avec les ordinateurs exécutant Windows NT 4.0, car ils peuvent synchroniser l’heure avec les ordinateurs qui exécutent Windows 2000 ou Windows Server 2003 ; Toutefois, un ordinateur exécutant Windows 2000 ou Windows Server 2003 ne détecte pas automatiquement les serveurs de temps de Windows NT 4.0. Par exemple, si votre domaine est configuré pour synchroniser l’heure à l’aide de la méthode basée sur la hiérarchie de domaine de synchronisation et que vous souhaitez que les ordinateurs de la hiérarchie de domaine pour synchroniser l’heure avec un contrôleur de domaine Windows NT 4.0, vous devez configurer ceux ordinateurs manuellement pour se synchroniser avec les contrôleurs de domaine Windows NT 4.0.  
  
Windows NT 4.0 utilise un mécanisme plus simple pour la synchronisation de temps que le service de temps de Windows utilise. Par conséquent, pour garantir la synchronisation date / heure précises de votre réseau, il est recommandé de mettre à niveau des contrôleurs de domaine Windows NT 4.0 vers Windows 2000 ou Windows Server 2003.  
  
## <a name="windows-time-service-processes-and-interactions"></a>Processus de Service et Interactions de temps Windows  

Le service de temps de Windows est conçu pour synchroniser les horloges des ordinateurs sur un réseau. Le processus de synchronisation de l’heure réseau, également appelé convergence de temps, se produit tout au long d’un réseau en tant que chaque heure d’accède à l’ordinateur à partir d’un serveur de temps plus précis. Convergence de temps implique un processus par lequel un serveur faisant autorité fournit l’heure actuelle sur les ordinateurs clients sous la forme de paquets NTP. Les informations contenues dans un paquet indiquent si un ajustement doit être effectué à l’heure actuelle de l’ordinateur afin qu’elles soient synchronisées avec le serveur plus précis.  
  
Dans le cadre du processus de convergence de temps, les membres du domaine tentent de synchroniser l’heure avec n’importe quel contrôleur de domaine situé dans le même domaine. Si l’ordinateur est un contrôleur de domaine, il tente de se synchroniser avec un contrôleur de domaine plus faisant autorité.  
  
Ordinateurs exécutant Windows XP Édition familiale ou les ordinateurs qui ne sont pas joints à un domaine n’essayez pas d’effectuer une synchronisation avec la hiérarchie de domaine, mais sont configurés par défaut pour obtenir l’heure à partir de time.windows.com.  
  
Pour établir un ordinateur exécutant Windows Server 2003 comme faisant autorité, l’ordinateur doit être configuré pour être une source de temps fiable. Par défaut, le premier contrôleur de domaine qui est installé sur un domaine Windows Server 2003 est automatiquement configuré pour être une source de temps fiable. Car il s’agit de l’ordinateur de référence pour le domaine, il doit être configuré pour se synchroniser avec une source externe plutôt qu’avec la hiérarchie de domaine. Également par défaut, tous les autres membres de domaine Windows Server 2003 sont configurés pour se synchroniser avec la hiérarchie de domaine.  
  
Une fois que vous avez établi un réseau de Windows Server 2003, vous pouvez configurer le service de temps de Windows pour utiliser une des options suivantes pour la synchronisation :  
  
-   Synchronisation basée sur la hiérarchie de domaine  
  
-   Une source de synchronisation manuelle  
  
-   Tous les mécanismes de synchronisation disponibles  
  
-   Aucune synchronisation.  
  
Chacune de ces types de synchronisation est abordée dans la section suivante.  
  
### <a name="domain-hierarchy-based-synchronization"></a>Synchronisation basée sur la hiérarchie de domaine  

Synchronisation repose sur une hiérarchie de domaine utilise la hiérarchie de domaine AD DS pour trouver une source fiable avec lequel synchroniser l’heure. Selon la hiérarchie de domaine, le service de temps de Windows détermine la précision de chaque serveur de temps. Dans une forêt Windows Server 2003, l’ordinateur qui conserve le domaine principal (PDC) de contrôleur émulateur rôle, situé dans le domaine racine de forêt, conserve la position de la meilleure source de temps, sauf si une autre source de temps fiable a été configurée. L’exemple suivant illustre le cas d’un chemin d’accès de la synchronisation horaire entre ordinateurs dans une hiérarchie de domaine.  
  
**Synchronisation date / heure dans une hiérarchie d’AD DS**  
![Temps de Windows](../media/Windows-Time-Service/How-the-Windows-Time-Service-Works/trnt_ntw_adhc.gif)
  
#### <a name="reliable-time-source-configuration"></a>Configuration de Source de temps fiable  
Un ordinateur qui est configuré pour être une source de temps fiable est identifié comme la racine du service de temps. La racine du service de temps est le serveur faisant autorité pour le domaine et est généralement configurée pour extraire le temps d’un serveur NTP externe ou un périphérique matériel. Un serveur de temps peut être configuré en tant que source de temps fiable pour optimiser la façon dont le temps est transféré tout au long de la hiérarchie de domaine. Si un contrôleur de domaine est configuré pour être une source de temps fiable, service Net Logon annonce ce contrôleur de domaine en tant que source de temps fiable lorsqu’il ouvre une session sur le réseau. Lorsque d’autres contrôleurs de domaine recherchent une source de temps pour se synchroniser avec, ils choisissent une source fiable tout d’abord s’il est disponible.  
  
#### <a name="time-source-selection"></a>Sélection de Source de temps  
Le processus de sélection de source de temps peut créer deux problèmes sur un réseau :  
  
-   Cycles de synchronisation supplémentaires.  
  
-   Augmentation du volume du trafic réseau.  
  
Un cycle dans le réseau de synchronisation se produit lors de la durée reste cohérente entre un groupe de contrôleurs de domaine et la même heure est partagée entre eux en continu sans une resynchronisation avec une autre source de temps fiable. Algorithme de sélection de source de temps du service de temps de Windows est conçu pour vous protéger contre ces types de problèmes.  
  
Un ordinateur utilise l’une des méthodes suivantes pour identifier une source de temps pour se synchroniser avec :  
  
-   Si l’ordinateur n’est pas un membre d’un domaine, il doit être configuré pour se synchroniser avec une source de temps spécifié.  
  
-   Si l’ordinateur est un serveur membre ou une station de travail au sein d’un domaine, par défaut, il suit la hiérarchie des services AD DS et synchronise son heure avec un contrôleur de domaine dans son domaine local qui est en cours d’exécution du service de temps de Windows.  
  
Si l’ordinateur est un contrôleur de domaine, il rend les requêtes jusqu'à six pour localiser les synchroniser avec un autre contrôleur de domaine. Chaque requête est conçue pour identifier une source de temps avec certains attributs, tels que le type de contrôleur de domaine, un emplacement particulier, et qu’il soit ou non d’une source de temps fiable. La source de temps doit également respecter les contraintes suivantes :  
  
-   Une source de temps fiable peut synchroniser uniquement avec un contrôleur de domaine dans le domaine parent.  
  
-   Un émulateur PDC peut effectuer une synchronisation avec une source de temps fiable dans son propre domaine ou n’importe quel contrôleur de domaine dans le domaine parent.  
  
Si le contrôleur de domaine n’est pas en mesure de se synchroniser avec le type de contrôleur de domaine qu’il effectue une requête, la requête n’est effectuée. Le contrôleur de domaine sait quel type d’ordinateur, il peut obtenir des temps d’avant d’effectuer la requête. Par exemple, un émulateur de contrôleur de domaine principal local ne tente pas aux numéros de requête trois ou six, car un contrôleur de domaine ne tente pas de se synchroniser avec lui-même.  
  
Le tableau suivant répertorie les requêtes par un contrôleur de domaine à rechercher une source de temps et l’ordre dans lequel les requêtes sont créées.  
  
**Requêtes de la Source de temps de contrôleur de domaine**  
  
|Nombre de requêtes|Contrôleur de domaine|Location|Fiabilité de la Source de temps|  
|----------------|---------------------|------------|------------------------------|  
|1|Contrôleur de domaine parent|Dans le site|Préfère un fiable source de temps, mais elle peut effectuer une synchronisation avec une source de temps non fiable si c’est tout ce qui est disponible.|  
|2|Contrôleur de domaine local|Dans le site|Synchronise uniquement avec une source de temps fiable.|  
|3|Émulateur de contrôleur de domaine principal local|Dans le site|Ne s’applique pas.<br /><br />Un contrôleur de domaine ne tente pas de se synchroniser avec lui-même.|  
|4|Contrôleur de domaine parent|Hors du site|Préfère un fiable source de temps, mais elle peut effectuer une synchronisation avec une source de temps non fiable si c’est tout ce qui est disponible.|  
|5|Contrôleur de domaine local|Hors du site|Synchronise uniquement avec une source de temps fiable.|  
|6|Émulateur de contrôleur de domaine principal local|Hors du site|Ne s’applique pas.<br /><br />Un contrôleur de domaine ne tente pas de se synchroniser avec lui-même.| 
  
**Remarque**  
  
-   Un ordinateur est jamais synchronisé avec lui-même. Si l’ordinateur la tentative de synchronisation est l’émulateur de contrôleur de domaine principal local, il n’essaie pas de requêtes 3 ou 6.  
  
Chaque requête retourne une liste des contrôleurs de domaine qui peut être utilisé comme source de temps. Heure de Windows attribue chaque contrôleur de domaine qui est interrogé un score basé sur la fiabilité et l’emplacement du contrôleur de domaine. Le tableau suivant répertorie les scores assignés par heure de Windows pour chaque type de contrôleur de domaine.  
  
**Détermination de score**  
  
|État de contrôleur de domaine|Score|  
|----------------------------|---------|  
|Contrôleur de domaine situé dans le même site|8|  
|Contrôleur de domaine marquée en tant que source de temps fiable|4|  
|Contrôleur de domaine situé dans le domaine parent|2|  
|Contrôleur de domaine qui est un émulateur de contrôleur de domaine principal|1|  
  
Lorsque le service de temps de Windows détermine qu’il a déterminé le contrôleur de domaine avec le meilleur score possible, aucuns plus de requêtes ne sont effectuées. Les scores assignés par le service de temps sont cumulatifs, ce qui signifie qu’un émulateur de contrôleur de domaine principal situé dans le même site reçoit un score de neuf.  
  
Si la racine du service de temps n’est pas configurée pour se synchroniser avec une source externe, l’horloge de matériels internes de l’ordinateur régit l’heure.  
  
### <a name="manually-specified-synchronization"></a>Synchronisation manuelle  
Synchronisation manuelle vous permet de désigner un homologue unique ou une liste d’homologues à partir de laquelle un ordinateur obtient le temps. Si l’ordinateur n’est pas un membre d’un domaine, il doit être configuré manuellement pour se synchroniser avec une source de temps spécifié. Un ordinateur qui est que membre d’un domaine est configuré par défaut pour synchroniser à partir de la hiérarchie de domaine, la synchronisation manuelle est plus utile pour la racine de la forêt du domaine ou pour les ordinateurs qui ne sont pas joints à un domaine. Spécifier manuellement un serveur NTP externe pour se synchroniser avec l’ordinateur de référence pour votre domaine fournit temps fiable. Toutefois, la configuration de l’ordinateur faisant autorité pour votre domaine pour effectuer une synchronisation avec une horloge de matériel est en fait une meilleure solution pour fournir l’heure la plus précise et sécurisée à votre domaine.  
  
Sources de temps spécifié manuellement ne sont pas authentifiés, sauf si un fournisseur de temps spécifique est écrit pour elles, et ils sont donc vulnérables aux attaquants. En outre, si un ordinateur se synchronise avec une source manuelle plutôt que son contrôleur de domaine authentification, les deux ordinateurs peut-être pas synchronisées, provoquant l’échec de l’authentification Kerberos. Cela peut entraîner d’autres actions nécessitant une authentification de réseau à échouer, telles que l’impression ou le partage de fichiers. Si seule la racine de forêt est configurée pour se synchroniser avec une source externe, tous les autres ordinateurs au sein de la forêt sont synchronisés entre eux, ce qui rend les attaques par relecture difficile.  
  
### <a name="all-available-synchronization-mechanisms"></a>Tous les mécanismes de synchronisation disponibles  

L’option « tous les mécanismes de synchronisation disponibles » est la meilleure méthode de synchronisation pour les utilisateurs sur un réseau. Cette méthode permet la synchronisation avec la hiérarchie de domaine et peut également fournir une autre source de temps si la hiérarchie de domaine devient indisponible, selon la configuration. Si le client ne peut pas synchroniser l’heure avec la hiérarchie de domaine, la source de temps repasse automatiquement à la source de temps spécifiée par le **NtpServer** paramètre. Cette méthode de synchronisation est plus probable fournir une heure précise aux clients.  

### <a name="stopping-time-synchronization"></a>Synchronisation de l’arrêt de l’heure  
Il existe certaines situations dans lesquelles vous souhaitez arrêter un ordinateur de synchroniser l’heure. Par exemple, si un ordinateur tente de synchroniser à partir d’une source de temps sur Internet ou d’un autre site via un WAN au moyen d’une connexion à distance, il peut engendrer des frais de téléphone coûteux. Lorsque vous désactivez la synchronisation sur cet ordinateur, vous empêchez l’ordinateur de tenter d’accéder à une source de temps via une connexion à distance.  
  
Vous pouvez également désactiver la synchronisation pour éviter la génération d’erreurs dans le journal des événements. Chaque fois qu’un ordinateur tente de synchroniser avec une source de temps qui n’est pas disponible, il génère une erreur dans le journal des événements. Si une source de temps est effectuée sur le réseau pour une maintenance planifiée et que vous ne souhaitez pas reconfigurer le client de synchronisation à partir d’une autre source, vous pouvez désactiver la synchronisation sur le client pour l’empêcher d’une tentative de synchronisation pendant que le serveur de temps n’est pas disponible.  
  
Il est utile de désactiver la synchronisation sur l’ordinateur qui est désigné comme la racine du réseau de synchronisation. Cela indique que l’ordinateur racine fait confiance à son horloge locale. Si la racine de la hiérarchie de synchronisation n’est pas définie sur **NoSync** et s’il est impossible de synchroniser avec une autre source de temps, les clients n’acceptent pas le paquet envoie cet ordinateur, car son heure ne peut pas être approuvé.
  
Le seul moment où les serveurs qui sont approuvés par les clients, même si elles n’ont pas été synchronisées avec une autre source de temps sont celles qui ont été identifiées par le client en tant que serveurs de temps fiable.  
  
### <a name="disabling-the-windows-time-service"></a>La désactivation du Service de temps Windows  
Le service de temps de Windows (W32Time) peut être complètement désactivé. Si vous choisissez d’implémenter un produit de synchronisation de temps tiers qui utilise le protocole NTP, vous devez désactiver le service de temps de Windows. Il s’agit, car tous les serveurs NTP ont besoin d’accéder au port de protocole UDP (User Datagram) 123, et tant que le service de temps de Windows est en cours d’exécution sur le système d’exploitation Windows Server 2003, le port 123 reste réservé par heure de Windows.  
  
## <a name="network-ports-used-by-windows-time-service"></a>Ports réseau utilisés par le Service de temps Windows  
Le service de temps de Windows communique sur un réseau pour identifier les sources de temps fiable, obtenir des informations de temps et fournissent des informations de temps à d’autres ordinateurs. Il effectue cette communication tel que défini par le protocole NTP et SNTP RFC.  
  
**Affectations de port pour le Service de temps Windows**  
  
|Nom du service|UDP|TCP|  
|----------------|-------|-------|  
|NTP|123|N/A|  
|SNTP|123|N/A|  
  
## <a name="see-also"></a>Voir aussi  
[Référence technique du Service de temps Windows](windows-time-service-tech-ref.md)
[paramètres et outils de Service de temps Windows](Windows-Time-Service-Tools-and-Settings.md)
[l’article 902229 de la Base de connaissances Microsoft](https://go.microsoft.com/fwlink/?LinkId=186066)