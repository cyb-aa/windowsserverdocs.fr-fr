---
title: Qualité de service de stockage
ms.prod: windows-server
manager: dongill
ms.author: JGerend
ms.technology: storage-qos
ms.topic: get-started-article
ms.assetid: 8dcb8cf9-0e08-4fdd-9d7e-ec577ce8d8a0
author: kumudd
ms.date: 10/10/2016
ms.openlocfilehash: 0e848260dd4ba3b37d1351fba7c24dd3cd283e69
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393937"
---
# <a name="storage-quality-of-service"></a>Qualité de service de stockage

> S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

La qualité de service (QoS) de stockage dans Windows Server 2016 permet d’analyser et de gérer de manière centralisée les performances de stockage pour les machines virtuelles à l’aide d’Hyper-V et des rôles de serveur de fichiers avec montée en puissance parallèle. La fonctionnalité améliore automatiquement l’équité des ressources de stockage entre plusieurs machines virtuelles avec le même cluster de serveurs de fichiers et permet la configuration en unités d’E/S par seconde normalisées des objectifs de performances minimales et maximales basés sur des stratégies.  

Vous pouvez utiliser la qualité de service de stockage dans Windows Server 2016 pour accomplir les tâches suivantes :  

-   **Atténuez les problèmes bruyants du voisin.** Par défaut, la qualité de service de stockage garantit qu’une seule machine virtuelle ne peut pas consommer toutes les ressources de stockage et priver les autres machines virtuelles de la bande passante de stockage.  

-   **Surveiller les performances de stockage de bout en bout.** Dès que les machines virtuelles stockées sur un serveur de fichiers avec montée en puissance parallèle sont démarrées, leurs performances sont analysées. Les détails des performances de toutes les machines virtuelles en cours d’exécution et la configuration du cluster de serveurs de fichiers avec montée en puissance parallèle peuvent être affichés depuis un emplacement unique  

-   **Gérer les E/S de stockage selon les besoins de l’entreprise en matière de charge de travail** Des stratégies de qualité de service de stockage définissent les valeurs de performances minimales et maximales des machines virtuelles et vérifient qu’elles sont respectées. Cela permet de garantir des performances cohérentes pour les machines virtuelles, même dans des environnements denses et surutilisés. Si les stratégies ne peuvent pas être satisfaites, des alertes sont disponibles pour assurer le suivi des machines virtuelles hors stratégie ou auxquelles des stratégies non valides ont été affectées.  

Ce document décrit comment votre entreprise peut bénéficier de la nouvelle fonctionnalité de qualité de service de stockage. Il part du principe que vous avez déjà utilisé Windows Server, le clustering de basculement Windows Server, le serveur de fichiers avec montée en puissance parallèle, Hyper-V et Windows PowerShell.

## <a name="BKMK_Overview"></a>Vue  
Cette section décrit la configuration requise pour l’utilisation de la qualité de service de stockage, une vue d’ensemble d’une solution définie par un logiciel à l’aide de la qualité de service de stockage et une liste de termes liés à la qualité de service de stockage.  

### <a name="BKMK_Requirements"></a>Exigences de qualité de service de stockage  
La qualité de service de stockage prend en charge deux scénarios de déploiement :  

-   **Hyper-V avec un serveur de fichiers avec montée en puissance parallèle** Ce scénario nécessite les deux éléments suivants :  

    -   Cluster de stockage qui est un cluster de serveurs de fichiers avec montée en puissance parallèle  

    -   Cluster de calcul qui dispose d’au moins un serveur avec le rôle Hyper-V activé  

    Pour la qualité de service de stockage, le cluster de basculement est nécessaire sur les serveurs de stockage, mais il n’est pas obligatoire que les serveurs de calcul soient dans un cluster de basculement. Tous les serveurs (utilisés à la fois pour le stockage et le calcul) doivent exécuter Windows Server 2016.  

    Si vous ne disposez pas d’un cluster Serveur de fichiers avec montée en puissance parallèle déployé à des fins d’évaluation, pour obtenir des instructions pas à pas pour en créer un à l’aide de serveurs ou de machines virtuelles existants, consultez [Windows Server 2012 R2 Storage : Pas à pas avec les espaces de stockage, la montée en charge SMB et le VHDX partagé (physique) ](http://blogs.technet.com/b/josebda/archive/2013/07/31/windows-server-2012-r2-storage-step-by-step-with-storage-spaces-smb-scale-out-and-shared-vhdx-physical.aspx).  

-   **Hyper-V à l’aide de volumes partagés de cluster.** Ce scénario nécessite les deux éléments suivants :  

    -   Cluster de calcul avec le rôle Hyper-V activé  

    -   Hyper-V avec des volumes partagés de cluster pour le stockage  

Un cluster de basculement est nécessaire. Tous les serveurs doivent exécuter la même version de Windows Server 2016.  

### <a name="BKMK_SolutionOverview"></a>Utilisation de la qualité de service de stockage dans une solution de stockage définie par logiciel  
La qualité de service de stockage est intégrée à la solution de stockage défini par le logiciel Microsoft et fournie par le serveur de fichiers avec montée en puissance parallèle et Hyper-V. Le serveur de fichiers avec montée en puissance parallèle expose les partages de fichiers pour les serveurs Hyper-V à l’aide du protocole SMB3. Un nouveau Gestionnaire de stratégie a été ajouté au cluster de serveurs de fichiers, qui fournit l’analyse des performances de stockage central.  

![Serveur de fichiers avec montée en puissance parallèle et qualité de service de stockage](media/overview-Clustering_SOFSStorageQoS.png)  

**Figure 1 : Utilisation de la qualité de service de stockage dans une solution de stockage définie par logiciel dans Serveur de fichiers avec montée en puissance parallèle @ no__t-0  

Quand les serveurs Hyper-V lancent des machines virtuelles, le Gestionnaire de stratégie les analyse. Le Gestionnaire de stratégie communique la stratégie de qualité de service de stockage ainsi que toutes les limites ou réserves au serveur Hyper-V, qui contrôle les performances de la machine virtuelle si besoin.  

Quand des modifications sont apportées aux stratégies de qualité de service de stockage ou aux exigences en matière de performances par les machines virtuelles, le Gestionnaire de stratégie demande aux serveurs Hyper-V d’adapter leur comportement. Cette boucle de commentaires garantit que tous les disques durs virtuels des machines virtuelles fonctionnent de façon cohérente conformément aux stratégies de qualité de service de stockage définies.  

### <a name="BKMK_Glossary"></a>Glossaire  

|Terme|Description|  
|--------|---------------|  
|E/S par seconde normalisées|Toute l’utilisation du stockage est mesurée en « E/S par seconde normalisées ».  Il s’agit du nombre d’opérations d’entrée/sortie de stockage par seconde.  Toute E/S d’une taille inférieure ou égale à 8 Ko est considérée comme étant une E/S normalisée.  Toute E/S d’une taille supérieure à 8 Ko est traitée comme représentant plusieurs E/S normalisées. Par exemple, une demande de 256 Ko est traitée comme 32 E/S par seconde normalisées.<br /><br />Windows Server 2016 inclut la possibilité de spécifier la taille utilisée pour normaliser les E/S.  Sur le cluster de stockage, la taille normalisée peut être spécifiée et prise en compte dans les calculs de normalisation à l’échelle du cluster.  La valeur par défaut demeure 8 Ko.|  
|Flux|Chaque descripteur de fichier ouvert par un serveur Hyper-V dans un fichier VHD ou VHDX est considéré comme un « flux ». Si une machine virtuelle est associée à deux disques durs virtuels, elle aura 1 flux vers le cluster de serveurs de fichiers par fichier. Si un VHDX est partagé avec plusieurs machines virtuelles, il aura 1 flux par machine virtuelle.|  
|InitiatorName|Nom de la machine virtuelle qui est indiqué au serveur de fichiers avec montée en puissance parallèle pour chaque flux.|  
|InitiatorID|Identificateur correspondant à l’ID de machine virtuelle.  Il peut toujours être utilisé pour identifier de façon unique les machines virtuelles des flux individuels même si les machines virtuelles ont le même InitiatorName.|  
|Stratégie|Les stratégies de qualité de service de stockage sont stockées dans la base de données de cluster et ont les propriétés suivantes : PolicyId, MinimumIOPS, MaximumIOPS, ParentPolicy et PolicyType.|  
|`PolicyId`|Identificateur unique pour une stratégie.  Il est généré par défaut, mais peut être spécifié si vous le souhaitez.|  
|MinimumIOPS|Nombre minimal d’opérations d’E/S par seconde normalisées qui sera fourni par une stratégie.  Le terme « réserve » est également employé.|  
|MaximumIOPS|Nombre maximal d’opérations d’E/S par seconde normalisées qui sera limité par une stratégie.  Le terme « limite » est également employé.|  
|Aggregated |Type de stratégie où les propriétés MinimumIOPS, MaximumIOPS et Bandwidth spécifiées sont partagées entre tous les flux affectés à la stratégie. Tous les disques durs virtuels auxquels la stratégie est appliquée sur ce système de stockage ont une seule allocation de bande passante d’E/S qu’ils doivent partager.|  
|Dedicated|Type de stratégie où les propriétés MinimumIOPS, MaximumIOPS et Bandwidth spécifiées sont gérées pour les VHD/VHDX individuels.|  

## <a name="BKMK_SetUpQoS"></a>Comment configurer la qualité de service de stockage et surveiller les performances de base  
Cette section décrit comment activer la nouvelle fonctionnalité de qualité de service de stockage et analyser les performances de stockage sans appliquer de stratégies personnalisées.  

### <a name="BKMK_SetupStorageQoSonStorageCluster"></a>Configurer la qualité de service de stockage sur un cluster de stockage  
Cette section explique comment activer la qualité de service de stockage sur un cluster de basculement nouveau ou existant et un serveur de fichiers avec montée en puissance parallèle qui exécute Windows Server 2016.  

#### <a name="set-up-storage-qos-on-a-new-installation"></a>Configurer la qualité de service de stockage sur une nouvelle installation  
Si vous avez configuré un nouveau cluster de basculement et un volume partagé de cluster sur Windows Server 2016, la fonctionnalité de qualité de service de stockage est automatiquement configurée.  

#### <a name="verify-storage-qos-installation"></a>Vérifier l’installation de la qualité de service de stockage  
Après avoir créé un cluster de basculement et configuré un disque CSV, **Ressource QoS du système de stockage** s’affiche comme ressource essentielle de cluster et est visible à la fois dans le Gestionnaire du cluster de basculement et Windows PowerShell. L’objectif est que le système de cluster de basculement gère cette ressource et que vous n’ayez aucune action à effectuer par rapport à elle.  Nous l’affichons dans le Gestionnaire du cluster de basculement et PowerShell pour être cohérents avec les autres ressources du système de cluster de basculement telles que le nouveau service d’intégrité.  

![Ressource QoS du système de stockage apparaît dans Principales ressources de cluster](media/overview-Clustering_StorageQoSFCM.png)  

**Figure 2: Ressource QoS de stockage affichée en tant que ressource principale du cluster dans Gestionnaire du cluster de basculement @ no__t-0  

Utilisez l’applet de commande PowerShell suivante pour afficher l’état de Ressource QoS du système de stockage.  

```PowerShell  
PS C:\> Get-ClusterResource -Name "Storage Qos Resource"  

Name                   State      OwnerGroup        ResourceType                 
----                   -----      ----------        ------------                 
Storage Qos Resource   Online     Cluster Group     Storage QoS Policy Manager  
```  

### <a name="BKMK_SetupStorageQoSonComputeCluster"></a>Configurer la qualité de service de stockage sur un cluster de calcul  
Le rôle Hyper-V dans Windows Server 2016 dispose de la prise en charge intégrée de la qualité de service de stockage et est activé par défaut.  

#### <a name="install-remote-administration-tools-to-manage-storage-qos-policies-from-remote-computers"></a>Installer des outils d’administration à distance pour gérer les stratégies de qualité de service de stockage à partir d’ordinateurs distants  
Vous pouvez gérer les stratégies de qualité de service de stockage et analyser des flux à partir des hôtes de calcul à l’aide des outils d’administration de serveur distant.  Ceux-ci sont disponibles en tant que fonctionnalités facultatives sur toutes les installations Windows Server 2016 et peuvent être téléchargés séparément pour Windows 10 sur le site web [Centre de téléchargement Microsoft](https://www.microsoft.com/download/details.aspx?id=45520).

La fonctionnalité facultative **RSAT-Clustering** inclut le module Windows PowerShell pour la gestion à distance du clustering de basculement, y compris la qualité de service de stockage.  

-   Windows PowerShell : Add-WindowsFeature RSAT-clustering  

La fonctionnalité facultative **RSAT-Hyper-V-Tools** inclut le module Windows PowerShell pour la gestion à distance d’Hyper-V.  

-   Windows PowerShell : Add-WindowsFeature RSAT-Hyper-V-Tools  

#### <a name="deploy-virtual-machines-to-run-workloads-for-testing"></a>Déployer des machines virtuelles pour exécuter des charges de travail de test  
Quelques machines virtuelles doivent être stockées sur le serveur de fichiers avec montée en puissance parallèle avec les charges de travail appropriées.  Pour obtenir des conseils sur la façon de simuler la charge et d’effectuer des tests de contrainte, consultez la page suivante pour un outil recommandé (DiskSpd) et un exemple d’utilisation : [DiskSpd, PowerShell et les performances de stockage : mesure de l’e/s par seconde, du débit et de la latence pour les disques locaux et les partages de fichiers SMB.](http://blogs.technet.com/b/josebda/archive/2014/10/13/diskspd-powershell-and-storage-performance-measuring-iops-throughput-and-latency-for-both-local-disks-and-smb-file-shares.aspx)  

Les exemples de scénarios présentés dans ce guide comprennent cinq machines virtuelles. BuildVM1, BuildVM2, BuildVM3 et BuildVM4 exécutent une charge de travail de bureau avec des demandes de stockage faibles à modérées. TestVm1 exécute un test d’évaluation de traitement transactionnel en ligne avec une demande de stockage élevée.  

### <a name="view-current-storage-performance-metrics"></a>Afficher les mesures de performances du stockage actuel  
Cette section comprend :  

-   Comment interroger les flux en utilisant l’applet de commande `Get-StorageQosFlow`.  

-   Comment afficher les performances d’un volume en utilisant l’applet de commande `Get-StorageQosVolume`.  

#### <a name="query-flows-using-the-get-storageqosflow-cmdlet"></a>Interroger les flux en utilisant l’applet de commande Get-StorageQosFlow  

L’applet de commande Get-StorageQosFlow affiche tous les flux actifs lancés par les serveurs Hyper-V. Toutes les données étant collectées par le cluster de serveurs de fichiers avec montée en puissance parallèle, l’applet de commande peut être utilisée sur n’importe quel nœud de ce cluster ou sur un serveur distant en utilisant le paramètre -CimSession.  

**L’exemple de commande suivant montre comment afficher tous les fichiers ouverts par Hyper-V sur le serveur à l’aide de Get-StorageQoSFlow**.  

```PowerShell
PS C:\> Get-StorageQosFlow  

InitiatorName    InitiatorNodeNam StorageNodeName  FilePath        Status  
                 e  
-------------    ---------------- ---------------  --------        ------  
                                  plang-fs3.pla... C:\ClusterSt... Ok  
                                  plang-fs2.pla... C:\ClusterSt... Ok  
                                  plang-fs1.pla... C:\ClusterSt... Ok  
                                  plang-fs3.pla... C:\ClusterSt... Ok  
                                  plang-fs2.pla... C:\ClusterSt... Ok  
                                  plang-fs1.pla... C:\ClusterSt... Ok  
TR20-VMM         plang-z400.pl... plang-fs1.pla... C:\ClusterSt... Ok  
BuildVM4         plang-c2.plan... plang-fs1.pla... C:\ClusterSt... Ok  
WinOltp1         plang-c1.plan... plang-fs1.pla... C:\ClusterSt... Ok  
BuildVM3         plang-c2.plan... plang-fs1.pla... C:\ClusterSt... Ok  
BuildVM1         plang-c2.plan... plang-fs1.pla... C:\ClusterSt... Ok  
TR20-VMM         plang-z400.pl... plang-fs1.pla... C:\ClusterSt... Ok  
BuildVM2         plang-c2.plan... plang-fs1.pla... C:\ClusterSt... Ok  
TR20-VMM         plang-z400.pl... plang-fs1.pla... C:\ClusterSt... Ok  
                                  plang-fs3.pla... C:\ClusterSt... Ok  
                                  plang-fs2.pla... C:\ClusterSt... Ok  
BuildVM4         plang-c2.plan... plang-fs2.pla... C:\ClusterSt... Ok  
WinOltp1         plang-c1.plan... plang-fs2.pla... C:\ClusterSt... Ok  
BuildVM3         plang-c2.plan... plang-fs2.pla... C:\ClusterSt... Ok  
WinOltp1         plang-c1.plan... plang-fs2.pla... C:\ClusterSt... Ok  
                                  plang-fs1.pla... C:\ClusterSt... Ok  
```  

L’exemple de commande suivant est mis en forme pour afficher le nom de la machine virtuelle, le nom d’hôte Hyper-V, les E/S par seconde et le nom de fichier VHD, triés par E/S par seconde.  

```PowerShell  
PS C:\> Get-StorageQosFlow | Sort-Object StorageNodeIOPs -Descending | ft InitiatorName, @{Expression={$_.InitiatorNodeName.Substring(0,$_.InitiatorNodeName.IndexOf('.'))};Label="InitiatorNodeName"}, StorageNodeIOPs, Status, @{Expression={$_.FilePath.Substring($_.FilePath.LastIndexOf('\')+1)};Label="File"} -AutoSize  

InitiatorName InitiatorNodeName StorageNodeIOPs Status File  
------------- ----------------- --------------- ------ ----  
WinOltp1      plang-c1                     3482     Ok IOMETER.VHDX  
BuildVM2      plang-c2                      544     Ok BUILDVM2.VHDX  
BuildVM1      plang-c2                      497     Ok BUILDVM1.VHDX  
BuildVM4      plang-c2                      316     Ok BUILDVM4.VHDX  
BuildVM3      plang-c2                      296     Ok BUILDVM3.VHDX  
BuildVM4      plang-c2                      195     Ok WIN8RTM_ENTERPRISE_VL_BU...  
TR20-VMM      plang-z400                    156     Ok DATA1.VHDX  
BuildVM3      plang-c2                       81     Ok WIN8RTM_ENTERPRISE_VL_BU...  
WinOltp1      plang-c1                       65     Ok BOOT.VHDX  
                                             18     Ok DefaultFlow  
                                             12     Ok DefaultFlow  
WinOltp1      plang-c1                        4     Ok 9914.0.AMD64FRE.WINMAIN....  
TR20-VMM      plang-z400                      4     Ok DATA2.VHDX  
TR20-VMM      plang-z400                      3     Ok BOOT.VHDX  
                                              0     Ok DefaultFlow  
                                              0     Ok DefaultFlow  
                                              0     Ok DefaultFlow  
                                              0     Ok DefaultFlow  
                                              0     Ok DefaultFlow  
                                              0     Ok DefaultFlow  
                                              0     Ok DefaultFlow  
```  

L’exemple de commande suivant montre comment filtrer les flux en fonction de la valeur InitiatorName pour trouver facilement les paramètres et les performances de stockage pour une machine virtuelle donnée.  

```PowerShell
PS C:\> Get-StorageQosFlow -InitiatorName BuildVm1 | Format-List

FilePath           : C:\ClusterStorage\Volume2\SHARES\TWO\BUILDWORKLOAD\BUILDVM1.V  
                     HDX  
FlowId             : ebfecb54-e47a-5a2d-8ec0-0940994ff21c  
InitiatorId        : ae4e3dd0-3bde-42ef-b035-9064309e6fec  
InitiatorIOPS      : 464  
InitiatorLatency   : 26.2684  
InitiatorName      : BuildVM1  
InitiatorNodeName  : plang-c2.plang.nttest.microsoft.com  
Interval           : 300000  
Limit              : 500  
PolicyId           : b145e63a-3c9e-48a8-87f8-1dfc2abfe5f4  
Reservation        : 500  
Status             : Ok  
StorageNodeIOPS    : 475  
StorageNodeLatency : 7.9725  
StorageNodeName    : plang-fs1.plang.nttest.microsoft.com  
TimeStamp          : 2/12/2015 2:58:49 PM  
VolumeId           : 4d91fc3a-1a1e-4917-86f6-54853b2a6787  
PSComputerName     :  
MaximumIops        : 500  
MinimumIops        : 500  
```  

Les données retournées par l’applet de commande `Get-StorageQosFlow` sont notamment les suivantes :  

-   Nom d’hôte Hyper-V (InitiatorNodeName)  

-   Nom de la machine virtuelle et son ID (InitiatorName et InitiatorId)  

-   Performances moyennes récentes telles qu’elles ont été observées par l’hôte Hyper-V pour le disque virtuel (InitiatorIOPS, InitiatorLatency)  

-   Performances moyennes récentes telles qu’elles ont été observées par le cluster de stockage pour le disque virtuel (StorageNodeIOPS, StorageNodeLatency)  

-   Stratégie actuelle appliquée au fichier, le cas échéant, et configuration obtenue (PolicyId, Reservation, Limit)  

-   État de la stratégie  

    -   **OK** : il n’existe aucun problème  

    -   InsufficientThroughput : une stratégie est appliquée, mais le nombre minimal d’opérations d’E/S par seconde ne peut pas être fourni.  Cela peut se produire si le nombre minimal pour une machine virtuelle, ou toutes les machines virtuelles réunies, est supérieur à la capacité du volume de stockage.  

    -   **UnknownPolicyId** : une stratégie a été attribuée à la machine virtuelle sur l’hôte Hyper-V, mais ne figure pas sur le serveur de fichiers.  Cette stratégie doit être supprimée de la configuration de la machine virtuelle, ou une stratégie correspondante doit être créée sur le cluster de serveurs de fichiers.  

#### <a name="view-performance-for-a-volume-using-get-storageqosvolume"></a>Afficher les performances d’un volume en utilisant Get-StorageQosVolume  
Les mesures de performances de stockage sont aussi collectées à un niveau de volume par stockage, en plus des mesures de performances par flux.  Il est ainsi facile de voir l’utilisation totale moyenne en termes d’E/S par seconde normalisées, de latence, et de réserves et de limites d’agrégats appliquées à un volume.  

```PowerShell
PS C:\> Get-StorageQosVolume | Format-List  

Interval       : 300000  
IOPS           : 0  
Latency        : 0  
Limit          : 0  
Reservation    : 0  
Status         : Ok  
TimeStamp      : 2/12/2015 2:59:38 PM  
VolumeId       : 434f561f-88ae-46c0-a152-8c6641561504  
PSComputerName :  
MaximumIops    : 0  
MinimumIops    : 0  

Interval       : 300000  
IOPS           : 1097  
Latency        : 3.1524  
Limit          : 0  
Reservation    : 1568  
Status         : Ok  
TimeStamp      : 2/12/2015 2:59:38 PM  
VolumeId       : 4d91fc3a-1a1e-4917-86f6-54853b2a6787  
PSComputerName :  
MaximumIops    : 0  
MinimumIops    : 1568  

Interval       : 300000  
IOPS           : 5354  
Latency        : 6.5084  
Limit          : 0  
Reservation    : 781  
Status         : Ok  
TimeStamp      : 2/12/2015 2:59:38 PM  
VolumeId       : 0d2fd367-8d74-4146-9934-306726913dda  
PSComputerName :  
MaximumIops    : 0  
MinimumIops    : 781  
```  

## <a name="BKMK_CreateQoSPolicies"></a>Comment créer et surveiller des stratégies QoS de stockage  
Cette section décrit comment créer des stratégies de qualité de service de stockage, appliquer ces stratégies à des machines virtuelles et analyser un cluster de stockage après l’application des stratégies.  

### <a name="create-storage-qos-policies"></a>Créer des stratégies de qualité de service de stockage  
Les stratégies de qualité de service de stockage sont définies et gérées dans le cluster de serveurs de fichiers avec montée en puissance parallèle.  Vous pouvez créer autant de stratégies que nécessaire pour des déploiements flexibles (jusqu’à 10 000 par cluster de stockage).  

Chaque fichier VHD/VHDX affecté à une machine virtuelle peut être configuré avec une stratégie. Différents fichiers et machines virtuelles peuvent utiliser la même stratégie, ou chacun peut être configuré avec des stratégies distinctes.  Si plusieurs fichiers VHD/VHDX ou machines virtuelles sont configurés avec la même stratégie, ils sont regroupés et partagent de façon équitable MinimumIOPS et MaximumIOPS. Si vous utilisez des stratégies distinctes pour plusieurs fichiers VHD/VHDX ou machines virtuelles, les valeurs maximales et minimales sont suivies séparément pour chaque.  

Si vous créez plusieurs stratégies similaires pour différentes machines virtuelles et que celles-ci ont des demandes de stockage identiques, elles reçoivent le même partage d’E/S par seconde.  Si une machine virtuelle demande plus et l’autre moins, les E/S par seconde s’adaptent à la demande.  

### <a name="types-of-storage-qos-policies"></a>Types de stratégies de qualité de service de stockage  
Il existe deux types de stratégies : Agrégatd (précédemment appelé SingleInstance) et dédié (précédemment connu sous le nom de MultiInstance). Les stratégies Aggregated appliquent les valeurs maximales et minimales pour l’ensemble combiné de fichiers VHD/VHDX et de machines virtuelles auquel elles s’appliquent. En réalité, elles partagent un ensemble défini d’E/S par seconde et de bande passante. Les stratégies Dedicated appliquent les valeurs maximales et minimales pour chaque VHD/VHDX, séparément. Il est ainsi facile de créer une stratégie unique qui applique des limites similaires à plusieurs fichiers VHD/VHDX.  

Créez par exemple une stratégie Aggregated avec une valeur minimale de 300 E/S par seconde et une valeur maximale de 500 E/S par seconde. Si vous appliquez cette stratégie à 5 fichiers VHD/VHDX différents, vous vous assurez que les 5 fichiers VHD/VHDX combinés obtiennent au moins 300 E/S par seconde (en cas de demande et si le système de stockage peut y répondre) et pas plus de 500 E/S par seconde. Si les fichiers VHD/VHDX ont la même demande élevée d’E/S par seconde et que le système de stockage peut la satisfaire, chaque fichier VHD/VHDX obtient environ 100 E/S par seconde.  

Toutefois, si vous créez une stratégie Dedicated avec des limites identiques et l’appliquez aux fichiers VHD/VHDX sur 5 machines virtuelles différentes, chacune obtient au moins 300 E/S par seconde et pas plus de 500 E/S par seconde. Si les machines virtuelles ont la même demande élevée d’E/S par seconde et que le système de stockage peut la satisfaire, chaque machine virtuelle obtient environ 500 E/S par seconde. .  Si l’une des machines virtuelles a plusieurs fichiers VHD/VHDX avec la même stratégie MultiInstance configurée, elle partage le nombre total d’E/S de ses fichiers ayant cette stratégie pour ne pas dépasser les limites.  

Par conséquent, si vous avez un groupe de fichiers VHD/VHDX qui doivent présenter les mêmes caractéristiques de performances et que vous ne voulez pas prendre la peine de créer plusieurs stratégies similaires, vous pouvez utiliser une stratégie Dedicated unique et l’appliquer aux fichiers de chaque machine virtuelle.

Gardez le nombre de fichiers VHD/VHDx affectés à une stratégie agrégée unique à 20 ou moins.  Ce type de stratégie a été conçu pour effectuer une agrégation avec quelques machines virtuelles sur un cluster.

### <a name="create-and-apply-a-dedicated-policy"></a>Créer et appliquer une stratégie Dedicated  
Tout d’abord, utilisez l’applet de commande `New-StorageQosPolicy` pour créer une stratégie sur le serveur de fichiers avec montée en puissance parallèle, comme illustré dans l’exemple suivant :  

```PowerShell
$desktopVmPolicy = New-StorageQosPolicy -Name Desktop -PolicyType Dedicated -MinimumIops 100 -MaximumIops 200  
```  

Appliquez-la ensuite aux lecteurs de disque dur des machines virtuelles appropriées sur le serveur Hyper-V.  Notez le PolicyId de l’étape précédente ou stockez-le dans une variable dans vos scripts.  

Sur le serveur de fichiers avec montée en puissance parallèle, à l’aide de PowerShell, créez une stratégie de qualité de service de stockage et obtenez son ID de stratégie, comme illustré dans l’exemple suivant :  

```PowerShell
PS C:\> $desktopVmPolicy = New-StorageQosPolicy -Name Desktop -PolicyType Dedicated -MinimumIops 100 -MaximumIops 200  

C:\> $desktopVmPolicy.PolicyId  

Guid  
----  
cd6e6b87-fb13-492b-9103-41c6f631f8e0  
```  

Sur le serveur Hyper-V, à l’aide de PowerShell, définissez la stratégie de qualité de service de stockage en utilisant l’ID de stratégie, comme illustré dans l’exemple suivant :  

```PowerShell
Get-VM -Name Build* | Get-VMHardDiskDrive | Set-VMHardDiskDrive -QoSPolicyID cd6e6b87-fb13-492b-9103-41c6f631f8e0  
```  

### <a name="confirm-that-the-policies-are-applied"></a>Confirmer que les stratégies sont appliquées  
Utilisez l’applet de commande `Get-StorageQosFlow` PowerShell pour confirmer que les propriétés MinimumIOPs et MaximumIOPs ont été appliquées aux flux appropriés, comme illustré dans l’exemple suivant.  

```PowerShell
PS C:\> Get-StorageQoSflow | Sort-Object InitiatorName |  
 ft InitiatorName, Status, MinimumIOPs, MaximumIOPs, StorageNodeIOPs, Status, @{Expression={$_.FilePath.Substring($_.FilePath.LastIndexOf('\')+1)};Label="File"} -AutoSize  

InitiatorName Status MinimumIops MaximumIops StorageNodeIOPs Status File  
------------- ------ ----------- ----------- --------------- ------ ----  
BuildVM1          Ok         100         200             250     Ok BUILDVM1.VHDX  
BuildVM2          Ok         100         200             251     Ok BUILDVM2.VHDX  
BuildVM3          Ok         100         200             252     Ok BUILDVM3.VHDX  
BuildVM4          Ok         100         200             233     Ok BUILDVM4.VHDX  
TR20-VMM          Ok          33         666               1     Ok DATA2.VHDX  
TR20-VMM          Ok          33         666               5     Ok DATA1.VHDX  
TR20-VMM          Ok          33         666               4     Ok BOOT.VHDX  
WinOltp1          Ok           0           0               0     Ok 9914.0.AMD6...  
WinOltp1          Ok           0           0            5166     Ok IOMETER.VHDX  
WinOltp1          Ok           0           0               0     Ok BOOT.VHDX  
```  

Sur le serveur Hyper-V, vous pouvez également utiliser le script fourni **Get-VMHardDiskDrivePolicy.ps1** pour identifier la stratégie appliquée à un lecteur de disque dur virtuel.  

```PowerShell
PS C:\> Get-VM -Name BuildVM1 | Get-VMHardDiskDrive | Format-List  

Path                          : \\plang-fs.plang.nttest.microsoft.com\two\BuildWorkload  
                                \BuildVM1.vhdx  
DiskNumber                    :  
MaximumIOPS                   : 0  
MinimumIOPS                   : 0  
QoSPolicyID                   : cd6e6b87-fb13-492b-9103-41c6f631f8e0  
SupportPersistentReservations : False  
ControllerLocation            : 0  
ControllerNumber              : 0  
ControllerType                : IDE  
PoolName                      : Primordial  
Name                          : Hard Drive  
Id                            : Microsoft:AE4E3DD0-3BDE-42EF-B035-9064309E6FEC\83F8638B  
                                -8DCA-4152-9EDA-2CA8B33039B4\0\0\D  
VMId                          : ae4e3dd0-3bde-42ef-b035-9064309e6fec  
VMName                        : BuildVM1  
VMSnapshotId                  : 00000000-0000-0000-0000-000000000000  
VMSnapshotName                :  
ComputerName                  : PLANG-C2  
IsDeleted                     : False  
```  

### <a name="query-for-storage-qos-policies"></a>Rechercher des stratégies de qualité de service de stockage  
`Get-StorageQosPolicy` répertorie toutes les stratégies configurées et leur état sur un Serveur de fichiers avec montée en puissance parallèle.  

```PowerShell
PS C:\> Get-StorageQosPolicy  

Name                 MinimumIops          MaximumIops          Status  
----                 -----------          -----------          ------  
Default              0                    0                    Ok  
Limit500             0                    500                  Ok  
SilverVm             500                  500                  Ok  
Desktop              100                  200                  Ok  
Limit500             0                    0                    Ok  
VMM                  100                  2000                 Ok  
Vdi                  1                    100                  Ok  
```  

L’état peut changer au fil du temps, selon le fonctionnement du système.  

-   **OK** : tous les flux utilisant cette stratégie reçoivent le nombre minimal d’opérations d’E/S par seconde demandé.  

-   **InsufficientThroughput** : un ou plusieurs des flux utilisant cette stratégie ne reçoivent pas le nombre minimal d’opérations d’E/S par seconde  

Vous pouvez également diriger une stratégie vers `Get-StorageQosPolicy` pour obtenir l’état de tous les flux configurés pour utiliser la stratégie comme suit :  

```PowerShell
PS C:\> Get-StorageQosPolicy -Name Desktop | Get-StorageQosFlow | ft InitiatorName, *IOPS, Status, FilePath -AutoSize  

InitiatorName MaximumIops MinimumIops InitiatorIOPS StorageNodeIOPS Status FilePat  
                                                                           h  
------------- ----------- ----------- ------------- --------------- ------ -------  
BuildVM4              100          50           187              17     Ok C:\C...  
BuildVM3              100          50           194              25     Ok C:\C...  
BuildVM1              200         100           195             196     Ok C:\C...  
BuildVM2              200         100           193             192     Ok C:\C...  
BuildVM4              200         100           187             169     Ok C:\C...  
BuildVM3              200         100           194             169     Ok C:\C...  
```  

### <a name="create-an-aggregated-policy"></a>Créer une stratégie Aggregated  
Les stratégies Aggregated peuvent être utilisées si vous souhaitez que plusieurs disques durs virtuels partagent un pool unique d’E/S par seconde et de bande passante.  Par exemple, si vous appliquez la même stratégie Aggregated aux disques durs de deux machines virtuelles, la valeur minimale est répartie entre les deux en fonction de la demande.  Un minimum combiné est garanti pour les deux disques. S’ils sont réunis, ils ne dépassent pas le nombre maximal d’opérations d’E/S par seconde ou la bande passante indiqués.  

La même approche peut également servir à fournir une allocation unique à tous les fichiers VHD/VHDX pour les machines virtuelles comprenant un service ou appartenant à un locataire dans un environnement hébergé à plusieurs endroits.  

Il n’existe aucune différence dans le processus de création des stratégies Dedicated et Aggregated si ce n’est l’élément PolicyType spécifié.  

L’exemple suivant montre comment créer une stratégie de qualité de service de stockage Aggregated et obtenir son policyID sur un serveur de fichiers avec montée en puissance parallèle :  

```PowerShell
PS C:\> $highPerf = New-StorageQosPolicy -Name SqlWorkload -MinimumIops 1000 -MaximumIops 5000 -PolicyType Aggregated  
[plang-fs]: PS C:\Users\plang\Documents> $highPerf.PolicyId  

Guid  
----  
7e2f3e73-1ae4-4710-8219-0769a4aba072  
```  

L’exemple suivant montre comment appliquer la stratégie de qualité de service de stockage sur le serveur Hyper-V à l’aide du policyID obtenu dans l’exemple précédent :  

```PowerShell
PS C:\> Get-VM -Name WinOltp1 | Get-VMHardDiskDrive | Set-VMHardDiskDrive -QoSPolicyID 7e2f3e73-1ae4-4710-8219-0769a4aba072  
```  

L’exemple suivant montre comment afficher les effets de la stratégie de qualité de service de stockage du serveur de fichiers :  

```PowerShell
PS C:\> Get-StorageQosFlow -InitiatorName WinOltp1 | format-list InitiatorName, PolicyId, MinimumIOPs, MaximumIOPs, StorageNodeIOPs, FilePath  

InitiatorName   : WinOltp1  
PolicyId        : 7e2f3e73-1ae4-4710-8219-0769a4aba072  
MinimumIops     : 250  
MaximumIops     : 1250  
StorageNodeIOPs : 0  
FilePath        : C:\ClusterStorage\Volume2\SHARES\TWO\BASEVHD\9914.0.AMD64FRE.WIN  
                  MAIN.141218-1718_SERVER_SERVERDATACENTER_EN-US.VHDX  

InitiatorName   : WinOltp1  
PolicyId        : 7e2f3e73-1ae4-4710-8219-0769a4aba072  
MinimumIops     : 250  
MaximumIops     : 1250  
StorageNodeIOPs : 0  
FilePath        : C:\ClusterStorage\Volume3\SHARES\THREE\WINOLTP1\BOOT.VHDX  

InitiatorName   : WinOltp1  
PolicyId        : 7e2f3e73-1ae4-4710-8219-0769a4aba072  
MinimumIops     : 1000  
MaximumIops     : 5000  
StorageNodeIOPs : 4550  
FilePath        : C:\ClusterStorage\Volume3\SHARES\THREE\WINOLTP1\IOMETER.VHDX  
PS C:\> Get-StorageQosFlow -InitiatorName WinOltp1 | for  
mat-list InitiatorName, PolicyId, MinimumIOPs, MaximumIOPs, StorageNodeIOPs, FilePath  

InitiatorName   : WinOltp1  
PolicyId        : 7e2f3e73-1ae4-4710-8219-0769a4aba072  
MinimumIops     : 250  
MaximumIops     : 1250  
StorageNodeIOPs : 0  
FilePath        : C:\ClusterStorage\Volume2\SHARES\TWO\BASEVHD\9914.0.AMD64FRE.WIN  
                  MAIN.141218-1718_SERVER_SERVERDATACENTER_EN-US.VHDX  

InitiatorName   : WinOltp1  
PolicyId        : 7e2f3e73-1ae4-4710-8219-0769a4aba072  
MinimumIops     : 250  
MaximumIops     : 1250  
StorageNodeIOPs : 0  
FilePath        : C:\ClusterStorage\Volume3\SHARES\THREE\WINOLTP1\BOOT.VHDX  

InitiatorName   : WinOltp1  
PolicyId        : 7e2f3e73-1ae4-4710-8219-0769a4aba072  
MinimumIops     : 1000  
MaximumIops     : 5000  
StorageNodeIOPs : 4550  
FilePath        : C:\ClusterStorage\Volume3\SHARES\THREE\WINOLTP1\IOMETER.VHDX  
```  

Les valeurs MinimumIOPs, MaximumIOPs et MaximumIobandwidth de chaque disque dur virtuel sont ajustées en fonction de sa charge.  Cela garantit que la quantité totale de bande passante utilisée pour le groupe de disques reste dans la plage définie par la stratégie.  Dans l’exemple ci-dessus, les deux premiers disques sont inactifs et le troisième est autorisé à utiliser le nombre maximal d’opérations d’E/S par seconde.  Si les deux premiers disques recommencent à émettre des E/S, le nombre maximal d’opérations d’E/S par seconde du troisième disque est automatiquement réduit.  

### <a name="modify-an-existing-policy"></a>Modifier une stratégie existante  
Les propriétés de Name, MinimumIOPs, MaximumIOPs et MaximumIoBandwidth peuvent être modifiées après la création d’une stratégie.  Toutefois, le type de stratégie (Aggregated/Dedicated) ne peut pas être modifié une fois la stratégie créée.  

L’applet de commande Windows PowerShell suivante montre comment modifier la propriété MaximumIOPs pour une stratégie existante :  

```PowerShell
[DBG]: PS C:\demoscripts>> Get-StorageQosPolicy -Name SqlWorkload | Set-StorageQosPolicy -MaximumIops 6000  
```  

L’applet de commande suivante vérifie la modification :  

```PowerShell
PS C:\> Get-StorageQosPolicy -Name SqlWorkload  

Name                    MinimumIops            MaximumIops            Status  
----                    -----------            -----------            ------  
SqlWorkload             1000                   6000                   Ok    

[plang-fs1]: PS C:\Users\plang\Documents> Get-StorageQosPolicy -Name SqlWorkload | Get-Storag  
eQosFlow | Format-Table InitiatorName, PolicyId, MaximumIops, MinimumIops, StorageNodeIops -A  
utoSize  

InitiatorName PolicyId                             MaximumIops MinimumIops StorageNodeIops  
------------- --------                             ----------- ----------- ---------------  
WinOltp1      7e2f3e73-1ae4-4710-8219-0769a4aba072        1500         250               0  
WinOltp1      7e2f3e73-1ae4-4710-8219-0769a4aba072        1500         250               0  
WinOltp1      7e2f3e73-1ae4-4710-8219-0769a4aba072        6000        1000            4507  
```  

## <a name="BKMK_KnownIssues"></a>Comment identifier et résoudre les problèmes courants  
Cette section décrit comment rechercher des machines virtuelles avec des stratégies de qualité de service de stockage non valides, comment recréer une stratégie correspondante, comment supprimer une stratégie d’une machine virtuelle et comment identifier les machines virtuelles qui ne répondent pas aux critères de la stratégie de qualité de service de stockage.  

### <a name="BKMK_FindingVMsWithInvalidPolicies"></a>Identifier les machines virtuelles avec des stratégies non valides  

Si une stratégie est supprimée du serveur de fichiers avant d’être supprimée d’une machine virtuelle, la machine virtuelle continue de fonctionner comme si aucune stratégie n’avait été appliquée.  

```PowerShell
PS C:\> Get-StorageQosPolicy -Name SqlWorkload | Remove-StorageQosPolicy  

Confirm  
Are you sure you want to perform this action?  
Performing the operation "DeletePolicy" on target "MSFT_StorageQoSPolicy (PolicyId =  
"7e2f3e73-1ae4-4710-8219-0769a4aba072")".  
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [?] Help (default is "Y"):  
```  

L’état relatif aux flux affiche désormais « UnknownPolicyId »  

```PowerShell
PS C:\> Get-StorageQoSflow | Sort-Object InitiatorName | ft InitiatorName, Status, MinimumIOPs, MaximumIOPs, StorageNodeIOPs, Status, @{Expression={$_.FilePath.Substring($_.FilePath.LastIndexOf('\')+1)};Label="File"} -AutoSize  

InitiatorName          Status MinimumIops MaximumIops StorageNodeIOPs          Status File  
-------------          ------ ----------- ----------- ---------------          ------ ----  
                           Ok           0           0               0              Ok Def...  
                           Ok           0           0              10              Ok Def...  
                           Ok           0           0              13              Ok Def...  
                           Ok           0           0               0              Ok Def...  
                           Ok           0           0               0              Ok Def...  
                           Ok           0           0               0              Ok Def...  
                           Ok           0           0               0              Ok Def...  
                           Ok           0           0               0              Ok Def...  
                           Ok           0           0               0              Ok Def...  
BuildVM1                   Ok         100         200             193              Ok BUI...  
BuildVM2                   Ok         100         200             196              Ok BUI...  
BuildVM3                   Ok          50          64              17              Ok WIN...  
BuildVM3                   Ok          50         136             179              Ok BUI...  
BuildVM4                   Ok          50         100              23              Ok WIN...  
BuildVM4                   Ok         100         200             173              Ok BUI...  
TR20-VMM                   Ok          33         666               2              Ok DAT...  
TR20-VMM                   Ok          25         975               3              Ok DAT...  
TR20-VMM                   Ok          75        1025              12              Ok BOO...  
WinOltp1      UnknownPolicyId           0           0               0 UnknownPolicyId 991...  
WinOltp1      UnknownPolicyId           0           0            4926 UnknownPolicyId IOM...  
WinOltp1      UnknownPolicyId           0           0               0 UnknownPolicyId BOO...  
```  

#### <a name="BKMK_RecreateMatchingPolicy"></a>Recréer une stratégie de qualité de service de stockage correspondante  
Si une stratégie a été accidentellement supprimée, vous pouvez en créer une autre à l’aide de l’ancien PolicyId.  Tout d’abord, obtenez le PolicyId nécessaire  

```PowerShell
PS C:\> Get-StorageQosFlow -Status UnknownPolicyId | ft InitiatorName, PolicyId -AutoSize  

InitiatorName PolicyId  
------------- --------  
WinOltp1      7e2f3e73-1ae4-4710-8219-0769a4aba072  
WinOltp1      7e2f3e73-1ae4-4710-8219-0769a4aba072  
WinOltp1      7e2f3e73-1ae4-4710-8219-0769a4aba072  
```  

Ensuite, créez une stratégie à l’aide de ce PolicyId  

```PowerShell
PS C:\> New-StorageQosPolicy -PolicyId 7e2f3e73-1ae4-4710-8219-0769a4aba072 -PolicyType Aggregated -Name RestoredPolicy -MinimumIops 100 -MaximumIops 2000  

Name                    MinimumIops            MaximumIops            Status  
----                    -----------            -----------            ------  
RestoredPolicy          100                    2000                   Ok  
```  

Enfin, vérifiez qu’elle a été appliquée.  

```PowerShell
PS C:\> Get-StorageQoSflow | Sort-Object InitiatorName | ft InitiatorName, Status, MinimumIOPs, MaximumIOPs, StorageNodeIOPs, Status, @{Expression={$_.FilePath.Substring($_.FilePath.LastIndexOf('\')+1)};Label="File"} -AutoSize  

InitiatorName Status MinimumIops MaximumIops StorageNodeIOPs Status File  
------------- ------ ----------- ----------- --------------- ------ ----  
                  Ok           0           0               0     Ok DefaultFlow  
                  Ok           0           0               8     Ok DefaultFlow  
                  Ok           0           0               9     Ok DefaultFlow  
                  Ok           0           0               0     Ok DefaultFlow  
                  Ok           0           0               0     Ok DefaultFlow  
                  Ok           0           0               0     Ok DefaultFlow  
                  Ok           0           0               0     Ok DefaultFlow  
                  Ok           0           0               0     Ok DefaultFlow  
                  Ok           0           0               0     Ok DefaultFlow  
BuildVM1          Ok         100         200             192     Ok BUILDVM1.VHDX  
BuildVM2          Ok         100         200             193     Ok BUILDVM2.VHDX  
BuildVM3          Ok          50         100              24     Ok WIN8RTM_ENTERPRISE_VL...  
BuildVM3          Ok         100         200             166     Ok BUILDVM3.VHDX  
BuildVM4          Ok          50         100              12     Ok WIN8RTM_ENTERPRISE_VL...  
BuildVM4          Ok         100         200             178     Ok BUILDVM4.VHDX  
TR20-VMM          Ok          33         666               2     Ok DATA2.VHDX  
TR20-VMM          Ok          33         666               2     Ok DATA1.VHDX  
TR20-VMM          Ok          33         666              10     Ok BOOT.VHDX  
WinOltp1          Ok          25         500               0     Ok 9914.0.AMD64FRE.WINMA...  
```  

#### <a name="BKMK_RemovePolicyFromVM"></a>Supprimer les stratégies QoS de stockage  

Si la stratégie a été intentionnellement supprimée, ou si une machine virtuelle a été importée avec une stratégie dont vous n’avez pas besoin, elle peut être supprimée.  

```PowerShell
PS C:\> Get-VM -Name WinOltp1 | Get-VMHardDiskDrive | Set-VMHardDiskDrive -QoSPolicyID $null  
```  

Une fois le PolicyId supprimé des paramètres du disque dur virtuel, l’état affiche « OK » et aucune valeur minimale ni maximale n’est appliquée.  

```PowerShell
PS C:\> Get-StorageQoSflow | Sort-Object InitiatorName | ft InitiatorName, MinimumIOPs, MaximumIOPs, StorageNodeIOPs, Status, @{Expression={$_.FilePath.Substring($_.FilePath.LastIndexOf('\')+1)};Label="File"} -AutoSize  

InitiatorName MinimumIops MaximumIops StorageNodeIOPs Status File  
------------- ----------- ----------- --------------- ------ ----  
                        0           0               0     Ok DefaultFlow  
                        0           0              16     Ok DefaultFlow  
                        0           0              12     Ok DefaultFlow  
                        0           0               0     Ok DefaultFlow  
                        0           0               0     Ok DefaultFlow  
                        0           0               0     Ok DefaultFlow  
                        0           0               0     Ok DefaultFlow  
                        0           0               0     Ok DefaultFlow  
                        0           0               0     Ok DefaultFlow  
BuildVM1              100         200             197     Ok BUILDVM1.VHDX  
BuildVM2              100         200             192     Ok BUILDVM2.VHDX  
BuildVM3                9           9              23     Ok WIN8RTM_ENTERPRISE_VL_BUILDW...  
BuildVM3               91         191             171     Ok BUILDVM3.VHDX  
BuildVM4                8           8              18     Ok WIN8RTM_ENTERPRISE_VL_BUILDW...  
BuildVM4               92         192             163     Ok BUILDVM4.VHDX  
TR20-VMM               33         666               2     Ok DATA2.VHDX  
TR20-VMM               33         666               1     Ok DATA1.VHDX  
TR20-VMM               33         666               5     Ok BOOT.VHDX  
WinOltp1                0           0               0     Ok 9914.0.AMD64FRE.WINMAIN.1412...  
WinOltp1                0           0            1811     Ok IOMETER.VHDX  
WinOltp1                0           0               0     Ok BOOT.VHDX  
```  

### <a name="BKMK_VMsThatDoNotMeetStorageQoSPoilicies"></a>Rechercher les machines virtuelles qui ne répondent pas aux stratégies QoS de stockage  
L’état **InsufficientThroughput** est affecté à n’importe quel flux qui :  

-   a un nombre minimal d’opérations d’E/S par seconde défini par une stratégie ; et  

-   lance des E/S à une fréquence supérieure ou égale à la valeur minimale ; et  

-   n’atteint pas la fréquence d’opérations d’E/S minimale.  

```PowerShell
PS C:\> Get-StorageQoSflow | Sort-Object InitiatorName | ft InitiatorName, MinimumIOPs, MaximumIOPs, StorageNodeIOPs, Status, @{Expression={$_.FilePath.Substring($_.FilePath.LastIndexOf('\')+1)};Label="File"} -AutoSize  

InitiatorName MinimumIops MaximumIops StorageNodeIOPs                 Status File  
------------- ----------- ----------- ---------------                 ------ ----  
                        0           0               0                     Ok DefaultFlow  
                        0           0               0                     Ok DefaultFlow  
                        0           0              15                     Ok DefaultFlow  
                        0           0               0                     Ok DefaultFlow  
                        0           0               0                     Ok DefaultFlow  
                        0           0               0                     Ok DefaultFlow  
                        0           0               0                     Ok DefaultFlow  
                        0           0               0                     Ok DefaultFlow  
                        0           0               0                     Ok DefaultFlow  
BuildVM3               50         100              20                     Ok WIN8RTM_ENTE...  
BuildVM3              100         200             174                     Ok BUILDVM3.VHDX  
BuildVM4               50         100              11                     Ok WIN8RTM_ENTE...  
BuildVM4              100         200             188                     Ok BUILDVM4.VHDX  
TR20-VMM               33         666               3                     Ok DATA1.VHDX  
TR20-VMM               78        1032             180                     Ok BOOT.VHDX  
TR20-VMM               22         968               4                     Ok DATA2.VHDX  
WinOltp1             3750        5000               0                     Ok 9914.0.AMD64...  
WinOltp1            15000       20000           11679 InsufficientThroughput IOMETER.VHDX  
WinOltp1             3750        5000               0                     Ok BOOT.VHDX  
```  

Vous pouvez déterminer des flux pour n’importe quel état, notamment **InsufficientThroughput**, comme illustré dans l’exemple suivant :  

```PowerShell
PS C:\> Get-StorageQosFlow -Status InsufficientThroughput | fl  

FilePath           : C:\ClusterStorage\Volume3\SHARES\THREE\WINOLTP1\IOMETER.VHDX  
FlowId             : 1ca356ff-fd33-5b5d-b60a-2c8659dc803e  
InitiatorId        : 2ceabcef-2eba-4f1b-9e66-10f960b50bbf  
InitiatorIOPS      : 12168  
InitiatorLatency   : 22.983  
InitiatorName      : WinOltp1  
InitiatorNodeName  : plang-c1.plang.nttest.microsoft.com  
Interval           : 300000  
Limit              : 20000  
PolicyId           : 5d1bf221-c8f0-4368-abcf-aa139e8a7c72  
Reservation        : 15000  
Status             : InsufficientThroughput  
StorageNodeIOPS    : 12181  
StorageNodeLatency : 22.0514  
StorageNodeName    : plang-fs2.plang.nttest.microsoft.com  
TimeStamp          : 2/13/2015 12:07:30 PM  
VolumeId           : 0d2fd367-8d74-4146-9934-306726913dda  
PSComputerName     :  
MaximumIops        : 20000  
MinimumIops        : 15000  
```  

## <a name="BKMK_Health"></a>Surveiller l’intégrité avec la qualité de service de stockage  
Le nouveau service d’intégrité simplifie l’analyse du cluster de stockage, en fournissant un emplacement unique pour rechercher des événements pouvant être actionnés dans les nœuds. Cette section décrit comment analyser l’intégrité de votre cluster de stockage à l’aide de l’applet de commande `debug-storagesubsystem`.  

### <a name="view-storage-status-with-debug-storagesubsystem"></a>Afficher l’état de stockage avec Debug-StorageSubSystem  
Les espaces de stockage en cluster fournissent également des informations sur l’intégrité du cluster de stockage à un emplacement unique. Cela peut aider les administrateurs à identifier rapidement les problèmes actuels dans les déploiements de stockage et à effectuer l’analyse quand les problèmes surviennent ou sont rejetés.  

#### <a name="vm-with-invalid-policy"></a>Machine virtuelle avec une stratégie non valide  
Les machines virtuelles avec des stratégies non valides sont également signalées par l’analyse du fonctionnement du sous-système de stockage.  Voici un exemple du même état décrit dans la section [Identifier les machines virtuelles avec des stratégies non valides](#BKMK_FindingVMsWithInvalidPolicies) de ce document.  

```PowerShell
C:\> Get-StorageSubSystem -FriendlyName Clustered* | Debug-StorageSubSystem  

EventTime                 :  
FaultId                   : 0d16d034-9f15-4920-a305-f9852abf47c3  
FaultingObject            :  
FaultingObjectDescription : Storage QoS Policy 5d1bf221-c8f0-4368-abcf-aa139e8a7c72  
FaultingObjectLocation    :  
FaultType                 : Storage QoS policy used by consumer does not exist.  
PerceivedSeverity         : Minor  
Reason                    : One or more storage consumers (usually Virtual Machines) are  
                            using a non-existent policy with id  
                            5d1bf221-c8f0-4368-abcf-aa139e8a7c72. Consumer details:  

                            Flow ID: 1ca356ff-fd33-5b5d-b60a-2c8659dc803e  
                            Initiator ID: 2ceabcef-2eba-4f1b-9e66-10f960b50bbf  
                            Initiator Name: WinOltp1  
                            Initiator Node: plang-c1.plang.nttest.microsoft.com  
                            File Path:  
                            C:\ClusterStorage\Volume3\SHARES\THREE\WINOLTP1\IOMETER.VHDX  
RecommendedActions        : {Reconfigure the storage consumers (usually Virtual Machines)  
                            to use a valid policy., Recreate any missing Storage QoS  
                            policies.}  
PSComputerName            :  
```  

#### <a name="lost-redundancy-for-a-storage-spaces-virtual-disk"></a>Perte de redondance pour un disque virtuel d’espaces de stockage  

Dans cet exemple, un espace de stockage en cluster possède un disque virtuel créé comme un miroir triple.  Un disque défectueux a été supprimé du système, mais aucun disque de remplacement n’a été ajouté.  Le sous-système de stockage signale une perte de redondance avec la valeur **Avertissement** pour HealthStatus, mais **OK** pour OperationalStatus, car le volume est encore en ligne.  

```PowerShell
PS C:\> Get-StorageSubSystem -FriendlyName Clustered*  

FriendlyName                   HealthStatus                   OperationalStatus  
------------                   ------------                   -----------------  
Clustered Windows Storage o... Warning                        OK  

[plang-fs1]: PS C:\Users\plang\Documents> Get-StorageSubSystem -FriendlyName Clustered* | Deb  
ug-StorageSubSystem  

EventTime                 :  
FaultId                   : dfb4b672-22a6-4229-b2ed-c29d7485bede  
FaultingObject            :  
FaultingObjectDescription : Virtual disk 'Two'  
FaultingObjectLocation    :  
FaultType                 : VirtualDiskDegradedFaultType  
PerceivedSeverity         : Minor  
Reason                    : Virtual disk 'Two' does not have enough redundancy remaining to  
                            successfully repair or regenerate its data.  
RecommendedActions        : {Rebalance the pool, replace failed physical disks, or add new  
                            physical disks to the storage pool, then repair the virtual  
                            disk.}  
PSComputerName            :  
```  

### <a name="sample-script-for-continuous-monitoring-of-storage-qos"></a>Exemple de script pour une analyse continue de la qualité de service de stockage  

Cette section comprend un exemple de script illustrant comment analyser les défaillances courantes à l’aide d’un script WMI.  Il est conçu comme point de départ pour que les développeurs puissent récupérer des événements d’intégrité en temps réel.  

**Exemple de script :**  

```PowerShell
param($cimSession)  
# Register and display events  
Register-CimIndicationEvent -Namespace root\microsoft\windows\storage -ClassName msft_storagefaultevent -CimSession $cimSession  

while ($true)  
{  
     $e = (Wait-Event)  
     $e.SourceEventArgs.NewEvent  
     Remove-Event $e.SourceIdentifier  
}  
```  

## <a name="frequently-asked-questions"></a>Questions fréquemment posées  

### <a name="how-do-i-retain-a-storage-qos-policy-being-enforced-for-my-virtual-machine-if-i-move-its-vhdvhdx-files-to-another-storage-cluster"></a>Comment faire pour qu’une stratégie de qualité de service de stockage reste appliquée pour ma machine virtuelle si je déplace ses fichiers VHD/VHDX vers un autre cluster de stockage ?  

Le paramètre sur le fichier VHD/VHDX qui spécifie la stratégie est le GUID d’un ID de stratégie.  Lors de la création d’une stratégie, le GUID peut être spécifié à l’aide du paramètre **PolicyID**.  Si ce paramètre n’est pas spécifié, un GUID aléatoire est créé.  Par conséquent, vous pouvez obtenir le PolicyID sur le cluster de stockage où les machines virtuelles stockent actuellement leurs fichiers VHD/VHDX et créer une stratégie identique sur le cluster de stockage de destination, puis spécifier qu’il soit créé avec le même GUID.  Quand les fichiers de machines virtuelles sont déplacés vers les nouveaux clusters de stockage, la stratégie avec le même GUID est appliquée.  

System Center Virtual Machine Manager peut être utilisé pour appliquer des stratégies sur plusieurs clusters de stockage,ce qui rend ce scénario beaucoup plus simple.  
### <a name="if-i-change-the-storage-qos-policy-why-dont-i-see-it-take-effect-immediately-when-i-run-get-storageqosflow"></a>Si je modifie la stratégie de qualité de service de stockage, pourquoi n’est-elle pas immédiatement en vigueur quand j’exécute Get-StorageQoSFlow ?  

Si un flux atteint le maximum d’une stratégie, que vous modifiez la stratégie pour lui affecter une valeur supérieure ou inférieure et que vous définissez immédiatement la latence/les E/S par seconde/la bande passante des flux à l’aide des applets de commande PowerShell, jusqu’à 5 minutes peuvent être nécessaires pour voir les effets globaux de la modification de stratégie sur les flux.  Les nouvelles limites sont valables en quelques secondes, mais l’applet de commande **Get-StorgeQoSFlow** PowerShell utilise une moyenne de chaque compteur avec une fenêtre glissante de 5 minutes.  Sinon, si une valeur était affichée quand vous avez exécuté l’applet de commande PowerShell plusieurs fois d’affilée, vous pouvez constater des valeurs très différentes, car les valeurs relatives aux E/S par seconde et latences peuvent varier considérablement d’une seconde à l’autre.

### <a name="BKMK_Updates"></a>Quelles nouvelles fonctionnalités ont été ajoutées dans Windows Server 2016

Dans Windows Server 2016, les types de stratégies de qualité de service de stockage ont été renommés.  Le nom du type de stratégie **MultiInstance** est remplacé par **Dedicated** et celui de **SingleInstance** est remplacé par **Aggregated**. Le comportement de gestion des stratégies Dedicated est également modifié : les fichiers VHD/VHDX dans la même machine virtuelle auxquels la même stratégie **Dedicated** est appliquée ne partagent pas les allocations d’E/S.  

Windows Server 2016 offre deux nouvelles fonctionnalités de qualité de service de stockage :  

-   **Bande passante maximale**  

    La qualité de service de stockage dans Windows Server 2016 offre la possibilité de spécifier la bande passante maximale qui peut être consommée par les flux affectés à la stratégie.  Le paramètre est **MaximumIOBandwidth** quand vous la spécifiez dans les applets de commande **StorageQosPolicy** et la sortie est exprimée en octets par seconde.  
    Si les deux paramètres **MaximimIops** et **MaximumIOBandwidth** sont définis dans une stratégie, ils sont tous deux en vigueur et le premier qui est atteint par le ou les flux limite les E/S des flux.  

-   **La normalisation des e/s par seconde est configurable**  

    La qualité de service de stockage utilise la normalisation des E/S par seconde.  La valeur par défaut consiste à utiliser une taille de normalisation de 8 Ko.  La qualité de service de stockage dans Windows Server 2016 offre la possibilité de spécifier une taille de normalisation différente pour le cluster de stockage.  Cette taille de normalisation affecte tous les flux sur le cluster de stockage et prend effet immédiatement (en quelques secondes) une fois qu’elle a été modifiée.  La valeur minimale est 1 Ko et la valeur maximale est 4 Go (il est recommandé de ne pas définir une valeur supérieure à 4 Mo, car il est rare d’avoir des E/S de plus de 4 Mo).  

    Vous devez également tenir compte du fait que le même modèle/débit d’E/S s’affiche avec différents nombres d’E/S par seconde dans la sortie de qualité de service de stockage quand vous modifiez la normalisation des E/S par seconde en raison de la modification dans le calcul de normalisation.  Si vous comparez les E/S par seconde entre les clusters de stockage, vous pouvez également vérifier la valeur de normalisation que chacun utilise, car elle a un impact sur les E/S par seconde normalisées signalées.    

#### <a name="example-1-creating-a-new-policy-and-viewing-the-maximum-bandwidth-on-the-storage-cluster"></a>Exemple 1 : Création d’une nouvelle stratégie et affichage de la bande passante maximale sur le cluster de stockage  
Dans PowerShell, vous pouvez spécifier en quelles unités un nombre est exprimé.  Dans l’exemple suivant, 10 Mo sont utilisés comme valeur de la bande passante maximale.  La qualité de service de stockage convertit cette valeur et l’enregistre sous forme d’octets par seconde, ce qui fait que 10 Mo sont convertis en 10 485 760 octets par seconde.  

```PowerShell
PS C:\Windows\system32> New-StorageQosPolicy -Name HR_VMs -MaximumIops 1000 -MinimumIops 20 -MaximumIOBandwidth 10MB  

Name   MinimumIops MaximumIops MaximumIOBandwidth Status  
----   ----------- ----------- ------------------ ------  
HR_VMs 20          1000        10485760           Ok  

PS C:\Windows\system32> Get-StorageQosPolicy  

Name    MinimumIops MaximumIops MaximumIOBandwidth Status  
----    ----------- ----------- ------------------ ------  
Default 0           0           0                  Ok  
HR_VMs  20          1000        10485760           Ok  

PS C:\Windows\system32> Get-StorageQoSFlow | fL InitiatorName,FilePath,InitiatorIOPS,InitiatorLatency,InitiatorBandwidth  

InitiatorName      : testsQoS  
FilePath           : C:\ClusterStorage\Volume2\TESTSQOS\VIRTUAL HARD DISKS\TESTSQOS.VHDX  
InitiatorIOPS      : 5  
InitiatorLatency   : 1.5455  
InitiatorBandwidth : 37888  
```  

#### <a name="example-2-get-iops-normalization-settings-and-specify--a-new-value"></a>Exemple 2 : Obtenir les paramètres de normalisation des e/s par seconde et spécifier une nouvelle valeur  

L’exemple suivant montre comment obtenir les paramètres de normalisation des E/S par seconde des clusters de stockage (8 Ko par défaut), leur affecter la valeur de 32 Ko, puis les afficher à nouveau.  Notez que vous pouvez indiquer « 32 Ko » dans cet exemple, car PowerShell permet de spécifier l’unité au lieu de demander la conversion en octets.   La sortie n’affiche pas la valeur en octets par seconde.  

```PowerShell
PS C:\Windows\system32> Get-StorageQosPolicyStore  

IOPSNormalizationSize  
---------------------  
8192  

PS C:\Windows\system32> Set-StorageQosPolicyStore -IOPSNormalizationSize 32KB  
PS C:\Windows\system32> Get-StorageQosPolicyStore  

IOPSNormalizationSize  
---------------------  
32768  
```    

## <a name="see-also"></a>Voir aussi  
- [Windows Server 2016](../../get-started/windows-server-2016.md)  
- [Réplica de stockage dans Windows Server 2016](../storage-replica/storage-replica-overview.md)  
- [espaces de stockage direct dans Windows Server 2016](../storage-spaces/storage-spaces-direct-overview.md)  
