---
title: Réglage des performances des cartes réseau
description: Cette rubrique fait partie du Guide d’optimisation des performances du sous-système réseau pour Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 0b9b0f80-415c-4f5e-8377-c09b51d9c5dd
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e9c0ba73a1df874edc63001812d059a9e70f187f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405525"
---
# <a name="performance-tuning-network-adapters"></a>Réglage des performances des cartes réseau

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour régler les performances des cartes réseau installées sur les ordinateurs qui exécutent Windows Server 2016.

Les variables suivantes déterminent les paramètres de réglage adéquats de votre carte réseau :

- La carte réseau et son jeu de fonctionnalités  

- Le type de charge de travail réalisée par le serveur  

- Les ressources matérielles et logicielles du serveur  

- Vos objectifs de performances pour le serveur  

Si votre carte réseau propose des options de réglage, vous pouvez optimiser le débit du réseau et l'utilisation des ressources pour atteindre un débit optimal sur la base des paramètres décrits plus haut.  

Les sections qui suivent décrivent certaines options de réglage des performances.  

##  <a name="bkmk_offload"></a>Activation des fonctionnalités de déchargement

L'activation des fonctionnalités de déchargement de la carte réseau est souvent bénéfique. Toutefois, la carte réseau n'est pas toujours suffisamment puissante pour gérer les capacités de déchargement lorsque le débit est élevé.

>[!IMPORTANT]
>N’utilisez pas les fonctionnalités de déchargement déchargement de **tâche IPSec** ou **déchargement TCP Chimney**. Ces technologies sont déconseillées dans Windows Server 2016 et peuvent nuire aux performances du serveur et de la mise en réseau. En outre, ces technologies peuvent ne pas être prises en charge par Microsoft à l’avenir.

Par exemple, l'activation du déchargement de segmentation peut réduire le débit maximal sur certaines cartes réseau en raison de ressources matérielles limitées. Toutefois, si le débit réduit ne constitue pas une limitation, vous devez activer les capacités de déchargement même pour ce type de carte réseau.

>[!NOTE]
> Certaines cartes réseau exigent que les fonctionnalités de déchargement soient activées indépendamment pour l'envoi et la réception.

##  <a name="bkmk_rss_web"></a>Activation de la mise à l’échelle côté réception (RSS) pour les serveurs Web

RSS peut améliorer l'extensibilité web et les performances lorsque le serveur comprend moins de cartes réseau que de processeurs logiques. Lorsque l'ensemble du trafic web passe par les cartes réseau prenant en charge RSS, les demandes web entrantes provenant de connexions différentes peuvent être traitées simultanément dans différents processeurs.

Il est important de noter qu’en raison de la logique de RSS et du protocole \(http\) pour la distribution de la charge, les performances risquent d’être gravement détériorées si une carte réseau non conforme à RSS accepte le trafic Web sur un serveur doté d’un ou de cartes réseau plus RSS. Dans cette situation, vous devez utiliser les cartes prenant en charge RSS ou désactiver RSS sous l'onglet **Propriétés avancées** dans les propriétés de la carte réseau. Pour déterminer si une carte réseau prend en charge RSS, vous pouvez consulter l'onglet **Propriétés avancées** dans les propriétés de la carte réseau.

### <a name="rss-profiles-and-rss-queues"></a>Profils RSS et Files d'attente RSS

Le profil prédéfini RSS par défaut est NUMA static, ce qui modifie le comportement par défaut des versions précédentes du système d’exploitation. Pour vous aider à démarrer avec les profils RSS, passez en revue les profils existants pour comprendre quand ils sont bénéfiques et comment ils s'appliquent à votre environnement et matériel réseau.

Par exemple, si en ouvrant le Gestionnaire des tâches et en passant en revue les processeurs logiques de votre serveur, il vous semble que ces derniers sont sous-utilisés pour le trafic de réception, vous pouvez essayer de passer le nombre de files d'attente de 2 (qui est la valeur par défaut) au maximum pris en charge par votre carte réseau. Votre carte réseau peut proposer des options permettant de changer le nombre de files d'attente RSS dans le pilote.

##  <a name="bkmk_resources"></a>Amélioration des ressources de la carte réseau

Pour les cartes réseau qui autorisent la configuration manuelle des ressources, comme les tampons de réception et d'envoi, vous devez augmenter les ressources allouées. 

Certaines cartes réseau définissent des tampons de réception faibles afin de conserver la mémoire allouée depuis l'hôte. Cela conduit à des paquets ignorés et des performances réduites. Par conséquent, dans les scénarios de réception intensive, nous vous recommandons d'augmenter la valeur du tampon de réception à son maximum.

>[!NOTE]
>Si une carte réseau ne propose pas la configuration manuelle des ressources, soit elle configure les ressources de manière dynamique, soit les ressources sont définies sur une valeur fixe qui ne peut pas être modifiée.

### <a name="enabling-interrupt-moderation"></a>Activation de Modération de l’interruption

Pour contrôler la modération de l'interruption, certaines cartes réseau proposent différents niveaux de modération de l'interruption, des paramètres de fusion de tampons (parfois séparément pour les tampons d'envoi et de réception), ou les deux.

Vous devez considérer la modération de l'interruption pour les charges de travail liées au processeur et réfléchir au compromis entre les ressources processeur économisées côté hôte et la latence, versus les ressources processeur supplémentaires économisées côté hôte en raison des interruptions plus nombreuses et de la latence plus faible. Si la carte réseau ne réalise pas la modération de l'interruption, mais qu'elle propose la fusion de tampons, l'augmentation du nombre de tampons fusionnés autorise davantage de tampons par envoi ou réception, ce qui améliore les performances.

##  <a name="bkmk_low"></a>Réglage des performances pour le traitement des paquets à faible latence

De nombreuses cartes réseaux proposent des options permettant d'optimiser la latence induite par le système d'exploitation. La latence est la durée qui sépare le traitement d'un paquet entrant par le pilote réseau et le renvoi du paquet par ce pilote. Cette durée est généralement mesurée en microsecondes. Pour comparaison, le temps de transmission pour les transmissions de paquets sur de longues distances est généralement \(mesuré en millisecondes, avec\)un ordre de grandeur magnitude. Ce réglage ne réduira pas la durée de transit d'un paquet.

Voici quelques suggestions de réglage des performances pour les réseaux sensibles à la microseconde.

- Définissez le BIOS de l'ordinateur sur **Performances élevées** et désactivez les états C. Notez toutefois que cela dépend du système et du BIOS et que les performances de certains systèmes seront meilleures si le système d'exploitation contrôle la gestion de l'alimentation. Vous pouvez vérifier et ajuster vos paramètres de gestion de l’alimentation à partir de **paramètres** ou à l’aide de la commande **powercfg** . Pour plus d’informations, consultez [options de ligne de commande powercfg](https://technet.microsoft.com/library/cc748940.aspx) .

- Définissez le profil de gestion de l'alimentation du système d'exploitation sur **Système hautes performances**. Notez que cela ne fonctionnera pas correctement si le BIOS système a été défini pour désactiver le contrôle de la gestion de l'alimentation par le système d'exploitation.

- Activez les déchargements statiques, par exemple, Déchargement de la somme de contrôle UDP, Déchargement de la somme de contrôle TCP et Déchargement d’envoi important (LSO).

- Activez RSS si le trafic est multi-flux, comme la réception par diffusion de volumes élevés.

-   Désactivez le paramètre **Modération de l'interruption** pour les pilotes de carte réseau qui requièrent la latence la plus faible possible. Rappelez-vous que ce paramètre peut utiliser plus de temps processeur et qu'il représente un compromis.

- Gérez les interruptions de carte réseau et les appels de procédure différés (DPC) sur un processeur cœur qui partage le cache d'UC avec le cœur qui est utilisé par le programme (thread utilisateur) qui traite le paquet. Le réglage de l'affinité de l'UC peut être utilisé pour diriger un processus vers certains processeurs logiques, conjointement avec la configuration RSS. L'utilisation du même cœur pour le thread d'interruption, le thread DPC et le thread de mode utilisateur réduit les performances à mesure que la charge augmente du fait de la compétition entre ISR, DPC et le thread pour utiliser le cœur.

##  <a name="bkmk_smi"></a>Interruptions de gestion du système

De nombreux systèmes matériels utilisent le système de \(gestion\) des interruptions SMI pour diverses fonctions de maintenance, notamment la création \(de\) rapports sur le code de correction des erreurs erreurs de mémoire ECC, compatibilité USB héritée, ventilateur gestion de l’alimentation contrôlée par le BIOS et le contrôle. 

SMI est l'interruption prioritaire du système et place le processeur en mode gestion, qui devance toutes les autres activités tant qu'elle exécute une routine de service d'interruption, généralement contenue dans le BIOS.

Malheureusement, cela peut conduire à des pointes de latence de 100 microsecondes ou davantage. 

Si vous avez besoin d'une latence très faible, demandez à votre fournisseur de matériel une version du BIOS qui réduit au maximum les SMI. On appelle généralement ces BIOS « BIOS à faible latence » ou « BIO sans SMI » Dans certains cas, une plateforme matérielle ne peut pas complètement éliminer l'activité SMI car elle est utilisée pour contrôler des fonctions essentielles (ventilateurs de refroidissement par exemple).

>[!NOTE]
>Le système d'exploitation ne peut exercer aucun contrôle sur les SMI car le processeur logique s'exécute dans un mode de maintenance spécial qui empêche l'intervention du système d'exploitation.

##  <a name="bkmk_tcp"></a>Réglage des performances TCP

 Vous pouvez régler les performances de TCP à l'aide des éléments suivants.

###  <a name="bkmk_tcp_params"></a>Réglage automatique de la fenêtre de réception TCP

Avant Windows Server 2008, la pile réseau utilisait une fenêtre côté de réception à taille fixe (65 535 octets) qui limitait le débit potentiel global pour les connexions. L'une des modifications les plus importantes apportées à la pile TCP est le réglage automatique de la fenêtre de réception TCP. 

Vous pouvez calculer le débit total d’une connexion unique lorsque vous utilisez une fenêtre de réception TCP de taille fixe comme :

**Débit total réalisable en octets = taille de la fenêtre de \* réception TCP en octets (1/latence de connexion en secondes)**

Par exemple, le débit total réalisable est seulement de 51 Mbits/s sur une connexion avec \(une latence de 10 ms, ce qui représente\)une valeur raisonnable pour une grande infrastructure réseau d’entreprise. 

Toutefois, avec le réglage automatique, la fenêtre côté réception est ajustable et peut s'agrandir pour répondre aux demandes de l'expéditeur. Une connexion peut atteindre un débit total d’une connexion de 1 Gbit/s. Les scénarios d'utilisation du réseau qui ont pu être limités par le passé par le débit total réalisable sur les connexions TCP peuvent maintenant faire plein usage du réseau.

#### <a name="deprecated-tcp-parameters"></a>Paramètres TCP déconseillés

Les paramètres de registre suivants de Windows Server 2003 ne sont plus pris en charge et sont ignorés dans les versions ultérieures.

Tous ces paramètres ont l’emplacement de Registre suivant :

    ```  
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Tcpip\Parameters  
    ```  

- TcpWindowSize

- NumTcbTablePartitions  

- MaxHashTableSize  



###  <a name="bkmk_wfp"></a>Plateforme de filtrage Windows

La plateforme de filtrage Windows (WFP) introduite dans Windows Vista et Windows Server 2008 fournit des API aux éditeurs de logiciels indépendants (ISV) non-Microsoft pour créer des filtres de traitement de paquets. Les logiciels pare-feu et antivirus en sont des exemples.

>[!NOTE]
>Un filtre WFP mal écrit peut réduire considérablement les performances réseau d’un serveur. Pour plus d’informations, consultez [Portage de pilotes et d’applications de traitement de paquets vers WFP](https://msdn.microsoft.com/windows/hardware/gg463267.aspx) dans le centre de développement Windows.


Pour obtenir des liens vers toutes les rubriques de ce guide, consultez [réglage des performances du sous-système réseau](net-sub-performance-top.md).
