---
title: Hôtes de Session Bureau à distance de réglage des performances
description: Réglage des performances instructions pour les hôtes de Session Bureau à distance
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: HammadBu; VladmiS
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: e95671718616fc7c81977434e83a227c858fca17
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811422"
---
# <a name="performance-tuning-remote-desktop-session-hosts"></a>Hôtes de Session Bureau à distance de réglage des performances


Cette rubrique explique comment choisir le matériel de l’hôte de Session Bureau à distance (hôte de Session Bureau à distance), paramétrer l’hôte et ajuster les applications.

**Dans cette rubrique :**

-   [Sélection du matériel approprié pour les performances](#selecting-the-proper-hardware-for-performance)

-   [Paramétrage des applications pour l’hôte de Session Bureau à distance](#tuning-applications-for-remote-desktop-session-host)

-   [Hôte de Session Bureau à distance, réglage des paramètres](#remote-desktop-session-host-tuning-parameters)

## <a name="selecting-the-proper-hardware-for-performance"></a>Sélection du matériel approprié pour favoriser la performance


Pour un déploiement de serveur hôte de Session Bureau à distance, le choix du matériel est régi par l’ensemble de l’application et la façon dont les utilisateurs les utiliser. Les facteurs clés qui affectent le nombre d’utilisateurs et leur expérience sont le processeur, mémoire, disque et graphiques. Cette section contient des indications supplémentaires qui sont spécifiques aux serveurs hôtes de Session Bureau à distance et est principalement liée à l’environnement multi-utilisateur, des serveurs hôtes de Session Bureau à distance.

### <a name="cpu-configuration"></a>Configuration de l’UC

Configuration de l’UC est conceptuellement déterminée en multipliant le processeur requis pour prendre en charge d’une session par le nombre de sessions que le système est censé prendre en charge, tout en conservant une zone de mémoire tampon pour gérer les pics temporaires. Plusieurs processeurs logiques peuvent aider à réduire des situations de congestion UC anormales, qui sont généralement provoquées par plusieurs threads hyperactif qui sont contenus par un nombre similaire de processeurs logiques.

Par conséquent, les processeurs plus logiques sur un système, moins la marge COUSSIN doit être intégrée à l’estimation de l’utilisation du processeur, ce qui entraîne une plus grande de la charge active par UC. Un facteur important à retenir est que le fait de doubler le nombre de processeurs ne double pas les capacités du processeur.

### <a name="memory-configuration"></a>Configuration de la mémoire

Configuration de la mémoire est dépendante sur les applications qui emploient les utilisateurs ; Toutefois, la quantité de mémoire requise peut être estimée à l’aide de la formule suivante : TotalMem = OSMem + SessionMem \* NS

OSMem est la quantité est de mémoire que du système d’exploitation doit pour exécuter (par exemple, les images binaires du système, des structures de données et ainsi de suite), SessionMem combien nécessitent des processus de mémoire en cours d’exécution dans une session et NS est le nombre cible de sessions actives. La quantité de mémoire requise pour une session est principalement déterminée par la référence de la mémoire privée définie pour les applications et les processus système qui sont en cours d’exécution à l’intérieur de la session. Les pages de code ou de données partagées ont peu d’effet, car une seule copie est présente sur le système.

Une observation intéressante (en supposant que le système de disque qui sauvegarde le fichier d’échange ne change pas) qui est plus le nombre de sessions actives simultanées du système prévoit de prendre en charge, plus l’allocation de mémoire par session doit être. Si la quantité de mémoire est allouée par session n’est pas augmentée, le nombre de défauts de page que les sessions actives générer augmente avec le nombre de sessions. Finalement, ces erreurs saturer le sous-système d’e/s. En augmentant la quantité de mémoire est allouée par session, la probabilité de subir des défauts de page réduit, ce qui permet de réduire le taux global de défauts de page.

### <a name="disk-configuration"></a>Configuration de disque

Le stockage est un des aspects plus souvent ignorés lorsque vous configurez des serveurs hôtes de Session Bureau à distance, et il peut être la limitation courante dans les systèmes qui sont déployés dans le champ.

L’activité du disque qui est générée sur un serveur hôte de Session Bureau à distance classique affecte les domaines suivants :

-   Fichiers système et les fichiers binaires d’application

-   Fichiers de page

-   Les profils utilisateur et les données utilisateur

Dans l’idéal, ces zones doivent être sauvegardées par les périphériques de stockage distinct. À l’aide des configurations RAID agrégés par bande ou autres types de stockage hautes performances davantage améliore les performances. Nous recommandons vivement d’utiliser des cartes de stockage avec la mise en cache d’écriture de batterie. Contrôleurs de disque cache d’écriture offrent une prise en charge améliorée pour les opérations d’écriture synchrone. Étant donné que tous les utilisateurs ont une ruche distincte, les opérations d’écriture synchrones sont beaucoup plus courantes sur un serveur hôte de Session Bureau à distance. Ruches du Registre sont régulièrement enregistrées sur le disque à l’aide d’opérations d’écriture synchrone. Pour activer ces optimisations, à partir de la console de gestion des disques, ouvrez le **propriétés** boîte de dialogue pour le disque de destination et sur le **stratégies** onglet, sélectionnez le **activer cache d’écriture sur le disque** et **désactiver de vidage de mémoire tampon de cache d’écriture Windows** appareil cases à cocher.

### <a name="network-configuration"></a>Configuration réseau

L’utilisation du réseau pour un serveur hôte de Session Bureau à distance inclut deux catégories principales :

-   L’utilisation du trafic de connexion hôte de Session Bureau à distance est déterminée presque exclusivement par les modèles de dessins qui sont exposés par les applications qui s’exécutent dans les sessions et le trafic d’e/s redirigé d’appareils.

    Par exemple, les applications de traitement de texte et des données d’entrée consomment la bande passante d’environ 10 à 100 kilobits par seconde, tandis que des graphiques et lecture vidéo entraînent une augmentation considérable dans l’utilisation de la bande passante.

-   Connexions back-end tels que les profils itinérants, application à accéder à des partages de fichiers, des serveurs de base de données, des serveurs de messagerie et des serveurs HTTP.

    Le volume et le profil de trafic réseau est spécifique à chaque déploiement.

## <a name="tuning-applications-for-remote-desktop-session-host"></a>Paramétrage des applications pour l’hôte de Session Bureau à distance


La majeure partie de l’utilisation du processeur sur un serveur hôte de Session Bureau à distance est piloté par les applications. Applications de bureau sont généralement optimisées vers réactivité dans le but de réduire au minimum la durée d’une application de répondre à une demande de l’utilisateur. Toutefois, dans un environnement de serveur, il est également important réduire la quantité totale de l’utilisation du processeur qui est nécessaire pour effectuer une action pour éviter de pénaliser les autres sessions.

Lorsque vous configurez les applications qui doivent être utilisées sur un serveur hôte de Session Bureau à distance, prenez en compte les suggestions suivantes :

-   Réduire le traitement des boucles inactives en arrière-plan

    Exemples typiques sont la désactivation d’arrière-plan vérification orthographique et grammaticale, l’indexation pour la recherche, des données et les enregistrements en arrière-plan.

-   Réduire la fréquence à laquelle une application effectue une vérification de l’état ou la mise à jour.

    La désactivation de ces comportements ou en augmentant l’intervalle entre les itérations d’interrogation et de minuteur déclenche considérablement bénéficie l’utilisation du processeur, car l’effet de ces activités est amplifié rapidement de nombreuses sessions actives. Exemples typiques sont des icônes d’état de connexion et de la barre d’informations mises à jour.

-   Réduire les conflits de ressources entre les applications en réduisant leur fréquence de synchronisation.

    Clés de Registre et les fichiers de configuration sont des exemples de ces ressources. Exemples de fonctionnalités et composants d’application sont l’indicateur d’état (telles que les notifications de l’interpréteur de commandes), l’indexation en arrière-plan ou surveillance des modifications et la synchronisation hors connexion.

-   Désactiver les processus inutiles qui sont inscrits pour démarrer avec un démarrage de session ou de connexion de l’utilisateur.

    Ces processus peuvent considérablement contribuer au coût d’utilisation du processeur lors de la création d’une nouvelle session d’utilisateur, ce qui est généralement un processus d’utilisation intensive du processeur, et il peut être très coûteux dans les scénarios du matin. Utilisez MsConfig.exe ou MsInfo32.exe pour obtenir une liste des processus qui sont démarrés à la connexion de l’utilisateur. Pour plus d’informations, vous pouvez utiliser [Autoruns pour Windows](https://technet.microsoft.com/sysinternals/bb963902.aspx).

Pour la consommation de mémoire, vous devez envisager les éléments suivants :

-   Vérifiez que les DLL chargées par une application ne sont pas déplacés.

    -   Déplacé DLL peuvent être vérifiés en sélectionnant la vue de la DLL de processus, comme indiqué dans l’illustration suivante, à l’aide de [Process Explorer](https://technet.microsoft.com/sysinternals/bb896653.aspx).

    -   Ici, nous pouvons voir que y.dll a été déplacées, car x.dll déjà occupé son adresse de base par défaut et ASLR n’a pas été activée.

        ![DLL déplacés](../../media/perftune-guide-relocated-dlls.png)

        Si la DLL sont déplacées, il est impossible de partager leur code entre les sessions, ce qui augmente considérablement l’encombrement d’une session. Il s’agit d’un des problèmes de performances liés à la mémoire plus courants sur un serveur hôte de Session Bureau à distance.

-   Pour les applications de common language runtime (CLR), utilisez Native Image Generator (Ngen.exe) pour augmenter le partage de page et de réduire la surcharge du processeur.

    Dans la mesure du possible, appliquer des techniques similaires à d’autres moteurs d’exécution similaire.

## <a name="remote-desktop-session-host-tuning-parameters"></a>Hôte de Session Bureau à distance, réglage des paramètres


### <a name="page-file"></a>Fichier d’échange

Taille de fichier page insuffisante peut provoquer mémoire échecs d’allocation dans les applications ou composants du système. Vous pouvez utiliser le compteur de performances mémoire-à octets alloués en surveiller la quantité de mémoire virtuelle validée sur le système.

### <a name="antivirus"></a>Antivirus

L’installation d’un logiciel antivirus sur un serveur hôte de Session Bureau à distance considérablement affecte les performances globales du système, en particulier l’utilisation du processeur. Nous recommandons vivement que vous excluez de la liste de surveillance active tous les dossiers qui contiennent des fichiers temporaires, en particulier ceux qui les services et autres composants système génèrent.

### <a name="task-scheduler"></a>Planificateur de tâches

Planificateur de tâches vous permet d’examiner la liste des tâches qui sont planifiées pour les différents événements. Pour un serveur hôte de Session Bureau à distance, il est utile de se concentrer plus précisément sur les tâches qui sont configurés pour s’exécuter sur inactif, lors de l’authentification de l’utilisateur, ou sur la session se connecter et déconnecter. En raison des spécificités du déploiement, nombre de ces tâches peuvent s’avérer superflues.

### <a name="desktop-notification-icons"></a>Icônes de notification du bureau

Icônes de notification sur le bureau peuvent avoir des mécanismes de l’actualisation relativement coûteuses. Vous devez désactiver les notifications en supprimant le composant qui les enregistre dans la liste de démarrage ou en modifiant la configuration sur les applications et composants du système pour les désactiver. Vous pouvez utiliser **personnaliser des icônes de Notifications** pour examiner la liste des notifications qui sont disponibles sur le serveur.

### <a name="remotefx-data-compression"></a>Compression de données de RemoteFX

La compression de Microsoft RemoteFX peut être configurée à l’aide de stratégie de groupe sous **Configuration ordinateur &gt; modèles d’administration &gt; les composants Windows &gt; Services Bureau à distance &gt; à distance Hôte de Session bureau &gt; environnement de Session à distance &gt; configurer la compression des données de RemoteFX**. Trois valeurs sont possibles :

-   **Optimisé pour utiliser moins de mémoire** consomme le moins de mémoire par session, mais a le taux de compression la plus basse et, par conséquent, la consommation de bande passante plus élevée.

-   **Équilibre la bande passante réseau et mémoire** réduit la consommation de bande passante tout en augmentant légèrement la consommation de mémoire (environ 200 Ko par session).

-   **Optimisé pour utiliser moins de bande passante réseau** réduit encore la bande passante réseau pour un coût d’environ 2 Mo par session. Si vous souhaitez utiliser ce paramètre, vous devez évaluer le nombre maximal de sessions et tester à ce niveau avec ce paramètre avant de placer le serveur de production.

Vous pouvez également choisir de ne pas utiliser un algorithme de compression de RemoteFX. En choisissant de ne pas utiliser un algorithme de compression RemoteFX utilisera davantage de bande passante réseau, et il est recommandé uniquement si vous utilisez un périphérique matériel qui est conçu pour optimiser le trafic réseau. Même si vous choisissez de ne pas utiliser un algorithme de compression de RemoteFX, certaines données de graphique seront compressées.

### <a name="device-redirection"></a>Redirection de périphérique

La redirection de périphérique peut être configurée à l’aide de stratégie de groupe sous **Configuration ordinateur &gt; modèles d’administration &gt; les composants Windows &gt; Services Bureau à distance &gt; Bureau à distance Hôte de session &gt; appareil et des ressources** ou à l’aide de la **Collection de sessions** zone de propriétés dans le Gestionnaire de serveur.

En règle générale, la redirection de périphérique augmente combien les connexions au serveur hôte de Session Bureau à distance la bande passante réseau utilisent, car les données sont échangées entre les appareils sur les ordinateurs clients et les processus qui s’exécutent dans la session du serveur. L’étendue de l’augmentation est une fonction de la fréquence des opérations qui sont effectuées par les applications qui sont exécutent sur le serveur sur les appareils redirigés.

La redirection d’imprimante et des Plug-and-Play la redirection de périphérique augmente également l’utilisation du processeur à la connexion. Vous pouvez rediriger les imprimantes de deux manières :

-   Correspondance en fonction du pilote la redirection d’imprimante lorsqu’un pilote d’imprimante doit être installée sur le serveur. Les versions antérieures de Windows Server utilisé cette méthode.

-   Introduite dans Windows Server 2008, la redirection pilote d’imprimante Easy Print utilise un pilote d’imprimante commun pour toutes les imprimantes.

Nous vous recommandons de la méthode Print facile, car elle entraîne moins l’utilisation du processeur pour l’installation de l’imprimante au moment de la connexion. La méthode de pilote correspondant entraîne une utilisation accrue du processeur, car elle requiert le service de spouleur pour charger des pilotes différents. Pour l’utilisation de la bande passante, Easy Print provoque l’utilisation de la bande passante réseau ont légèrement augmenté, mais pas assez importante pour les autres avantages de performances, la facilité de gestion et la fiabilité de décalage.

La redirection audio provoque un flux constant de trafic réseau. La redirection audio permet également aux utilisateurs d’exécuter des applications multimédias qui ont généralement une consommation élevée du processeur.

### <a name="client-experience-settings"></a>Paramètres de l’expérience client

Par défaut, connexion Bureau à distance (RDC) choisit automatiquement le droit d’expérience de paramètre en fonction de l’adéquation de la connexion réseau entre le serveur et les ordinateurs clients. Nous recommandons que la configuration de la compression différentielle à distance reste à **détecter automatiquement la qualité de la connexion**.

Pour les utilisateurs expérimentés, RDC permet de contrôler un éventail de paramètres qui influencent les performances de la bande passante réseau pour la connexion des Services Bureau à distance. Vous pouvez accéder à des paramètres suivants à l’aide de la **expérience** onglet Connexion Bureau à distance ou en tant que paramètres dans le fichier RDP.

Les paramètres suivants s’appliquent lors de la connexion à n’importe quel ordinateur :

-   **Désactiver le papier peint** (désactivation du fond d’écran : i : 0) n’affiche pas de papier peint du bureau sur les connexions redirigées. Ce paramètre peut réduire considérablement la bande passante si le papier peint du bureau se compose d’une image ou tout autre contenu avec des coûts importants pour le dessin.

-   **Cache de bitmap** (Bitmapcachepersistenable:i:1) lorsque ce paramètre est activé, il crée un cache côté client de bitmaps qui sont rendus dans la session. Il fournit une amélioration significative sur l’utilisation de la bande passante, et elle doit toujours être activée (sauf s’il existe des autres considérations de sécurité).

-   **Affichez le contenu de windows tout en faisant glisser** (désactiver la fenêtre entière, faites glisser : i : 1) lorsque ce paramètre est désactivé, il réduit la bande passante en affichant le frame de fenêtre au lieu de tout le contenu lorsque vous faites glisser la fenêtre.

-   **Animation des menus et des fenêtres** (Disable menu anims:i:1 et Disable curseur paramètre : i : 1) : Lorsque ces paramètres sont désactivés, il réduit la bande passante en désactivant l’animation dans les menus (par exemple, le fondu) et les curseurs.

-   **Lissage des polices** (autoriser le lissage des polices : i : 0) prise en charge du rendu des polices ClearType de contrôles. Lors de la connexion aux ordinateurs exécutant Windows 8 ou Windows Server 2012 et versions ultérieures, l’activation ou désactivation de ce paramètre n’a pas un impact significatif sur l’utilisation de la bande passante. Toutefois, pour les ordinateurs exécutant des versions antérieures à Windows 7 et Windows 2008 R2, l’activation de ce paramètre affecte la consommation de bande passante réseau considérablement.

Les paramètres suivants s’appliquent uniquement lors de la connexion à des ordinateurs exécutant Windows 7 et les versions antérieures du système d’exploitation :

-   **Composition du bureau** ce paramètre est pris en charge uniquement pour une session à distance sur un ordinateur exécutant Windows 7 ou Windows Server 2008 R2.

-   **Styles visuels** (désactiver les thèmes : i : 1) lorsque ce paramètre est désactivé, il réduit la bande passante en simplifiant les dessins de thème qui utilisent le thème classique.

À l’aide de la **expérience** onglet au sein de la connexion Bureau à distance, vous pouvez choisir votre vitesse de connexion pour influencer les performances de la bande passante réseau. Ce qui suit répertorie les options qui sont disponibles pour configurer votre vitesse de connexion :

-   **Détecter automatiquement la qualité de la connexion** (type de connexion : i:7) lorsque ce paramètre est activé, connexion Bureau à distance choisit automatiquement les paramètres qui entraîneront dans expérience utilisateur optimale en fonction de la qualité de la connexion. (Cette configuration est recommandée lors de la connexion aux ordinateurs exécutant Windows 8 ou Windows Server 2012 et versions ultérieures).

-   **Modem (56 Kbits/s)** (connexion type : i : 1) ce paramètre permet la mise en cache de bitmap persistant.

-   **Faible vitesse à large bande (de 256 Kbits/s - 2 Mbits/s)** (type de connexion : i:2) ce paramètre active les styles de mise en cache et visuelles bitmap persistant.

-   **Un réseau cellulaire/Satellite (2 Mbits/s - 16 Mbits/s avec une latence élevée)** (type de connexion : i:3) ce paramètre permet la composition du bureau, la mise en cache des bitmaps permanente, les styles visuels et arrière-plan du poste.

-   **Débit (de 2 Mbits/s – 10 Mbits/s)** (type de connexion : i:4) ce paramètre permet la composition du bureau, affichez le contenu de windows tout en faisant glisser, animation des menus et des fenêtres, la mise en cache des bitmaps permanente, styles visuels et d’arrière-plan du poste.

-   **Réseau étendu (10 Mbits/s ou version ultérieure avec une latence élevée)** (type de connexion : i:5) ce paramètre permet la composition du bureau, affichez le contenu de windows tout en faisant glisser, animation des menus et des fenêtres, la mise en cache des bitmaps permanente, styles visuels et d’arrière-plan du poste.

-   **LAN (10 Mbits/s ou version ultérieure)** (type de connexion : i:6) ce paramètre permet la composition du bureau, affichez le contenu de windows tout en faisant glisser, animation des menus et des fenêtres, la mise en cache des bitmaps permanente, thèmes et d’arrière-plan du poste.

### <a name="desktop-size"></a>Taille du bureau

Taille du bureau pour les sessions à distance peut être contrôlé à l’aide de l’onglet Affichage dans Connexion Bureau à distance ou en utilisant le fichier de configuration de RDP (desktopwidth:i:1152 et desktopheight:i:864). Plus la taille du bureau, plus la consommation de mémoire et la bande passante qui est associée à cette session. La taille maximale de postes de travail actuelle est de 4096 x 2048.
