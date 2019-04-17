---
title: Utiliser des Volumes partagés de Cluster dans un cluster de basculement
description: L’utilisation des Volumes partagés de Cluster dans un cluster de basculement.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: f5bd0ad05bdc2573a5ea0abbe165de2d3e7f5c8f
ms.sourcegitcommit: 375e94dc70c76e7aa5679c32f0f4dbc26cf7dd83
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "2233515"
---
# <a name="use-cluster-shared-volumes-in-a-failover-cluster"></a>Utiliser des Volumes partagés de Cluster dans un cluster de basculement

>S’applique à: Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

Cluster partagés Volumes (CSV) activer plusieurs nœuds dans un cluster de basculement à simultanément accéder en lecture-écriture à la même LUN (disque) est en tant qu’un volume NTFS. (Dans Windows Server 2012 R2, le disque peut être mis en service en tant que NTFS ou le système de fichiers résilient (ReFS).) Avec CSV, rôles en cluster peuvent basculer rapidement d’un nœud à un autre nœud sans nécessiter un changement de propriété de lecteur, ou démontage et remontage un volume. CSV également de simplifier la gestion d’un grand nombre de LUN dans un cluster de basculement.

CSV fournissent un système de fichiers à usage général, en cluster, ce qui se superpose NTFS (ou ReFS dans Windows Server 2012 R2). Applications CSV sont les suivantes:

- Cluster de fichiers de disque dur virtuel (VHD) pour les machines virtuelles en cluster Hyper-V
- Partages de fichiers de la montée en puissance parallèle pour stocker les données d’application pour le rôle de cluster de serveur de fichiers montée en puissance parallèle. Fichiers d’ordinateur virtuel Hyper-V et de données Microsoft SQL Server sont des exemples des données d’application pour ce rôle. (Sachez que ReFS n’est pas pris en charge pour un serveur de fichiers montée en puissance parallèle). Pour plus d’informations sur le serveur de fichiers de mise à niveau, voir [Serveur de fichiers de mise à niveau pour les données d’Application](sofs-overview.md).

>[!NOTE]
>CSV ne prend pas en charge la charge de travail en cluster Microsoft SQL Server dans SQL Server 2012 et les versions antérieures de SQL Server.

Dans Windows Server 2012, la fonctionnalité CSV a été considérablement améliorée. Par exemple, les dépendances sur les Services de domaine Active Directory ont été supprimées. Prise en charge a été ajoutée pour l’intégration aux fonctionnalités de stockage global tels que des volumes chiffrés BitLocker et les espaces de stockage pour les améliorations fonctionnelles de **chkdsk**et pour l’interopérabilité avec les applications de sauvegarde et antivirus. Pour une vue d’ensemble des fonctionnalités CSV qui a été introduite dans Windows Server 2012, consultez la rubrique [Nouveautés dans le clustering avec basculement de Windows Server 2012 \[redirected\]](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265972(v%3dws.11)>).

Windows Server 2012 R2 présente des fonctionnalités supplémentaires, telles que distribuée propriétaire de résilience accrue grâce à la disponibilité du service de serveur, une plus grande souplesse dans la quantité de mémoire physique que vous pouvez allouer au cache CSV, mieux, CSV interopérabilité améliorée prenant en charge ReFS et déduplication et diagnosibility. Pour plus d’informations, voir [Nouveautés de Clustering de basculement](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265972(v%3dws.11)>).

>[!NOTE]
>Pour plus d’informations sur l’utilisation de la déduplication des données sur CSV pour les scénarios d’Infrastructure VDI (Virtual Desktop), voir que le blog de billets [de déduplication des données de déploiement pour le stockage VDI dans Windows Server 2012 R2](https://blogs.technet.com/b/filecab/archive/2013/07/31/deploying-data-deduplication-for-vdi-storage-in-windows-server-2012-r2.aspx) et [Extending déduplication à nouveau charges de travail Windows Server 2012 R2](https://blogs.technet.com/b/filecab/archive/2013/07/31/extending-data-deduplication-to-new-workloads-in-windows-server-2012-r2.aspx).

## <a name="review-requirements-and-considerations-for-using-csv-in-a-failover-cluster"></a>Passer en revue les exigences et les considérations liées à l’aide de CSV dans un cluster de basculement

Avant d’utiliser CSV dans un cluster de basculement, passez en revue le réseau, de stockage et autres exigences et les considérations en matière de cette section.

### <a name="network-configuration-considerations"></a>Considérations relatives à la configuration réseau

Considérez les points suivants lorsque vous configurez les réseaux qui prennent en charge CSV.

- **Plusieurs réseaux et plusieurs cartes réseau**. Pour activer la tolérance de panne en cas de défaillance du réseau, nous vous recommandons que plusieurs réseaux de cluster CSV trafic ou que vous configurez collaboré cartes réseau.
    
    Si les nœuds du cluster sont connectés à des réseaux qui ne doivent pas être utilisés par le cluster, vous devez désactiver les. Par exemple, nous vous recommandons de désactiver les réseaux iSCSI pour utiliser le cluster afin d’empêcher le trafic CSV sur les réseaux. Pour désactiver un réseau, dans le Gestionnaire de Cluster de basculement, sélectionnez **réseaux**, sélectionnez le réseau, sélectionnez l’action de **Propriétés** , puis sélectionnez **ne pas autoriser la communication réseau de cluster sur ce réseau**. Autrement, vous pouvez configurer la propriété **rôle** du réseau à l’aide de l’applet de commande [Get-ClusterNetwork](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusternetwork?view=win10-ps) Windows PowerShell.
- **Propriétés de la carte réseau**. Dans les propriétés de toutes les cartes comportant la communication du cluster, assurez-vous que les paramètres suivants sont activés:

  - **Client pour les réseaux Microsoft** et le **fichier de partage et d’imprimantes pour les réseaux Microsoft**. Ces paramètres prennent en charge SMB Server Message Block () 3.0, qui est utilisé par défaut pour le trafic CSV entre les nœuds. Pour activer SMB, vérifiez également que le service du serveur et la station de travail sont en cours d’exécution et qu’ils sont configurés pour démarrer automatiquement sur chaque nœud du cluster.

    >[!NOTE]
    >Dans Windows Server 2012 R2, il existe plusieurs instances du service de serveur par le nœud de cluster de basculement. Il est l’instance par défaut qui gère le trafic entrant à partir de clients SMB qui accèdent aux partages de fichiers régulière et une deuxième instance CSV qui gère uniquement le trafic entre des batteries nœud CSV. En outre, si le service serveur sur un nœud est défectueux, possession CSV passe automatiquement à un autre nœud.

    SMB 3.0 inclut les fonctionnalités de SMB multicanal et SMB Direct, qui permettent le trafic CSV pour les flux de données sur plusieurs réseaux du cluster et optimiser les cartes réseau qui prennent en charge l’accès de mémoire Direct à distance (RDMA). Par défaut, SMB multicanal est utilisé pour le trafic CSV. Pour plus d’informations, voir [vue d’ensemble de Server Message Block](../storage/file-server/file-server-smb-overview.md).
  - **Filtre de performances de Cluster de basculement Microsoft carte virtuelle**. Ce paramètre améliore la capacité de nœuds pour effectuer la redirection d’e/s, lorsque cela est nécessaire pour atteindre CSV, par exemple, lorsqu’un échec de connectivité empêche un nœud de connexion directement sur le disque CSV. Pour plus d’informations, voir [e/s sur la synchronisation et la redirection d’e/s en communication CSV](#about-i/o-synchronization-and-i/o-redirection-in-csv-communication) plus loin dans cette rubrique.
- **Définition des priorités du réseau de cluster**. Il est généralement recommandé que vous ne modifiez pas les préférences de cluster configuré pour les réseaux.
- **Configuration de sous-réseau IP**. Aucune configuration spécifique de sous-réseau n’est requise pour les nœuds dans un réseau qui utilisent CSV. CSV peut prendre en charge les clusters composé de plusieurs sous-réseaux.
- **Qualité de Service (QoS) basée sur la stratégie**. Nous vous recommandons de configurer une stratégie de QoS priorité et une stratégie de bande passante minimale requise pour le trafic réseau pour chaque nœud lorsque vous utilisez CSV. Pour plus d’informations, consultez [Qualité de Service (QoS)](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831679(v%3dws.11)>).
- **Réseau de stockage**. Pour obtenir des recommandations stockage réseau, passez en revue les instructions fournies par votre fournisseur de stockage. Pour des considérations supplémentaires à propos du stockage pour CSV, consultez [configuration de stockage et de disque](#storage-and-disk-configuration-requirements) plus loin dans cette rubrique.

Pour une vue d’ensemble du matériel, le réseau et les exigences de stockage pour les clusters de basculement, voir [matérielle clustering avec basculement et les Options de stockage](clustering-requirements.md).

#### <a name="about-io-synchronization-and-io-redirection-in-csv-communication"></a>À propos de la synchronisation d’e/s et la redirection d’e/s en communication CSV

- **Synchronisation d’e/s**: CSV permet à plusieurs nœuds d’accéder simultanément en lecture-écriture pour le stockage partagé même. Lorsqu’un nœud effectue d’entrée/sortie (e/s) de disque sur un volume CSV, le nœud communique directement avec le stockage, par exemple, via un réseau de stockage (SAN). Toutefois, à tout moment, un seul nœud (appelé le nœud coordinateur) «propriétaire» la ressource de disque physique qui est associée au LUN. Le nœud coordinateur pour un volume CSV est affiché dans le Gestionnaire de Cluster de basculement en tant que **Propriétaire du nœud** sous **disques**. Elle apparaît également dans la sortie de l’applet de commande [Get-ClusterSharedVolume](https://docs.microsoft.com/powershell/module/failoverclusters/get-clustersharedvolume?view=win10-ps) Windows PowerShell.

  >[!NOTE]
  >Dans Windows Server 2012 R2, la propriété CSV est réparti uniformément entre les nœuds du cluster de basculement en fonction du nombre de volumes CSV qui appartiennent à chaque nœud. En outre, la propriété est automatiquement rééquilibrage lorsque les conditions, telles que le basculement CSV, un nœud rejoint le cluster, vous ajoutez un nouveau nœud au cluster, vous redémarrez un nœud de cluster, ou vous démarrez le cluster de basculement après que qu’il a été arrêté.

  Lorsque certaines modifications se produisent dans le système de fichiers sur un volume CSV, celles-ci doit être synchronisée sur chacun des nœuds physiques qui accèdent à la LUN, non seulement sur le nœud coordinateur unique. Par exemple, lorsqu’un ordinateur virtuel sur un volume CSV est démarré, créé ou supprimé, ou lorsque vous migrez un ordinateur virtuel, ces informations doivent être synchronisées sur chacun des nœuds physiques qui accèdent à l’ordinateur virtuel. Ces opérations de mise à jour de métadonnées se produisent en parallèle sur les réseaux de cluster à l’aide de SMB 3.0. Ces opérations ne nécessitent pas tous les nœuds physiques pour communiquer avec le stockage partagé.

- **La redirection d’e/s**: certaines opérations de stockage et les défaillances de stockage peuvent empêcher un nœud donné de communiquer directement avec le stockage. Pour maintenir la fonction alors que le nœud n’est pas communiquer avec le stockage, le nœud redirige l’e/s disque via un réseau de cluster vers le nœud coordinateur où le disque est actuellement monté. Si le nœud actuel coordinateur rencontre un échec de connectivité de stockage, toutes les opérations d’e/s disque sont en file d’attente momentanément un nouveau nœud est établi en tant que coordinateur.

Le serveur utilise l’un des modes de redirection e/s suivants, selon la situation:

- **Redirection du système de fichiers** La redirection est par volume — par exemple, lorsque CSV des instantanés par une application de sauvegarde lorsqu’un volume CSV est placé manuellement en mode d’e/s redirigé.
- **Redirection de bloc** La redirection est au niveau de blocage des fichiers — par exemple, lorsque le connectivité du stockage est perdue à un volume. Bloquer la redirection est beaucoup plus rapide que la redirection du système de fichiers.

Dans Windows Server 2012 R2, vous pouvez afficher l’état d’un volume CSV par nœud. Par exemple, vous pouvez voir si les e/s est direct ou redirigé, ou si le volume CSV n’est pas disponible. Si un volume CSV est en mode de redirection e/s, vous pouvez également afficher la raison. Utilisez l’applet de commande Windows PowerShell, **Get-ClusterSharedVolumeState** pour afficher ces informations.

>[!NOTE]
> * Dans Windows Server 2012, en raison des améliorations dans la conception CSV, CSV effectuer des opérations plus en mode d’e/s direct celle qui se produit dans Windows Server 2008 R2.
> * En raison de l’intégration de CSV avec SMB 3.0 telles que SMB multicanal et SMB Direct, rediriger le trafic e/s peut diffuser sur plusieurs réseaux de cluster.
> * Vous devez planifier vos réseaux de cluster pour permettre l’augmentation du trafic réseau au nœud coordinateur potentielle lors de la redirection d’e/s.

### <a name="storage-and-disk-configuration-requirements"></a>Configuration de stockage et de disque

Pour utiliser CSV, le stockage et les disques doivent satisfaire les conditions suivantes:

- **Format de système de fichiers**. Dans Windows Server 2012 R2, un espace disque ou le stockage d’un volume CSV doit être un disque de base qui est partitionné NTFS et ReFS. Dans Windows Server 2012, un espace disque ou le stockage d’un volume CSV doit être un disque de base qui est partitionné avec NTFS.

  Un fichier CSV comprend les exigences supplémentaires suivantes:
    
  - Dans Windows Server 2012 R2, vous ne pouvez pas utiliser un disque pour un fichier CSV qui est formaté avec FAT ou FAT32.
  - Dans Windows Server 2012, vous ne pouvez pas utiliser un disque pour un fichier CSV qui est formaté avec FAT, FAT32 ou ReFS.
  - Si vous souhaitez utiliser un espace de stockage pour un fichier CSV, vous pouvez configurer un espace simple ou une mise en miroir. Dans Windows Server 2012 R2, vous pouvez également configurer un espace de parité. (Dans Windows Server 2012, CSV ne prend pas en charge les espaces de parité.)
  - Un fichier CSV ne peut pas être utilisé comme un disque quorum de témoin. Pour plus d’informations sur le quorum du cluster, voir [Understanding Quorum dans les espaces de stockage Direct](../storage/storage-spaces/understand-quorum.md).
  - Après avoir ajouté un disque au format CSV, il est indiqué dans le format CSVFS (pour le système de fichiers CSV). Ainsi, le cluster et autres logiciels pour différencier le stockage CSV à partir d’autres NTFS ou stockage ReFS. En règle générale, CSVFS prend en charge les mêmes fonctionnalités que NTFS ou ReFS. Toutefois, certaines fonctionnalités ne sont pas pris en charge. Par exemple, dans Windows Server 2012 R2, vous ne pouvez pas activer la compression CSV. Dans Windows Server 2012, vous ne pouvez pas activer déduplication ou compression CSV.
- **Type de ressource dans le cluster**. Pour un volume CSV, vous devez utiliser le type de ressource de disque physique. Par défaut, un espace disque ou de stockage qui est ajouté au stockage de cluster est automatiquement configuré de cette manière.
- **Choix de CSV des disques ou des autres disques de stockage de cluster**. Lors du choix d’un ou plusieurs disques d’un ordinateur virtuel en cluster, envisagez l’utilisation de chaque disque. Si un disque doit être utilisé pour stocker les fichiers qui sont créés par Hyper-V, tels que des fichiers VHD ou les fichiers de configuration, vous pouvez choisir des disques CSV ou autres disques disponibles de stockage de cluster. Si un disque est un disque physique est directement attaché à l’ordinateur virtuel (également appelé un disque pass-through), vous ne pouvez pas choisir un disque CSV, et vous devez choisir à partir des autres disques du stockage de cluster disponibles.
- **Nom de chemin d’accès de l’identification des disques**. Les disques dans le fichier CSV sont identifiés par un nom de chemin d’accès. Chaque chemin d’accès apparaît sur le lecteur système du nœud comme un volume numéroté sous le dossier **\\ClusterStorage** . Ce chemin d’accès est la même lorsqu’ils sont affichés à partir de n’importe quel nœud du cluster. Vous pouvez renommer les volumes si nécessaire.

Pour les besoins de stockage pour CSV, consultez les instructions fournies par votre fournisseur de stockage. Pour les considérations de planification de stockage supplémentaire pour CSV, voir [planifier l’utilisation CSV dans un cluster de basculement](#plan-to-use-csv-in-a-failover-cluster) , plus loin dans cette rubrique.

### <a name="node-requirements"></a>Configuration requise du nœud

Pour utiliser CSV, les nœuds doivent remplir les conditions suivantes:

- **Lecteur de disque système**. Sur tous les nœuds, la lettre du lecteur de disque système doit être identiques.
- **Protocole d’authentification**. Le protocole NTLM doit être activé sur tous les nœuds. Il est activé par défaut.

## <a name="plan-to-use-csv-in-a-failover-cluster"></a>Envisagez d’utiliser CSV dans un cluster de basculement

Cette section répertorie les considérations et recommandations pour l’utilisation CSV dans un cluster de basculement exécutant Windows Server 2012 R2 ou Windows Server 2012 de planification.

>[!IMPORTANT]
>Demandez à votre fournisseur de stockage pour obtenir des recommandations sur la façon de configurer l’unité de stockage spécifique pour CSV. Si les recommandations du fournisseur de stockage diffèrent des informations contenues dans cette rubrique, utilisez les recommandations du fournisseur de stockage.

### <a name="arrangement-of-luns-volumes-and-vhd-files"></a>Disposition de LUN, les volumes et les fichiers de disque dur virtuel

Pour tirer le meilleur parti de CSV pour fournir le stockage pour les ordinateurs virtuels en cluster, il est utile passer en revue la façon dont vous organisez les LUN (disques) lorsque vous configurez des serveurs physiques. Lorsque vous configurez les ordinateurs virtuels correspondants, essayez d’organiser les fichiers de disque dur virtuel de la même manière.

Envisagez un serveur physique pour laquelle vous le feriez organiser les disques et les fichiers comme suit:

- Fichiers système, y compris un fichier d’échange sur un disque physique
- Fichiers de données sur un autre disque physique

Un ordinateur virtuel en cluster équivalente, vous devez organiser les volumes et les fichiers de la même façon:

- Fichiers système, y compris un fichier de page, dans un fichier un CSV disque dur virtuel
- Fichiers de données dans un fichier de disque dur virtuel sur un autre CSV

Si vous ajoutez une autre machine virtuelle, si possible, vous devez garder les disques durs virtuels de la même manière sur cet ordinateur virtuel.

### <a name="number-and-size-of-luns-and-volumes"></a>Nombre et taille des LUN et des volumes

Lorsque vous planifiez la configuration du stockage pour un cluster de basculement qui utilise CSV, tenez compte des recommandations suivantes:

- Pour déterminer combien de LUN à configurer, consultez votre fournisseur de stockage. Par exemple, votre fournisseur de stockage peut vous recommandons de configurer chaque LUN avec une partition placer un volume CSV.
- Il n’existe aucune limitation pour le nombre de machines virtuelles pouvant être pris en charge sur un seul volume CSV. Toutefois, vous devez prendre en compte le nombre d’ordinateurs virtuels que vous envisagez d’avoir dans le cluster et de la charge de travail (opérations d’e/s par seconde) pour chaque ordinateur virtuel. Prenons les exemples suivants :

  - Une organisation consiste à déployer des machines virtuelles qui prendra en charge une infrastructure de bureau virtuelle (VDI), qui est une charge de travail relativement faible. Le cluster utilise le stockage hautes performances. L’administrateur de cluster, après consultation avec le fournisseur de stockage, décide de placer un relativement grand nombre de machines virtuelles par volume CSV.
  - Une autre organisation déploie un grand nombre d’ordinateurs virtuels qui prendra en charge une application de base de données très utilisés, qui est une charge de travail plus importante. Le cluster utilise plus de stockage. L’administrateur de cluster, après consultation avec le fournisseur de stockage, décide de placer un petit nombre d’ordinateurs virtuels par volume CSV.
- Lorsque vous planifiez la configuration du stockage d’un ordinateur virtuel particulier, prenez en compte les exigences de disque de rôle qui prend en charge l’ordinateur virtuel, l’application ou le service. Présentation de ces conditions vous permet d’éviter la contention de disque qui peut entraîner une dégradation des performances. La configuration du stockage de l’ordinateur virtuel doit ressembler étroitement à la configuration du stockage que vous utiliserez pour un serveur physique qui exécute le service, d’une application ou de rôle. Pour plus d’informations, voir [réorganisation de LUN, les volumes et les fichiers VHD](#arrangement-of-luns,-volumes,-and-vhd-files) précédemment dans cette rubrique.

    Vous pouvez également réduire la contention de disque par un stockage avec un grand nombre d’independent disques durs physiques distincts. Sélectionnez votre matériel de stockage en conséquence, puis consultez votre fournisseur afin d’optimiser les performances de votre système de stockage.
- En fonction de vos charges de travail de cluster et la nécessité d’opérations d’e/s, vous pouvez envisager de configuration qu’un pourcentage d’ordinateurs virtuels pour accéder à chaque LUN, tandis qu’autres ordinateurs virtuels ne possèdent pas de connectivité et sont dédiés à calculer des opérations.

## <a name="add-a-disk-to-csv-on-a-failover-cluster"></a>Ajout d’un disque au format CSV sur un cluster de basculement

La fonctionnalité CSV est activée par défaut dans le Clustering de basculement. Pour ajouter un disque au format CSV, vous devez ajouter un disque au groupe de **Stockage disponible** du cluster (si ce n’est pas déjà fait) et puis ajouter le disque au format CSV sur le cluster. Vous pouvez utiliser le Gestionnaire du Cluster de basculement ou les applets de commande Windows PowerShell Clusters de basculement pour effectuer ces procédures.

### <a name="add-a-disk-to-available-storage"></a>Ajout d’un disque vers un stockage disponible

1. Dans le gestionnaire Cluster de basculement, dans l’arborescence de la console, développez le nom du cluster, puis développez **stockage**.
2. Avec le bouton droit de **disques**, puis sélectionnez **Ajouter un disque**. Une liste affiche les disques qui peuvent être ajoutés pour une utilisation dans un cluster de basculement.
3. Sélectionnez l’ou les disques que vous souhaitez ajouter, puis sélectionnez **OK**.

    Les disques sont désormais affectés au groupe de **Stockage disponible** .

#### <a name="windows-powershell-equivalent-commands-add-a-disk-to-available-storage"></a>Commandes de Windows PowerShell équivalentes (ajouter un disque au stockage disponible)

L’applet de commande Windows PowerShell ou les applets de commande suivants exécutent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles apparaissent renvoyées sur plusieurs lignes ici en raison des contraintes de mise en forme.

L’exemple suivant identifie les disques qui sont prêts à ajouter au cluster, puis les ajoute au groupe de **Stockage disponible** .

```PowerShell
Get-ClusterAvailableDisk | Add-ClusterDisk
```

### <a name="add-a-disk-in-available-storage-to-csv"></a>Ajout d’un disque de stockage disponible au format CSV

1. Dans le gestionnaire Cluster de basculement, dans l’arborescence de la console, développez le nom du cluster, développez **stockage**, puis sélectionnez **disques**.
2. Sélectionnez un ou plusieurs disques qui sont assignés au **Stockage disponible**, avec le bouton droit de la sélection, puis sélectionnez **Ajouter aux Volumes partagés de Cluster**.

    Les disques sont désormais affectés au groupe de **Volume partagé de Cluster** du cluster. Les disques sont exposés à chaque nœud du cluster en tant que volumes numérotées (points de montage) sous le dossier % SystemDisk ClusterStorage. Les volumes apparaissent dans le système de fichiers CSVFS.

>[!NOTE]
>Vous pouvez renommer les volumes CSV dans le dossier % SystemDisk ClusterStorage.

#### <a name="windows-powershell-equivalent-commands-add-a-disk-to-csv"></a>Commandes de Windows PowerShell équivalentes (ajouter un disque au format CSV)

L’applet de commande Windows PowerShell ou les applets de commande suivants exécutent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles apparaissent renvoyées sur plusieurs lignes ici en raison des contraintes de mise en forme.

L’exemple suivant ajoute des *disques de Cluster 1* dans **l’Espace de stockage disponible** au format CSV sur le cluster local.

```PowerShell
Add-ClusterSharedVolume –Name "Cluster Disk 1"
```

## <a name="enable-the-csv-cache-for-read-intensive-workloads-optional"></a>Activer le cache CSV pour lecture intensive des charges de travail (facultatifs)

Le cache CSV fournit la mise en cache au niveau du bloc d’opérations e/s sans tampon en lecture seule à l’allocation de mémoire (RAM) comme un cache d’écriture. (Opérations d’e/s sans tampon ne sont pas mis en cache par le Gestionnaire de cache.) Cela peut améliorer les performances pour les applications telles que Hyper-V, qui effectue des opérations d’e/s sans tampon lors de l’accès d’un disque dur virtuel. Le cache CSV peut optimiser les performances des requêtes de lecture sans mise en cache des requêtes d’écriture. Activation du cache CSV est également utile pour les scénarios de serveur de fichiers montée en puissance parallèle.

>[!NOTE]
>Nous vous recommandons d’activer le cache CSV pour les déploiements tout en cluster Hyper-V et le serveur de fichiers montée en puissance parallèle.

Par défaut dans Windows Server 2012, le cache CSV est désactivé. Dans Windows Server 2012 R2, le cache CSV est activé par défaut. Toutefois, vous devez toujours affecter la taille du cache du bloc à réserver.

Le tableau suivant décrit les deux paramètres de configuration qui contrôlent le cache CSV.

<table>
<thead>
<tr class="header">
<th>Nom de la propriété dans Windows Server 2012 R2</th>
<th>Nom de la propriété dans Windows Server 2012</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>BlockCacheSize</strong></td>
<td><strong>SharedVolumeBlockCacheSizeInMB</strong></td>
<td>Il s’agit d’une propriété cluster commune qui vous permet de définir la quantité de mémoire (en mégaoctets) pour réserver pour le cache CSV sur chaque nœud du cluster. Par exemple, si une valeur de 512 est définie, 512 Mo de mémoire système est réservé sur chaque nœud. (Dans le nombre de clusters, 512 Mo est la valeur recommandée). Le paramètre par défaut est 0 (pour désactivé).</td>
</tr>
<tr class="even">
<td><strong>EnableBlockCache</strong></td>
<td><strong>CsvEnableBlockCache</strong></td>
<td>Il s’agit d’une propriété privée de la ressource de disque physique du cluster. Il permet d’activer le cache CSV sur un disque individuel qui est ajouté au format CSV. Dans Windows Server 2012, le paramètre par défaut est 0 (pour désactivé). Pour activer le cache CSV sur un disque, définissez la valeur 1. Par défaut, dans Windows Server 2012 R2, ce paramètre est activé.</td>
</tr>
</tbody>
</table>

Vous pouvez surveiller le cache CSV dans l’Analyseur de performances en ajoutant les compteurs de **Cache de Cluster CSV en Volume**.

#### <a name="configure-the-csv-cache"></a>Configurer le cache CSV

1. Démarrez Windows PowerShell en tant qu’administrateur.
2. Pour définir un cache de *512* Mo à réserver sur chaque nœud, tapez ce qui suit:

    - Pour Windows Server 2012 R2:

        ```PowerShell
        (Get-Cluster).BlockCacheSize = 512  
        ```

    - Pour Windows Server 2012:

        ```PowerShell
        (Get-Cluster).SharedVolumeBlockCacheSizeInMB = 512  
        ```
3. Dans Windows Server 2012, pour activer le cache CSV dans un fichier CSV nommé *1 disque de Cluster*, entrez les informations suivantes:

    ```PowerShell
    Get-ClusterSharedVolume "Cluster Disk 1" | Set-ClusterParameter CsvEnableBlockCache 1
    ```

>[!NOTE]
> * Dans Windows Server 2012, vous pouvez affecter que 20 % de la mémoire RAM physique totale au cache CSV. Dans Windows Server 2012 R2, vous pouvez affecter jusqu'à 80 %. Étant donné que les serveurs de fichiers de mise à niveau ne sont pas généralement limitée par la mémoire, vous pouvez effectuer des gains de performances de grande taille à l’aide de la mémoire supplémentaire pour le cache CSV.
> * Pour éviter les conflits de ressources, vous devez redémarrer chaque nœud du cluster après avoir modifié la mémoire allouée au cache CSV. Dans Windows Server 2012 R2, un redémarrage n’est plus nécessaire.
> * Une fois que vous activez ou désactivez le cache CSV sur un disque individuel, pour le paramètre prenne effet, vous devez déconnecter la ressource de disque physique et mettez-le en ligne. (Par défaut, dans Windows Server 2012 R2, le cache CSV est activé.) 
> * Pour plus d’informations sur le cache CSV qui inclut des informations sur les compteurs de performances, voir le billet de blog [comment activer le Cache CSV](https://blogs.msdn.microsoft.com/clustering/2013/07/19/how-to-enable-csv-cache/).

## <a name="back-up-csv"></a>Sauvegarder CSV

Il existe plusieurs méthodes pour sauvegarder les informations stockées sur CSV dans un cluster de basculement. Vous pouvez utiliser une application de sauvegarde de Microsoft ou d’une application non Microsoft. En règle générale, CSV n’imposent pas les exigences de sauvegarde particulières au-delà de celles pour le stockage en cluster au format NTFS ou ReFS. Sauvegardes CSV n’également pas interrompre autres opérations de stockage CSV.

Tenez compte des facteurs suivants lorsque vous sélectionnez une application de sauvegarde et de la planification de sauvegarde pour CSV:

- Sauvegarde au niveau du volume de volume CSV peut être exécutée à partir de n’importe quel nœud qui se connecte au volume CSV.
- Votre application de sauvegarde peut utiliser des instantanés de logiciels ou des captures instantanées matérielles. En fonction de la capacité de votre application de sauvegarde pour les prendre en charge, les sauvegardes peuvent utiliser les captures instantanées de Volume Shadow Copy Service (VSS) cohérents avec les applications et cohérent après sinistre.
- Si vous sauvegardez CSV ayant plusieurs machines virtuelles, vous devez généralement choisir une méthode de sauvegarde basée sur le système d’exploitation de gestion. Si votre application de sauvegarde prend en charge, plusieurs ordinateurs virtuels peuvent être sauvegardés simultanément.
- CSV prend en charge les demandeurs de sauvegarde qui exécutent Windows Server 2012 R2 sauvegarde, la sauvegarde de Windows Server 2012 ou la sauvegarde de Windows Server 2008 R2. Toutefois, sauvegarde Windows Server offre généralement seulement une solution de sauvegarde qui ne peut-être pas être adaptée pour les organisations avec les clusters de grande taille. Sauvegarde de Windows Server ne prend pas en charge sauvegarde des machines virtuelles cohérentes avec les applications sur CSV. Il prend en charge uniquement des sauvegarde au niveau de volume cohérent après sinistre. (Si vous restaurez une sauvegarde cohérente de l’incident, l’ordinateur virtuel sera dans l’état que qui était si l’ordinateur virtuel a été bloqué au moment où la sauvegarde a été prise.) Une sauvegarde d’un ordinateur virtuel sur un volume CSV réussira, mais un événement d’erreur avoir ouvert une session indiquant qu’il n’est pas pris en charge.
- Vous pouvez demander des informations d’identification administratives lorsque vous sauvegardez un cluster de basculement.

>[!IMPORTANT]
>Veillez à lire attentivement les données de votre application de sauvegarde sauvegarde et restauration, les fonctionnalités CSV pris en charge, et les ressources requises pour l’application sur chaque nœud du cluster.

>[!WARNING]
>Si vous avez besoin restaurer les données de sauvegarde sur un volume CSV, n’oubliez pas des fonctionnalités et limitations de l’application de sauvegarde pour gérer et restaurer des données cohérentes avec les applications entre les nœuds du cluster. Par exemple, avec certaines applications, si le CSV est restaurée sur un nœud différent à partir du nœud où le volume CSV a été sauvegardé, vous pouvez par inadvertance remplacer les données importantes sur l’état de l’application sur le nœud où la restauration est en cours.

## <a name="more-information"></a>Informations supplémentaires

- [Clustering de basculement](failover-clustering.md)
- [Déployer des espaces de stockage en cluster](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>)