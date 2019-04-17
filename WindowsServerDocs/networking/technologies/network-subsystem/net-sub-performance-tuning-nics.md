---
title: Cartes réseau réglage de performance
description: Cette rubrique fait partie du guide de réglage des performances sous-système réseau pour Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0b9b0f80-415c-4f5e-8377-c09b51d9c5dd
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1c6d36966ce6e2d407b6568e16946745256ee69b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="performance-tuning-network-adapters"></a>Cartes réseau réglage de performance

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour les cartes réseau de réglage performances qui sont installées sur des ordinateurs qui exécutent Windows Server 2016.

Détermination des paramètres de réglage adéquats pour votre carte réseau dépendent les variables suivantes:

- La carte réseau et son jeu de fonctionnalités  

- Le type de charge de travail effectué par le serveur  

- Les ressources matérielles et logicielles du serveur  

- Vos objectifs de performances pour le serveur  

Si votre carte réseau propose des options de réglage, vous pouvez optimiser l’utilisation de débit et de ressources réseau pour atteindre un débit optimal basé sur les paramètres décrits ci-dessus.  

Les sections suivantes décrivent certaines options de réglage des performances.  

##  <a name="bkmk_offload"></a>L’activation des fonctionnalités de déchargement

Activation des fonctionnalités de déchargement de carte réseau est souvent bénéfique. Parfois, cependant, la carte réseau n’est pas toujours suffisamment puissante pour gérer les capacités de déchargement avec un débit élevé.

>[!IMPORTANT]
>N’utilisez pas les fonctionnalités de déchargement **déchargement de tâche IPsec** ou **le déchargement TCP Chimney**. Ces technologies sont déconseillées dans Windows Server 2016 et susceptibles d’affecter les performances du réseau et serveur. En outre, ces technologies ne peuvent pas être pris en charge par Microsoft à l’avenir.

Par exemple, l’activation du déchargement de segmentation peut réduire le débit maximal sur certaines cartes réseau en raison de ressources matérielles limitées. Toutefois, si le débit réduit ne constitue pas une limitation, vous devez activer les capacités de déchargement, de même pour ce type de carte réseau.

>[!NOTE]
> Certaines cartes réseau requièrent des fonctionnalités de déchargement soient activées indépendamment pour envoyer et recevoir des chemins d’accès.

##  <a name="bkmk_rss_web"></a>L’activation de recevoir du trafic entrant (RSS) pour les serveurs Web

RSS peut améliorer les performances et l’extensibilité web lorsqu’il y a moins de cartes réseau que de processeurs logiques sur le serveur. Lorsque tout le trafic web passe par les cartes réseau compatible RSS, les demandes web entrantes provenant de connexions différentes peuvent être traitées simultanément dans différents processeurs.

Il est important de noter que, en raison de la logique de RSS et Hypertext Transfer \(HTTP\) de protocole pour la distribution de la charge, performances risquent d’être considérablement altérées si une carte réseau non compatible RSS accepte le trafic web sur un serveur qui a une ou plusieurs cartes réseau compatible RSS. Dans ce cas, vous devez utiliser des cartes réseau compatible RSS ou désactiver RSS sur les propriétés de la carte réseau **propriétés avancées** onglet. Pour déterminer si une carte réseau est compatible RSS, vous pouvez afficher les informations de RSS sur les propriétés de la carte réseau **propriétés avancées** onglet.

### <a name="rss-profiles-and-rss-queues"></a>Les profils RSS et files d’attente RSS

Le profil RSS prédéfinis par défaut est NUMA Static, qui modifie le comportement par défaut à partir de versions précédentes du système d’exploitation. Pour vous familiariser avec les profils RSS, vous pouvez consulter les profils disponibles pour comprendre quand ils sont bénéfiques et comment elles s’appliquent à votre environnement réseau et de matériel.

Par exemple, si vous ouvrez le Gestionnaire des tâches et passez en revue les processeurs logiques sur votre serveur, et ils semblent être sous-utilisés pour le trafic de réception, vous pouvez essayer d’augmentation du nombre de files d’attente RSS à partir de la valeur par défaut de 2 à la valeur maximale est pris en charge par votre carte réseau. Votre carte réseau peut proposer des options pour modifier le nombre de files d’attente RSS dans le cadre du pilote.

##  <a name="bkmk_resources"></a>Augmentation des ressources de la carte réseau

Pour les cartes réseau qui autorisent la configuration manuelle des ressources, telles que recevoir et envoient des mémoires tampons, vous devez augmenter les ressources allouées. 

Certaines cartes réseau définissent les tampons de réception faible pour conserver la mémoire allouée à partir de l’ordinateur hôte. La valeur faible entraîne une diminution des performances et les paquets ignorés. Par conséquent, pour les scénarios de réception intensive, nous vous recommandons d’augmenter la valeur de mémoire tampon de réception au maximum.

>[!NOTE]
>Si une carte réseau n’expose pas la configuration manuelle des ressources, qu’il soit configure dynamiquement les ressources, ou les ressources sont définies sur une valeur fixe qui ne peut pas être modifiée.

### <a name="enabling-interrupt-moderation"></a>Activation de modération

Pour contrôler la modération, certaines cartes réseau exposer les niveaux de modération de l’interruption différente, paramètres de fusion de tampons (parfois séparément pour envoyer et recevoir des mémoires tampons), ou les deux.

Vous devez considérer la modération des charges de travail intensif du processeur et réfléchir au compromis entre les ressources processeur économisées côté hôte et la latence et l’ordinateur hôte accrue des économies de processeur en raison des interruptions plus nombreuses et moins de latence. Si la carte réseau n’effectue pas la modération, mais qu’elle propose la fusion de tampons, augmentant le nombre de tampons fusionnés autorise davantage de tampons par envoi ou réception, ce qui améliore les performances.

##  <a name="bkmk_low"></a>Réglage des performances pour le traitement des paquets à faible latence

De nombreuses cartes réseaux proposent des options pour optimiser la latence induite par le système d’exploitation. La latence est le temps écoulé entre le pilote réseau de traitement d’un paquet entrant et le pilote réseau renvoi du paquet. Cette durée est généralement mesurée en microsecondes. Par comparaison, la durée de transmission de paquets sur de longues distances est généralement mesurée en millisecondes \ (une larger\ d’ordre de grandeur). Ce réglage ne réduira pas le temps d’un paquet en transit.

Voici quelques suggestions pour les réseaux sensibles à la microseconde de réglage des performances.

- Définissez le BIOS de l’ordinateur sur **hautes performances**, avec les États C désactivés. Toutefois, notez que cela est système et dépendant du BIOS, et certains systèmes seront obtenir de meilleures performances si les contrôles système d’exploitation de gestion de l’alimentation. Vous pouvez vérifier et ajuster vos paramètres de gestion de l’alimentation à partir de **paramètres** ou à l’aide de la **powercfg** commande. Pour plus d’informations, voir [Options de ligne de commande Powercfg](https://technet.microsoft.com/library/cc748940.aspx)

- Définissez le profil de gestion de l’alimentation système d’exploitation sur **système hautes performances**. Notez que cela ne fonctionnera pas correctement si le BIOS système a été défini pour désactiver le contrôle du système d’exploitation de gestion de l’alimentation.

- Activez les déchargements statiques, par exemple, une somme de contrôle UDP, une somme de contrôle TCP et envoyer déchargement important (LSO).

- Activez RSS si le trafic est multi-diffusé en continu, telles que reçoivent la multidiffusion de haut volume.

-   Désactiver le **modération** définition pour les pilotes de carte réseau qui requièrent la latence la plus faible possible. N’oubliez pas que ce paramètre peut utiliser plus de temps processeur et représente un compromis.

- Gérer des interruptions de carte réseau et les appels DPC sur un processeur cœur qui partage le cache de l’UC avec le cœur qui est utilisé par le programme (thread utilisateur) qui gère le paquet. Réglage de l’affinité du processeur peut servir à diriger un processus vers certains processeurs logiques conjointement avec la configuration de RSS pour effectuer cette opération. À l’aide de la même cœur pour le thread d’interruption, DPC et utilisateur en mode réduit les performances en tant que la charge augmente, car les ISR, DPC et thread en compétition pour l’utilisation de la base.

##  <a name="bkmk_smi"></a>Interruptions d’administration système

De nombreux systèmes matériels utilisent \(SMI\) système interrompt de gestion pour un large éventail de fonctions de maintenance, notamment le signalement des erreurs de mémoire erreur correction code \(ECC\), compatibilité USB, le contrôle du ventilateur et BIOS hérité de gestion de l’alimentation contrôlée. 

SMI est l’interruption prioritaire sur le système et place le processeur dans un mode de gestion, qui devance toutes les autres activités tant qu’elle exécute une routine de service d’interruption, généralement contenue dans le BIOS.

Malheureusement, cela peut entraîner des pointes de latence de 100 microsecondes ou plus. 

Si vous avez besoin pour une latence, vous devez demander une version du BIOS de votre fournisseur de matériel qui réduit au maximum les SMI au niveau le plus bas possible. Ils sont souvent appelés «BIOS à faible latence» ou «BIOS libre SMI». Dans certains cas, il n’est pas possible pour une plateforme matérielle complètement éliminer l’activité SMI car elle est utilisée pour contrôler des fonctions essentielles (par exemple, les ventilateurs de refroidissement).

>[!NOTE]
>Le système d’exploitation ne peut exercer aucun contrôle sur les SMI car le processeur logique s’exécute dans un mode de maintenance spécial qui empêche l’intervention du système d’exploitation.

##  <a name="bkmk_tcp"></a>TCP de réglage des performances

 Vous pouvez régler les performances de TCP à l’aide des éléments suivants.

###  <a name="bkmk_tcp_params"></a>Réglage automatique de la fenêtre de réception TCP

Avant Windows Server2008, la pile réseau utilisait une fenêtre côté réception de taille fixe (65 535octets) qui limité le débit potentiel global pour les connexions. Une des modifications plus importantes à la pile TCP est TCP réglage automatique de la fenêtre de réception. 

Vous pouvez calculer le débit total d’une connexion unique lorsque vous utilisez une fenêtre de réception TCP de taille fixe:

**Débit total réalisable en octets = TCP recevoir la taille de fenêtre en octets \ * (1 / latence de la connexion en secondes)**

Par exemple, le débit total réalisable est seulement 51Mbits/s sur une connexion avec une latence de 10ms \ (une valeur raisonnable pour un réseau de grande entreprise d’infrastructure\). 

Toutefois, avec réglage automatique, la fenêtre côté réception est ajustable et peut s’agrandir pour répondre aux besoins de l’expéditeur. Il est possible pour une connexion d’atteindre un débit de ligne total d’une connexion de 1Gbit/s. Scénarios d’utilisation réseau qui ont peuvent être limités par le passé par le débit total réalisable de connexions TCP peuvent désormais entièrement à utiliser le réseau.

#### <a name="deprecated-tcp-parameters"></a>Paramètres TCP déconseillées

Les paramètres de Registre suivants à partir de Windows Server 2003 ne sont plus pris en charge et sont ignorés dans les versions ultérieures.

Tous ces paramètres avaient l’emplacement de Registre suivant:

    ```  
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Tcpip\Parameters  
    ```  

- TcpWindowSize

- NumTcbTablePartitions  

- MaxHashTableSize  



###  <a name="bkmk_wfp"></a>Plateforme de filtrage Windows

Le filtrage de plateforme Windows (WFP) introduite dans Windows Vista et Windows Server 2008 fournit des API aux éditeurs de logiciels non Microsoft (ISV) pour créer des filtres de traitement de paquets. Exemples pare-feu et antivirus.

>[!NOTE]
>Un filtre WFP mal écrit peut réduire considérablement les performances de mise en réseau d’un serveur. Pour plus d’informations, voir [traitement de paquets de portage des pilotes et des applications pour WFP](https://msdn.microsoft.com/windows/hardware/gg463267.aspx) dans le centre de développement Windows.


Pour obtenir des liens vers toutes les rubriques de ce guide, voir [réglage des performances réseau sous-système](net-sub-performance-top.md).
