---
ms.assetid: 350aa5a3-5938-4921-93dc-289660f26bad
title: Quelles sont les nouveautés dans le Clustering de basculement dans Windows Server
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: get-started-article
manager: dongill
author: JasonGerend
ms.author: jgerend
ms.date: 10/18/2018
ms.openlocfilehash: 3c0792347aaa70fe80d346cc51cbc44b73c42f39
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65476022"
---
# <a name="whats-new-in-failover-clustering"></a>Nouveautés du clustering de basculement

> S’applique à : Windows Server 2019, Windows Server 2016

Cette rubrique décrit les fonctionnalités nouvelles et modifiées dans le Clustering de basculement pour Windows Server 2019 et Windows Server 2016.

## <a name="whats-new-in-windows-server-2019"></a>Nouveautés de Windows Server 2019

- **Jeux de clusters**

    Ensembles de cluster permettent d’augmenter le nombre de serveurs dans une solution unique défini par logiciel Centre de données (SDDC) au-delà des limites d’un cluster en cours. Cela est accompli en regroupant plusieurs clusters dans un ensemble de cluster--un regroupement faiblement couplés de plusieurs clusters de basculement : calcul, stockage et hyperconvergés.
    Avec des jeux de cluster, vous pouvez déplacer des machines virtuelles en ligne (migration dynamique) entre les clusters au sein du cluster défini.

    Pour plus d’informations, consultez [ensembles de Cluster](../storage/storage-spaces/cluster-sets.md).

- **Clusters prenant en charge Azure**

    Clusters de basculement détectent désormais automatiquement lorsqu’ils sont exécutent dans des machines virtuelles Azure IaaS et optimiser la configuration pour permettre le basculement proactive et la journalisation des événements de maintenance planifiée Azure pour obtenir les meilleurs niveaux de disponibilité. Le déploiement est également simplifié en supprimant la nécessité de configurer l’équilibrage de charge avec les noms de réseau dynamique pour le nom du cluster.

- **Migration de cluster de domaines**

    Clusters de basculement peut maintenant dynamiquement déplacer un domaine Active Directory vers un autre, ce qui simplifie la consolidation des domaines et autorisant les clusters créés par des partenaires de matériel et joint au domaine du client plus tard.
- **Témoin USB**

    Vous pouvez maintenant utiliser une simple clé USB attachée à un commutateur réseau en tant que témoin dans la détermination du quorum pour un cluster. Cela permet d’étendre le témoin de partage de fichier pour prendre en charge n’importe quel appareil SMB2 conformes.

- **Améliorations de l’infrastructure cluster**

    Il est désormais activé par défaut pour améliorer les performances de la machine virtuelle. MSDTC prend désormais en charge les volumes partagés de cluster afin d'autoriser le déploiement de charges de travail MSDTC sur les espaces de stockage direct comme avec SQL Server. Amélioration de la logique de détection de nœuds partitionnés avec autoréparation pour que les nœuds redeviennent membres du cluster. Détection d'itinéraire de réseau en cluster et autoréparation améliorées.

- **La mise à jour prenant en charge du cluster prend en charge les espaces de stockage Direct**

    La mise à jour adaptée aux clusters (Cluster Aware Updating ou CAU) est désormais intégrée et détecte les espaces de stockage direct, vérifiant et garantissant que la resynchronisation des données est terminée sur chaque nœud. La mise à jour prenant en charge du cluster inspecte les mises à jour redémarrent intelligemment uniquement si nécessaire. Cela permet d’orchestrer des redémarrages de tous les serveurs du cluster pour la maintenance planifiée.

- **Améliorations de témoin de partage de fichiers** nous avons activé l’utilisation d’un témoin de partage de fichiers dans les scénarios suivants : 
    - Accès Internet absent ou extrêmement médiocre en raison d’un emplacement distant, empêchant l’utilisation d’un témoin de cloud. 
    - Absence des lecteurs partagés pour un témoin de disque. Cela peut être une configuration hyperconvergé espaces de stockage Direct, un SQL Server toujours sur les groupes de disponibilité, ou un * Exchange de base de données groupe de disponibilité (DAG), aucun d'entre eux utilise des disques partagés. 
    - Absence d’une connexion de contrôleur de domaine en raison du cluster en cours derrière une zone DMZ. 
    - Un groupe de travail ou domaines cluster pour lequel il n’existe aucun objet de nom Active Directory cluster (CNO). Pour en savoir plus sur ces améliorations dans le billet suivant dans les Blogs de gestion & serveur : Témoin de partage de fichiers de Cluster de basculement et DFS.
    
    Nous avons maintenant également bloquer explicitement l’utilisation d’un partage d’espaces de noms DFS en tant qu’emplacement. Ajout d’un témoin de partage de fichier pour une recherche DFS partage peut entraîner des problèmes de stabilité de votre cluster, et cette configuration n’a jamais été prise en charge. Nous avons ajouté la logique permettant de détecter si un partage utilise des espaces de noms DFS, et si les espaces de noms DFS est détectée, le Gestionnaire du Cluster de basculement bloque la création du témoin et affiche un message d’erreur concernant n’est ne pas pris en charge.
- **Sécurisation renforcée du cluster**

    La communication intra-cluster via Server Message Block (SMB) pour les volumes partagés de cluster et les espaces de stockage direct tire maintenant parti des certificats pour fournir la plateforme la plus sécurisée. Les clusters de basculement peuvent ainsi fonctionner sans dépendances sur NTLM et autoriser les bases de référence de sécurité.
- **Cluster de basculement n’utilise plus l’authentification NTLM**

    Clusters de basculement ne plus utiliser l’authentification NTLM. Au lieu de cela Kerberos et l’authentification basée sur le certificat est utilisé exclusivement. Il n’y a aucune modification requise par l’utilisateur ou les outils de déploiement, pour tirer parti de cette amélioration de la sécurité. Il permet également de clusters de basculement pour être déployé dans les environnements où NTLM a été désactivé. 


## <a name="whats-new-in-windows-server-2016"></a>Quelles sont les nouveautés dans Windows Server 2016

### <a name="BKMK_RollingUpgrade"></a>Système d’exploitation en cluster mise à niveau propagée

Niveau propagée de cluster système d’exploitation permet à un administrateur mettre à niveau le système d’exploitation des nœuds du cluster à partir de Windows Server 2012 R2 vers une version plus récente sans arrêter les charges de travail du serveur de fichiers avec montée en puissance parallèle ni Hyper-V. Avec cette fonctionnalité, les pénalités de temps d’arrêt sur les contrats de niveau de service peuvent être évitées. 

**Quels avantages cette modification procure-t-elle ?**  

La mise à niveau un cluster Hyper-V ou le serveur de fichiers avec montée en puissance de Windows Server 2012 R2 vers Windows Server 2016 n’est plus nécessite l’arrêt. Le cluster continue de fonctionner à un niveau de Windows Server 2012 R2, jusqu'à ce que tous les nœuds du cluster exécutent Windows Server 2016. Le niveau fonctionnel du cluster est mis à niveau vers Windows Server 2016 à l’aide de l’applet de commande Windows PowerShell `Update-ClusterFunctionalLevel`. 

> [!WARNING]  
> -   Une fois que vous mettez à jour le niveau fonctionnel du cluster, vous ne pouvez pas revenir à un niveau fonctionnel de cluster Windows Server 2012 R2. 
> -   Jusqu'à ce que le `Update-ClusterFunctionalLevel` applet de commande est exécutée, le processus est réversible et les nœuds de Windows Server 2012 R2 peuvent être ajoutés et les nœuds Windows Server 2016 peuvent être supprimés. 

**En quoi le fonctionnement est-il différent ?**  

Un cluster de basculement Hyper-V ou le serveur de fichiers avec montée en puissance peut désormais facilement être mis à niveau sans interruption de service ou avez besoin pour créer un nouveau cluster avec des nœuds qui exécutent le système d’exploitation Windows Server 2016. Clusters de migration vers Windows Server 2012 R2 impliquant mise hors connexion de cluster existant et le nouveau système d’exploitation pour chaque nœud de la réinstallation et puis mettre le cluster en ligne. L’ancien processus a été requise et lourde temps d’arrêt. Toutefois, dans Windows Server 2016, le cluster n’a pas besoin de se déconnecter à tout moment. 

Les systèmes d’exploitation de cluster pour la mise à niveau en plusieurs phases sont les suivantes pour chaque nœud dans un cluster :  
-   Le nœud est suspendu et purgé de toutes les machines virtuelles qui s’exécutent sur celui-ci. 
-   Les machines virtuelles (ou autre charge de travail de cluster) est migrée vers un autre nœud du cluster. 
-   Le système d’exploitation est supprimé et une nouvelle installation du système d’exploitation Windows Server 2016 sur le nœud est exécutée. 
-   Le nœud qui exécute le système d’exploitation Windows Server 2016 est ajouté au cluster. 
-   À ce stade, le cluster est dit être en cours d’exécution en mode mixte, car les nœuds du cluster exécutent Windows Server 2012 R2 ou Windows Server 2016. 
-   Le niveau fonctionnel du cluster reste à Windows Server 2012 R2. À ce niveau fonctionnel, les nouvelles fonctionnalités dans Windows Server 2016 qui affectent la compatibilité avec les versions précédentes du système d’exploitation ne seront pas disponibles. 
-   Finalement, tous les nœuds sont mis à niveau vers Windows Server 2016. 
-   Niveau fonctionnel du cluster est ensuite modifié vers Windows Server 2016 à l’aide de l’applet de commande Windows PowerShell `Update-ClusterFunctionalLevel`. À ce stade, vous pouvez tirer parti des fonctionnalités de Windows Server 2016. 

Pour plus d’informations, consultez [niveau propagée de Cluster système d’exploitation](cluster-operating-system-rolling-upgrade.md). 

### <a name="BKMK_SR"></a>Réplica de stockage  
Réplica de stockage est une nouvelle fonctionnalité qui permet d’indépendante du stockage, la réplication synchrone au niveau du bloc entre des serveurs ou clusters pour la récupération d’urgence, ainsi que l’extension d’un cluster de basculement entre sites. La réplication synchrone permet la mise en miroir des données dans des sites physiques avec des volumes cohérents en cas d’incident, ce qui garantit aucune perte de données au niveau du système de fichiers. La réplication asynchrone permet l’extension de site au-delà de plages métropolitaines avec la possibilité de perte de données. 

**Quels avantages cette modification procure-t-elle ?**  

Le réplica de stockage vous permet d’effectuer les opérations suivantes :  

-   Proposer une seule solution de reprise d’activité fournisseur pour les interruptions planifiées et non planifiées des charges de travail critiques. 

-   Utiliser le transport SMB3 avec des performances, une scalabilité et une fiabilité reconnues. 

-   Étendre les clusters de basculement Windows aux distances métropolitaines. 

-   Utiliser des logiciels de Microsoft bout en bout pour le stockage et le clustering, telles que Hyper-V, le réplica de stockage, les espaces de stockage, Cluster, serveur de fichiers avec montée en puissance, SMB3, la déduplication des données et ReFS/NTFS. 

-   Réduire les coûts et la complexité comme suit :  

    -   La réplication est indépendante du matériel et ne nécessite pas de configuration de stockage particulière, comme DAS ou SAN. 

    -   Elle autorise des technologies de mise en réseau et de stockage des produits. 

    -   Elle facilite la gestion graphique pour les nœuds individuels et les clusters via le Gestionnaire du cluster de basculement. 

    -   Elle inclut des options de script complètes et à grande échelle via Windows PowerShell. 

-   Réduire les temps d’arrêt et augmenter la fiabilité et la productivité intrinsèques à Windows. 

-   Fournir la capacité de prise en charge, les mesures de performances et les fonctionnalités de diagnostic. 

Pour plus d’informations, voir [Réplica de stockage dans Windows Server 2016](../storage/storage-replica/storage-replica-overview.md). 


### <a name="BKMK_CloudWitness"></a>Témoin de cloud  
Dans Windows Server 2016, Témoin cloud est un nouveau type de témoin de quorum de cluster avec basculement, qui utilise Microsoft Azure comme point d’arbitrage. Comme les autres témoins de quorum, Témoin cloud obtient un vote et peut prendre part à des calculs de quorum. Vous pouvez le configurer comme témoin de quorum à l’aide de l’Assistant Configuration de quorum du cluster. 

**Quels avantages cette modification procure-t-elle ?**  

À l’aide d’un témoin de Cloud comme un Cluster de basculement témoin de quorum offre les avantages suivants :  

-   S’appuie sur Microsoft Azure et élimine la nécessité d’un centre de données distinct tiers. 

-   Utilise le standard disponible publiquement Microsoft stockage Blob Azure qui élimine la surcharge de maintenance supplémentaire de machines virtuelles hébergées dans un cloud public. 

-   Même compte de stockage Microsoft Azure peut être utilisé pour plusieurs clusters (fichier d’un objet blob par cluster ; id unique de cluster utilisé comme nom de fichier blob). 

-   Fournit un très faible coût en cours pour le compte de stockage (très petites données écrites par fichier blob, fichier blob mis à jour une seule fois lorsque l’état des nœuds de cluster passe). 

Pour plus d’informations, consultez [déployer un Cloud témoin pour un Cluster de basculement](deploy-cloud-witness.md). 

**En quoi le fonctionnement est-il différent ?**  

Cette fonctionnalité est une nouveauté de Windows Server 2016. 

### <a name="BKMK_VMs"></a>Résilience de Machine virtuelle  
**Calcul de la résilience** Windows Server 2016 inclut la résilience de calcul des machines virtuelles accrue pour aider à réduire les problèmes de communication intracluster dans votre cluster de calcul comme suit : 

-   **Options de résilience disponibles pour les machines virtuelles :**  Vous pouvez désormais configurer les options de résilience de machine virtuelle qui définissent le comportement des ordinateurs virtuels lors de pannes temporaires :  

    -   **Niveau de résilience :** Vous permet de définir la manière dont sont gérées les échecs temporaires. 

    -   **Période de la résilience :**  Vous permet de définir la durée pendant laquelle toutes les machines virtuelles sont autorisés à exécuter isolé. 

-   **Mise en quarantaine de nœuds défectueux :** Nœuds défectueux sont mis en quarantaine et ne sont plus autorisés à rejoindre le cluster. Cela empêche les nœuds ailes d’incidence négative sur le cluster global et les autres nœuds. 

Pour plus d’informations machine virtuelle calcul la résilience des flux de travail et le nœud mise en quarantaine les paramètres qui contrôlent comment votre nœud est placé dans isolation ou mettre en quarantaine, consultez [résilience de calcul de Machine virtuelle dans Windows Server 2016](http://blogs.msdn.com/b/clustering/archive/2015/06/03/10619308.aspx). 

**La résilience du stockage** dans Windows Server 2016, les machines virtuelles sont plus résistantes aux défaillances de stockage temporaire. La résilience de machine virtuelle améliorée permet de conserver les États de session de machine virtuelle de locataire en cas de perturbation de stockage. Cela, réponse de machine virtuelle intelligent et rapide pour les problèmes d’infrastructure de stockage. 

Lorsqu’une machine virtuelle se déconnecte de son stockage sous-jacent, il est maintenu et attend que le stockage à récupérer. Pendant la suspension, la machine virtuelle conserve le contexte des applications qui sont en cours d’exécution qu’il contient. En cas de connexion de l’ordinateur virtuel à son stockage est restaurée, la machine virtuelle retourne à son état en cours d’exécution. Par conséquent, l’état de session de l’ordinateur client est conservée sur la récupération. 

Dans Windows Server 2016, la résilience de stockage de machine virtuelle est trop prenant en charge et optimisé pour les clusters invités. 

### <a name="BKMK_Diagnostics"></a>Améliorations des diagnostics dans le Clustering de basculement  
Pour aider à diagnostiquer les problèmes avec les clusters de basculement, Windows Server 2016 inclut les éléments suivants :  

-   Plusieurs améliorations pour les fichiers journaux de cluster (par exemple, le journal des informations de fuseau horaire et DiagnosticVerbose) qui rend est plus facile de résoudre les problèmes de clustering de basculement. Pour plus d’informations, consultez [2016 basculement Cluster dépannage des améliorations de Windows Server - journal de Cluster](http://blogs.msdn.com/b/clustering/archive/2015/05/15/10614930.aspx). 

-   Un nouveau type d’un vidage de **vidage de mémoire Active**, qui élimine la plupart des pages de mémoire allouée aux machines virtuelles et est donc memory.dmp beaucoup plus petite et plus facile à enregistrer ou copier. Pour plus d’informations, consultez [2016 basculement Cluster dépannage des améliorations de Windows Server - vider Active](http://blogs.msdn.com/b/clustering/archive/2015/05/18/10615526.aspx). 

### <a name="BKMK_SiteAware"></a>Clusters de basculement reconnaissant les sites  
Windows Server 2016 inclut un site prenant en charge les clusters de basculement permettent à des nœuds de groupe de clusters étendus en fonction de leur emplacement physique (site). Cluster-la reconnaissance des sites améliore des opérations clés pendant le cycle de vie de cluster telles que le comportement de basculement, les stratégies de positionnement, les pulsations entre les nœuds et le comportement du quorum. Pour plus d’informations, consultez [Site prenant en charge les Clusters de basculement dans Windows Server 2016](http://blogs.msdn.com/b/clustering/archive/2015/08/19/10636304.aspx). 

### <a name="BKMK_multidomainclusters"></a>Clusters de groupe de travail et à domaines multiples  
Dans Windows Server 2012 R2 et versions antérieures, un cluster peut uniquement être créé entre les nœuds membres joints au même domaine. Windows Server 2016 déjoue cet obstacle en introduisant la possibilité de créer un cluster de basculement sans dépendances Active Directory. Vous pouvez maintenant créer des clusters de basculement dans les configurations suivantes :  

-   **Clusters à domaine unique.** Clusters avec tous les nœuds sont joints au même domaine. 

-   **Clusters à domaines multiples.** Clusters de nœuds qui sont membres de domaines différents. 

-   **Clusters de groupe de travail.** Les clusters de nœuds qui sont des serveurs membres / groupe de travail (pas joint au domaine). 

Pour plus d’informations, consultez [clusters de groupe de travail et à domaines multiples dans Windows Server 2016](http://blogs.msdn.com/b/clustering/archive/2015/08/17/10635825.aspx)  
### <a name="BKMK_VMLoadBalancing"></a>Équilibrage de charge de Machine virtuelle  
Équilibrage de charge d’ordinateur virtuel est une nouvelle fonctionnalité de Clustering de basculement qui facilite l’équilibrage de charge transparente des machines virtuelles entre les nœuds dans un cluster. Nœuds sollicités sont identifiés en fonction de la machine virtuelle de mémoire et l’utilisation du processeur sur le nœud. Les machines virtuelles sont ensuite déplacées (migration en direct) à partir d’un nœud sollicité aux nœuds avec la bande passante disponible (le cas échéant). L’intensité de l’équilibrage de peut être paramétrée pour garantir des performances de cluster optimale et l’utilisation. Équilibrage de charge est activée par défaut dans Windows Server 2016 Technical Preview. Toutefois, l’équilibrage de charge est désactivée lorsque l’optimisation dynamique SCVMM est activée. 

### <a name="BKMK_VMStartOrder"></a>Ordre de démarrage de Machine virtuelle  
Ordre de démarrage de machine virtuelle est une nouvelle fonctionnalité de Clustering de basculement qui présente l’orchestration d’ordre de démarrage pour les machines virtuelles (et tous les groupes) dans un cluster. Machines virtuelles peuvent maintenant être regroupés en niveaux et les dépendances d’ordre de démarrage peuvent être créés entre les différents niveaux. Cela garantit que les machines virtuelles plus importantes (par exemple, les contrôleurs de domaine ou l’utilitaire de machines virtuelles) sont démarrés en premier. Machines virtuelles ne sont pas démarrés jusqu'à ce que les ordinateurs virtuels qu’ils ont une dépendance sur sont également démarrés. 

### <a name="BKMK_SMBMultiChannel"></a> Les réseaux de Cluster de plusieurs cartes réseau et SMB Multichannel simplifiés  
Les réseaux de Cluster de basculement ne sont plus limités à une seule carte réseau par sous-réseau / réseau. Avec simplifié SMB Multichannel et les réseaux de Cluster de plusieurs cartes réseau, la configuration réseau est automatique, et chaque carte réseau sur le sous-réseau peut être utilisé pour le trafic de cluster et de la charge de travail. Cette amélioration permet aux clients d’optimiser le débit réseau pour Hyper-V, Instance de Cluster de basculement SQL Server et autres charges de travail SMB. 

Pour plus d’informations, consultez [simplifié SMB Multichannel et les réseaux de Cluster Multi-NIC](smb-multichannel.md).

## <a name="see-also"></a>Voir aussi  
* [Stockage](../storage/storage.md)  
* [Quelles sont les nouveautés dans le stockage dans Windows Server 2016](../storage/whats-new-in-storage.md)  
