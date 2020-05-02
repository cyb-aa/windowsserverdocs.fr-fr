---
ms.assetid: 350aa5a3-5938-4921-93dc-289660f26bad
title: Nouveautés du clustering de basculement dans Windows Server
ms.prod: windows-server
ms.technology: storage-failover-clustering
ms.topic: get-started-article
manager: lizross
author: JasonGerend
ms.author: jgerend
ms.date: 10/18/2018
ms.openlocfilehash: 926c9c862d77c9fe082274a44af57e3b8339a655
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720502"
---
# <a name="whats-new-in-failover-clustering"></a>Nouveautés du clustering de basculement

> S'applique à : Windows Server 2019, Windows Server 2016

Cette rubrique décrit les fonctionnalités nouvelles et modifiées du clustering de basculement pour Windows Server 2019 et Windows Server 2016.

## <a name="whats-new-in-windows-server-2019"></a>Nouveautés de Windows Server 2019

- **Jeux de clusters**

    Les ensembles de clusters vous permettent d’augmenter le nombre de serveurs dans une solution SDDC (Software-Defined Datacenter) unique au-delà des limites actuelles d’un cluster. Pour ce faire, vous devez regrouper plusieurs clusters dans un ensemble de clusters, un regroupement faiblement couplé de plusieurs clusters de basculement : calcul, stockage et hyper-convergé.
    Avec les ensembles de clusters, vous pouvez déplacer des machines virtuelles en ligne (migration dynamique) entre des clusters au sein de l’ensemble de clusters.

    Pour plus d’informations, consultez [cluster sets](../storage/storage-spaces/cluster-sets.md).

- **Clusters adaptés à Azure**
  
    Désormais, les clusters de basculement détectent automatiquement quand ils s’exécutent sur des machines virtuelles Azure IaaS et optimisent la configuration pour fournir un basculement et une journalisation proactive des événements de maintenance planifiée Azure pour atteindre les niveaux de disponibilité les plus élevés. Le déploiement est également simplifié en éliminant la nécessité de configurer l’équilibreur de charge avec un nom de réseau dynamique pour le nom du cluster.

- **Migration de cluster entre domaines**

    Les clusters de basculement peuvent désormais migrer dynamiquement d’un domaine de Active Directory à un autre, ce qui simplifie la consolidation de domaine et permet de créer des clusters par des partenaires matériels et de les joindre ultérieurement au domaine du client.

- **Témoin USB**

    Vous pouvez maintenant utiliser un lecteur USB connecté à un commutateur réseau comme témoin pour déterminer le quorum d’un cluster. Cela étend le témoin de partage de fichiers pour prendre en charge tout appareil conforme à SMB2.

- **Améliorations de l’infrastructure de cluster**

    Le cache du volume partagé de cluster est désormais activé par défaut pour renforcer les performances des machines virtuelles. MSDTC prend désormais en charge les volumes partagés de cluster pour permettre le déploiement de charges de travail MSDTC sur espaces de stockage direct comme avec SQL Server. Logique améliorée pour détecter les nœuds partitionnés avec réparation spontanée afin de renvoyer des nœuds à l’appartenance au cluster. Détection et réparation automatique des itinéraires réseau de cluster améliorés.

- **Mise à jour adaptée aux clusters qui prend en charge les espaces de stockage direct**

    La mise à jour adaptée aux clusters est désormais intégrée et consciente des espaces de stockage direct, de la validation et de la vérification de la resynchronisation des données sur chaque nœud. La mise à jour adaptée aux clusters inspecte les mises à jour pour qu’elles redémarrent intelligemment uniquement si nécessaire. Cela permet d’orchestrer les redémarrages de tous les serveurs du cluster en vue d’une maintenance planifiée.

- **Améliorations du témoin de partage de fichiers**

    Nous avons activé l’utilisation d’un témoin de partage de fichiers dans les scénarios suivants :
  - Accès Internet absent ou très médiocre en raison d’un emplacement distant empêchant l’utilisation d’un témoin Cloud.
  - Absence de lecteurs partagés pour un témoin de disque. Il peut s’agir d’un espaces de stockage direct configuration hypervergé, d’un SQL Server Always On groupes de disponibilité (AG) ou d’un groupe de disponibilité de base de données Exchange (DAG), qui n’utilisent pas de disques partagés.
  - Absence de connexion du contrôleur de domaine, car le cluster se trouve derrière une zone DMZ.
  - Un groupe de travail ou un cluster inter-domaines pour lequel il n’existe aucun objet de nom de cluster Active Directory (CNO). Pour plus d’informations sur ces améliorations, consultez le billet de blog de gestion de & de serveur suivant : témoin de partage de fichiers de cluster de basculement et DFS.

    Nous allons maintenant également bloquer explicitement l’utilisation d’un partage d’espaces de noms DFS comme emplacement. L’ajout d’un témoin de partage de fichiers à un partage DFS peut entraîner des problèmes de stabilité pour votre cluster, et cette configuration n’a jamais été prise en charge. Nous avons ajouté une logique permettant de détecter si un partage utilise des espaces de noms DFS et, si des espaces de noms DFS sont détectés, Gestionnaire du cluster de basculement bloque la création du témoin et affiche un message d’erreur indiquant qu’il n’est pas pris en charge.

- **Renforcement de cluster**

    La communication intra-cluster par le biais du protocole SMB (Server Message Block) pour les volumes partagés de cluster et espaces de stockage direct utilise désormais des certificats pour offrir la plateforme la plus sécurisée. Cela permet aux clusters de basculement de fonctionner sans dépendances sur NTLM et d’activer les lignes de base de sécurité.

- **Le cluster de basculement n’utilise plus l’authentification NTLM**

    Les clusters de basculement n’utilisent plus l’authentification NTLM. Au lieu de cela, Kerberos et l’authentification basée sur les certificats sont utilisés exclusivement. Aucune modification n’est requise par l’utilisateur ou les outils de déploiement pour tirer parti de cette amélioration de la sécurité. Il permet également de déployer des clusters de basculement dans des environnements où NTLM a été désactivé.

## <a name="whats-new-in-windows-server-2016"></a>Nouveautés de Windows Server 2016

### <a name="cluster-operating-system-rolling-upgrade"></a><a name="BKMK_RollingUpgrade"></a>Mise à niveau propagée de système d’exploitation de cluster

La mise à niveau propagée du système d’exploitation de cluster permet à un administrateur de mettre à niveau le système d’exploitation des nœuds de cluster de Windows Server 2012 R2 vers une version plus récente sans arrêter les charges de travail Hyper-V ou Serveur de fichiers avec montée en puissance parallèle. Avec cette fonctionnalité, les pénalités de temps d’arrêt sur les contrats de niveau de service peuvent être évitées.

**Quels avantages cette modification procure-t-elle ?**  

La mise à niveau d’un cluster Hyper-V ou Serveur de fichiers avec montée en puissance parallèle de Windows Server 2012 R2 vers Windows Server 2016 ne nécessite plus de temps d’arrêt. Le cluster continue à fonctionner à un niveau Windows Server 2012 R2, jusqu’à ce que tous les nœuds du cluster exécutent Windows Server 2016. Le niveau fonctionnel de cluster est mis à niveau vers Windows Server 2016 à l’aide de l' `Update-ClusterFunctionalLevel`applet Windows PowerShell.

> [!WARNING]  
> - Après avoir mis à jour le niveau fonctionnel du cluster, vous ne pouvez pas revenir au niveau fonctionnel d’un cluster Windows Server 2012 R2.
>
> - Tant que `Update-ClusterFunctionalLevel` l’applet de commande n’est pas exécutée, le processus est réversible et les nœuds windows server 2012 R2 peuvent être ajoutés et les nœuds windows server 2016 peuvent être supprimés.

**En quoi le fonctionnement est-il différent ?**  

Un cluster de basculement Hyper-V ou Serveur de fichiers avec montée en puissance parallèle peut désormais être facilement mis à niveau sans temps d’arrêt ou nécessiter la création d’un nouveau cluster avec des nœuds qui exécutent le système d’exploitation Windows Server 2016. La migration de clusters vers Windows Server 2012 R2 impliquait la mise hors connexion du cluster existant et la réinstallation du nouveau système d’exploitation pour chaque nœud, puis la remise en ligne du cluster. L’ancien processus était fastidieux et nécessitait un temps d’arrêt. Toutefois, dans Windows Server 2016, le cluster n’a pas besoin de passer en mode hors connexion à tout moment.

Les systèmes d’exploitation de cluster pour la mise à niveau en plusieurs phases sont les suivants pour chaque nœud dans un cluster :  
-   Le nœud est suspendu et vidé de tous les ordinateurs virtuels qui s’exécutent sur celui-ci. 
-   Les machines virtuelles (ou une autre charge de travail de cluster) sont migrées vers un autre nœud du cluster. 
-   Le système d’exploitation existant est supprimé et une nouvelle installation du système d’exploitation Windows Server 2016 sur le nœud est effectuée. 
-   Le nœud exécutant le système d’exploitation Windows Server 2016 est rajouté au cluster. 
-   À ce stade, le cluster est considéré comme s’exécutant en mode mixte, car les nœuds de cluster exécutent Windows Server 2012 R2 ou Windows Server 2016. 
-   Le niveau fonctionnel du cluster reste sur Windows Server 2012 R2. À ce niveau fonctionnel, les nouvelles fonctionnalités de Windows Server 2016 qui affectent la compatibilité avec les versions précédentes du système d’exploitation ne seront pas disponibles. 
-   Finalement, tous les nœuds sont mis à niveau vers Windows Server 2016. 
-   Le niveau fonctionnel du cluster est ensuite remplacé par Windows Server 2016 à l’aide `Update-ClusterFunctionalLevel`de l’applet de commande Windows PowerShell. À ce stade, vous pouvez tirer parti des fonctionnalités de Windows Server 2016. 

Pour plus d’informations, consultez [mise à niveau propagée du système d’exploitation de cluster](cluster-operating-system-rolling-upgrade.md). 

### <a name="storage-replica"></a><a name="BKMK_SR"></a>Réplica de stockage  
Le réplica de stockage est une nouvelle fonctionnalité qui permet la réplication synchrone de niveau bloc et indépendante du stockage entre les serveurs ou les clusters pour la récupération d’urgence, ainsi que l’étirement d’un cluster de basculement entre les sites. La réplication synchrone permet la mise en miroir des données dans des sites physiques avec des volumes cohérents en cas d’incident, ce qui garantit aucune perte de données au niveau du système de fichiers. La réplication asynchrone permet l’extension de site au-delà de plages métropolitaines avec la possibilité de perte de données. 

**Quels avantages cette modification procure-t-elle ?**  

Le réplica de stockage vous permet d’effectuer les opérations suivantes :  

-   Proposer une seule solution de reprise d’activité fournisseur pour les interruptions planifiées et non planifiées des charges de travail critiques. 

-   Utiliser le transport SMB3 avec des performances, une scalabilité et une fiabilité reconnues. 

-   Étendre les clusters de basculement Windows aux distances métropolitaines. 

-   Utilisez les logiciels Microsoft de bout en bout pour le stockage et le clustering, comme Hyper-V, le réplica de stockage, les espaces de stockage, le cluster, Serveur de fichiers avec montée en puissance parallèle, SMB3, la déduplication des données et ReFS/NTFS. 

-   Réduire les coûts et la complexité comme suit :  

    -   La réplication est indépendante du matériel et ne nécessite pas de configuration de stockage particulière, comme DAS ou SAN. 

    -   Elle autorise des technologies de mise en réseau et de stockage des produits. 

    -   Elle facilite la gestion graphique pour les nœuds individuels et les clusters via le Gestionnaire du cluster de basculement. 

    -   Elle inclut des options de script complètes et à grande échelle via Windows PowerShell. 

-   Réduire les temps d’arrêt et augmenter la fiabilité et la productivité intrinsèques à Windows. 

-   Fournir la capacité de prise en charge, les mesures de performances et les fonctionnalités de diagnostic. 

Pour plus d’informations, voir [Réplica de stockage dans Windows Server 2016](../storage/storage-replica/storage-replica-overview.md). 

### <a name="cloud-witness"></a><a name="BKMK_CloudWitness"></a>Témoin cloud

Dans Windows Server 2016, Témoin cloud est un nouveau type de témoin de quorum de cluster avec basculement, qui utilise Microsoft Azure comme point d’arbitrage. Comme les autres témoins de quorum, Témoin cloud obtient un vote et peut prendre part à des calculs de quorum. Vous pouvez le configurer comme témoin de quorum à l’aide de l’Assistant Configuration de quorum du cluster. 

**Quels avantages cette modification procure-t-elle ?**  

L’utilisation du témoin Cloud comme témoin de quorum de cluster de basculement offre les avantages suivants :  

-   Tire parti de Microsoft Azure et élimine la nécessité de disposer d’un troisième centre de centres distinct. 

-   Utilise la mise à la disposition publique standard Microsoft Azure le stockage d’objets BLOB, ce qui élimine la surcharge de maintenance supplémentaire des machines virtuelles hébergées dans un cloud public. 

-   Le même compte de Stockage Microsoft Azure peut être utilisé pour plusieurs clusters (un fichier d’objets BLOB par cluster ; ID unique du cluster utilisé comme nom de fichier BLOB). 

-   Offre un coût très faible pour le compte de stockage (données très petites écrites par fichier BLOB, fichier BLOB mis à jour une seule fois lorsque l’état des nœuds du cluster change). 

Pour plus d’informations, consultez [déployer un témoin de Cloud pour un cluster de basculement](deploy-cloud-witness.md). 

**En quoi le fonctionnement est-il différent ?**  

Cette fonctionnalité est une nouveauté de Windows Server 2016. 

### <a name="virtual-machine-resiliency"></a><a name="BKMK_VMs"></a>Résilience de la machine virtuelle

**Résilience de calcul** Windows Server 2016 comprend une plus grande résilience de calcul des machines virtuelles afin de réduire les problèmes de communication intra-cluster dans votre cluster de calcul, comme suit : 

-   **Options de résilience disponibles pour les machines virtuelles :**  Vous pouvez maintenant configurer des options de résilience de machine virtuelle qui définissent le comportement des machines virtuelles pendant les défaillances temporaires :  

    -   **Niveau de résilience :** Vous aide à définir la manière dont les échecs temporaires sont gérés. 

    -   **Période de résilience :**  Permet de définir la durée pendant laquelle toutes les machines virtuelles sont autorisées à s’exécuter isolées. 

-   **Mise en quarantaine des nœuds défectueux :** Les nœuds défectueux sont mis en quarantaine et ne sont plus autorisés à joindre le cluster. Cela empêche les nœuds battants d’avoir un effet négatif sur les autres nœuds et le cluster global. 

Pour plus d’informations sur les paramètres de mise en quarantaine du workflow et de la résilience des ordinateurs virtuels qui contrôlent la façon dont votre nœud est placé en isolation ou en quarantaine, consultez [résilience de calcul des machines virtuelles dans Windows Server 2016](https://blogs.msdn.com/b/clustering/archive/2015/06/03/10619308.aspx). 

**Résilience du stockage** Dans Windows Server 2016, les machines virtuelles sont plus résistantes aux échecs de stockage temporaire. La résilience améliorée des machines virtuelles permet de conserver les États de session des machines virtuelles locataires en cas d’interruption du stockage. Cela est possible grâce à une réponse de l’ordinateur virtuel intelligent et rapide aux problèmes liés à l’infrastructure de stockage. 

Lorsqu’un ordinateur virtuel se déconnecte de son stockage sous-jacent, il s’interrompt et attend le stockage pour le récupérer. Pendant la suspension, la machine virtuelle conserve le contexte des applications qui s’y exécutent. Lorsque la connexion de l’ordinateur virtuel à son stockage est restaurée, la machine virtuelle revient à son état d’exécution. Par conséquent, l’état de session de l’ordinateur locataire est conservé lors de la récupération. 

Dans Windows Server 2016, la résilience du stockage d’ordinateur virtuel est également prise en charge et optimisée pour les clusters invités. 

### <a name="diagnostic-improvements-in-failover-clustering"></a><a name="BKMK_Diagnostics"></a>Améliorations des diagnostics dans le clustering de basculement

Pour faciliter le diagnostic des problèmes liés aux clusters de basculement, Windows Server 2016 comprend les éléments suivants :  

- Plusieurs améliorations apportées aux fichiers journaux de cluster (tels que les informations de fuseau horaire et le journal DiagnosticVerbose) facilitent la résolution des problèmes de clustering de basculement. Pour plus d’informations, consultez améliorations de la [résolution des problèmes liés au cluster de basculement Windows Server 2016-journal de cluster](https://blogs.msdn.com/b/clustering/archive/2015/05/15/10614930.aspx). 

- Nouveau type de vidage de la **mémoire active**, qui filtre la plupart des pages mémoire allouées aux ordinateurs virtuels, et rend donc la mémoire. dmp beaucoup plus petite et plus facile à enregistrer ou à copier. Pour plus d’informations, consultez améliorations de la [résolution des problèmes liés au cluster de basculement Windows Server 2016-vidage actif](https://blogs.msdn.com/b/clustering/archive/2015/05/18/10615526.aspx). 

### <a name="site-aware-failover-clusters"></a><a name="BKMK_SiteAware"></a>Clusters de basculement reconnaissant les sites

Windows Server 2016 comprend des clusters de basculement prenant en charge les sites qui permettent de regrouper des nœuds dans des clusters étendus en fonction de leur emplacement physique (site). La sensibilisation au site du cluster améliore les opérations clés pendant le cycle de vie du cluster, telles que le comportement de basculement, les stratégies de positionnement, les pulsations entre les nœuds et le comportement du quorum. Pour plus d’informations, consultez [clusters de basculement prenant en charge les sites dans Windows Server 2016](https://blogs.msdn.com/b/clustering/archive/2015/08/19/10636304.aspx). 

### <a name="workgroup-and-multi-domain-clusters"></a><a name="BKMK_multidomainclusters"></a>Clusters de groupes de travail et à domaines multiples

Dans Windows Server 2012 R2 et les versions antérieures, un cluster peut uniquement être créé entre des nœuds membres joints au même domaine. Windows Server 2016 déjoue cet obstacle en introduisant la possibilité de créer un cluster de basculement sans dépendances Active Directory. Vous pouvez maintenant créer des clusters de basculement dans les configurations suivantes :  

-   **Clusters à domaine unique.** Clusters avec tous les nœuds joints au même domaine. 

-   **Clusters à plusieurs domaines.** Clusters avec des nœuds qui sont membres de domaines différents. 

-   **Clusters de groupe de travail.** Clusters avec des nœuds qui sont des serveurs membres/groupe de travail (et non joints à un domaine). 

Pour plus d’informations, consultez [groupes de travail et clusters à plusieurs domaines dans Windows Server 2016](https://blogs.msdn.com/b/clustering/archive/2015/08/17/10635825.aspx)

### <a name="virtual-machine-load-balancing"></a><a name="BKMK_VMLoadBalancing"></a>Équilibrage de charge de la machine virtuelle  

L’équilibrage de charge de l’ordinateur virtuel est une nouvelle fonctionnalité du clustering de basculement qui facilite l’équilibrage de charge transparent des ordinateurs virtuels sur les nœuds d’un cluster. Les nœuds surdédiés sont identifiés en fonction de la mémoire de l’ordinateur virtuel et de l’utilisation du processeur sur le nœud. Les machines virtuelles sont ensuite déplacées (migration dynamique) d’un nœud trop sollicitée vers des nœuds avec une bande passante disponible (le cas échéant). L’intensité de l’équilibrage peut être réglée pour garantir des performances et une utilisation optimales du cluster. L’équilibrage de charge est activé par défaut dans la version d’évaluation technique de Windows Server 2016. Toutefois, l’équilibrage de charge est désactivé lorsque l’optimisation dynamique SCVMM est activée. 

### <a name="virtual-machine-start-order"></a><a name="BKMK_VMStartOrder"></a>Ordre de démarrage de la machine virtuelle

L’ordre de démarrage des machines virtuelles est une nouvelle fonctionnalité du clustering de basculement qui présente l’orchestration de l’ordre de démarrage des machines virtuelles (et de tous les groupes) dans un cluster. Les machines virtuelles peuvent désormais être regroupées en niveaux, et les dépendances de début d’ordre peuvent être créées entre différents niveaux. Cela permet de s’assurer que les machines virtuelles les plus importantes (comme les contrôleurs de domaine ou les machines virtuelles utilitaires) sont démarrées en premier. Les ordinateurs virtuels ne sont pas démarrés tant que les ordinateurs virtuels sur lesquels ils ont une dépendance ne sont pas également démarrés. 

### <a name="simplified-smb-multichannel-and-multi-nic-cluster-networks"></a><a name="BKMK_SMBMultiChannel"></a>Réseaux de clusters à plusieurs cartes réseau et SMB Multichannel simplifiés

Les réseaux de cluster de basculement ne sont plus limités à une seule carte réseau par sous-réseau/réseau. Avec des réseaux de clusters à plusieurs cartes réseau et multicanaux SMB simplifiés, la configuration réseau est automatique et chaque carte réseau sur le sous-réseau peut être utilisée pour le trafic de cluster et de charge de travail. Cette amélioration permet aux clients d’optimiser le débit du réseau pour Hyper-V, SQL Server instance de cluster de basculement et d’autres charges de travail SMB. 

Pour plus d’informations, consultez la [page simplification des réseaux de clusters SMB multicanaux et à plusieurs cartes réseau](smb-multichannel.md).

## <a name="see-also"></a> Voir aussi

* [Stockage](../storage/storage.md)  
* [Nouveautés du stockage dans Windows Server 2016](../storage/whats-new-in-storage.md)  
