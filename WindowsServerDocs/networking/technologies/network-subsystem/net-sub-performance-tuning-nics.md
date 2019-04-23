---
title: Réglage des performances des cartes réseau
description: Cette rubrique fait partie du guide de réglage de performances du sous-système de réseau pour Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0b9b0f80-415c-4f5e-8377-c09b51d9c5dd
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d6d54f33108d1cdb936b02fc556acca1e5518b9b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861530"
---
# <a name="performance-tuning-network-adapters"></a>Réglage des performances des cartes réseau

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour les cartes de réseau de régler de performances qui sont installés sur les ordinateurs qui exécutent Windows Server 2016.

Les variables suivantes déterminent les paramètres de réglage adéquats de votre carte réseau :

- La carte réseau et son jeu de fonctionnalités  

- Le type de charge de travail réalisée par le serveur  

- Les ressources matérielles et logicielles du serveur  

- Vos objectifs de performances pour le serveur  

Si votre carte réseau propose des options de réglage, vous pouvez optimiser le débit du réseau et l'utilisation des ressources pour atteindre un débit optimal sur la base des paramètres décrits plus haut.  

Les sections qui suivent décrivent certaines options de réglage des performances.  

##  <a name="bkmk_offload"></a> L’activation des fonctionnalités de déchargement

L'activation des fonctionnalités de déchargement de la carte réseau est souvent bénéfique. Toutefois, la carte réseau n'est pas toujours suffisamment puissante pour gérer les capacités de déchargement lorsque le débit est élevé.

>[!IMPORTANT]
>N’utilisez pas les fonctionnalités de déchargement **déchargement de tâche IPsec** ou **le déchargement TCP Chimney**. Ces technologies sont déconseillés dans Windows Server 2016 et peuvent nuire aux serveur et les performances du réseau. En outre, ces technologies ne peuvent pas être pris en charge par Microsoft dans le futur.

Par exemple, l'activation du déchargement de segmentation peut réduire le débit maximal sur certaines cartes réseau en raison de ressources matérielles limitées. Toutefois, si le débit réduit ne constitue pas une limitation, vous devez activer les capacités de déchargement même pour ce type de carte réseau.

>[!NOTE]
> Certaines cartes réseau exigent que les fonctionnalités de déchargement soient activées indépendamment pour l'envoi et la réception.

##  <a name="bkmk_rss_web"></a> L’activation de recevoir le trafic entrant (RSS) pour les serveurs Web

RSS peut améliorer l'extensibilité web et les performances lorsque le serveur comprend moins de cartes réseau que de processeurs logiques. Lorsque l'ensemble du trafic web passe par les cartes réseau prenant en charge RSS, les demandes web entrantes provenant de connexions différentes peuvent être traitées simultanément dans différents processeurs.

Il est important de noter que, en raison de la logique de RSS et Hypertext Transfer Protocol \(HTTP\) pour la distribution de charge, performances peuvent être fortement dégradées si une carte réseau non-prenant en charge RSS accepte le trafic web sur un serveur qui possède un ou plus de cartes réseau prenant en charge RSS. Dans cette situation, vous devez utiliser les cartes prenant en charge RSS ou désactiver RSS sous l'onglet **Propriétés avancées** dans les propriétés de la carte réseau. Pour déterminer si une carte réseau prend en charge RSS, vous pouvez consulter l'onglet **Propriétés avancées** dans les propriétés de la carte réseau.

### <a name="rss-profiles-and-rss-queues"></a>Profils RSS et Files d'attente RSS

Le profil RSS prédéfinis par défaut est NUMA Static, ce qui modifie le comportement par défaut des versions précédentes du système d’exploitation. Pour vous aider à démarrer avec les profils RSS, passez en revue les profils existants pour comprendre quand ils sont bénéfiques et comment ils s'appliquent à votre environnement et matériel réseau.

Par exemple, si en ouvrant le Gestionnaire des tâches et en passant en revue les processeurs logiques de votre serveur, il vous semble que ces derniers sont sous-utilisés pour le trafic de réception, vous pouvez essayer de passer le nombre de files d'attente de 2 (qui est la valeur par défaut) au maximum pris en charge par votre carte réseau. Votre carte réseau peut proposer des options permettant de changer le nombre de files d'attente RSS dans le pilote.

##  <a name="bkmk_resources"></a> Augmentation des ressources de la carte réseau

Pour les cartes réseau qui autorisent la configuration manuelle des ressources, comme les tampons de réception et d'envoi, vous devez augmenter les ressources allouées. 

Certaines cartes réseau définissent des tampons de réception faibles afin de conserver la mémoire allouée depuis l'hôte. Cela conduit à des paquets ignorés et des performances réduites. Par conséquent, dans les scénarios de réception intensive, nous vous recommandons d'augmenter la valeur du tampon de réception à son maximum.

>[!NOTE]
>Si une carte réseau ne propose pas la configuration manuelle des ressources, soit elle configure les ressources de manière dynamique, soit les ressources sont définies sur une valeur fixe qui ne peut pas être modifiée.

### <a name="enabling-interrupt-moderation"></a>Activation de Modération de l’interruption

Pour contrôler la modération de l'interruption, certaines cartes réseau proposent différents niveaux de modération de l'interruption, des paramètres de fusion de tampons (parfois séparément pour les tampons d'envoi et de réception), ou les deux.

Vous devez considérer la modération de l'interruption pour les charges de travail liées au processeur et réfléchir au compromis entre les ressources processeur économisées côté hôte et la latence, versus les ressources processeur supplémentaires économisées côté hôte en raison des interruptions plus nombreuses et de la latence plus faible. Si la carte réseau ne réalise pas la modération de l'interruption, mais qu'elle propose la fusion de tampons, l'augmentation du nombre de tampons fusionnés autorise davantage de tampons par envoi ou réception, ce qui améliore les performances.

##  <a name="bkmk_low"></a> Réglage des performances pour le traitement des paquets à faible latence

De nombreuses cartes réseaux proposent des options permettant d'optimiser la latence induite par le système d'exploitation. La latence est la durée qui sépare le traitement d'un paquet entrant par le pilote réseau et le renvoi du paquet par ce pilote. Cette durée est généralement mesurée en microsecondes. Pour la comparaison, la durée de transmission de paquets sur de longues distances est généralement mesurée en millisecondes \(d’un ordre de grandeur supérieur\). Ce réglage ne réduira pas la durée de transit d'un paquet.

Voici quelques suggestions de réglage des performances pour les réseaux sensibles à la microseconde.

- Définissez le BIOS de l'ordinateur sur **Performances élevées** et désactivez les états C. Notez toutefois que cela dépend du système et du BIOS et que les performances de certains systèmes seront meilleures si le système d'exploitation contrôle la gestion de l'alimentation. Vous pouvez vérifier et ajuster vos paramètres de gestion d’alimentation à partir de **paramètres** ou à l’aide de la **powercfg** commande. Pour plus d’informations, consultez [Options de ligne de commande Powercfg](https://technet.microsoft.com/library/cc748940.aspx)

- Définissez le profil de gestion de l'alimentation du système d'exploitation sur **Système hautes performances**. Notez que cela ne fonctionnera pas correctement si le BIOS système a été défini pour désactiver le contrôle de la gestion de l'alimentation par le système d'exploitation.

- Activez les déchargements statiques, par exemple, Déchargement de la somme de contrôle UDP, Déchargement de la somme de contrôle TCP et Déchargement d’envoi important (LSO).

- Activez RSS si le trafic est multi-flux, comme la réception par diffusion de volumes élevés.

-   Désactivez le paramètre **Modération de l'interruption** pour les pilotes de carte réseau qui requièrent la latence la plus faible possible. Rappelez-vous que ce paramètre peut utiliser plus de temps processeur et qu'il représente un compromis.

- Gérez les interruptions de carte réseau et les appels de procédure différés (DPC) sur un processeur cœur qui partage le cache d'UC avec le cœur qui est utilisé par le programme (thread utilisateur) qui traite le paquet. Le réglage de l'affinité de l'UC peut être utilisé pour diriger un processus vers certains processeurs logiques, conjointement avec la configuration RSS. L'utilisation du même cœur pour le thread d'interruption, le thread DPC et le thread de mode utilisateur réduit les performances à mesure que la charge augmente du fait de la compétition entre ISR, DPC et le thread pour utiliser le cœur.

##  <a name="bkmk_smi"></a> Interruptions d’administration système

De nombreux systèmes matériels utilisent interruptions d’administration système \(SMI\) pour une variété de fonctions de maintenance, y compris la création de rapports de code de correction d’erreur \(ECC\) des erreurs de mémoire, compatibilité USB héritée, ventilateur contrôle et BIOS contrôlé gestion de l’alimentation. 

SMI est l'interruption prioritaire du système et place le processeur en mode gestion, qui devance toutes les autres activités tant qu'elle exécute une routine de service d'interruption, généralement contenue dans le BIOS.

Malheureusement, cela peut conduire à des pointes de latence de 100 microsecondes ou davantage. 

Si vous avez besoin d'une latence très faible, demandez à votre fournisseur de matériel une version du BIOS qui réduit au maximum les SMI. On appelle généralement ces BIOS « BIOS à faible latence » ou « BIO sans SMI » Dans certains cas, une plateforme matérielle ne peut pas complètement éliminer l'activité SMI car elle est utilisée pour contrôler des fonctions essentielles (ventilateurs de refroidissement par exemple).

>[!NOTE]
>Le système d'exploitation ne peut exercer aucun contrôle sur les SMI car le processeur logique s'exécute dans un mode de maintenance spécial qui empêche l'intervention du système d'exploitation.

##  <a name="bkmk_tcp"></a> TCP de réglage des performances

 Vous pouvez régler les performances de TCP à l'aide des éléments suivants.

###  <a name="bkmk_tcp_params"></a>  Réglage automatique de la fenêtre de réception TCP

Avant Windows Server 2008, la pile réseau utilisait une fenêtre côté réception de taille fixe (65 535 octets) qui limitait le débit potentiel global des connexions. L'une des modifications les plus importantes apportées à la pile TCP est le réglage automatique de la fenêtre de réception TCP. 

Vous pouvez calculer le débit total d’une connexion unique lorsque vous utilisez une fenêtre de réception TCP de taille fixe :

**Débit total réalisable en octets = TCP taille de la fenêtre de réception en octets \* (1 / latence de connexion en secondes)**

Par exemple, le débit total réalisable est seulement 51 Mbits/s sur une connexion avec une latence de 10 ms \(une valeur raisonnable pour une infrastructure de réseau de grande entreprise\). 

Toutefois, avec le réglage automatique, la fenêtre côté réception est ajustable et peut s'agrandir pour répondre aux demandes de l'expéditeur. Il est possible pour une connexion à atteindre un taux de ligne complète d’une connexion de 1 Gbit/s. Les scénarios d'utilisation du réseau qui ont pu être limités par le passé par le débit total réalisable sur les connexions TCP peuvent maintenant faire plein usage du réseau.

#### <a name="deprecated-tcp-parameters"></a>Paramètres TCP déconseillées

Les paramètres de Registre suivants à partir de Windows Server 2003 ne sont plus prises en charge et sont ignorés dans les versions ultérieures.

Tous ces paramètres avaient l’emplacement de Registre suivant :

    ```  
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Tcpip\Parameters  
    ```  

- TcpWindowSize

- NumTcbTablePartitions  

- MaxHashTableSize  



###  <a name="bkmk_wfp"></a> Plateforme de filtrage Windows

Le Windows WFP (Filtering Platform) qui a été introduite dans Windows Vista et Windows Server 2008 fournit des API aux éditeurs de logiciels indépendants non-Microsoft (ISV) pour créer des filtres de traitement de paquet. Les logiciels pare-feu et antivirus en sont des exemples.

>[!NOTE]
>Un filtre WFP mal écrit peut réduire significativement les performances de mise en réseau d'un serveur. Pour plus d’informations, consultez [-traitement des paquets de portage de pilotes et des applications pour WFP](https://msdn.microsoft.com/windows/hardware/gg463267.aspx) dans le centre de développement Windows.


Pour obtenir des liens vers toutes les rubriques de ce guide, consultez [réglage des performances réseau sous-système](net-sub-performance-top.md).
