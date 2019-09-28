---
ms.assetid: 898d72f1-01e7-4b87-8eb3-a8e0e2e6e6da
title: Ajout de serveurs ou de lecteurs aux espaces de stockage direct
ms.prod: windows-server
ms.author: cosdar
ms.manager: dongill
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 11/06/2017
description: Ajout de serveurs ou de lecteurs à un cluster espaces de stockage direct
ms.localizationpriority: medium
ms.openlocfilehash: 3d5949b8fce7253371ee7ecea5118596f713f037
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393783"
---
# <a name="adding-servers-or-drives-to-storage-spaces-direct"></a>Ajout de serveurs ou de lecteurs aux espaces de stockage direct

>S’applique à : Windows Server 2019, Windows Server 2016

Cette rubrique décrit comment ajouter des serveurs ou des lecteurs aux espaces de stockage direct.

## <a name="adding-servers"></a>Ajout de serveurs

L’ajout de serveurs, qui est souvent appelé montée en charge, permet d’ajouter de la capacité de stockage en vue d’améliorer les performances et de profiter d’une plus grande efficacité de stockage. Dans le cas d’un déploiement hyper-convergé, l’ajout de serveurs fournit également une plus grande quantité de ressources de calcul pour votre charge de travail.

![Animation de l’ajout d’un serveur à un cluster à quatre nœuds](media/add-nodes/Scaling-Out.gif)

L’ajout de serveurs facilite la montée en charge des déploiements classiques. Deux étapes suffisent :

1. Exécutez l’[Assistant Validation de cluster](https://technet.microsoft.com/library/cc732035(v=ws.10).aspx) en utilisant le composant logiciel enfichable du cluster de basculement ou en exécutant l’applet de commande **Test-Cluster** dans PowerShell en tant qu’administrateur. Spécifiez le *\<NouveauNœud>* du nouveau serveur à ajouter.

   ```PowerShell
   Test-Cluster -Node <Node>, <Node>, <Node>, <NewNode> -Include "Storage Spaces Direct", Inventory, Network, "System Configuration"
   ```

   Cette action confirme que le nouveau serveur exécute Windows Server 2016 Datacenter Edition, qu’il a rejoint le même domaine des services de domaine Active Directory que celui des serveurs existants, qu’il présente l’ensemble des rôles et fonctionnalités nécessaires et que sa mise en réseau est correctement configurée.

   >[!IMPORTANT]
   > Si vous réutilisez des lecteurs contenant d’anciennes données ou métadonnées dont vous n’avez plus besoin, effacez leur contenu à l’aide de la **Gestion des disques** ou de l’applet de commande **Reset-PhysicalDisk**. Si d’anciennes données ou métadonnées sont détectées, les lecteurs ne sont pas regroupés.

2. Exécutez l’applet de commande suivante sur le cluster pour terminer l’ajout du serveur :

```
Add-ClusterNode -Name NewNode 
```

   >[!NOTE]
   > Le regroupement automatique implique de ne disposer que d’un seul pool. Si vous avez contourné la configuration standard pour créer plusieurs pools, vous devez ajouter de nouveaux lecteurs à votre pool préféré à l’aide de l’applet de commande **Add-PhysicalDisk**.

### <a name="from-2-to-3-servers-unlocking-three-way-mirroring"></a>De deux à trois serveurs : déverrouiller la mise en miroir triple

![Ajout d’un troisième serveur à un cluster à deux nœuds](media/add-nodes/Scaling-2-to-3.png)

Avec deux serveurs, vous pouvez uniquement créer des volumes en miroir double (en comparaison avec le système RAID-1 distribué). Avec trois serveurs, vous pouvez créer des volumes en miroir triple pour offrir une plus grande tolérance de panne. Nous recommandons d’utiliser la mise en miroir triple chaque fois que cela est possible.

Les volumes en miroir double ne peuvent pas être directement mis à niveau en volumes en miroir triple. À la place, vous pouvez créer un autre volume et y transférer vos données (en les copiant à l’aide de la fonctionnalité [Réplica de stockage](../storage-replica/server-to-server-storage-replication.md), par exemple), puis supprimer l’ancien volume.

Pour commencer à créer des volumes en miroir triple, vous avez plusieurs options possibles. Choisissez celle qui vous convient le mieux. 

#### <a name="option-1"></a>Option 1

Spécifiez **PhysicalDiskRedundancy = 2** sur chaque nouveau volume que vous créez.

```PowerShell
New-Volume -FriendlyName <Name> -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size <Size> -PhysicalDiskRedundancy 2
```

#### <a name="option-2"></a>Option 2

Définissez **PhysicalDiskRedundancyDefault = 2** sur l’objet **ResiliencySetting** du regroupement appelé **Mirror**. Tous les nouveaux volumes en miroir utiliseront automatiquement une mise en miroir *triple* si vous ne spécifiez pas le type de mise en miroir.

```PowerShell
Get-StoragePool S2D* | Get-ResiliencySetting -Name Mirror | Set-ResiliencySetting -PhysicalDiskRedundancyDefault 2

New-Volume -FriendlyName <Name> -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size <Size>
```

#### <a name="option-3"></a>Option 3

Définissez **PhysicalDiskRedundancy = 2** sur le modèle **StorageTier** appelé *Capacity*, puis créez les volumes en référençant le niveau.

```PowerShell
Set-StorageTier -FriendlyName Capacity -PhysicalDiskRedundancy 2 

New-Volume -FriendlyName <Name> -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -StorageTierFriendlyNames Capacity -StorageTierSizes <Size>
```

### <a name="from-3-to-4-servers-unlocking-dual-parity"></a>De trois à quatre serveurs : déverrouiller la double parité

![Ajout d’un quatrième serveur à un cluster à trois nœuds](media/add-nodes/Scaling-3-to-4.png)

Avec quatre serveurs, vous pouvez utiliser la double parité, également appelée codage d’effacement (en comparaison avec le système RAID-6 distribué). Cela offre la même tolérance de panne que la mise en miroir triple, mais avec une meilleure efficacité de stockage. Pour en savoir plus, consultez [Tolérance de pannes et efficacité du stockage dans les espaces de stockage direct](storage-spaces-fault-tolerance.md).

À partir d’un déploiement de plus petite taille, vous avez plusieurs options possibles pour commencer à créer des volumes à double parité. Choisissez celle qui vous convient le mieux.

#### <a name="option-1"></a>Option 1

Spécifiez **PhysicalDiskRedundancy = 2** et **ResiliencySettingName = Parity** sur chaque nouveau volume que vous créez.

```PowerShell
New-Volume -FriendlyName <Name> -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size <Size> -PhysicalDiskRedundancy 2 -ResiliencySettingName Parity
```

#### <a name="option-2"></a>Option 2

Définissez **PhysicalDiskRedundancy = 2** sur l’objet **ResiliencySetting** du regroupement appelé **Parity**. Tous les nouveaux volumes avec parité utiliseront automatiquement une *double* parité si vous ne spécifiez pas le type de parité.

```PowerShell
Get-StoragePool S2D* | Get-ResiliencySetting -Name Parity | Set-ResiliencySetting -PhysicalDiskRedundancyDefault 2

New-Volume -FriendlyName <Name> -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size <Size> -ResiliencySettingName Parity
```

Avec quatre serveurs, vous pouvez également commencer à utiliser la parité accélérée grâce à la mise en miroir, où un volume individuel utilise à la fois la mise en miroir et la parité.

Pour cela, vous devez mettre à jour vos modèles **StorageTier** pour avoir les mêmes niveaux *Performance* et *Capacity* que ceux qui auraient été créés si vous aviez d’abord exécuté **Enable-ClusterS2D** sur quatre serveurs. Plus précisément, ces deux niveaux doivent avoir le même **MediaType** que vos appareils de capacité (comme un disque dur ou SSD) et **PhysicalDiskRedundancy = 2**. Le niveau *Performance* doit être **ResiliencySettingName = Mirror**, et le niveau *Capacity* doit être **ResiliencySettingName = Parity**.

#### <a name="option-3"></a>Option 3

Vous pouvez aussi simplement supprimer le modèle de niveau existant et créer les deux nouveaux. Cela n’affecte pas les volumes existants qui ont été créés préalablement en référençant le modèle de niveau, car il s’agit seulement d’un modèle.

```PowerShell
Remove-StorageTier -FriendlyName Capacity

New-StorageTier -StoragePoolFriendlyName S2D* -MediaType HDD -PhysicalDiskRedundancy 2 -ResiliencySettingName Mirror -FriendlyName Performance
New-StorageTier -StoragePoolFriendlyName S2D* -MediaType HDD -PhysicalDiskRedundancy 2 -ResiliencySettingName Parity -FriendlyName Capacity
```

C’est tout ! Vous êtes maintenant prêt à créer des volumes de parité accélérée grâce à la mise en miroir, en référençant ces modèles de niveau.

#### <a name="example"></a>Exemple

```PowerShell
New-Volume -FriendlyName "Sir-Mix-A-Lot" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -StorageTierFriendlyNames Performance, Capacity -StorageTierSizes <Size, Size> 
```

### <a name="beyond-4-servers-greater-parity-efficiency"></a>Au-delà de quatre serveurs : une plus grande efficacité de la parité

Cette montée en charge au-delà de quatre serveurs permet aux nouveaux volumes de bénéficier d’une efficacité d’encodage de parité encore plus grande. Par exemple, entre six et sept serveurs, l’efficacité passe de 50 % à 66,7 % dès qu’il devient possible d’utiliser Reed-Solomon 4 + 2 (plutôt que 2 + 2). Aucune étape que vous devez effectuer pour commencer à profiter de cette nouvelle efficacité ; l’encodage optimal est déterminé automatiquement chaque fois que vous créez un volume.

Toutefois, tous les volumes préexistants ne seront *pas* « convertis » en un nouvel encodage plus large. Une bonne raison à cela est que cela nécessiterait un calcul énorme affectant littéralement *chaque octet* du déploiement tout entier. Si vous souhaitez que les données préexistantes soient encodées de manière plus efficace, vous pouvez les faire migrer vers les nouveaux volumes.

Pour en savoir plus, consultez [Tolérance de pannes et efficacité du stockage dans les espaces de stockage direct](storage-spaces-fault-tolerance.md).

### <a name="adding-servers-when-using-chassis-or-rack-fault-tolerance"></a>Ajout de serveurs lors de l’utilisation de la tolérance de panne de châssis ou de rack

Si votre déploiement utilise la tolérance de panne de châssis ou de rack, vous devez spécifier le châssis ou le rack des nouveaux serveurs avant de les ajouter au cluster. Cela indique aux espaces de stockage direct comment distribuer au mieux les données afin d’optimiser la tolérance de panne.

1. Créez un domaine d’erreur temporaire pour le nœud en ouvrant une session PowerShell avec élévation de privilèges, puis en utilisant la commande suivante, où *\<NewNode >* est le nom du nouveau nœud de cluster :

   ```PowerShell
   New-ClusterFaultDomain -Type Node -Name <NewNode> 
   ```

2. Déplacez ce domaine d’erreur temporaire vers le châssis ou le rack dans lequel le nouveau serveur se trouve concrètement, comme spécifié par *\<ParentName &gt;*  :

   ```PowerShell
   Set-ClusterFaultDomain -Name <NewNode> -Parent <ParentName> 
   ```

   Pour plus d’informations, consultez [Reconnaissance des domaines d’erreur dans Windows Server 2016](../../failover-clustering/fault-domains.md).

3. Ajoutez le serveur au cluster, comme décrit dans [Ajout de serveurs](#adding-servers). Quand le nouveau serveur rejoint le cluster, il est automatiquement associé (par son nom) au domaine d’erreur de l’espace réservé.

## <a name="adding-drives"></a>Ajout de lecteurs

L’ajout de lecteurs, également appelé montée en puissance, permet d’ajouter de la capacité de stockage et également d’améliorer les performances. Si vous avez des emplacements disponibles, vous pouvez ajouter des lecteurs à chaque serveur pour augmenter votre capacité de stockage sans ajout de serveurs. Vous pouvez ajouter des lecteurs de cache ou de capacité de manière indépendante, à tout moment.

   >[!IMPORTANT]
   > Assurez-vous que tous les serveurs présentent la même configuration de stockage.

![Animation présentant l’ajout de lecteurs à un système](media/add-nodes/Scale-Up.gif)

Pour monter en puissance, connectez les lecteurs et vérifiez que Windows les détecte. Ils doivent apparaître dans la sortie de l’applet de commande **Get-PhysicalDisk** dans PowerShell et avoir leur propriété **CanPool** définie sur **True**. S’ils apparaissent avec la propriété **CanPool = False**, examinez la propriété **CannotPoolReason** pour en connaître la raison.

```PowerShell
Get-PhysicalDisk | Select SerialNumber, CanPool, CannotPoolReason
```

Dans un délai court, les lecteurs concernés sont automatiquement réclamés par les espaces de stockage direct et ajoutés au pool de stockage, et les volumes sont automatiquement [redistribués uniformément sur tous les lecteurs](https://blogs.technet.microsoft.com/filecab/2016/11/21/deep-dive-pool-in-spaces-direct/). À ce stade, vous avez terminé et êtes prêts à [étendre vos volumes](resize-volumes.md) ou à en [créer de nouveaux](create-volumes.md).

Si les lecteurs n’apparaissent pas, recherchez manuellement les modifications matérielles. Cela est possible en utilisant **Gestionnaire de périphériques**, sous le menu **Action**. S’ils contiennent d’anciennes données ou métadonnées, envisagez de les reformater. Pour ce faire, utilisez la **Gestion des disques** ou l’applet de commande **Reset-PhysicalDisk**.

   >[!NOTE]
   > Le regroupement automatique implique de ne disposer que d’un seul pool. Si vous avez contourné la configuration standard pour créer plusieurs pools, vous devez ajouter de nouveaux lecteurs à votre pool préféré à l’aide de l’applet de commande **Add-PhysicalDisk**.

## <a name="optimizing-drive-usage-after-adding-drives-or-servers"></a>Optimisation de l’utilisation des lecteurs après l’ajout de lecteurs ou de serveurs

Au fil du temps, à mesure que des lecteurs sont ajoutés ou supprimés, la distribution des données entre les disques du pool peut devenir inégale. Dans certains cas, cela peut entraîner la saturation de certains lecteurs, tandis que les autres lecteurs du pool ont une consommation beaucoup plus faible.

Pour aider à conserver l’allocation de disque même sur le pool, espaces de stockage direct optimise automatiquement l’utilisation des lecteurs après avoir ajouté des lecteurs ou des serveurs au pool (il s’agit d’un processus manuel pour les systèmes d’espaces de stockage qui utilisent des boîtiers SAS partagés). L’optimisation démarre 15 minutes après l’ajout d’un nouveau lecteur au pool. L’optimisation de pool s’exécute en tant qu’opération en arrière-plan basse priorité. par conséquent, l’exécution peut prendre plusieurs heures ou plusieurs jours, en particulier si vous utilisez des disques durs de grande taille.

L’optimisation utilise deux travaux : l’un nommé « *optimize* » et l’autre « *rééquilibrage* ». vous pouvez surveiller leur progression à l’aide de la commande suivante :

```powershell
Get-StorageJob
```

Vous pouvez optimiser manuellement un pool de stockage à l’aide de l’applet de commande [optimize-StoragePool](https://docs.microsoft.com/powershell/module/storage/optimize-storagepool?view=win10-ps) . Voici un exemple :

```powershell
Get-StoragePool <PoolName> | Optimize-StoragePool
```
