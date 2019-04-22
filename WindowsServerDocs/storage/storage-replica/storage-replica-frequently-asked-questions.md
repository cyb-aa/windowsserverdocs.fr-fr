---
title: Forum Aux Questions sur le réplica de stockage
ms.prod: windows-server-threshold
manager: siroy
ms.author: nedpyle
ms.technology: storage-replica
ms.topic: get-started-article
author: nedpyle
ms.date: 12/19/2018
ms.assetid: 12bc8e11-d63c-4aef-8129-f92324b2bf1b
ms.openlocfilehash: 0e010f0319b46e04cf9aa15cde9552af1191ab22
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824710"
---
# <a name="frequently-asked-questions-about-storage-replica"></a>Forum Aux Questions sur le réplica de stockage

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique contient des réponses aux questions fréquemment posées sur le réplica de stockage.

## <a name="FAQ1"></a> Le réplica de stockage est pris en charge sur Azure ?  
Oui. Vous pouvez utiliser les scénarios suivants avec Azure :

1. Réplication de serveur à serveur dans Azure (synchrone ou asynchrone entre les machines virtuelles IaaS dans un ou deux domaines d’erreur de centre de données ou asynchrone entre deux régions distinctes)
2. Réplication asynchrone de serveur à serveur entre Azure et en local (à l’aide de VPN ou Azure ExpressRoute)
3. Réplication de cluster à cluster à l’intérieur de Azure (synchrone ou asynchrone entre les machines virtuelles IaaS dans un ou deux domaines d’erreur de centre de données ou asynchrone entre deux régions distinctes)
4. Réplication asynchrone de cluster à cluster entre Azure et en local (à l’aide de VPN ou Azure ExpressRoute)

Vous trouverez des notes supplémentaires sur le clustering invité dans Azure à : [Déploiement de Clusters invités de machine virtuelle IaaS dans Microsoft Azure](https://blogs.msdn.microsoft.com/clustering/2017/02/14/deploying-an-iaas-vm-guest-clusters-in-microsoft-azure/).

Remarques importantes :

1. Azure ne prend pas en charge le clustering, invité VHDX partagé afin de machines virtuelles de Cluster de basculement Windows doit utiliser les cibles iSCSI pour le clustering de réservation classique disque persistant de stockage partagé ou espaces de stockage Direct.
2. Il existe des modèles Azure Resource Manager pour le clustering de réplica de stockage basé sur les espaces de stockage Direct à [créer un cluster SOFS espaces de stockage Direct (S2D) avec le réplica de stockage pour la récupération d’urgence dans les régions Azure](https://aka.ms/azure-storage-replica-cluster).  
3. Cluster à la communication RPC de cluster dans Azure (requis par l’API de cluster pour accorder l’accès entre le cluster) requiert la configuration de l’accès réseau pour le CNO. Vous devez autoriser le port TCP 135 et la plage dynamique au-dessus du port TCP 49152. Référence [construction Windows Server Failover Cluster sur la machine virtuelle Azure IAAS – partie 2 de réseau et la création de](https://blogs.technet.microsoft.com/askcore/2015/06/24/building-windows-server-failover-cluster-on-azure-iaas-vm-part-2-network-and-creation/).  
4. Il est possible d’utiliser des clusters invités de deux nœuds, où chaque nœud est à l’aide de bouclage iSCSI pour un cluster asymétrique répliqué par le réplica de stockage. Mais cela aura probablement des performances très médiocres et doit être utilisé uniquement pour le test ou de charges de travail très limités.  

## <a name="FAQ2"></a> Comment pour voir la progression de la réplication pendant la synchronisation initiale ?  
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

## <a name="FAQ3"></a>Puis-je spécifier des interfaces réseau spécifiques à utiliser pour la réplication ?  

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

    Set-SRNetworkConstraint -SourceComputerName sr-srv01 -SourceRGName group1 -SourceNWInterface "Cluster Network 1","Cluster Network 2" -DestinationComputerName sr-srv03 -DestinationRGName group2 -DestinationNWInterface "Cluster Network 1","Cluster Network 2"  

## <a name="FAQ4"></a> Puis-je configurer une réplication un-à-plusieurs ou transitive (de A à B à C) ?  
Pas dans Windows Server 2016. Cette version prend uniquement en charge la réplication un-à-un d’un serveur, cluster ou nœud de cluster étendu. Cela peut changer dans une version ultérieure. Vous pouvez bien sûr configurer la réplication entre différents serveurs d’une paire de volumes spécifique, dans les deux sens. Par exemple, le serveur 1 peut répliquer son volume D sur le serveur 2 et son volume E à partir du serveur 3.

## <a name="FAQ5"></a> Puis-je augmenter ou réduire les volumes répliqués par le réplica de stockage ?  
Vous pouvez développer (augmenter) des volumes, mais pas les réduire. Par défaut, le réplica de stockage empêche les administrateurs de développer les volumes répliqués ; utilisez l'option `Set-SRGroup -AllowVolumeResize $TRUE` sur le groupe source avant le redimensionnement. Exemple :

1. Utiliser sur l’ordinateur source : `Set-SRGroup -Name YourRG -AllowVolumeResize $TRUE`
2. Augmentez le volume en utilisant la technique de votre choix.
3. Utiliser sur l’ordinateur source : `Set-SRGroup -Name YourRG -AllowVolumeResize $FALSE` 

## <a name="FAQ6"></a>Puis-je mettre en ligne un volume de destination pour l’accès en lecture seule ?  
Pas dans Windows Server 2016 RTM, également appelé version « RS1 ». Le réplica de stockage démonte le volume de destination au début de la réplication. 

Toutefois, dans Windows Server, version 1709, il est désormais possible de monter le stockage de destination ; cette fonctionnalité est appelée « Test de basculement ». Pour ce faire, vous devez disposer d’un volume au format NTFS ou ReFS inutilisé qui ne réplique pas sur la destination actuellement. Ensuite, vous pouvez monter une capture instantanée du stockage répliqué temporairement à des fins de test ou de sauvegarde. 

Par exemple, pour créer un test de basculement dans lequel vous répliquez un volume « D : » dans le groupe de réplication « RG2 » sur le serveur de destination « SRV2 », avec un lecteur « T : » sur SRV2 qui n’est pas répliqué :

 `Mount-SRDestination -Name RG2 -Computername SRV2 -TemporaryPath T:\`
 
Le volume D : répliqué est désormais accessible sur SRV2. Vous pouvez lire et écrire dedans normalement, copier des fichiers dessus, ou exécuter une sauvegarde en ligne que vous enregistrez un autre emplacement pour conserver en lieu sûr, sous le chemin d’accès D:. T: volume contiendra uniquement les données de journal.

Pour supprimer l’instantané de test de basculement et supprimer les modifications :

 `Dismount-SRDestination -Name RG2 -Computername SRV2`
 
Vous devez uniquement utiliser la fonctionnalité de test de basculement pour des opérations temporaires à court terme. Elle n’est pas destinée à être utilisée à long terme. Lors de l’utilisation, la réplication continue sur le volume de destination réel. 

## <a name="FAQ7"></a> Puis-je configurer le serveur de fichiers avec montée en puissance (SOFS) dans un cluster étendu ?  
Bien que techniquement possible, cette configuration n’est pas recommandée dans Windows Server 2016, en raison du manque de reconnaissance des sites dans les nœuds de calcul qui contactent le serveur de fichiers avec montée en puissance parallèle. Si vous utilisez distance campus de mise en réseau, où les temps de latence sont généralement inférieure à la milliseconde, cette configuration fonctionne généralement sans problème.   

Si vous configurez une réplication de cluster à cluster, le réplica de stockage prend entièrement en charge les serveurs de fichiers avec montée en puissance parallèle, notamment l’utilisation des espaces de stockage direct, lors de la réplication entre deux clusters.  

## <a name="FAQ7.5"></a> CSV nécessaire à la réplication dans un cluster étendu ou entre les clusters ?  
Non. Vous pouvez répliquer avec CSV ou de réservation de disque persistant (disque physique) appartenant à une ressource de cluster, comme un rôle de serveur de fichiers. 

Si vous configurez une réplication de cluster à cluster, le réplica de stockage prend entièrement en charge les serveurs de fichiers avec montée en puissance parallèle, notamment l’utilisation des espaces de stockage direct, lors de la réplication entre deux clusters.  

## <a name="FAQ8"></a>Configurer les espaces de stockage Direct dans un cluster étendu avec le réplica de stockage ?  
Cette configuration n’est pas prise en charge dans Windows Server 2016.  Cela peut changer dans une version ultérieure. Si vous configurez une réplication de cluster à cluster, le réplica de stockage prend entièrement en charge les serveurs de fichiers avec montée en puissance parallèle et les serveurs Hyper-V, notamment l’utilisation des espaces de stockage direct.  

## <a name="FAQ9"></a>Comment configurer la réplication asynchrone ?  

Spécifiez `New-SRPartnership -ReplicationMode` et indiquez l’argument **Asynchronous**. Par défaut, toute la réplication dans le réplica de stockage est synchrone. Vous pouvez également modifier le mode avec `Set-SRPartnership -ReplicationMode`.  

## <a name="FAQ10"></a>Comment empêcher le basculement automatique d’un cluster étendu ?  
Pour empêcher le basculement automatique, vous pouvez utiliser PowerShell pour configurer `Get-ClusterNode -Name "NodeName").NodeWeight=0`. Cette action supprime le vote sur chaque nœud dans le site de récupération d’urgence. Vous pouvez ensuite utiliser `Start-ClusterNode -PreventQuorum` sur les nœuds dans le site principal et `Start-ClusterNode -ForceQuorum` sur les nœuds dans le site de récupération d’urgence pour forcer le basculement. Il n’existe aucune option graphique pour empêcher le basculement automatique et cette action n’est en outre pas recommandée.  

## <a name="FAQ11"></a>Comment désactiver la résilience de machine virtuelle ?  
Pour éviter que la fonctionnalité de résilience de machine virtuelle Hyper-V à partir d’en cours d’exécution et par conséquent suspension des machines virtuelles au lieu de les basculer vers le site de récupération d’urgence, exécutez `(Get-Cluster).ResiliencyDefaultPeriod=0`  

## <a name="FAQ12"></a> Comment puis-je réduire le temps pour la synchronisation initiale ?  

Vous pouvez utiliser un stockage alloué dynamiquement comme moyen d’accélérer les durées de synchronisation initiale. Le réplica de stockage recherche et utilise automatiquement le stockage alloué dynamiquement, notamment les espaces de stockage non cluster, les disques dynamiques Hyper-V et les LUN SAN.  

Vous pouvez également utiliser des volumes de données amorcés pour réduire l’utilisation de la bande passante et parfois de temps, en s’assurant que la destination volume a un sous-ensemble des données depuis le serveur principal puis à l’aide de l’option amorcé du Gestionnaire de Cluster de basculement ou `New-SRPartnership`. Si le volume est pratiquement vide, l’utilisation de la synchronisation amorcée peut réduire le temps et l’utilisation de la bande passante. Il existe plusieurs façons de données d’amorçage, avec un certain degré d’efficacité :

1. Réplication précédente - en répliquant avec initiale normal synchroniser localement entre les nœuds contenant les disques et volumes, suppression de la réplication, les disques de destination de livraison un autre emplacement, puis en ajoutant la réplication avec l’option amorcée. Il s’agit de la méthode la plus efficace que le réplica de stockage garanti un miroir de la copie de bloc et la seule chose à répliquer est blocs de delta.
2. Instantané de la restauration ou restauration basée sur un instantané de sauvegarde - en restaurant un instantané en fonction du volume sur le volume de destination, il doit y avoir grande différence dans la disposition de bloc. Il s’agit de la méthode la plus efficace suivante comme les blocs sont susceptibles de correspondre à grâce aux clichés instantanés de volume en cours des images miroirs.
3. Les fichiers copiés - création d’un volume de destination n’a jamais été utilisée et l’exécution d’une arborescence /MIR robocopy complète copier des données, sont susceptibles d’être des correspondances de bloc. L’Explorateur de fichiers Windows ou la copie d’une partie de l’arborescence ne crée pas de correspondances de bloc. Copie manuelle de fichiers est la méthode la moins efficace de l’amorçage.



## <a name="FAQ13"></a> Puis-je déléguer des utilisateurs pour gérer la réplication ?  

Vous pouvez utiliser l’applet de commande `Grant-SRDelegation` dans Windows Server 2016. Cela vous permet de définir des utilisateurs spécifiques dans des scénarios de réplication de serveur à serveur, de cluster à cluster et de cluster étendu comme ayant les autorisations de créer, modifier ou supprimer la réplication, sans être membres du groupe Administrateurs local. Exemple :  

    Grant-SRDelegation -UserName contso\tonywang  

L’applet de commande vous rappelle que l’utilisateur doit se déconnecter du serveur qu’il envisage de gérer, puis se reconnecter pour que la modification prenne effet. Vous pouvez utiliser `Get-SRDelegation` et `Revoke-SRDelegation` pour mieux contrôler cette opération.  

## <a name="FAQ13"></a> Les ma sauvegarde et options de restauration des volumes répliqués ?
Le réplica de stockage prend en charge la sauvegarde et la restauration du volume source. Il prend également en charge la création et la restauration d’instantanés du volume source. Vous ne pouvez pas sauvegarder ni restaurer le volume de destination tant qu’il est protégé par le réplica de stockage, car il n’est pas monté ni accessible. Si un incident survient et que le volume source est perdu, l’utilisation de `Set-SRPartnership` pour promouvoir le volume de destination précédent afin qu’il devienne une source accessible en lecture/écriture vous permet de sauvegarder ou restaurer ce volume. Vous pouvez également supprimer la réplication avec `Remove-SRPartnership` et `Remove-SRGroup` pour remonter ce volume comme accessible en lecture/écriture.
Pour créer des instantanés cohérents au niveau application réguliers, vous pouvez utiliser VSSADMIN. EXE sur le serveur source pour capturer les volumes de données répliqués. Par exemple, vous répliquez le volume F: avec le réplica de stockage :

    vssadmin create shadow /for=F:
Ensuite, après avoir basculé le sens de la réplication ou supprimé la réplication, ou si vous restez toujours sur le même volume source, vous pouvez restaurer tout instantané à son point dans le temps. Par exemple, en utilisant toujours F: :

    vssadmin list shadows
     vssadmin revert shadow /shadow={shadown copy ID GUID listed previously}
Vous pouvez également prévoir une exécution régulière de cet outil à l’aide d’une tâche planifiée. Pour plus d’informations sur l’utilisation de VSS, voir [Vssadmin](https://technet.microsoft.com/library/cc754968.aspx). Il est inutile et sans intérêt de sauvegarder les volumes de journaux. Une telle tentative sera ignorée par VSS.
L’utilisation de la sauvegarde Windows Server, de la sauvegarde Microsoft Azure, de Microsoft DPM, d’un autre instantané, de VSS, de la machine virtuelle ou des technologies basées sur des fichiers est prise en charge par le réplica de stockage tant que ces outils fonctionnent au niveau de la couche du volume. Le réplica de stockage ne prend pas en charge la sauvegarde et la restauration basées sur des blocs.

## <a name="FAQ14"></a> Puis-je configurer la réplication pour restreindre l’utilisation de la bande passante ?
Oui, via le limiteur de bande passante SMB. Il s’agit d’un paramètre global pour tout le trafic du réplica de stockage qui, par conséquent, affecte l’ensemble de la réplication de ce serveur. En règle générale, cela est nécessaire uniquement avec la configuration de la synchronisation initiale de réplica de stockage, où toutes les données des volumes doivent être transférées. Si, après la synchronisation initiale, la bande passante réseau est trop faible pour votre charge de travail d’E/S, diminuez les E/S ou augmentez la bande passante.

Vous ne devez l’utiliser qu’avec la réplication asynchrone (notez que la synchronisation initiale est toujours asynchrone même si vous avez spécifié synchrone).
Vous pouvez également utiliser des stratégies de qualité de service réseau pour adapter le trafic du réplica de stockage. L’utilisation de la réplication de réplica de stockage amorcée à forte correspondance permet également de réduire considérablement l’utilisation de la bande passante de synchronisation initiale globale.


Pour définir la limite de bande passante, utilisez :

    Set-SmbBandwidthLimit  -Category StorageReplication -BytesPerSecond x

Pour afficher la limite de bande passante, utilisez :

    Get-SmbBandwidthLimit -Category StorageReplication

Pour supprimer la limite de bande passante, utilisez :

    Remove-SmbBandwidthLimit -Category StorageReplication
    
## <a name="FAQ15"></a>Quels ports réseau réplica de stockage est-elle nécessaire ?
Le réplica de stockage s’appuie sur SMB et WSMAN pour sa réplication et sa gestion. Cela signifie qu'il nécessite les ports suivants :

 445 (SMB - protocole de transport de réplication, le protocole de gestion de cluster RPC) 5445 (SMB - nécessaire uniquement lorsque la mise en réseau iWARP RDMA iWARP) 5985 (WSManHTTP - protocole de gestion pour WMI/CIM/PowerShell)

Remarque: L’applet de commande Test-SRTopology nécessite ICMPv4/ICMPv6, mais pas pour la réplication ou de gestion.

## <a name="FAQ15.5"></a>Quelles sont les pratiques recommandées de volume de journal ?
La taille optimale du journal varie considérablement par l’environnement et de la charge de travail et est déterminée par la quantité écriture d’e/s exécute votre charge de travail. 

1.  Le volume du journal n'a aucune incidence sur la vitesse.
2.  Un journal plus ou moins volumineux n'a pas d'effet sur le volume des données, que celui-ci soit de 10 Go ou de 10 To, par exemple.

Un journal plus volumineux collecte et conserve simplement davantage d'E/S d'écriture avant qu’elles ne soient encapsulées. Cela permet de prolonger une interruption de service entre l’ordinateur source et de destination : par exemple, en cas de panne réseau ou si l'ordinateur de destination est hors ligne. Si le journal peut contenir 10 heures d’écriture et que le réseau tombe en panne pendant 2 heures, une fois le réseau rétabli, la source peut simplement lire le delta des modifications non synchronisées sur la destination très vite. Vous êtes donc à nouveau protégé très rapidement. Si la durée de conservation du journal est de 10 heures et que l’interruption dure 2 jours, la source doit alors relire un autre journal appelé bitmap. Il est alors probable qu'elle mette plus longtemps à se resynchroniser. Une fois synchronisée, elle revient à l'utilisation du journal.

Le réplica de stockage s’appuie sur le journal pour toutes les performances d’écriture. Les performances des journaux sont essentielles aux performances de la réplication. Vous devez vous assurer que le volume du journal soit plus performant que le volume de données, car le journal va sérialiser et séquentialiser toutes les E/S d’écriture. Vous devez toujours utiliser des supports Flash comme SSD sur les volumes de journaux. Vous ne devez jamais autoriser l’exécution d’autres charges de travail sur le volume du journal, tout comme vous ne devez jamais autoriser l’exécution d’autres charges de travail sur des volumes de journaux de base de données SQL. 

À nouveau : Microsoft recommande vivement que le stockage des journaux plus rapide que le stockage de données et que les volumes de journaux ne doivent jamais être utilisés pour les autres charges de travail.

Vous pouvez obtenir des recommandations de dimensionnement du journal en exécutant l’outil de Test-SRTopology. Ou bien, vous pouvez utiliser les compteurs de performances sur les serveurs existants pour faire une opinion de taille de journal. La formule est simple : surveiller le débit de disque de données (moyenne écrire des octets/s) sous la charge de travail et l’utiliser pour calculer la quantité de temps nécessaire pour saturer le journal de différentes tailles. Par exemple, débit de disque de données de 50 Mo/s provoquent le journal de 120 Go à la ligne dans les secondes de 120 Go/50 Mo ou 2400 secondes ou les 40 minutes. Par conséquent, la durée pendant laquelle le serveur de destination peut être inaccessible avant le journal encapsulé est 40 minutes. Si le journal encapsule mais la destination devient accessible à nouveau, la source serait relire les blocs via le journal de carte de bits au lieu du journal principal. La taille du journal n’a pas d’effet sur les performances.

Le disque de données à partir du cluster Source doit être sauvegardé. Les disques de journal de réplica de stockage ne doivent pas être sauvegardées dans la mesure où une sauvegarde peut entrer en conflit avec les opérations de réplica de stockage.

## <a name="FAQ16"></a> Pourquoi choisir un cluster étendu par rapport à cluster à cluster par rapport à la topologie de serveur à serveur ?  
Le réplica de stockage est disponible en trois configurations principales : cluster étendu, cluster à cluster et serveur à serveur. Chaque configuration présente différents avantages.

La topologie de cluster étendu est idéale pour les charges de travail nécessitant un basculement automatique avec orchestration, par exemple, des clusters de cloud privé Hyper-V et une instance de cluster de basculement (FCI) SQL Server. Elle présente également une interface graphique intégrée utilisant le Gestionnaire du cluster de basculement. Elle utilise l'architecture de stockage partagé en cluster asymétrique classique des espaces de stockage, SAN, iSCSI et RAID via la réservation persistante. Vous pouvez l'exécuter avec seulement 2 nœuds.

La topologie de cluster à cluster utilise deux clusters distincts. Elle est idéale pour les administrateurs qui veulent un basculement manuel, en particulier lorsque le deuxième site est configuré pour la récupération d’urgence et non pour l'utilisation quotidienne. L'orchestration est manuelle. Contrairement à un cluster étendu, espaces de stockage Direct peut servir dans cette configuration (avec les mises en garde - consultez le Forum aux questions sur le réplica de stockage et la documentation du cluster à cluster). Vous pouvez l'exécuter avec seulement quatre nœuds. 

La topologie de serveur à serveur convient parfaitement aux clients qui utilisent du matériel non compatible avec les clusters. Elle requiert une orchestration et un basculement manuels. Elle est parfaite pour les déploiements peu coûteux entre des filiales et des centres de données centralisés, en particulier lorsque vous utilisez la réplication asynchrone. Cette configuration peut souvent remplacer des instances de serveurs de fichiers protégés par DFSR utilisées dans les scénarios de récupération d’urgence à maître unique.

Dans tous les cas, ces topologies fonctionnent aussi bien sur du matériel physique que sur des ordinateurs virtuels. Dans le cas de machines virtuelles, l’hyperviseur sous-jacent ne nécessite pas obligatoirement Hyper-V ; il peut s'agir de VMware, KVM, Xen, etc.

Le réplica de stockage dispose également d’un mode serveur unique (server-to-self), dans lequel vous dirigez la réplication vers deux volumes différents sur le même ordinateur.

## <a name="FAQ18"></a> La déduplication des données est prise en charge avec le réplica de stockage ?

Oui, les données Deduplcation est pris en charge avec le réplica de stockage. Activer la déduplication des données sur un volume sur le serveur source, et lors de la réplication du serveur de destination reçoit une copie dédupliquée du volume.

Bien que vous devez *installer* la déduplication des données sur les serveurs source et de destination (consultez [installation et l’activation de la déduplication des données](../data-deduplication/install-enable.md)), il est important pas à *activer*La déduplication des données sur le serveur de destination. Réplica de stockage permet des écritures uniquement sur le serveur source. Étant donné que la déduplication des données effectue des écritures sur le volume, il doit s’exécuter uniquement sur le serveur source. 

## <a name="FAQ17"></a> Comment signaler un problème avec le réplica de stockage ou de ce guide ?  
Pour obtenir une assistance technique sur le réplica de stockage, publiez un message sur les [forums Microsoft TechNet](https://social.technet.microsoft.com/Forums/windowsserver/en-US/home?forum=WinServerPreview). Vous pouvez également envoyer un e-mail à l’adresse srfeed@microsoft.com pour toute question sur le réplica de stockage ou tout problème lié à cette documentation. Le https://windowsserver.uservoice.com site est préféré pour les demandes de modification de conception, car il permet à vos clients fournir des commentaires et support pour vos idées.



## <a name="related-topics"></a>Rubriques connexes  
- [Vue d’ensemble du réplica de stockage](storage-replica-overview.md) 
- [Réplication de Cluster étendu à l’aide d’un stockage partagé](stretch-cluster-replication-using-shared-storage.md)  
- [Réplication du stockage de serveur à serveur](server-to-server-storage-replication.md)  
- [Réplication du stockage de cluster à Cluster](cluster-to-cluster-storage-replication.md)  
- [Réplica de stockage : Problèmes connus](storage-replica-known-issues.md)  

## <a name="see-also"></a>Voir aussi  
- [Vue d’ensemble du stockage](../storage.md)  
- [Espaces de stockage Direct dans Windows Server 2016](../storage-spaces/storage-spaces-direct-overview.md)  
