---
title: Forum Aux Questions sur le réplica de stockage
ms.prod: windows-server-threshold
manager: siroy
ms.author: nedpyle
ms.technology: storage-replica
ms.topic: get-started-article
author: nedpyle
ms.date: 04/26/2019
ms.assetid: 12bc8e11-d63c-4aef-8129-f92324b2bf1b
ms.openlocfilehash: d4eb2ad65f436db28264650b8c8d7b0cf69b2cee
ms.sourcegitcommit: 6f968368c12b9dd699c197afb3a3d13c2211f85b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68544677"
---
# <a name="frequently-asked-questions-about-storage-replica"></a>Forum Aux Questions sur le réplica de stockage

>S’applique à : Windows Server 2019, Windows Server 2016, Windows Server (Canal semi-annuel)

Cette rubrique contient des réponses aux questions fréquemment posées sur le réplica de stockage.

## <a name="FAQ1"></a>Le réplica de stockage est-il pris en charge sur Azure?
Oui. Vous pouvez utiliser les scénarios suivants avec Azure:

1. Réplication de serveur à serveur dans Azure (de façon synchrone ou asynchrone entre des machines virtuelles IaaS dans un ou deux domaines d’erreur de centre de contenu ou de manière asynchrone entre deux régions distinctes)
2. Réplication asynchrone de serveur à serveur entre Azure et localement (à l’aide d’un VPN ou d’Azure ExpressRoute)
3. Réplication de cluster à cluster dans Azure (de façon synchrone ou asynchrone entre les machines virtuelles IaaS dans un ou deux domaines d’erreur de centre de contenu ou de manière asynchrone entre deux régions distinctes)
4. Réplication asynchrone de cluster à cluster entre Azure et localement (à l’aide d’un VPN ou d’Azure ExpressRoute)

Vous trouverez des remarques supplémentaires sur le clustering invité dans Azure à l’adresse suivante: [Déploiement de clusters invités de machine virtuelle IaaS dans Microsoft Azure](https://techcommunity.microsoft.com/t5/Failover-Clustering/Deploying-IaaS-VM-Guest-Clusters-in-Microsoft-Azure/ba-p/372126).

Remarques importantes:

1. Azure ne prend pas en charge le clustering invité VHDX partagé, de sorte que les machines virtuelles du cluster de basculement Windows doivent utiliser des cibles iSCSI pour le clustering de réservation de disque persistant classique à stockage partagé ou espaces de stockage direct.
2. Il existe Azure Resource Manager modèles pour le clustering de réplicas de stockage basés sur espaces de stockage direct dans [la création d’un espaces de stockage direct clusters SOFS avec un réplica de stockage pour la récupération d’urgence entre les régions Azure](https://aka.ms/azure-storage-replica-cluster).  
3. La communication RPC de cluster à cluster dans Azure (requise par les API de cluster pour accorder l’accès entre les clusters) requiert la configuration de l’accès réseau pour le CNO. Vous devez autoriser le port TCP 135 et la plage dynamique au-dessus du port TCP 49152. Informations [de référence sur la création d’un cluster de basculement Windows Server sur une machine virtuelle Azure IaaS-réseau et création](https://blogs.technet.microsoft.com/askcore/2015/06/24/building-windows-server-failover-cluster-on-azure-iaas-vm-part-2-network-and-creation/).  
4. Il est possible d’utiliser des clusters invités à deux nœuds, où chaque nœud utilise le protocole iSCSI de bouclage pour un cluster asymétrique répliqué par le réplica de stockage. Mais cela aura probablement des performances très médiocres et doit être utilisé uniquement pour les charges de travail ou les tests très limités.  

## <a name="FAQ2"></a>Comment faire voir la progression de la réplication pendant la synchronisation initiale?  
Les messages de l’événement 1237 affichés dans le journal des événements de l’administrateur de réplica de stockage sur le serveur de destination indiquent le nombre d’octets copiés et restants toutes les 10 secondes. Vous pouvez également utiliser le compteur de performance du réplica de stockage sur la destination affichant **\Statistiques du réplica du système de stockage\Total d’octets reçus** pour un ou plusieurs volumes répliqués. Vous pouvez également interroger le groupe de réplication à l’aide de Windows PowerShell. Par exemple, cet exemple de commande obtient le nom des groupes sur la destination, puis interroge un groupe nommé **Replication 2** toutes les 10 secondes pour afficher la progression :  

```  
Get-SRGroup

do{
    $r=(Get-SRGroup -Name "Replication 2").replicas
    [System.Console]::Write("Number of remaining bytes {0}`n", $r.NumOfBytesRemaining)
    Start-Sleep 10
}until($r.ReplicationStatus -eq 'ContinuouslyReplicating')
Write-Output "Replica Status: "$r.replicationstatus

```  

## <a name="FAQ3"></a>Puis-je spécifier des interfaces réseau spécifiques à utiliser pour la réplication?  

Oui, avec `Set-SRNetworkConstraint`. Cette applet de commande fonctionne au niveau de la couche d’interface et peut être utilisée dans des situations avec ou sans cluster.  
Par exemple, avec un serveur autonome (sur chaque nœud) :  

```  
Get-SRPartnership  

Get-NetIPConfiguration  
```  
Notez les informations de passerelle et d’interface (sur les deux serveurs) et les instructions de partenariat. Ensuite, exécutez :  

```  
Set-SRNetworkConstraint -SourceComputerName sr-srv06 -SourceRGName rg02 -  
SourceNWInterface 2 -DestinationComputerName sr-srv05 -DestinationNWInterface 3 -DestinationRGName rg01  

Get-SRNetworkConstraint  

Update-SmbMultichannelConnection  

```  

Pour configurer des contraintes de réseau sur un cluster étendu :  

    Set-SRNetworkConstraint -SourceComputerName sr-cluster01 -SourceRGName group1 -SourceNWInterface "Cluster Network 1","Cluster Network 2" -DestinationComputerName sr-cluster02 -DestinationRGName group2 -DestinationNWInterface "Cluster Network 1","Cluster Network 2"

## <a name="FAQ4"></a>Puis-je configurer la réplication un-à-plusieurs ou une réplication transitive (A à B vers C)?  
Non, le réplica de stockage ne prend en charge qu’une seule réplication d’un serveur, d’un cluster ou d’un nœud de cluster étendu. Cela peut changer dans une version ultérieure. Vous pouvez bien sûr configurer la réplication entre différents serveurs d’une paire de volumes spécifique, dans les deux sens. Par exemple, le serveur 1 peut répliquer son volume D sur le serveur 2 et son volume E à partir du serveur 3.

## <a name="FAQ5"></a>Puis-je augmenter ou réduire la taille des volumes répliqués répliqués par le réplica de stockage?  
Vous pouvez développer (augmenter) des volumes, mais pas les réduire. Par défaut, le réplica de stockage empêche les administrateurs de développer les volumes répliqués ; utilisez l'option `Set-SRGroup -AllowVolumeResize $TRUE` sur le groupe source avant le redimensionnement. Exemple :

1. À utiliser sur l’ordinateur source:`Set-SRGroup -Name YourRG -AllowVolumeResize $TRUE`
2. Augmentez le volume en utilisant la technique de votre choix.
3. À utiliser sur l’ordinateur source:`Set-SRGroup -Name YourRG -AllowVolumeResize $FALSE` 

## <a name="FAQ6"></a>Puis-je mettre un volume de destination en ligne pour un accès en lecture seule?  
Pas dans Windows Server 2016. Le réplica de stockage démonte le volume de destination au début de la réplication. 

Toutefois, dans Windows Server 2019 et le canal semi-annuel Windows Server à partir de la version 1709, l’option permettant de monter le stockage de destination est désormais possible: cette fonctionnalité est appelée «test de basculement». Pour ce faire, vous devez disposer d’un volume au format NTFS ou ReFS inutilisé qui ne réplique pas sur la destination actuellement. Ensuite, vous pouvez monter une capture instantanée du stockage répliqué temporairement à des fins de test ou de sauvegarde. 

Par exemple, pour créer un test de basculement dans lequel vous répliquez un volume « D : » dans le groupe de réplication « RG2 » sur le serveur de destination « SRV2 », avec un lecteur « T : » sur SRV2 qui n’est pas répliqué :

 `Mount-SRDestination -Name RG2 -Computername SRV2 -TemporaryPath T:\`
 
Le volume D : répliqué est désormais accessible sur SRV2. Vous pouvez les lire et les écrire normalement, y copier des fichiers ou exécuter une sauvegarde en ligne que vous enregistrez ailleurs pour la conservation, sous le chemin D:. Le volume T: ne contiendra que les données de journal.

Pour supprimer l’instantané de test de basculement et supprimer les modifications :

 `Dismount-SRDestination -Name RG2 -Computername SRV2`
 
Vous devez uniquement utiliser la fonctionnalité de test de basculement pour des opérations temporaires à court terme. Elle n’est pas destinée à être utilisée à long terme. Lors de l’utilisation, la réplication continue sur le volume de destination réel. 

## <a name="FAQ7"></a>Puis-je configurer le serveur de fichiers avec montée en puissance parallèle (SOFS) dans un cluster étendu?  
Bien que cela soit techniquement possible, il ne s’agit pas d’une configuration recommandée en raison de l’absence de reconnaissance de site dans les nœuds de calcul qui contactent le SOFS. Si vous utilisez une mise en réseau à distance campus, où les latences sont généralement inférieures à la milliseconde, cette configuration fonctionne généralement sans problème.   

Si vous configurez une réplication de cluster à cluster, le réplica de stockage prend entièrement en charge les serveurs de fichiers avec montée en puissance parallèle, notamment l’utilisation des espaces de stockage direct, lors de la réplication entre deux clusters.  

## <a name="FAQ7.5"></a>Le CSV est-il requis pour la réplication dans un cluster étendu ou entre les clusters?  
Non. Vous pouvez répliquer avec un volume partagé de cluster ou une réservation de disque persistant (PDR) appartenant à une ressource de cluster, telle qu’un rôle de serveur de fichiers. 

Si vous configurez une réplication de cluster à cluster, le réplica de stockage prend entièrement en charge les serveurs de fichiers avec montée en puissance parallèle, notamment l’utilisation des espaces de stockage direct, lors de la réplication entre deux clusters.  

## <a name="FAQ8"></a>Puis-je configurer espaces de stockage direct dans un cluster étendu avec un réplica de stockage?  
Il ne s’agit pas d’une configuration prise en charge dans Windows Server. Cela peut changer dans une version ultérieure. Si vous configurez une réplication de cluster à cluster, le réplica de stockage prend entièrement en charge les serveurs de fichiers avec montée en puissance parallèle et les serveurs Hyper-V, notamment l’utilisation des espaces de stockage direct.  

## <a name="FAQ9"></a>Comment faire configurer la réplication asynchrone?  

Spécifiez `New-SRPartnership -ReplicationMode` et indiquez l’argument **Asynchronous**. Par défaut, toute la réplication dans le réplica de stockage est synchrone. Vous pouvez également modifier le mode avec `Set-SRPartnership -ReplicationMode`.  

## <a name="FAQ10"></a>Comment faire empêcher le basculement automatique d’un cluster étendu?  
Pour empêcher le basculement automatique, vous pouvez utiliser PowerShell pour configurer `Get-ClusterNode -Name "NodeName").NodeWeight=0`. Cette action supprime le vote sur chaque nœud dans le site de récupération d’urgence. Vous pouvez ensuite utiliser `Start-ClusterNode -PreventQuorum` sur les nœuds dans le site principal et `Start-ClusterNode -ForceQuorum` sur les nœuds dans le site de récupération d’urgence pour forcer le basculement. Il n’existe aucune option graphique pour empêcher le basculement automatique et cette action n’est en outre pas recommandée.  

## <a name="FAQ11"></a>Comment faire désactiver la résilience de la machine virtuelle?
Pour empêcher la nouvelle fonctionnalité de résilience des machines virtuelles Hyper-V de s’exécuter et donc de suspendre les machines virtuelles au lieu de les basculer vers le site de récupération d’urgence, exécutez`(Get-Cluster).ResiliencyDefaultPeriod=0`  

## <a name="FAQ12"></a>Comment puis-je réduire l’heure de la synchronisation initiale?

Vous pouvez utiliser un stockage alloué dynamiquement comme moyen d’accélérer les durées de synchronisation initiale. Le réplica de stockage recherche et utilise automatiquement le stockage alloué dynamiquement, notamment les espaces de stockage non cluster, les disques dynamiques Hyper-V et les LUN SAN.  

Vous pouvez également utiliser des volumes de données amorcés pour réduire l’utilisation de la bande passante et parfois le temps, en veillant à ce que le volume de destination dispose d’un sous-ensemble `New-SRPartnership`de données du serveur principal, puis à l’aide de l’option Seed dans gestionnaire du cluster de basculement ou. Si le volume est pratiquement vide, l’utilisation de la synchronisation amorcée peut réduire le temps et l’utilisation de la bande passante. Il existe plusieurs façons de amorcer des données, avec différents degrés d’efficacité:

1. Réplication précédente: en répliquant avec la synchronisation initiale normale localement entre les nœuds contenant les disques et les volumes, en supprimant la réplication, en envoyant les disques de destination ailleurs, puis en ajoutant la réplication avec l’option Seed. Il s’agit de la méthode la plus efficace, car le réplica de stockage garantissait un miroir de copie de bloc et la seule chose à répliquer est les blocs Delta.
2. Restauration d’instantané ou de sauvegarde restaurée par capture instantanée: en restaurant une capture instantanée basée sur un volume sur le volume de destination, il doit y avoir des différences minimales dans la disposition du bloc. Il s’agit de la méthode la plus efficace, car les blocs sont susceptibles de correspondre grâce aux instantanés de volume en tant qu’images miroir.
3. Fichiers copiés: en créant un volume sur la destination qui n’a jamais été utilisé auparavant et effectuant une copie de l’arborescence/MIR complète de Robocopy des données, il existe probablement des correspondances de bloc. L’utilisation de l’Explorateur de fichiers Windows ou la copie d’une partie de l’arborescence ne crée pas de correspondances de bloc. La copie manuelle de fichiers est la méthode la moins efficace pour l’amorçage.

## <a name="FAQ13"></a>Puis-je déléguer des utilisateurs pour administrer la réplication?  

Vous pouvez utiliser l' `Grant-SRDelegation` applet de commande. Cela vous permet de définir des utilisateurs spécifiques dans des scénarios de réplication de serveur à serveur, de cluster à cluster et de cluster étendu comme ayant les autorisations de créer, modifier ou supprimer la réplication, sans être membres du groupe Administrateurs local. Exemple :  

    Grant-SRDelegation -UserName contso\tonywang  

L’applet de commande vous rappelle que l’utilisateur doit se déconnecter du serveur qu’il envisage de gérer, puis se reconnecter pour que la modification prenne effet. Vous pouvez utiliser `Get-SRDelegation` et `Revoke-SRDelegation` pour mieux contrôler cette opération.  

## <a name="FAQ13"></a>Quelles sont mes options de sauvegarde et de restauration pour les volumes répliqués?
Le réplica de stockage prend en charge la sauvegarde et la restauration du volume source. Il prend également en charge la création et la restauration d’instantanés du volume source. Vous ne pouvez pas sauvegarder ni restaurer le volume de destination tant qu’il est protégé par le réplica de stockage, car il n’est pas monté ni accessible. Si un incident survient et que le volume source est perdu, l’utilisation de `Set-SRPartnership` pour promouvoir le volume de destination précédent afin qu’il devienne une source accessible en lecture/écriture vous permet de sauvegarder ou restaurer ce volume. Vous pouvez également supprimer la réplication avec `Remove-SRPartnership` et `Remove-SRGroup` pour remonter ce volume comme accessible en lecture/écriture.
Pour créer des instantanés cohérents au niveau application réguliers, vous pouvez utiliser VSSADMIN. EXE sur le serveur source pour capturer les volumes de données répliqués. Par exemple, vous répliquez le volume F: avec le réplica de stockage :

    vssadmin create shadow /for=F:
Ensuite, après avoir basculé le sens de la réplication ou supprimé la réplication, ou si vous restez toujours sur le même volume source, vous pouvez restaurer tout instantané à son point dans le temps. Par exemple, en utilisant toujours F: :

    vssadmin list shadows
     vssadmin revert shadow /shadow={shadown copy ID GUID listed previously}
Vous pouvez également prévoir une exécution régulière de cet outil à l’aide d’une tâche planifiée. Pour plus d’informations sur l’utilisation de VSS, voir [Vssadmin](../../administration/windows-commands/vssadmin.md). Il est inutile et sans intérêt de sauvegarder les volumes de journaux. Une telle tentative sera ignorée par VSS.
L’utilisation de la sauvegarde Windows Server, de la sauvegarde Microsoft Azure, de Microsoft DPM, d’un autre instantané, de VSS, de la machine virtuelle ou des technologies basées sur des fichiers est prise en charge par le réplica de stockage tant que ces outils fonctionnent au niveau de la couche du volume. Le réplica de stockage ne prend pas en charge la sauvegarde et la restauration basées sur des blocs.

## <a name="FAQ14"></a>Puis-je configurer la réplication pour limiter l’utilisation de la bande passante?
Oui, via le limiteur de bande passante SMB. Il s’agit d’un paramètre global pour tout le trafic du réplica de stockage qui, par conséquent, affecte l’ensemble de la réplication de ce serveur. En règle générale, cela est nécessaire uniquement avec la configuration de la synchronisation initiale de réplica de stockage, où toutes les données des volumes doivent être transférées. Si, après la synchronisation initiale, la bande passante réseau est trop faible pour votre charge de travail d’E/S, diminuez les E/S ou augmentez la bande passante.

Vous ne devez l’utiliser qu’avec la réplication asynchrone (notez que la synchronisation initiale est toujours asynchrone même si vous avez spécifié synchrone).
Vous pouvez également utiliser des stratégies de qualité de service réseau pour adapter le trafic du réplica de stockage. L’utilisation de la réplication de réplica de stockage amorcée à forte correspondance permet également de réduire considérablement l’utilisation de la bande passante de synchronisation initiale globale.


Pour définir la limite de bande passante, utilisez :

    Set-SmbBandwidthLimit  -Category StorageReplication -BytesPerSecond x

Pour afficher la limite de bande passante, utilisez :

    Get-SmbBandwidthLimit -Category StorageReplication

Pour supprimer la limite de bande passante, utilisez :

    Remove-SmbBandwidthLimit -Category StorageReplication
    
## <a name="FAQ15"></a>Quels sont les ports réseau requis par le réplica de stockage?
Le réplica de stockage s’appuie sur SMB et WSMAN pour sa réplication et sa gestion. Cela signifie qu'il nécessite les ports suivants :

 445 (protocole de transport de réplication SMB, protocole de gestion RPC de cluster) 5445 (iWARP SMB-uniquement nécessaire lors de l’utilisation de la mise en réseau RDMA iWARP) 5985 (WSManHTTP-Management Protocol pour WMI/CIM/PowerShell)

Remarque : L’applet de commande test-SRTopology requiert ICMPv4/ICMPv6, mais pas pour la réplication ou la gestion.

## <a name="FAQ15.5"></a>Quelles sont les meilleures pratiques en matière de volume des journaux?
La taille optimale du journal varie considérablement en fonction de l’environnement et de la charge de travail, et est déterminée par la quantité d’e/s d’écriture que votre charge de travail effectue. 

1.  Le volume du journal n'a aucune incidence sur la vitesse.
2.  Un journal plus ou moins volumineux n'a pas d'effet sur le volume des données, que celui-ci soit de 10 Go ou de 10 To, par exemple.

Un journal plus volumineux collecte et conserve simplement davantage d'E/S d'écriture avant qu’elles ne soient encapsulées. Cela permet de prolonger une interruption de service entre l’ordinateur source et de destination : par exemple, en cas de panne réseau ou si l'ordinateur de destination est hors ligne. Si le journal peut contenir 10 heures d’écriture et que le réseau tombe en panne pendant 2 heures, une fois le réseau rétabli, la source peut simplement lire le delta des modifications non synchronisées sur la destination très vite. Vous êtes donc à nouveau protégé très rapidement. Si la durée de conservation du journal est de 10 heures et que l’interruption dure 2 jours, la source doit alors relire un autre journal appelé bitmap. Il est alors probable qu'elle mette plus longtemps à se resynchroniser. Une fois synchronisée, elle revient à l'utilisation du journal.

Le réplica de stockage s’appuie sur le journal pour toutes les performances d’écriture. Les performances des journaux sont essentielles aux performances de la réplication. Vous devez vous assurer que le volume du journal soit plus performant que le volume de données, car le journal va sérialiser et séquentialiser toutes les E/S d’écriture. Vous devez toujours utiliser des supports Flash comme SSD sur les volumes de journaux. Vous ne devez jamais autoriser l’exécution d’autres charges de travail sur le volume du journal, tout comme vous ne devez jamais autoriser l’exécution d’autres charges de travail sur des volumes de journaux de base de données SQL. 

Connecter Microsoft recommande fortement que le stockage des journaux soit plus rapide que le stockage de données et que les volumes de journaux ne doivent jamais être utilisés pour d’autres charges de travail.

Vous pouvez consulter les recommandations de dimensionnement du journal en exécutant l’outil test-SRTopology. Vous pouvez également utiliser des compteurs de performances sur des serveurs existants pour juger de la taille du journal. La formule est simple: surveiller le débit du disque de données (nombre moyen d’octets écrits/s) sous la charge de travail et l’utiliser pour calculer la durée nécessaire pour remplir le journal de différentes tailles. Par exemple, le débit de disque de données de 50 Mo/s entraînera un retour à la ligne de 120 Go de 120 Go/50 Mo ou de 2400 secondes ou de 40 minutes. Par conséquent, la durée pendant laquelle le serveur de destination peut être inaccessible avant l’encapsulage du journal est de 40 minutes. Si le journal est renvoyé à la ligne, mais que la destination redevient accessible, la source rejoue les blocs via le journal de mappage de bits au lieu du journal principal. La taille du journal n’a pas d’effet sur les performances.

SEUL le disque de données du cluster source doit être sauvegardé. Les disques journaux du réplica de stockage ne doivent pas être sauvegardés, car une sauvegarde peut être en conflit avec les opérations du réplica de stockage.

## <a name="FAQ16"></a>Pourquoi choisir un cluster étendu plutôt qu’une topologie de serveur à serveur ou de cluster à serveur?  
Le réplica de stockage est disponible en trois configurations principales: le cluster étendu, le cluster à un cluster et le serveur à serveur. Chaque configuration présente différents avantages.

La topologie de cluster étendu est idéale pour les charges de travail nécessitant un basculement automatique avec orchestration, par exemple, des clusters de cloud privé Hyper-V et une instance de cluster de basculement (FCI) SQL Server. Elle présente également une interface graphique intégrée utilisant le Gestionnaire du cluster de basculement. Elle utilise l'architecture de stockage partagé en cluster asymétrique classique des espaces de stockage, SAN, iSCSI et RAID via la réservation persistante. Vous pouvez l'exécuter avec seulement 2 nœuds.

La topologie de cluster à cluster utilise deux clusters distincts. Elle est idéale pour les administrateurs qui veulent un basculement manuel, en particulier lorsque le deuxième site est configuré pour la récupération d’urgence et non pour l'utilisation quotidienne. L'orchestration est manuelle. Contrairement au cluster Stretch, les espaces de stockage direct peuvent être utilisées dans cette configuration (avec des avertissements, consultez le Forum aux questions sur le réplica de stockage et la documentation de cluster à cluster). Vous pouvez l'exécuter avec seulement quatre nœuds. 

La topologie de serveur à serveur convient parfaitement aux clients qui utilisent du matériel non compatible avec les clusters. Elle requiert une orchestration et un basculement manuels. Il est idéal pour les déploiements peu coûteux entre les succursales et les centres de données centraux, en particulier lors de l’utilisation de la réplication asynchrone. Cette configuration peut souvent remplacer des instances de serveurs de fichiers protégés par DFSR utilisées dans les scénarios de récupération d’urgence à maître unique.

Dans tous les cas, ces topologies fonctionnent aussi bien sur du matériel physique que sur des ordinateurs virtuels. Dans le cas de machines virtuelles, l’hyperviseur sous-jacent ne nécessite pas obligatoirement Hyper-V ; il peut s'agir de VMware, KVM, Xen, etc.

Le réplica de stockage dispose également d’un mode serveur unique (server-to-self), dans lequel vous dirigez la réplication vers deux volumes différents sur le même ordinateur.

## <a name="FAQ18"></a>La déduplication des données est-elle prise en charge avec le réplica de stockage?

Oui, Data Deduplcation est pris en charge avec le réplica de stockage. Activez la déduplication des données sur un volume sur le serveur source et, lors de la réplication, le serveur de destination reçoit une copie dédupliquée du volume.

Si vous devez *installer* la déduplication des données sur les serveurs source et de destination (consultez [installation et activation](../data-deduplication/install-enable.md)de la déduplication des données), il est important de ne pas *activer* la déduplication des données sur le serveur de destination. Le réplica de stockage autorise les écritures uniquement sur le serveur source. Étant donné que la déduplication des données effectue des écritures sur le volume, elle doit s’exécuter uniquement sur le serveur source. 

## <a name="FAQ19"></a>Puis-je effectuer une réplication entre Windows Server 2019 et Windows Server 2016?

Malheureusement, nous ne prenons pas en charge la création d’un *nouveau* partenariat entre windows server 2019 et windows server 2016. Vous pouvez mettre à niveau en toute sécurité un serveur ou un cluster exécutant Windows Server 2016 vers Windows Server 2019 et les partenariats *existants* continuent de fonctionner.

Toutefois, pour bénéficier des meilleures performances de réplication de Windows Server 2019, tous les membres du partenariat doivent exécuter Windows Server 2019 et vous devez supprimer les partenariats existants et les groupes de réplication associés, puis les recréer avec des données amorcées ( lors de la création du partenariat dans le centre d’administration Windows ou avec l’applet de commande New-SRPartnership).

## <a name="FAQ17"></a>Comment faire signaler un problème lié au réplica de stockage ou à ce guide?  
Pour obtenir une assistance technique sur le réplica de stockage, publiez un message sur les [forums Microsoft TechNet](https://social.technet.microsoft.com/Forums/windowsserver/en-US/home?forum=WinServerPreview). Vous pouvez également envoyer un e-mail à l’adresse srfeed@microsoft.com pour toute question sur le réplica de stockage ou tout problème lié à cette documentation. Le <https://windowsserver.uservoice.com> site est préféré pour les demandes de modification de conception, car il permet à vos collègues de fournir un support et des commentaires pour vos idées.



## <a name="related-topics"></a>Rubriques connexes  
- [Vue d’ensemble du réplica de stockage](storage-replica-overview.md) 
- [Réplication de cluster étendu à l’aide d’un stockage partagé](stretch-cluster-replication-using-shared-storage.md)  
- [Réplication de stockage de serveur à serveur](server-to-server-storage-replication.md)  
- [Réplication de stockage de cluster à cluster](cluster-to-cluster-storage-replication.md)  
- [Réplica de stockage : Problèmes connus](storage-replica-known-issues.md)  

## <a name="see-also"></a>Voir aussi  
- [Vue d’ensemble du stockage](../storage.md)  
- [Espaces de stockage direct](../storage-spaces/storage-spaces-direct-overview.md)  
