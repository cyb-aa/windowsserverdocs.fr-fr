---
ms.assetid: 350aa5a3-5938-4921-93dc-289660f26bad
title: "Quelles sont les nouveautés dans le Clustering de basculement dans Windows Server"
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: get-started-article
manager: dongill
author: JasonGerend
ms.author: jgerend
ms.date: 10/11/2016
ms.openlocfilehash: a4330f62095e13f2f4736f15924ed31fb4893e7a
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="whats-new-in-failover-clustering-in-windows-server-2016"></a>Quelles sont les nouveautés dans le Clustering de basculement dans Windows Server 2016
> S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Cette rubrique décrit les fonctionnalités nouvelles et modifiées dans le Clustering de basculement dans Windows Server 2016.  

## <a name="BKMK_RollingUpgrade"></a>Mise à niveau propagée de système d’exploitation cluster  
Une nouvelle fonctionnalité de Clustering de basculement, Cluster système d’exploitation mise à niveau propagée, permet à un administrateur mettre à niveau le système d’exploitation des nœuds de cluster Windows Server 2012 R2 vers Windows Server 2016 sans arrêter l’Hyper-V ou les charges de travail du serveur de fichiers avec montée en puissance parallèle. À l’aide de cette fonctionnalité, les pénalités de temps d’arrêt contre les contrats de niveau de Service (SLA) peuvent être évitées.  

**La valeur ajouter par ce changement?**  

Mise à niveau un cluster Hyper-V ou le serveur de fichiers avec montée en puissance parallèle à partir de Windows Server 2012 R2 vers Windows Server 2016 ne sont plus nécessite l’arrêt. Le cluster continue de fonctionner à un niveau de Windows Server 2012 R2 jusqu'à ce que tous les nœuds du cluster exécutent Windows Server 2016. Le niveau fonctionnel du cluster est mis à niveau vers Windows Server 2016 à l’aide de l’applet de commande Windows PowerShell `Update-ClusterFunctionalLevel`.  

> [!WARNING]  
> -   Une fois que vous mettez à jour le niveau fonctionnel du cluster, vous ne pouvez pas revenir en arrière pour le niveau fonctionnel d’un cluster Windows Server 2012 R2.  
> -   Jusqu'à ce que le `Update-ClusterFunctionalLevel` applet de commande est exécutée, le processus est réversible et Windows Server 2012 R2 nœuds peuvent être ajoutés et Windows Server 2016 nœuds peuvent être supprimés.  

**Ce qui fonctionne différemment?**  

Un cluster de basculement Hyper-V ou le serveur de fichiers avec montée en puissance parallèle peuvent désormais facilement être mis à niveau sans aucun temps d’arrêt ou avez besoin pour créer un cluster avec les nœuds qui exécutent le système d’exploitation Windows Server 2016. Migration des clusters Windows Server 2012 R2 impliqués déconnexion du cluster existant et réinstaller le nouveau système d’exploitation pour chaque nœud, puis mettre le cluster en ligne. L’ancien processus a été requis et lourde temps d’arrêt. Toutefois, dans Windows Server 2016, le cluster n’a pas besoin de se déconnecter à tout moment.  

Les systèmes d’exploitation de cluster pour la mise à niveau dans les phases sont les suivantes pour chaque nœud dans un cluster:  
-   Le nœud est suspendu et vider tous les ordinateurs virtuels qui sont en cours d’exécution sur ce dernier.  
-   Les ordinateurs virtuels (ou autre charge de travail de cluster) est migré vers un autre nœud du cluster. Les ordinateurs virtuels sont migrés vers un autre nœud du cluster.  
-   Le système d’exploitation existant est supprimé et une nouvelle installation du système d’exploitation Windows Server 2016 sur le nœud est effectuée.  
-   Le nœud qui exécute le système d’exploitation Windows Server 2016 est ajouté au cluster.  
-   À ce stade, le cluster est dit être en cours d’exécution en mode mixte, car les nœuds de cluster exécutent Windows Server 2012 R2 ou Windows Server 2016.  
-   Le niveau fonctionnel du cluster reste à Windows Server 2012 R2. À ce niveau fonctionnel, les nouvelles fonctionnalités dans Windows Server 2016 qui affectent la compatibilité avec les versions précédentes du système d’exploitation ne seront pas disponibles.  
-   Finalement, tous les nœuds sont mis à niveau vers Windows Server 2016.  
-   Niveau fonctionnel du cluster est modifié puis vers Windows Server 2016 à l’aide de l’applet de commande Windows PowerShell `Update-ClusterFunctionalLevel`. À ce stade, vous pouvez tirer parti des fonctionnalités de Windows Server 2016.  

Pour plus d’informations, voir [niveau propagée de Cluster de système d’exploitation](cluster-operating-system-rolling-upgrade.md).  

## <a name="BKMK_SR"></a>Réplica de stockage  
Le réplica de stockage est une nouvelle fonctionnalité qui permet d’indépendante du stockage, la réplication synchrone au niveau du bloc entre des serveurs ou clusters pour la récupération d’urgence, ainsi que l’extension d’un cluster de basculement entre les sites. La réplication synchrone permet la mise en miroir des données dans des sites physiques avec des volumes cohérents pour garantir la perte de données au niveau du système de fichiers. La réplication asynchrone permet l’extension de site au-delà de plages métropolitaines avec la possibilité de perte de données.  

**La valeur ajouter par ce changement?**  

Le réplica de stockage vous permet d’effectuer les opérations suivantes:  

-   Fournir une seule solution de récupération d’urgence fournisseur pour les interruptions planifiées et des charges de travail critiques mission.  

-   Utiliser le transport SMB3 avec des performances, d’extensibilité et une fiabilité reconnues.  

-   Étirer des clusters de basculement Windows aux distances métropolitaines.  

-   Utilisez un logiciel Microsoft bout en bout pour le stockage et le clustering, comme Hyper-V, le réplica de stockage, les espaces de stockage, Cluster, serveur de fichiers avec montée en puissance parallèle, SMB3, la déduplication des données et ReFS/NTFS.  

-   Aider à réduire les coûts et la complexité comme suit:  

    -   Est indépendant du matériel, aucune configuration requise pour une configuration de stockage spécifique comme DAS ou SAN.  

    -   Elle autorise des technologies de mise en réseau et de stockage.  

    -   Facilité de fonctionnalités de gestion graphique pour des nœuds individuels et les clusters via le Gestionnaire de Cluster de basculement.  

    -   Inclut des options de script complètes et à grande échelle via Windows PowerShell.  

-   Permet de réduire les temps d’arrêt et augmenter la fiabilité et la productivité intrinsèques à Windows.  

-   Fournir une prise en charge, les mesures de performances et fonctionnalités de diagnostic.  

Pour plus d’informations, voir la [le réplica de stockage dans Windows Server 2016](../storage/storage-replica/storage-replica-overview.md).  


## <a name="BKMK_CloudWitness"></a>Témoin de cloud  
Un témoin de cloud est un nouveau type de témoin de quorum de Cluster de basculement dans Windows Server 2016 qui tire parti de Microsoft Azure comme point d’arbitrage. Comme n’importe quel autre témoin de quorum, témoin Cloud Obtient un vote et peut participer, dans les calculs de quorum. Vous pouvez configurer un témoin de cloud en tant que témoin de quorum à l’aide de l’Assistant Configuration d’un Cluster de Quorum.  

**La valeur ajouter par ce changement?**  

À l’aide d’un témoin de Cloud en tant qu’un Cluster de basculement témoin de quorum offre les avantages suivants:  

-   Tire parti de Microsoft Azure et élimine la nécessité pour un centre de données distinct tiers.  

-   Utilise le standard disponible publiquement Microsoft Azure stockage d’objets Blob qui élimine les coûts de maintenance supplémentaires d’ordinateurs virtuels hébergés dans un cloud public.  

-   Même compte de stockage Microsoft Azure peut être utilisé pour plusieurs clusters (fichier d’un objet blob par cluster; id unique de cluster utilisé comme nom de fichier d’objet blob).  

-   Fournit un très faible coût par la suite pour le compte de stockage (données très petites écrites par fichier blob, fichier blob mis à jour qu’une seule fois quand change état des nœuds de cluster).  

Pour plus d’informations, voir [déployer un Cloud témoin pour un Cluster de basculement](deploy-cloud-witness.md).  

**Ce qui fonctionne différemment?**  

Cette fonctionnalité est une nouveauté dans Windows Server 2016.  

## <a name="BKMK_VMs"></a>Résilience de Machine virtuelle  
**Calcul de résilience** Windows Server 2016 inclut la résilience de calcul accrue des ordinateurs virtuels pour aider à réduire les problèmes de communication intracluster dans votre cluster de calcul comme suit: 

-   **Options de résilience disponibles pour les ordinateurs virtuels:** vous pouvez maintenant configurer les options de résilience de machine virtuelle qui définissent le comportement des ordinateurs virtuels au cours des échecs passagers:  

    -   **Niveau de résilience:** vous permet de définir comment les échecs passagers sont gérées.  

    -   **Période de résilience:** vous permet de définir la durée pendant laquelle tous les ordinateurs virtuels sont autorisés à exécuter isolé.  

-   **Mise en quarantaine de nœuds n’est pas intègre:** n’est pas intègre nœuds sont mis en quarantaine et ne sont plus autorisées à rejoindre le cluster. Cela empêche les nœuds ailes d’incidence négative sur autres nœuds et le cluster global.  

Pour plus d’informations machine virtuelle calcul résilience du flux de travail et le nœud de mise en quarantaine paramètres qui contrôlent la façon dont votre nœud est placé dans isolation ou de mise en quarantaine, voir [résilience de calcul d’ordinateur virtuel dans Windows Server 2016](http://blogs.msdn.com/b/clustering/archive/2015/06/03/10619308.aspx).  

**Résilience du stockage** dans Windows Server 2016, les ordinateurs virtuels sont plus résistants aux défaillances de stockage temporaire. La résilience de machine virtuelle améliorée permet de conserver les États de session d’ordinateur virtuel client en cas d’une interruption de stockage. Pour ce faire, la réponse de machine virtuelle intelligent et rapide à des problèmes d’infrastructure de stockage.  

Lorsqu’un ordinateur virtuel se déconnecte de son stockage sous-jacent, il met en pause et attend que le stockage à récupérer. Pendant la suspension, la machine virtuelle conserve le contexte des applications qui s’exécutent dans celui-ci. Lors de la connexion de l’ordinateur virtuel à son emplacement de stockage est restaurée, l’ordinateur virtuel retourne à son état en cours d’exécution. Par conséquent, l’état de session de l’ordinateur client est conservée sur la récupération.  

Dans Windows Server 2016, résilience de stockage d’ordinateur virtuel est trop prenant en charge et optimisé pour les clusters invités.  

## <a name="BKMK_Diagnostics"></a>Améliorations apportées au diagnostic dans le Clustering de basculement  
Pour aider à diagnostiquer les problèmes avec les clusters de basculement, Windows Server 2016 inclut les éléments suivants:  

-   Plusieurs améliorations aux fichiers journaux cluster (par exemple, le journal des informations de fuseau horaire et DiagnosticVerbose) qui est plus facile résoudre les problèmes de clustering de basculement. Pour plus d’informations, voir [améliorations Windows Server 2016 basculement Cluster dépannage - journal de Cluster](http://blogs.msdn.com/b/clustering/archive/2015/05/15/10614930.aspx).  

-   Un nouveau type d’un vidage de **image mémoire Active**, qui exclut par la plupart des pages de mémoire allouées aux ordinateurs virtuels et est donc memory.dmp beaucoup plus petites et plus facile à enregistrer ou copier.  Pour plus d’informations, voir [améliorations Windows Server 2016 basculement Cluster dépannage - Dump Active](http://blogs.msdn.com/b/clustering/archive/2015/05/18/10615526.aspx).  

## <a name="BKMK_SiteAware"></a>Clusters de basculement reconnaissant les sites  
Windows Server 2016 inclut un site prenant en charge les clusters de basculement qui permettent de regrouper des nœuds de clusters étendus en fonction de leur emplacement physique (site). Reconnaissance des sites de cluster permet d’améliorer opérations clés pendant le cycle de vie du cluster comme le comportement de basculement, les stratégies de positionnement, les pulsations entre les nœuds et le comportement du quorum. Pour plus d’informations, voir [Site adaptée aux Clusters de basculement dans Windows Server 2016](http://blogs.msdn.com/b/clustering/archive/2015/08/19/10636304.aspx).  

## <a name="BKMK_multidomainclusters"></a>Clusters de groupes de travail et à domaines multiples  
Dans Windows Server 2012 R2 et versions antérieures, un cluster peut uniquement être créé entre les nœuds membres joints au même domaine.   Windows Server 2016 déjoue et offre la possibilité de créer un Cluster de basculement sans dépendances Active Directory. Vous pouvez désormais créer des clusters de basculement dans les configurations suivantes:  

-   **Clusters à domaine unique.** Clusters avec tous les nœuds joints au même domaine.  

-   **Clusters à domaines multiples.** Clusters de nœuds qui sont membres de domaines différents.  

-   **Clusters de groupe de travail.** Les clusters de nœuds qui sont des serveurs membres / groupe de travail (non au domaine).  

Pour plus d’informations, voir [clusters de groupes de travail et à domaines multiples dans Windows Server 2016](http://blogs.msdn.com/b/clustering/archive/2015/08/17/10635825.aspx)  
## <a name="BKMK_VMLoadBalancing"></a>Équilibrage de la charge de l’ordinateur virtuel  
L’équilibrage de la machine virtuelle est une nouvelle fonctionnalité de Clustering de basculement qui facilite l’équilibrage de charge transparente des ordinateurs virtuels entre les nœuds dans un cluster. Trop sollicités nœuds sont identifiés basées sur l’ordinateur virtuel la mémoire et l’utilisation du processeur sur le nœud. Les ordinateurs virtuels sont déplacés (migration dynamique) à partir d’un nœud sollicité vers des nœuds avec la bande passante disponible (le cas échéant). L’intensité de l’équilibrage de la puisse être ajustée pour vous assurer de l’utilisation et les performances optimales de cluster.  L’équilibrage de charge est activée par défaut dans Windows Server 2016 Technical Preview. Toutefois, l’équilibrage de charge est désactivée lorsque l’optimisation dynamique SCVMM est activée.  

## <a name="BKMK_VMStartOrder"></a>Ordre de démarrage d’ordinateur virtuel  
Ordre de démarrage de machine virtuelle est une nouvelle fonctionnalité de Clustering de basculement qui présente l’orchestration ordre de démarrage pour les machines virtuelles (et tous les groupes) dans un cluster. Les ordinateurs virtuels peuvent maintenant être regroupés en niveaux et dépendances d’ordre de démarrage peuvent être créés entre les différents niveaux. Cela garantit que les ordinateurs virtuels plus importants (tels que les contrôleurs de domaine ou l’utilitaire de machines virtuelles) sont démarrés en premier. Les ordinateurs virtuels ne sont pas démarrés jusqu'à ce que les ordinateurs virtuels qu’ils ont une dépendance sur sont également démarrés.  

## <a name="BKMK_SMBMultiChannel"></a>Les réseaux de Cluster de plusieurs cartes réseau et SMB Multichannel simplifiés  
Réseaux de Cluster de basculement ne sont plus limitées à une seule carte réseau par sous-réseau / réseau. Avec simplifié SMB Multichannel et réseaux de Cluster de plusieurs cartes réseau, la configuration réseau est automatique et chaque carte réseau sur le sous-réseau peut être utilisé pour le trafic de cluster et de la charge de travail. Cette amélioration permet aux clients optimiser le débit de réseau pour les autres charges de travail SMB, Instance de Cluster de basculement SQL Server et Hyper-V.  

Pour plus d’informations, voir [simplifié SMB Multichannel et réseaux de Cluster de plusieurs cartes réseau](smb-multichannel.md).

## <a name="see-also"></a>Voir aussi  
* [Stockage](../storage/storage.md)  
* [Nouveautés du stockage dans Windows Server 2016](../storage/whats-new-in-storage.md)  
