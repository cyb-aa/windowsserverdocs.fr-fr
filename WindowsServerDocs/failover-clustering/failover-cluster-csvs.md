---
title: Utiliser des volumes partagés de cluster dans un cluster de basculement
description: Comment utiliser des volumes partagés de cluster dans un cluster de basculement.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 06/07/2019
ms.localizationpriority: medium
ms.openlocfilehash: da0f541c34c7f8687822bec365364fdd406fa3c3
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78370707"
---
# <a name="use-cluster-shared-volumes-in-a-failover-cluster"></a>Utiliser des volumes partagés de cluster dans un cluster de basculement

>S’applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2

Les volumes partagés de cluster permettent à plusieurs nœuds au sein d'un cluster de basculement d'avoir simultanément accès en lecture-écriture à un même numéro d'unité logique (disque) approvisionné en tant que volume NTFS. (Dans Windows Server 2012 R2, le disque peut être approvisionné comme NTFS ou le système de fichiers résilient (ReFS).) Avec CSV, les rôles en cluster peuvent basculer rapidement d’un nœud à un autre sans qu’il soit nécessaire de modifier la propriété des lecteurs, ou de démonter et de remonter un volume. Les volumes partagés de cluster peuvent aussi contribuer à simplifier la gestion d'un nombre potentiellement important de numéros d'unités logiques dans un cluster de basculement.

Le format CSV fournit un système de fichiers en cluster à usage général, qui se situe au-dessus de NTFS (ou ReFS dans Windows Server 2012 R2). Voici quelques exemples d'applications pour les volumes partagés de cluster :

- fichiers de disque dur virtuel (VHD) en cluster pour les ordinateurs virtuels Hyper-V en cluster ;
- partages de fichiers avec montée en puissance parallèle pour stocker les données d'application pour le rôle en cluster de serveur de fichiers avec montée en puissance parallèle. Les données d'application dans le cadre de ce rôle peuvent consister notamment dans des fichiers d'ordinateur virtuel Hyper-V ou des données Microsoft SQL Server. (Sachez que ReFS n’est pas pris en charge pour un Serveur de fichiers avec montée en puissance parallèle.) Pour plus d’informations sur la Serveur de fichiers avec montée en puissance parallèle, consultez [serveur de fichiers avec montée en puissance parallèle pour les données d’application](sofs-overview.md).

> [!NOTE]
> CSV ne prend pas en charge la charge de travail en cluster Microsoft SQL Server dans SQL Server 2012 et les versions antérieures de SQL Server.

Dans Windows Server 2012, la fonctionnalité CSV a été considérablement améliorée. Par exemple, les dépendances vis-à-vis des services de domaine Active Directory ont été éliminées. Les améliorations fonctionnelles apportées à **chkdsk**sont désormais prises en charge, tout comme l’interopérabilité avec les applications antivirus et de sauvegarde et l’intégration avec les fonctionnalités de stockage générales, telles que les volumes chiffrés par BitLocker et les espaces de stockage. Pour obtenir une vue d’ensemble de la fonctionnalité de volume partagé de cluster qui a été introduite dans Windows Server 2012, voir [Nouveautés du clustering de basculement dans Windows server 2012 \[Redirigé\]](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265972(v%3dws.11)>).

Windows Server 2012 R2 introduit des fonctionnalités supplémentaires, telles que la propriété du volume partagé de cluster distribué, une résilience accrue grâce à la disponibilité du service serveur, une plus grande souplesse dans la quantité de mémoire physique que vous pouvez allouer au cache de volume partagé de cluster, mieux capacité et l’interopérabilité améliorée qui incluent la prise en charge de ReFS et de la déduplication. Pour plus d’informations, consultez [Nouveautés du clustering de basculement](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265972(v%3dws.11)>).

> [!NOTE]
> Pour plus d’informations sur l’utilisation de la déduplication de données sur un volume partagé de cluster dans le cadre de scénarios d’infrastructure VDI (Virtual Desktop Infrastructure), consultez les billets de blog [Deploying Data Deduplication for VDI storage in Windows Server 2012 R2](https://blogs.technet.com/b/filecab/archive/2013/07/31/deploying-data-deduplication-for-vdi-storage-in-windows-server-2012-r2.aspx) et [Extending Data Deduplication to new workloads in Windows Server 2012 R2](https://blogs.technet.com/b/filecab/archive/2013/07/31/extending-data-deduplication-to-new-workloads-in-windows-server-2012-r2.aspx).

## <a name="review-requirements-and-considerations-for-using-csv-in-a-failover-cluster"></a>Configuration requise et éléments à prendre en considération pour l'utilisation de volumes partagés de cluster dans un cluster de basculement

Avant d'utiliser des volumes partagés de cluster dans un cluster de basculement, passez en revue cette section pour connaître la configuration requise et les différents éléments à prendre en considération pour ce qui est notamment du réseau et du stockage.

### <a name="network-configuration-considerations"></a>Éléments à prendre considération en matière de configuration réseau

Au moment de configurer les réseaux qui prennent en charge les volumes partagés de cluster, tenez compte des points suivants.

- **Plusieurs réseaux et plusieurs cartes réseau**. Pour bénéficier d'une tolérance de panne en cas de défaillance réseau, nous vous recommandons de faire transiter le trafic des volumes partagés de cluster par plusieurs réseaux de cluster ou de configurer des cartes réseau associées.
    
    Si les nœuds du cluster sont connectés à des réseaux que le cluster ne doit pas utiliser, vous devez les désactiver. Par exemple, nous vous recommandons de désactiver l'utilisation de réseaux iSCSI dans le cadre d'un cluster pour empêcher tout trafic de volume partagé de cluster sur ces réseaux. Pour désactiver un réseau, dans Gestionnaire du cluster de basculement, sélectionnez **réseaux**, sélectionnez le réseau, sélectionnez l’action **Propriétés** , puis sélectionnez **ne pas autoriser la communication réseau de cluster sur ce réseau**. Vous pouvez également configurer la propriété **role** du réseau à l’aide de l’applet de commande Windows PowerShell [ClusterNetwork](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusternetwork?view=win10-ps) .
- **Propriétés des cartes réseau**. Dans les propriétés de toutes les cartes par lesquelles transite la communication du cluster, vérifiez que les paramètres suivants sont activés :

  - **Client pour les réseaux Microsoft** et **Partage de fichiers et d'imprimantes pour les réseaux Microsoft**. Ces paramètres prennent en charge le protocole SMB (Server Message Block) 3.0, qui est utilisé par défaut pour transporter le trafic de volume partagé de cluster entre les nœuds. Pour activer SMB, vérifiez aussi que le service Serveur et le service Station de travail sont en cours d'exécution et qu'ils sont configurés pour démarrer automatiquement sur chaque nœud du cluster.

    >[!NOTE]
    >Dans Windows Server 2012 R2, il existe plusieurs instances de service serveur par nœud de cluster de basculement. Il y a l'instance par défaut chargée de traiter le trafic entrant en provenance des clients SMB qui accèdent à des partages de fichiers normaux et une deuxième instance de volume partagé de cluster qui traite uniquement le trafic de volume partagé de cluster entre les nœuds. De même, si le service Serveur se dégrade sur un nœud, la propriété du volume partagé de cluster est transférée à un autre nœud.

    Le protocole SMB 3.0 intègre les fonctionnalités SMB Multichannel et SMB Direct, qui permettent au trafic de volume partagé de cluster d'être transmis sur plusieurs réseaux dans le cluster et d'exploiter les cartes réseau prenant en charge l'accès direct à la mémoire à distance (RDMA). Par défaut, SMB Multichannel est utilisé pour le trafic de volume partagé de cluster. Pour plus d'informations, voir [Vue d'ensemble du protocole SMB (Server Message Block)](../storage/file-server/file-server-smb-overview.md).
  - **Filtre de performance de carte virtuelle de cluster de basculement Microsoft**. Ce paramètre améliore l'aptitude des nœuds à rediriger les E/S quand cela s'avère nécessaire pour accéder à un volume partagé de cluster, par exemple, quand une panne de connectivité empêche un nœud de se connecter directement au disque du volume partagé de cluster. Pour plus d’informations, consultez [à propos de la synchronisation des e/s et de la redirection des e/s dans la communication CSV](#about-io-synchronization-and-io-redirection-in-csv-communication) plus loin dans cette rubrique.
- **Définition des priorités concernant les réseaux de cluster**. Nous déconseillons généralement de modifier les préférences configurées au niveau du cluster pour les réseaux.
- **Configuration des sous-réseaux IP**. Aucune configuration de sous-réseau spécifique n'est nécessaire pour les nœuds d'un réseau qui utilise des volumes partagés de cluster. Les volumes partagés de cluster peuvent prendre en charge les clusters constitués de plusieurs sous-réseaux.
- **Qualité de service (QoS) basée sur la stratégie**. Dans le cadre d'une utilisation de volumes partagés de cluster, nous vous recommandons de configurer une stratégie de priorité QoS et une stratégie de bande passante minimale pour le trafic réseau dirigé vers chaque nœud. Pour plus d’informations, voir [qualité de service (QoS)](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831679(v%3dws.11)>).
- **Réseau de stockage**. Pour obtenir des recommandations sur les réseaux de stockage, reportez-vous aux conseils donnés par votre fournisseur de stockage. Pour plus d’informations sur le stockage pour les volumes partagés de cluster, consultez [Configuration requise pour le stockage et les disques](#storage-and-disk-configuration-requirements) , plus loin dans cette rubrique.

Pour obtenir une vue d’ensemble de la configuration requise pour les composants matériels, le réseau et le stockage des clusters de basculement, consultez [Failover Clustering Hardware Requirements and Storage Options](clustering-requirements.md).

#### <a name="about-io-synchronization-and-io-redirection-in-csv-communication"></a>À propos de la synchronisation et de la redirection des E/S dans le cadre de la communication des volumes partagés de cluster

- **Synchronisation des e/s**: les volumes partagés de cluster permettent à plusieurs nœuds d’avoir un accès en lecture-écriture simultané au même stockage partagé. Quand un nœud exécute une entrée/sortie (E/S) de disque sur un volume partagé de cluster, le nœud communique directement avec le stockage, par exemple, via un réseau de zone de stockage (SAN). Cependant, à tout moment, un seul nœud (appelé « nœud coordinateur ») est « propriétaire » de la ressource de disque physique associée au numéro d'unité logique. Le nœud coordinateur d'un volume partagé de cluster s'affiche dans le Gestionnaire du cluster de basculement en tant que **Nœud propriétaire** sous **Disques**. Il apparaît également dans la sortie de l’applet de commande Windows PowerShell [ClusterSharedVolume](https://docs.microsoft.com/powershell/module/failoverclusters/get-clustersharedvolume?view=win10-ps) .

  >[!NOTE]
  >Dans Windows Server 2012 R2, la propriété CSV est répartie uniformément entre les nœuds de cluster de basculement en fonction du nombre de volumes CSV détenus par chaque nœud. De plus, la propriété subit un rééquilibrage automatique dans certaines situations : basculement d'un volume partagé de cluster, un nœud qui rejoint le cluster, ajout d'un nœud au cluster, redémarrage d'un nœud du cluster ou démarrage du cluster de basculement après avoir été arrêté.

  Quand certaines modifications mineures interviennent dans le système de fichiers d'un volume partagé de cluster, ces métadonnées doivent être synchronisées sur chaque nœud physique qui accède au numéro d'unité logique, et pas seulement sur le nœud coordinateur. Par exemple, dès lors qu'un ordinateur virtuel est démarré, créé ou supprimé au niveau d'un volume partagé de cluster ou qu'un ordinateur virtuel fait l'objet d'une migration, ces informations doivent être synchronisées sur chaque nœud physique qui accède à l'ordinateur virtuel. Ces opérations de mise à jour des métadonnées se produisent en parallèle sur les réseaux du cluster via SMB 3.0. Dans le cadre de ces opérations, les nœuds physiques ne sont pas tous tenus de communiquer avec le stockage partagé.

- **Redirection d’e/s**: les défaillances de connectivité du stockage et certaines opérations de stockage peuvent empêcher un nœud donné de communiquer directement avec le stockage. Pour maintenir le service pendant l'absence de communication entre le nœud et le stockage, le nœud redirige les E/S de disque via un réseau du cluster vers le nœud coordinateur où le disque est actuellement monté. Si le nœud coordinateur actuel subit une défaillance de connectivité avec le stockage, toutes les opérations d'E/S de disque sont mises provisoirement en file d'attente en attendant qu'un nouveau nœud soit défini comme nœud coordinateur.

Le serveur utilise l'un des modes de redirection d'E/S suivants, qui varie selon la situation :

- **Redirection du système de fichiers** : la redirection se fait par volume, par exemple, quand une application de sauvegarde prend des captures instantanées de volume partagé de cluster au moment où un volume partagé de cluster est mis manuellement en mode E/S redirigé.
- **Redirection de bloc** : la redirection intervient au niveau du bloc de fichier, par exemple, en cas de perte de connectivité entre le stockage et un volume. Le redirection de bloc est nettement plus rapide que la redirection du système de fichiers.

Dans Windows Server 2012 R2, vous pouvez afficher l’état d’un volume partagé de cluster par nœud. Par exemple, vous pouvez voir si les E/S sont directes ou redirigées ou si le volume partagé de cluster est indisponible. Si un volume partagé de cluster est en mode E/S redirigé, vous pouvez aussi en voir la raison. Utilisez l’applet de commande Windows PowerShell **Get-ClusterSharedVolumeState** pour afficher ces informations.

> [!NOTE]
> * Dans Windows Server 2012, en raison des améliorations apportées à la conception des volumes partagés de cluster, le volume partagé de cluster effectue plus d’opérations en mode e/s directes que dans Windows Server 2008 R2.
> * Grâce à l'intégration des volumes partagés de cluster aux fonctionnalités du protocole SMB 3.0, telles que SMB Multichannel et SMB Direct, le trafic d'E/S redirigé peut être diffusé sur plusieurs réseaux de cluster.
> * Vous devez prévoir sur vos réseaux de cluster une possible augmentation du trafic réseau vers le nœud coordinateur lors de la redirection des E/S.

### <a name="storage-and-disk-configuration-requirements"></a>Exigences en matière de stockage et de configuration de disque

Pour utiliser des volumes partagés de cluster, votre stockage et vos disques doivent répondre aux exigences suivantes :

- **Format du système de fichiers**. Dans Windows Server 2012 R2, un disque ou un espace de stockage pour un volume partagé de cluster doit être un disque de base partitionné en NTFS ou ReFS. Dans Windows Server 2012, un disque ou un espace de stockage pour un volume partagé de cluster doit être un disque de base partitionné avec NTFS.

  Un volume partagé de cluster présente d'autres exigences, à savoir :
    
  - Dans Windows Server 2012 R2, vous ne pouvez pas utiliser de disque pour un volume partagé de cluster formaté avec FAT ou FAT32.
  - Dans Windows Server 2012, vous ne pouvez pas utiliser de disque pour un volume partagé de cluster formaté en FAT, FAT32 ou ReFS.
  - Si vous voulez utiliser un espace de stockage pour un volume partagé de cluster, vous pouvez configurer un espace simple ou un espace en miroir. Dans Windows Server 2012 R2, vous pouvez également configurer un espace de parité. (Dans Windows Server 2012, CSV ne prend pas en charge les espaces de parité.)
  - Un volume partagé de cluster ne peut pas être utilisé comme disque témoin de quorum. Pour plus d’informations sur le quorum de cluster, consultez [Présentation du quorum dans espaces de stockage direct](../storage/storage-spaces/understand-quorum.md).
  - Une fois que vous avez ajouté un disque en tant que volume partagé de cluster, il est désigné au format CSVFS (CSV File System). Cela permet au cluster et aux autres logiciels de faire la distinction entre le stockage de volumes partagés de cluster et tout autre stockage NTFS ou ReFS. En règle générale, CSVFS prend en charge les mêmes fonctionnalités que NTFS ou ReFS. Cependant, certaines d'entre elles ne le sont pas. Par exemple, dans Windows Server 2012 R2, vous ne pouvez pas activer la compression sur les volumes partagés de cluster. Dans Windows Server 2012, vous ne pouvez pas activer la déduplication des données ou la compression sur les volumes partagés de cluster.
- **Type de ressource dans le cluster**. Pour un volume partagé de cluster, vous devez utiliser le type de ressource Disque physique. Par défaut, un disque ou un espace de stockage ajouté à un stockage de cluster est automatiquement configuré de cette façon.
- **Choix de disques de volume partagé de cluster ou d'autres disques dans un stockage de cluster**. Au moment de choisir un ou plusieurs disques pour un ordinateur virtuel en cluster, tenez compte de l'utilisation qui sera faite de chaque disque. Si un disque doit servir à stocker les fichiers créés par Hyper-V, tels que des fichiers VHD ou des fichiers de configuration, vous pouvez faire un choix parmi les disques de volume partagé de cluster ou les autres disques disponibles dans le stockage de cluster. Si un disque est destiné à être un disque physique directement attaché à l'ordinateur virtuel (aussi appelé « disque pass-through » ou « relais »), votre choix ne peut pas se porter sur un disque de volume partagé de cluster, mais nécessairement sur l'un des autres disques disponibles dans le stockage de cluster.
- **Nom de chemin d'accès pour l'identification des disques**. Les disques de volume partagé de cluster sont identifiés à l'aide d'un nom de chemin d'accès. Chaque chemin d’accès apparaît sur le lecteur système du nœud sous la forme d’un volume numéroté sous le dossier **\\ClusterStorage** . Ce chemin d'accès s'affiche à l'identique sur n'importe quel un nœud du cluster. Vous pouvez renommer les volumes, si nécessaire.

Pour connaître les exigences imposées par les volumes partagés de cluster, reportez-vous aux conseils fournis par votre fournisseur de stockage. Vous découvrirez d’autres éléments à prendre en considération pour la planification du stockage pour les volumes partagés de cluster dans la section [Planifier l'utilisation de volumes partagés de cluster dans un cluster de basculement](#plan-to-use-csv-in-a-failover-cluster) , plus loin dans cette rubrique.

### <a name="node-requirements"></a>Configuration requise des nœuds

Pour utiliser des volumes partagés de cluster, vos nœuds doivent répondre aux exigences suivantes :

- **Lettre de lecteur du disque système**. Sur tous les nœuds, la lettre de lecteur du disque système doit être la même.
- **Protocole d’authentification**. Le protocole NTLM doit être activé sur tous les nœuds. Ceci est activé par défaut.

## <a name="plan-to-use-csv-in-a-failover-cluster"></a>Planifier l'utilisation de volumes partagés de cluster dans un cluster de basculement

Cette section répertorie les considérations relatives à la planification et les recommandations relatives à l’utilisation des volumes partagés de cluster dans un cluster de basculement exécutant Windows Server 2012 R2 ou Windows Server 2012.

> [!IMPORTANT]
> Interrogez votre fournisseur de stockage pour savoir comment configurer votre unité de stockage spécifique pour les volumes partagés de cluster. Si les recommandations émanant du fournisseur de stockage sont différentes des informations contenues dans cette rubrique, suivez les recommandations du fournisseur de stockage.

### <a name="arrangement-of-luns-volumes-and-vhd-files"></a>Organisation des numéros d'unités logiques, des volumes et des fichiers VHD

Pour exploiter au mieux les volumes partagés de cluster et fournir ainsi du stockage pour les ordinateurs virtuels en cluster, il est utile de réfléchir à la façon d'organiser les numéros d'unités logiques (disques) au moment de configurer les serveurs physiques. Pendant que vous configurez les ordinateurs virtuels correspondants, essayez d'organiser les fichiers VHD de la même façon.

Songez à un serveur physique pour lequel vous organiseriez les disques et les fichiers de la façon suivante :

- système de fichiers, y compris un fichier d'échange, sur un disque physique ;
- fichiers de données sur un autre disque dur.

Pour un ordinateur virtuel en cluster équivalent, vous devriez organiser les volumes de façon analogue :

- système de fichiers, y compris un fichier d'échange, dans un fichier VHD sur un volume partagé de cluster ;
- fichiers de données dans un fichier VHD sur un autre volume partagé de cluster.

Si vous ajoutez un autre ordinateur virtuel, gardez dans la mesure du possible la même organisation pour les fichiers VHD sur cet ordinateur virtuel.

### <a name="number-and-size-of-luns-and-volumes"></a>Nombre et taille des numéros d'unités logiques et des volumes

Au moment de planifier la configuration du stockage pour un cluster de basculement faisant appel à des volumes partagés de cluster, tenez compte des recommandations suivantes :

- Pour décider du nombre de numéros d'unités logiques à configurer, consultez votre fournisseur de stockage. Par exemple, celui-ci peut vous conseiller de configurer chaque numéro d'unité logique avec une partition et d'y placer un volume partagé de cluster.
- Il n'existe aucune limite quant au nombre d'ordinateurs virtuels pouvant être pris en charge sur un même volume partagé de cluster. Cependant, réfléchissez au nombre d'ordinateurs virtuels que vous prévoyez d'utiliser dans le cluster et à la charge de travail (nombre d'opérations d'E/S par seconde) pour chaque ordinateur virtuel. Penchez-vous sur les exemples suivants :

  - Une organisation déploie des ordinateurs virtuels destinés à prendre en charge une infrastructure VDI, dont la charge de travail est relativement faible. Le cluster utilise un stockage à hautes performances. Après consultation du fournisseur de stockage, l'administrateur du cluster décide de placer un nombre relativement important d'ordinateurs virtuels par volume partagé de cluster.
  - Une autre organisation déploie un grand nombre d'ordinateurs virtuels destinés à prendre en charge une application de base de données fortement sollicitée, dont la charge de travail est plus importante. Le cluster utilise un stockage moins performant. Après consultation du fournisseur de stockage, l'administrateur du cluster décide de placer un nombre relativement faible d'ordinateurs virtuels par volume partagé de cluster.
- Au moment de planifier la configuration du stockage pour un ordinateur virtuel déterminé, réfléchissez aux caractéristiques que doit présenter le disque pour permettre à l'ordinateur virtuel de prendre en charge le service, l'application ou le rôle qui lui est imparti. L'analyse de ces besoins vous évitera une contention de disque, qui risque de se traduire par des performances médiocres. La configuration du stockage pour l'ordinateur virtuel doit être très proche ce celle que vous utiliseriez pour un serveur physique exécutant un service, une application ou un rôle analogue. Pour plus d’informations, consultez [Organisation des numéros d’unités logiques, des volumes et des fichiers VHD](#arrangement-of-luns-volumes-and-vhd-files) , plus haut dans cette rubrique.

    Vous pouvez aussi limiter les risques de contention de disque en prévoyant un stockage constitué d'un grand nombre de disques durs physiques indépendants. Choisissez votre matériel de stockage en conséquence et consultez votre fournisseur pour optimiser les performances de votre stockage.
- Selon les charges de travail de votre cluster et leurs besoins en opérations d'E/S, vous pouvez envisager de configurer seulement un pourcentage d'ordinateurs virtuels ayant accès à chaque numéro d'unité logique, tandis que les autres n'auront pas de connectivité et seront dédiés aux opérations de calcul.

## <a name="add-a-disk-to-csv-on-a-failover-cluster"></a>Ajouter un disque à un volume partagé de cluster sur un cluster de basculement

La fonctionnalité de volume partagé de cluster est activée par défaut dans le clustering de basculement. Pour ajouter un disque à un volume partagé de cluster, vous devez ajouter un disque au groupe **Stockage disponible** du cluster (s'il n'est pas déjà ajouté), puis ajouter le disque au volume partagé de cluster sur le cluster. Vous pouvez utiliser Gestionnaire du cluster de basculement ou les applets de commande Windows PowerShell des clusters de basculement pour exécuter ces procédures.

### <a name="add-a-disk-to-available-storage"></a>Ajouter un disque au stockage disponible

1. Dans le Gestionnaire du cluster de basculement, dans l'arborescence de la console, développez le nom du cluster, puis **Stockage**.
2. Cliquez avec le bouton droit sur **disques**, puis sélectionnez **Ajouter un disque**. Une liste présentant les disques pouvant être ajoutés pour une utilisation dans un cluster de basculement s'affiche à l'écran.
3. Sélectionnez le ou les disques que vous souhaitez ajouter, puis sélectionnez **OK**.

    Les disques sont dès lors affectés au groupe **Stockage disponible**.

#### <a name="windows-powershell-equivalent-commands-add-a-disk-to-available-storage"></a>Commandes Windows PowerShell équivalentes (ajouter un disque au stockage disponible)

L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.

L'exemple suivant identifie les disques prêts à être ajoutés au cluster avant d'être ajoutés au groupe **Stockage disponible** .

```PowerShell
Get-ClusterAvailableDisk | Add-ClusterDisk
```

### <a name="add-a-disk-in-available-storage-to-csv"></a>Ajouter un disque dans stockage disponible au volume partagé de cluster

1. Dans Gestionnaire du cluster de basculement, dans l’arborescence de la console, développez le nom du cluster, développez **stockage**, puis sélectionnez **disques**.
2. Sélectionnez un ou plusieurs disques affectés au **stockage disponible**, cliquez avec le bouton droit sur la sélection, puis sélectionnez **Ajouter aux volumes partagés de cluster**.

    Les disques sont à présent affectés au groupe **Volume partagé de cluster** du cluster. Les disques sont exposés à chaque nœud de cluster en tant que volumes numérotés (points de montage) sous le dossier %SystemDisk%ClusterStorage. Les volumes s'affichent dans le système de fichiers CSVFS.

>[!NOTE]
>Vous pouvez renommer les volumes partagés de cluster dans le dossier %SystemDisk%ClusterStorage.

#### <a name="windows-powershell-equivalent-commands-add-a-disk-to-csv"></a>Commandes Windows PowerShell équivalentes (ajouter un disque au fichier CSV)

L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.

L'exemple suivant ajoute *Cluster Disk 1* présent dans **Stockage disponible** au volume partagé de cluster du cluster local.

```PowerShell
Add-ClusterSharedVolume –Name "Cluster Disk 1"
```

## <a name="enable-the-csv-cache-for-read-intensive-workloads-optional"></a>Activer le cache de volume partagé de cluster pour les charges de travail imposant des accès en lecture intensifs (facultatif)

Le cache de volume partagé de cluster assure une mise en cache au niveau du bloc des opérations d'E/S en lecture seule non mises en mémoire tampon en allouant de la mémoire système (RAM) en tant que cache à écriture immédiate (« write-through »). (Les opérations d’e/s non mises en mémoire tampon ne sont pas mises en cache par le gestionnaire de cache.) Cela peut améliorer les performances pour les applications telles que Hyper-V, qui exécutent des opérations d’e/s non mises en mémoire tampon lors de l’accès à un disque dur virtuel. Le cache de volume partagé de cluster peut dynamiser les performances des demandes de lecture sans mettre en cache les demandes d'écriture. L'activation du cache de volume partagé de cluster est aussi utile dans le cadre des scénarios de serveur de fichiers avec montée en puissance parallèle.

>[!NOTE]
>Nous vous recommandons d'activer le cache de volume partagé de cluster pour tous les déploiements en cluster d'Hyper-V et de serveurs de fichiers avec montée en puissance parallèle en cluster.

Par défaut, dans Windows Server 2012, le cache de volume partagé de cluster est désactivé. Dans Windows Server 2012 R2 et versions ultérieures, le cache de volume partagé de cluster est activé par défaut. Cependant, vous devez toujours allouer la taille du cache de bloc à réserver.

Le tableau suivant décrit les deux paramètres de configuration qui contrôlent le cache de volume partagé de cluster.

| Windows Server 2012 R2 et versions ultérieures |  Windows Server 2012                 | Description |
| -------------------------------- | ------------------------------------ | ----------- |
| BlockCacheSize                   | SharedVolumeBlockCacheSizeInMB       | Cette propriété de cluster courante permet de définir la quantité de mémoire (en mégaoctets) à réserver pour le cache de volume partagé de cluster sur chaque nœud du cluster. Par exemple, si la valeur définie est 512, la mémoire système réservée sur chaque nœud est de 512 Mo. (Dans de nombreux clusters, 512 Mo est une valeur recommandée.) La valeur par défaut est 0 (pour désactivé). |
| EnableBlockCache                 | CsvEnableBlockCache                  | Propriété privée de la ressource Disque physique de cluster. Elle permet d'activer le cache de volume partagé de cluster sur un disque individuel ajouté au volume partagé de cluster. Dans Windows Server 2012, le paramètre par défaut est 0 (pour désactivé). Pour activer le cache de volume partagé de cluster sur un disque, définissez la valeur 1. Par défaut, ce paramètre est activé dans Windows Server 2012 R2. |

Vous pouvez analyser le cache de volume partagé de cluster dans l'Analyseur de performances en ajoutant les compteurs sous **Cache de volume partagé de cluster**.

#### <a name="configure-the-csv-cache"></a>Configurer le cache de volume partagé de cluster

1. Démarrez Windows PowerShell en tant qu’administrateur.
2. Pour définir un cache de *512* Mo à réserver sur chaque nœud, tapez ce qui suit :

    - Pour Windows Server 2012 R2 et versions ultérieures :

        ```PowerShell
        (Get-Cluster).BlockCacheSize = 512  
        ```

    - Pour Windows Server 2012 :

        ```PowerShell
        (Get-Cluster).SharedVolumeBlockCacheSizeInMB = 512  
        ```
3. Dans Windows Server 2012, pour activer le cache de volume partagé de cluster sur un fichier CSV nommé *cluster 1*, entrez ce qui suit :

    ```PowerShell
    Get-ClusterSharedVolume "Cluster Disk 1" | Set-ClusterParameter CsvEnableBlockCache 1
    ```

>[!NOTE]
> * Dans Windows Server 2012, vous pouvez allouer seulement 20% de la quantité totale de RAM physique au cache de volume partagé de cluster. Dans Windows Server 2012 R2 et versions ultérieures, vous pouvez allouer jusqu’à 80%. Comme les serveurs de fichiers avec montée en puissance parallèle ne sont généralement pas limités en mémoire, vous pouvez obtenir des gains de performances importants en utilisant la mémoire supplémentaire pour le cache de volume partagé de cluster.
> * Pour éviter les conflits de ressources, vous devez redémarrer chaque nœud du cluster après avoir modifié la mémoire qui est allouée au cache de volume partagé de cluster. Dans Windows Server 2012 R2 et versions ultérieures, un redémarrage n’est plus nécessaire.
> * Après avoir activé ou désactivé le cache de volume partagé de cluster sur un disque individuel, pour que le paramètre prenne effet, vous devez mettre la ressource Disque physique hors connexion, puis la remettre en ligne. (Par défaut, dans Windows Server 2012 R2 et versions ultérieures, le cache de volume partagé de cluster est activé.) 
> * Pour plus d’informations sur le cache de volume partagé de cluster contenant des informations sur les compteurs de performances, consultez le billet de blog [How to Enable CSV Cache](https://blogs.msdn.microsoft.com/clustering/2013/07/19/how-to-enable-csv-cache/).

## <a name="backing-up-csvs"></a>Sauvegarde de CSV

Il existe plusieurs méthodes pour sauvegarder des informations qui sont stockées sur CSV dans un cluster de basculement. Vous pouvez utiliser une application de sauvegarde Microsoft ou une application non fournie par Microsoft. En règle générale, les volumes partagés de cluster n'imposent pas de conditions de sauvegarde particulières en dehors de celles liées au stockage en cluster formaté en NTFS ou ReFS. De même, les sauvegardes de volumes partagés de cluster ne perturbent pas les autres opérations de stockage de volume partagé de cluster.

Au moment de choisir une application de sauvegarde et un programme de sauvegarde, voici les éléments dont vous devez tenir compte :

- Une sauvegarde au niveau du volume d'un volume partagé de cluster peut être exécutée à partir de n'importe quel nœud connecté au volume partagé de cluster.
- Votre application de sauvegarde peut utiliser des captures instantanées logicielles ou matérielles. Selon la capacité de votre application de sauvegarde à les prendre en charge, les sauvegardes peuvent utiliser des captures instantanées VSS (Volume Shadow Copy Service) cohérentes du point de vue des applications et des incidents.
- Si vous sauvegardez des volumes partagés de cluster sur lesquels s'exécutent plusieurs ordinateurs virtuels, vous avez généralement tout intérêt à opter pour une méthode de sauvegarde basée sur le système d'exploitation de gestion. Si votre application de sauvegarde le permet, plusieurs ordinateurs virtuels peuvent être sauvegardés simultanément.
- CSV prend en charge les demandeurs de sauvegarde qui exécutent la sauvegarde Windows Server 2012 R2, la sauvegarde Windows Server 2012 ou la sauvegarde Windows Server 2008 R2. Cependant, la Sauvegarde Windows Server n'offre généralement qu'une solution de sauvegarde de base qui n'est pas nécessairement adaptée aux organisations dotées de clusters volumineux. La Sauvegarde Windows Server ne prend pas en charge la sauvegarde d'ordinateurs virtuels cohérente du point de vue des applications sur les volumes partagés de cluster. Elle ne prend en charge que la sauvegarde au niveau du volume cohérente du point de vue des incidents. (Si vous restaurez une sauvegarde cohérente en cas d’incident, l’ordinateur virtuel sera dans le même État que celui dans lequel la machine virtuelle était bloquée au moment précis où la sauvegarde a été effectuée.) Une sauvegarde d’un ordinateur virtuel sur un volume partagé de cluster est réussie, mais un événement d’erreur est consigné, indiquant que cela n’est pas pris en charge.
- Vous aurez peut-être besoin d'informations d'identification d'administrateur au moment de sauvegarde un cluster de basculement.

> [!IMPORTANT]
>Veillez à examiner attentivement le type de données sauvegardées et restaurées par votre application de sauvegarde, les fonctionnalités de volume partagé de cluster qu'elle prend en charge, ainsi que les besoins en ressources de l'application sur chaque nœud du cluster.

> [!WARNING]
> Si vous devez restaurer les données de sauvegarde sur un volume partagé de cluster, tenez compte des capacités et des limitations de l'application de sauvegarde pour conserver et restaurer des données cohérentes du point de vue des applications dans les nœuds du cluster. Par exemple, avec certaines applications, si le volume partagé de cluster est restauré sur un nœud différent de celui sur lequel le volume partagé de cluster a été sauvegardé, vous risquez par mégarde de remplacer des données importantes concernant l'état des applications sur le nœud où la restauration a eu lieu.

## <a name="more-information"></a>Informations supplémentaires

- [Clustering avec basculement](failover-clustering.md)
- [Déployer des espaces de stockage en cluster](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>)