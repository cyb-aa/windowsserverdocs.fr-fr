---
title: Réglage des performances Bureau à distance hôtes de session
description: Instructions de réglage des performances pour les hôtes de session Bureau à distance
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: hammadbu; vladmis; denisgun
author: phstee
ms.date: 10/22/2019
ms.openlocfilehash: 3227bfe3bf21343ca9b7e85a07f550b4684a2fb7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851712"
---
# <a name="performance-tuning-remote-desktop-session-hosts"></a>Réglage des performances Bureau à distance hôtes de session


Cette rubrique explique comment sélectionner Bureau à distance matériel hôte de session Bureau à distance (hôte de session Bureau à distance), régler l’hôte et paramétrer les applications.

**Dans cette rubrique :**

-   [Sélection du matériel approprié pour les performances](#selecting-the-proper-hardware-for-performance)

-   [Paramétrage des applications pour Bureau à distance hôte de session](#tuning-applications-for-remote-desktop-session-host)

-   [Paramètres de paramétrage de l’hôte de session Bureau à distance](#remote-desktop-session-host-tuning-parameters)

## <a name="selecting-the-proper-hardware-for-performance"></a>Sélection du matériel approprié pour favoriser la performance


Pour un déploiement de serveur hôte de session Bureau à distance, le choix du matériel est régi par le jeu d’applications et la manière dont les utilisateurs les utilisent. Les facteurs clés qui affectent le nombre d’utilisateurs et leur expérience sont le processeur, la mémoire, le disque et les graphiques. Cette section contient des instructions supplémentaires spécifiques aux serveurs hôtes de session Bureau à distance qui sont principalement liées à l’environnement multi-utilisateur des serveurs hôtes de session Bureau à distance.

### <a name="cpu-configuration"></a>Configuration de l’UC

La configuration de l’UC est déterminée d’un point de vue conceptuel en multipliant le processeur requis pour prendre en charge une session par le nombre de sessions que le système est censé prendre en charge, tout en conservant une zone de mémoire tampon pour gérer les pics temporaires. Plusieurs processeurs logiques permettent de réduire les situations anormales de congestion du processeur, qui sont généralement dues à quelques threads suractifs contenus dans un nombre similaire de processeurs logiques.

Par conséquent, plus il y a de processeurs logiques sur un système, moins la marge de coussin doit être intégrée à l’estimation de l’utilisation du processeur, ce qui entraîne un pourcentage plus élevé de charge active par UC. Un facteur important à retenir est que le doublement du nombre de processeurs ne double pas la capacité de l’UC.

### <a name="memory-configuration"></a>Configuration de la mémoire

La configuration de la mémoire dépend des applications que les utilisateurs emploient. Toutefois, la quantité de mémoire requise peut être estimée à l’aide de la formule suivante : TotalMem = OSMem + SessionMem \* NS

OSMem indique la quantité de mémoire nécessaire pour exécuter le système d’exploitation (par exemple, les images binaires système, les structures de données, etc.), SessionMem est la quantité de processus de mémoire en cours d’exécution dans une session, et NS est le nombre cible de sessions actives. La quantité de mémoire requise pour une session est principalement déterminée par la référence de mémoire privée définie pour les applications et les processus système qui s’exécutent dans la session. Le code partagé ou les pages de données ont un effet minime, car une seule copie est présente sur le système.

Une observation intéressante (en supposant que le système de disque qui sauvegarde le fichier d’échange ne change pas) est que plus le nombre de sessions actives simultanées que le système prévoit de prendre en charge est élevé, plus l’allocation de mémoire par session doit être importante. Si la quantité de mémoire allouée par session n’est pas augmentée, le nombre de défauts de page générés par les sessions actives augmente avec le nombre de sessions. Ces erreurs sont susceptibles de saturer le sous-système d’e/s. En renforçant la quantité de mémoire allouée par session, la probabilité de défaillances de page incalculées diminue, ce qui permet de réduire le taux global de défauts de page.

### <a name="disk-configuration"></a>Configuration de disque

Le stockage est l’un des aspects les plus négligés lorsque vous configurez des serveurs hôtes de session Bureau à distance, et il peut s’agir de la limitation la plus courante des systèmes déployés dans le champ.

L’activité du disque qui est générée sur un serveur hôte de session Bureau à distance classique affecte les zones suivantes :

-   Fichiers système et fichiers binaires d’application

-   Fichiers d’échange

-   Profils utilisateur et données utilisateur

Dans l’idéal, ces zones doivent être sauvegardées par des dispositifs de stockage distincts. L’utilisation de configurations RAID entrelacées ou d’autres types de stockage à hautes performances améliore les performances. Nous vous recommandons vivement d’utiliser des adaptateurs de stockage avec une mise en cache d’écriture avec batterie de secours. Les contrôleurs avec le cache d’écriture de disque offrent une meilleure prise en charge des opérations d’écriture synchrones. Étant donné que tous les utilisateurs disposent d’une ruche distincte, les opérations d’écriture synchrones sont beaucoup plus courantes sur un serveur hôte de session Bureau à distance. Les ruches de Registre sont régulièrement enregistrées sur le disque à l’aide d’opérations d’écriture synchrones. Pour activer ces optimisations, à partir de la console Gestion des disques, ouvrez la boîte de dialogue **Propriétés** du disque de destination et, sous l’onglet **stratégies** , activez la case à cocher **activer le cache d’écriture sur le disque** et **désactiver le vidage du cache d’écriture Windows** sur le périphérique.

### <a name="network-configuration"></a>Configuration réseau

L’utilisation du réseau pour un serveur hôte de session Bureau à distance comprend deux catégories principales :

-   L’utilisation du trafic de connexion de l’hôte de session Bureau à distance est déterminée presque exclusivement par les modèles de dessin exposés par les applications qui s’exécutent dans les sessions et le trafic d’e/s des appareils redirigés.

    Par exemple, les applications gérant le traitement du texte et les entrées de données consomment de la bande passante d’environ 10 à 100 kilobits par seconde, tandis que les graphiques enrichis et la lecture vidéo augmentent considérablement l’utilisation de la bande passante.

-   Les connexions principales, telles que les profils itinérants, l’accès des applications aux partages de fichiers, les serveurs de base de données, les serveurs de messagerie et les serveurs HTTP.

    Le volume et le profil du trafic réseau sont spécifiques à chaque déploiement.

## <a name="tuning-applications-for-remote-desktop-session-host"></a>Paramétrage des applications pour Bureau à distance hôte de session


La majeure partie de l’utilisation du processeur sur un serveur hôte de session Bureau à distance est pilotée par les applications. Les applications de bureau sont généralement optimisées pour la réactivité dans le but de réduire le temps nécessaire à une application pour répondre à une demande de l’utilisateur. Toutefois, dans un environnement de serveur, il est tout aussi important de réduire la quantité totale d’utilisation de l’UC nécessaire pour effectuer une action afin d’éviter d’affecter négativement d’autres sessions.

Tenez compte des suggestions suivantes lorsque vous configurez des applications qui doivent être utilisées sur un serveur hôte de session Bureau à distance :

-   Réduire le traitement de la boucle inactive d’arrière-plan

    Les exemples classiques sont la désactivation de la grammaire et de l’orthographe en arrière-plan, l’indexation des données pour la recherche et les enregistrements en arrière-plan.

-   Réduisez la fréquence à laquelle une application effectue un contrôle d’État ou une mise à jour.

    La désactivation de tels comportements ou l’amélioration de l’intervalle entre les itérations d’interrogation et l’activation du minuteur améliore considérablement l’utilisation du processeur, car l’effet de ces activités est rapidement amplifié pour de nombreuses sessions actives. Les icônes d’état de connexion et les mises à jour des informations sur la barre d’État en sont des exemples typiques.

-   Minimiser la contention des ressources entre les applications en réduisant leur fréquence de synchronisation.

    Les clés de Registre et les fichiers de configuration sont des exemples de ressources de ce type. Des exemples de composants et fonctionnalités d’application sont des indicateurs d’État (tels que les notifications de Shell), l’indexation en arrière-plan ou la surveillance des modifications et la synchronisation hors connexion.

-   Désactivez les processus inutiles inscrits pour démarrer avec la connexion de l’utilisateur ou au démarrage d’une session.

    Ces processus peuvent contribuer de manière significative au coût de l’utilisation du processeur lors de la création d’une nouvelle session utilisateur, qui est généralement un processus gourmand en ressources processeur et qui peut être très coûteux dans les matins. Utilisez MsConfig. exe ou MsInfo32. exe pour obtenir une liste des processus démarrés lors de la connexion de l’utilisateur. Pour obtenir des informations plus détaillées, vous pouvez utiliser [Autoruns pour Windows](https://technet.microsoft.com/sysinternals/bb963902.aspx).

Pour la consommation de mémoire, vous devez prendre en compte les éléments suivants :

-   Vérifiez que les DLL chargées par une application ne sont pas déplacées.

    -   Les dll déplacées peuvent être vérifiées en sélectionnant traiter la vue DLL, comme illustré dans la figure suivante, à l’aide de l' [Explorateur de processus](https://technet.microsoft.com/sysinternals/bb896653.aspx).

    -   Ici, nous voyons que la dll y a été déplacée car x. dll était déjà occupé à son adresse de base par défaut et que ASLR n’était pas activé

        ![dll déplacées](../../media/perftune-guide-relocated-dlls.png)

        Si les dll sont déplacées, il est impossible de partager leur code entre les sessions, ce qui augmente considérablement l’encombrement d’une session. Il s’agit de l’un des problèmes de performances les plus courants liés à la mémoire sur un serveur hôte de session Bureau à distance.

-   Pour les applications common language runtime (CLR), utilisez le générateur d’images natives (Ngen. exe) pour augmenter le partage des pages et réduire la surcharge du processeur.

    Dans la mesure du possible, appliquez des techniques similaires à d’autres moteurs d’exécution similaires.

## <a name="remote-desktop-session-host-tuning-parameters"></a>Paramètres de paramétrage de l’hôte de session Bureau à distance


### <a name="page-file"></a>Fichier d’échange

Une taille de fichier d’échange insuffisante peut provoquer des échecs d’allocation de mémoire dans les applications ou les composants système. Vous pouvez utiliser le compteur de performances mémoire-octets alloués pour surveiller la quantité de mémoire virtuelle validée sur le système.

### <a name="antivirus"></a>Antivirus

L’installation d’un logiciel antivirus sur un serveur hôte de session Bureau à distance affecte les performances globales du système, notamment l’utilisation du processeur. Nous vous recommandons vivement d’exclure de la liste de surveillance active tous les dossiers qui contiennent des fichiers temporaires, en particulier ceux qui sont générés par les services et d’autres composants système.

### <a name="task-scheduler"></a>Planificateur de tâches

Planificateur de tâches vous permet d’examiner la liste des tâches qui sont planifiées pour des événements différents. Pour un serveur hôte de session Bureau à distance, il est utile de se concentrer spécifiquement sur les tâches qui sont configurées pour s’exécuter en mode inactif, lors de la connexion de l’utilisateur ou lors de la connexion et de la déconnexion de session. En raison des spécificités du déploiement, un grand nombre de ces tâches peuvent être inutiles.

### <a name="desktop-notification-icons"></a>Icônes de notification du Bureau

Les icônes de notification sur le bureau peuvent avoir des mécanismes d’actualisation relativement coûteux. Vous devez désactiver les notifications en supprimant le composant qui les enregistre dans la liste de démarrage ou en modifiant la configuration des applications et des composants système pour les désactiver. Vous pouvez utiliser les **icônes de personnalisation des notifications** pour examiner la liste des notifications disponibles sur le serveur.

### <a name="remote-desktop-protocol-data-compression"></a>Compression des données de protocole RDP (Remote Desktop Protocol)

La compression de protocole RDP (Remote Desktop Protocol) peut être configurée à l’aide de stratégie de groupe sous **Configuration ordinateur** &gt; **modèles d’administration** &gt; **composants Windows** **&gt; services Bureau à distance** &gt; Bureau à distance **hôte de session** &gt; de session **à distance** &gt; **configurer la compression pour les données RemoteFX**. Trois valeurs sont possibles :

-   **Optimisé pour utiliser moins de mémoire** Consomme la quantité de mémoire minimale par session, mais a le taux de compression le plus faible et, par conséquent, la consommation de bande passante la plus élevée.

-   **Équilibre la mémoire et la bande passante réseau** Une consommation de bande passante réduite tout en renforçant la consommation de mémoire (environ 200 Ko par session).

-   **Optimisé pour utiliser moins de bande passante réseau** Réduit davantage l’utilisation de la bande passante réseau à un coût d’environ 2 Mo par session. Si vous souhaitez utiliser ce paramètre, évaluez le nombre maximal de sessions et testez ce niveau avec ce paramètre avant de placer le serveur en production.

Vous pouvez également choisir de ne pas utiliser un algorithme de compression protocole RDP (Remote Desktop Protocol), donc nous vous recommandons de l’utiliser uniquement avec un périphérique matériel conçu pour optimiser le trafic réseau. Même si vous choisissez de ne pas utiliser un algorithme de compression, certaines données graphiques sont compressées.

### <a name="device-redirection"></a>Redirection de périphérique

La redirection de périphérique peut être configurée à l’aide de stratégie de groupe sous **Configuration ordinateur** &gt; **Modèles d’administration** **composants Windows** **&gt; &gt; services Bureau à distance &gt; Bureau à distance** &gt; hôte de **session** **et la redirection de ressources** , ou à l’aide de la zone de propriétés de la collection de **sessions** dans Gestionnaire de serveur.

En règle générale, la redirection de périphérique augmente la quantité de connexions au serveur hôte de session Bureau à distance réseau utilisée, car les données sont échangées entre les appareils sur les ordinateurs clients et les processus qui s’exécutent dans la session serveur. L’étendue de l’augmentation est une fonction de la fréquence des opérations exécutées par les applications qui s’exécutent sur le serveur sur les appareils redirigés.

La redirection de l’imprimante et la redirection de l’appareil Plug-and-Play augmentent également l’utilisation du processeur lors de la connexion. Vous pouvez rediriger les imprimantes de deux manières :

-   Correspondance de la redirection basée sur le pilote d’imprimante lorsqu’un pilote pour l’imprimante doit être installé sur le serveur. Les versions antérieures de Windows Server utilisaient cette méthode.

-   Introduite dans Windows Server 2008, la redirection de pilote d’imprimante facile à imprimer utilise un pilote d’imprimante commun pour toutes les imprimantes.

Nous vous recommandons d’utiliser la méthode d’impression simple, car elle entraîne moins d’utilisation de l’UC pour l’installation de l’imprimante au moment de la connexion. La méthode de pilote correspondante entraîne une augmentation de l’utilisation du processeur, car elle nécessite que le service de spouleur charge des pilotes différents. Pour l’utilisation de la bande passante, l’impression simple entraîne une utilisation plus importante de la bande passante réseau, mais n’est pas suffisamment importante pour compenser les autres avantages en matière de performances, de gestion et de fiabilité.

La redirection audio provoque un flux de trafic réseau stable. La redirection audio permet également aux utilisateurs d’exécuter des applications multimédias qui ont généralement une consommation élevée de l’UC.

### <a name="client-experience-settings"></a>Paramètres d’expérience client

Par défaut, Connexion Bureau à distance (RDC) choisit automatiquement le paramètre d’expérience approprié en fonction de l’aptitude de la connexion réseau entre le serveur et les ordinateurs clients. Nous vous recommandons d’utiliser la configuration RDC pour **détecter automatiquement la qualité**de la connexion.

Pour les utilisateurs expérimentés, RDC permet de contrôler une gamme de paramètres qui influencent les performances de la bande passante réseau pour la connexion Services Bureau à distance. Vous pouvez accéder aux paramètres suivants à l’aide de l’onglet **expérience** dans Connexion Bureau à distance ou en tant que paramètres dans le fichier RDP.

Les paramètres suivants s’appliquent lors de la connexion à un ordinateur :

-   **Désactiver** le papier peint (désactiver le papier peint : i : 0) n’affiche pas le papier peint du Bureau sur les connexions redirigées. Ce paramètre peut réduire considérablement l’utilisation de la bande passante si le papier peint du Bureau est constitué d’une image ou d’un autre contenu, avec des coûts importants pour le dessin.

-   **Cache bitmap** (Bitmapcachepersistenable : i : 1) quand ce paramètre est activé, il crée un cache côté client des bitmaps qui sont rendus dans la session. Il offre une amélioration significative de l’utilisation de la bande passante et doit toujours être activé (à moins qu’il n’y ait d’autres considérations relatives à la sécurité).

-   **Afficher le contenu des fenêtres pendant le glissement** (désactiver le glissement de fenêtre entière : i : 1) quand ce paramètre est désactivé, il réduit la bande passante en affichant uniquement le cadre de la fenêtre au lieu de tout le contenu quand la fenêtre est glissée.

-   **Animation de menu et de fenêtre** (désactiver le menu des paramètres de curseur : i : 1 et désactiver le paramètre de curseur : i : 1) : quand ces paramètres sont désactivés, cela réduit la bande passante en désactivant l’animation dans les menus (tels que le fondu) et les curseurs.

-   **Lissage des polices** (autoriser le lissage des polices : i : 0) contrôle la prise en charge de la conversion des polices ClearType. Lorsque vous vous connectez à des ordinateurs exécutant Windows 8 ou Windows Server 2012 et versions ultérieures, l’activation ou la désactivation de ce paramètre n’a pas d’impact significatif sur l’utilisation de la bande passante. Toutefois, pour les ordinateurs exécutant des versions antérieures à Windows 7 et Windows 2008 R2, l’activation de ce paramètre a une incidence significative sur la consommation de bande passante réseau.

Les paramètres suivants s’appliquent uniquement lors de la connexion à des ordinateurs exécutant Windows 7 et versions antérieures du système d’exploitation :

-   **Composition du Bureau** Ce paramètre est pris en charge uniquement pour une session à distance sur un ordinateur exécutant Windows 7 ou Windows Server 2008 R2.

-   **Styles visuels** (désactiver les thèmes : i : 1) quand ce paramètre est désactivé, il réduit la bande passante en simplifiant les dessins de thème qui utilisent le thème classique.

À l’aide de l’onglet **expérience** dans Connexion Bureau à distance, vous pouvez choisir la vitesse de connexion pour influencer les performances de la bande passante réseau. La liste suivante répertorie les options disponibles pour configurer la vitesse de connexion :

-   **Détecter automatiquement la qualité** de la connexion (type de connexion : i : 7) quand ce paramètre est activé, connexion Bureau à distance choisit automatiquement les paramètres qui donneront une expérience utilisateur optimale en fonction de la qualité de la connexion. (Cette configuration est recommandée lors de la connexion à des ordinateurs exécutant Windows 8 ou Windows Server 2012 et versions ultérieures).

-   **Modem (56 Kbits/s)** (type de connexion : i : 1) ce paramètre active la mise en cache des bitmaps persistante.

-   **Haut débit à basse vitesse (256 kbits/s-2 Mbits/s)** (type de connexion : i : 2) ce paramètre active la mise en cache des bitmaps persistante et les styles visuels.

-   **Cellulaire/satellite (2Mbps-16 Mbits/s avec une latence élevée)** (type de connexion : i : 3) ce paramètre active la composition du bureau, la mise en cache des bitmaps persistante, les styles visuels et l’arrière-plan du bureau.

-   Haut **débit haute vitesse (2 Mbits/s – 10 Mbits/s)** (type de connexion : i : 4) ce paramètre active la composition du bureau, affiche le contenu des fenêtres pendant le glissement, l’animation des menus et des fenêtres, la mise en cache des bitmaps persistante, les styles visuels et l’arrière-plan du bureau.

-   **WAN (10 Mbits/s ou plus avec une latence élevée)** (type de connexion : i : 5) ce paramètre active la composition du bureau, affiche le contenu des fenêtres pendant le glissement, l’animation des menus et des fenêtres, la mise en cache des bitmaps persistantes, les styles visuels et l’arrière-plan du bureau.

-   **Réseau local (10 Mbits/s ou plus)** (type de connexion : i : 6) ce paramètre active la composition du bureau, affiche le contenu des fenêtres pendant le glissement, l’animation des menus et des fenêtres, la mise en cache des bitmaps persistante, les thèmes et l’arrière-plan du bureau.

### <a name="desktop-size"></a>Taille du Bureau

La taille du Bureau pour les sessions à distance peut être contrôlée à l’aide de l’onglet Affichage de Connexion Bureau à distance ou à l’aide du fichier de configuration RDP (DesktopWidth : i : 1152 et DesktopHeight : i : 864). Plus la taille du Bureau est grande, plus la consommation de mémoire et de bande passante associée à cette session est élevée. La taille maximale actuelle du Bureau est de 4096 x 2048.
