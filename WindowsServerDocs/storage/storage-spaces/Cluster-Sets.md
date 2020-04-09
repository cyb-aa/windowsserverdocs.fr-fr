---
title: Jeux de clusters
ms.prod: windows-server
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: johnmarlin-msft
ms.author: johnmar
ms.date: 01/30/2019
description: Cet article décrit le scénario des ensembles de clusters
ms.localizationpriority: medium
ms.openlocfilehash: 3c7ddef1831a82f7fc068ec4241bb1a72bd888bd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861042"
---
# <a name="cluster-sets"></a>Jeux de clusters

> S’applique à : Windows Server 2019

Cluster sets est la nouvelle technologie de montée en charge du Cloud dans la version 2019 de Windows Server qui augmente le nombre de nœuds de cluster dans un Cloud SDDC (Software Defined Data Center) unique par ordre de grandeur. Un ensemble de clusters est un regroupement faiblement couplé de plusieurs clusters de basculement : calcul, stockage ou hyper-convergé. La technologie des ensembles de clusters permet la fluidité des machines virtuelles entre les clusters membres au sein d’un ensemble de clusters et un espace de noms de stockage unifié dans l’ensemble pour prendre en charge la fluidité des machines virtuelles.

Tout en conservant les expériences de gestion du cluster de basculement existantes sur les clusters membres, une instance d’ensemble de clusters offre en outre des cas d’utilisation clés autour de la gestion du cycle de vie de l’agrégat. Ce guide d’évaluation du scénario Windows Server 2019 fournit les informations de base nécessaires, ainsi que des instructions pas à pas pour évaluer la technologie des ensembles de clusters à l’aide de PowerShell.

## <a name="technology-introduction"></a>Présentation de la technologie

La technologie des ensembles de clusters est développée pour répondre à des demandes spécifiques de clients (SDDC) Cloud à grande échelle. La proposition de valeur des ensembles de clusters peut être résumée comme suit :  

- Augmentez considérablement la mise à l’échelle du Cloud SDDC pour exécuter des ordinateurs virtuels à haut niveau de disponibilité en combinant plusieurs clusters plus petits dans une seule grande structure, même en conservant la limite d’erreurs logicielles à un seul cluster.
- Gérez le cycle de vie de cluster de basculement entier, y compris les clusters d’intégration et de mise hors service, sans impact sur la disponibilité des machines virtuelles du locataire, via la migration fluide des machines virtuelles dans cette grande structure
- Modifiez facilement le ratio de calcul à stockage dans votre hyper-convergé
- Tirer parti des [domaines d’erreur de type Azure et des groupes à haute disponibilité](htttps://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) sur les clusters dans le positionnement initial des machines virtuelles et la migration des machines virtuelles suivante
- Mélangez et faites correspondre différentes générations de matériel de processeur dans la même structure d’ensemble de clusters, même en conservant des domaines d’erreur individuels homogènes pour une efficacité maximale.  Notez que la recommandation du même matériel est toujours présente dans chaque cluster individuel, ainsi que dans l’ensemble du cluster.

À partir d’une vue générale, les ensembles de clusters peuvent ressembler à.

![Vue de la solution ensembles de clusters](media/Cluster-sets-Overview/Cluster-sets-solution-View.png)

Vous trouverez ci-dessous un résumé rapide de chacun des éléments de l’image ci-dessus :

**Cluster de gestion**

Le cluster de gestion dans un ensemble de clusters est un cluster de basculement qui héberge le plan de gestion hautement disponible de l’ensemble du cluster et l’espace de noms de l’espace de noms de stockage unifié (espace de noms de l’ensemble d’espaces de cluster) Serveur de fichiers avec montée en puissance parallèle (SOFS). Un cluster de gestion est logiquement découplé des clusters membres qui exécutent les charges de travail des ordinateurs virtuels. Le plan de gestion de l’ensemble de clusters est ainsi résilient aux défaillances localisées à l’ensemble du cluster, par exemple, la perte de puissance d’un cluster membre.   

**Cluster membre**

Un cluster membre dans un ensemble de clusters est généralement un cluster hyper-convergé traditionnel exécutant des charges de travail de machine virtuelle et de espaces de stockage direct. Plusieurs clusters membres participent à un déploiement avec un seul ensemble de clusters, formant ainsi la plus grande structure de Cloud SDDC. Les clusters membres diffèrent d’un cluster de gestion en deux aspects clés : les clusters membres participent aux constructions de groupe de disponibilité et de domaine d’erreur, et les clusters membres sont également dimensionnés pour héberger des machines virtuelles et des charges de travail de espaces de stockage direct. Les machines virtuelles de l’ensemble de clusters qui se déplacent entre les limites du cluster dans un ensemble de clusters ne doivent pas être hébergées sur le cluster de gestion pour cette raison.

**Référence de l’espace de noms du jeu de clusters SOFS**

Une référence de l’espace de noms de l’ensemble de clusters (espace de noms de l’ensemble de clusters) SOFS est un Serveur de fichiers avec montée en puissance parallèle dans lequel chaque partage SMB sur l’espace de noms de l’ensemble de clusters SOFS est un partage de référence, de type « SimpleReferral » récemment introduit dans Windows Server 2019. Cette référence permet aux clients SMB (Server Message Block) d’accéder au partage SMB cible hébergé sur le cluster membre SOFS. La référence de l’espace de noms de l’ensemble de clusters SOFS est un mécanisme de référence léger qui, en tant que tel, ne participe pas au chemin d’e/s. Les références SMB sont mises en cache de façon perpétuelle sur chacun des nœuds du client et l’espace de noms ensembles de clusters est automatiquement mis à jour automatiquement en fonction des besoins.

**Maître d’ensemble de clusters**

Dans un ensemble de clusters, la communication entre les clusters membres est faiblement couplée et est coordonnée par une nouvelle ressource de cluster appelée « maître d’ensemble de clusters » (CS-Master). Comme n’importe quelle autre ressource de cluster, CS-Master est hautement disponible et résiste aux défaillances de clusters membres individuels et/ou aux défaillances de nœud de cluster de gestion. Via un nouveau fournisseur WMI d’ensemble de clusters, CS-Master fournit le point de terminaison de gestion pour toutes les interactions de gestion d’ensemble de clusters.

**Worker Set Worker**

Dans un déploiement d’ensemble de clusters, le maître CS interagit avec une nouvelle ressource de cluster sur les clusters membres appelée « cluster Set Worker » (CS-Worker). CS-Worker agit comme la seule liaison sur le cluster pour orchestrer les interactions du cluster local comme demandé par CS-Master. Parmi ces interactions, citons notamment le placement de l’ordinateur virtuel et l’inventaire des ressources locales du cluster. Il n’existe qu’une seule instance CS-Worker pour chacun des clusters membres d’un ensemble de clusters. 

**Domaine d’erreur**

Un domaine d’erreur est le regroupement d’artefacts logiciels et matériels que l’administrateur détermine peut échouer en cas d’échec.  Alors qu’un administrateur peut désigner un ou plusieurs clusters ensemble comme un domaine d’erreur, chaque nœud peut faire partie d’un domaine d’erreur dans un groupe à haute disponibilité. Les ensembles de clusters par conception quittent la détermination de la limite de domaine d’erreur à l’administrateur qui est bien connu avec les considérations relatives à la topologie du centre de données (par exemple, PDU, mise en réseau) que les clusters membres partagent. 

**Groupe à haute disponibilité**

Un groupe à haute disponibilité permet à l’administrateur de configurer la redondance souhaitée des charges de travail en cluster entre les domaines d’erreur, en les organisant dans un groupe à haute disponibilité et en déployant des charges de travail dans ce groupe à haute disponibilité. Par exemple, si vous déployez une application à deux niveaux, nous vous recommandons de configurer au moins deux machines virtuelles dans un groupe à haute disponibilité pour chaque niveau, ce qui garantit qu’en cas de défaillance d’un domaine d’erreur dans ce groupe à haute disponibilité, votre application disposera au moins d’une machine virtuelle dans chaque niveau hébergée sur un domaine d’erreur différent de ce même groupe à

## <a name="why-use-cluster-sets"></a>Pourquoi utiliser des ensembles de clusters ?

Les ensembles de clusters offrent l’avantage de mettre à l’échelle sans sacrifier la résilience.  

Les ensembles de clusters permettent de regrouper plusieurs clusters pour créer une structure de grande taille, tandis que chaque cluster reste indépendant pour la résilience.  Par exemple, vous avez plusieurs clusters HCI à 4 nœuds exécutant des machines virtuelles.  Chaque cluster fournit la résilience nécessaire pour lui-même.  Si le stockage ou la mémoire commence à se remplir, la mise à l’échelle est l’étape suivante.  Avec la montée en puissance, il existe des options et des considérations à prendre en compte.

1. Ajoutez de l’espace de stockage au cluster actuel.  Avec espaces de stockage direct, cela peut être délicat, car les mêmes lecteurs de modèle/microprogramme peuvent ne pas être disponibles.  La prise en compte des temps de reconstruction doit également être prise en compte.
2. Ajoutez davantage de mémoire.  Que se passe-t-il si vous avez atteint la mémoire que les ordinateurs peuvent gérer ?  Que se passe-t-il si tous les emplacements de mémoire disponibles sont saturés ?
3. Ajoutez des nœuds de calcul supplémentaires avec des lecteurs dans le cluster actuel.  Nous revenons à l’option 1 qui doit être prise en compte.
4. Acheter un tout nouveau cluster

C’est là que les ensembles de clusters offrent l’avantage de mettre à l’échelle.  Si j’ajoute mes clusters à un ensemble de clusters, je peux tirer parti du stockage ou de la mémoire qui peut être disponible sur un autre cluster sans achat supplémentaire.  Du point de vue de la résilience, l’ajout de nœuds supplémentaires à un espaces de stockage direct ne va pas fournir de votes supplémentaires pour le quorum.  Comme indiqué [ici](drive-symmetry-considerations.md), un cluster espaces de stockage direct peut survivre à la perte de 2 nœuds avant de se fermer.  Si vous disposez d’un cluster HCI à 4 nœuds, la taille de l’ensemble du cluster est inférieure à 3 nœuds.  Si vous disposez d’un cluster à 8 nœuds, 3 nœuds diminuent la taille du cluster.  Avec les ensembles de clusters qui ont deux clusters HCI à 4 nœuds dans l’ensemble, 2 nœuds d’un HCI s’affichent et 1 nœud de l’autre HCI s’éteint, les deux clusters restent opérationnels.  Est-il préférable de créer un cluster de espaces de stockage direct à 16 nœuds ou de le diviser en quatre clusters à 4 nœuds et à utiliser des ensembles de clusters ?  Le fait d’avoir quatre clusters à 4 nœuds avec des ensembles de clusters offre la même échelle, mais une meilleure résilience dans le fait que plusieurs nœuds de calcul peuvent descendre (de façon inattendue ou de maintenance) et que la production reste.

## <a name="considerations-for-deploying-cluster-sets"></a>Considérations relatives au déploiement d’ensembles de clusters

Si vous envisagez d’utiliser des ensembles de clusters, posez-vous les questions suivantes :

- Avez-vous besoin de dépasser les limites actuelles de l’échelle de calcul et de stockage HCI ?
- Tous les calculs et le stockage ne sont-ils pas identiques de la même façon ?
- Effectuez-vous une migration dynamique des ordinateurs virtuels entre les clusters ?
- Souhaitez-vous que les groupes à haute disponibilité des ordinateurs Azure et les domaines d’erreur sur plusieurs clusters ?
- Avez-vous besoin de prendre le temps d’examiner tous vos clusters pour déterminer où les nouvelles machines virtuelles doivent être placées ?

Si votre réponse est oui, les ensembles de clusters sont les éléments dont vous avez besoin.

Il y a quelques autres éléments à prendre en compte lorsqu’un SDDC plus grand peut modifier vos stratégies de centre de données globales.  SQL Server est un bon exemple.  Le déplacement des machines virtuelles SQL Server entre les clusters nécessite-t-il des licences SQL pour s’exécuter sur des nœuds supplémentaires ?  

## <a name="scale-out-file-server-and-cluster-sets"></a>Serveurs de fichiers avec montée en puissance parallèle et ensembles de clusters

Dans Windows Server 2019, il existe un nouveau rôle de serveur de fichiers avec montée en puissance parallèle appelé infrastructure Serveur de fichiers avec montée en puissance parallèle (SOFS). 

Les considérations suivantes s’appliquent à un rôle de SOFS d’infrastructure :

1.    Il ne peut y avoir qu’un seul rôle de cluster SOFS d’infrastructure sur un cluster de basculement. Le rôle de SOFS d’infrastructure est créé en spécifiant le paramètre de commutateur « **-infrastructure**» à l’applet de commande **Add-ClusterScaleOutFileServerRole** .  Par exemple :

        Add-ClusterScaleoutFileServerRole-Name « my_infra_sofs_name »-infrastructure

2.    Chaque volume CSV créé dans le basculement déclenche automatiquement la création d’un partage SMB avec un nom généré automatiquement en fonction du nom du volume CSV. Un administrateur ne peut pas créer ou modifier directement des partages SMB sous un rôle SOFS, à l’exception des opérations de création/modification de volume CSV.

3.    Dans les configurations hyper-convergées, une infrastructure SOFS permet à un client SMB (hôte Hyper-V) de communiquer avec la disponibilité continue garantie (CA) au serveur SMB d’infrastructure SOFS. Cette autorité de certification de bouclage SMB hyper-convergé est obtenue via des machines virtuelles accédant à leurs fichiers VHDx (Virtual Disk) où l’identité de l’ordinateur virtuel propriétaire est transférée entre le client et le serveur. Ce transfert d’identité permet aux fichiers VHDx de liste de contrôle d’accès (ACL) de la même façon que dans les configurations de cluster hyper-convergé standard.

Une fois qu’un ensemble de clusters est créé, l’espace de noms de l’ensemble de clusters s’appuie sur un SOFS d’infrastructure sur chacun des clusters membres, ainsi qu’une infrastructure SOFS dans le cluster de gestion.

Au moment où un cluster membre est ajouté à un ensemble de clusters, l’administrateur spécifie le nom d’un SOFS d’infrastructure sur ce cluster s’il en existe déjà un. Si l’infrastructure SOFS n’existe pas, cette opération crée un nouveau rôle SOFS d’infrastructure sur le nouveau cluster membre. Si un rôle de SOFS d’infrastructure existe déjà sur le cluster membre, l’opération d’ajout le renomme implicitement en fonction du nom spécifié, si nécessaire. Les serveurs SMB Singleton existants ou les rôles SOFS non-infrastructure sur les clusters membres ne sont pas utilisés par l’ensemble de clusters. 

Au moment de la création de l’ensemble de clusters, l’administrateur a la possibilité d’utiliser un objet ordinateur AD déjà existant comme racine d’espace de noms sur le cluster de gestion. Les opérations de création d’un ensemble de clusters créent le rôle de cluster SOFS d’infrastructure sur le cluster de gestion ou renomme le rôle de SOFS d’infrastructure existant comme décrit précédemment pour les clusters membres. L’infrastructure SOFS sur le cluster de gestion est utilisée comme référence de l’espace de noms de l’ensemble de clusters (espace de noms de l’ensemble de clusters) SOFS. Cela signifie simplement que chaque partage SMB sur l’espace de noms de l’ensemble de clusters SOFS est un partage de référence, de type « SimpleReferral », récemment introduit dans Windows Server 2019.  Cette référence permet aux clients SMB d’accéder au partage SMB cible hébergé sur le cluster membre SOFS. La référence de l’espace de noms de l’ensemble de clusters SOFS est un mécanisme de référence léger qui, en tant que tel, ne participe pas au chemin d’e/s. Les références SMB sont mises en cache de façon perpétuelle sur chacun des nœuds du client et l’espace de noms ensembles de clusters est automatiquement mis à jour automatiquement en fonction des besoins

## <a name="creating-a-cluster-set"></a>Création d’un ensemble de clusters

### <a name="prerequisites"></a>Composants requis

Lorsque vous créez un ensemble de clusters, les conditions préalables suivantes sont recommandées :

1. Configurez un client de gestion exécutant Windows Server 2019.
2. Installez les outils de cluster de basculement sur ce serveur d’administration.
3. Créer des membres de cluster (au moins deux clusters avec au moins deux volumes partagés de cluster sur chaque cluster)
4. Créez un cluster de gestion (physique ou invité) qui chevauche les clusters membres.  Cette approche garantit que le plan de gestion des ensembles de clusters continue d’être disponible malgré les échecs possibles du cluster.

### <a name="steps"></a>Étapes

1. Créez un ensemble de clusters à partir de trois clusters, comme défini dans les conditions préalables.  Le graphique ci-dessous donne un exemple de clusters à créer.  Dans cet exemple, le nom de l’ensemble de clusters sera **CSMASTER**.

   | Nom du cluster               | Nom de SOFS de l’infrastructure à utiliser ultérieurement | 
   |----------------------------|-------------------------------------------|
   | SET-CLUSTER                | SOFS-CLUSTERSET                           |
   | CLUSTER1                   | SOFS-CLUSTER1                             |
   | CLUSTER2                   | SOFS-CLUSTER2                             |

2. Une fois que tous les clusters ont été créés, utilisez les commandes suivantes pour créer le maître du jeu de clusters.

        New-ClusterSet -Name CSMASTER -NamespaceRoot SOFS-CLUSTERSET -CimSession SET-CLUSTER

3. Pour ajouter un serveur de cluster à l’ensemble de clusters, vous devez utiliser la configuration ci-dessous.

        Add-ClusterSetMember -ClusterName CLUSTER1 -CimSession CSMASTER -InfraSOFSName SOFS-CLUSTER1
        Add-ClusterSetMember -ClusterName CLUSTER2 -CimSession CSMASTER -InfraSOFSName SOFS-CLUSTER2

   > [!NOTE]
   > Si vous utilisez un modèle d’adresse IP statique, vous devez inclure *-StaticAddress x.x.x. x* sur la commande **New-ClusterSet** .

4. Une fois que vous avez créé le cluster à partir des membres du cluster, vous pouvez répertorier l’ensemble des nœuds et ses propriétés.  Pour énumérer tous les clusters membres de l’ensemble de clusters :

        Get-ClusterSetMember -CimSession CSMASTER

5. Pour énumérer tous les clusters membres de l’ensemble de clusters, y compris les nœuds de cluster de gestion :

        Get-ClusterSet -CimSession CSMASTER | Get-Cluster | Get-ClusterNode

6. Pour répertorier tous les nœuds des clusters membres :

        Get-ClusterSetNode -CimSession CSMASTER

7. Pour répertorier tous les groupes de ressources dans l’ensemble de clusters :

        Get-ClusterSet -CimSession CSMASTER | Get-Cluster | Get-ClusterGroup 

8. Pour vérifier que le processus de création de l’ensemble de clusters a créé un partage SMB (identifié en tant que volume1 ou quel que soit le dossier du volume partagé de cluster avec le nom de serveur de fichiers d’infrastructure et le chemin d’accès) sur le SOFS d’infrastructure pour le volume CSV de chaque membre du cluster :

        Get-SmbShare -CimSession CSMASTER

8. Les ensembles de clusters ont des journaux de débogage qui peuvent être collectés à des fins de révision.  Les journaux d’ensemble de cluster et de débogage de cluster peuvent être rassemblés pour tous les membres et le cluster de gestion.

        Get-ClusterSetLog -ClusterSetCimSession CSMASTER -IncludeClusterLog -IncludeManagementClusterLog -DestinationFolderPath <path>

9. Configurez la délégation Kerberos avec [restriction](https://techcommunity.microsoft.com/t5/virtualization/live-migration-via-constrained-delegation-with-kerberos-in/ba-p/382334) entre tous les membres du groupe de clusters.

10. Configurez le type d’authentification de la migration dynamique de l’ordinateur virtuel entre clusters sur Kerberos sur chaque nœud de l’ensemble de clusters.

        foreach($h in $hosts){ Set-VMHost -VirtualMachineMigrationAuthenticationType Kerberos -ComputerName $h }

11. Ajoutez le cluster de gestion au groupe Administrateurs local sur chaque nœud de l’ensemble de clusters.

        foreach($h in $hosts){ Invoke-Command -ComputerName $h -ScriptBlock {Net localgroup administrators /add <management_cluster_name>$} }

## <a name="creating-new-virtual-machines-and-adding-to-cluster-sets"></a>Création de machines virtuelles et ajout à des ensembles de clusters

Après avoir créé l’ensemble de clusters, l’étape suivante consiste à créer des machines virtuelles.  En règle générale, lorsqu’il est temps de créer des machines virtuelles et de les ajouter à un cluster, vous devez effectuer des vérifications sur les clusters pour voir lequel il est préférable de les exécuter.  Ces contrôles peuvent inclure les éléments suivants :

- Quelle quantité de mémoire est disponible sur les nœuds du cluster ?
- Quelle quantité d’espace disque est disponible sur les nœuds du cluster ?
- L’ordinateur virtuel nécessite-t-il des exigences de stockage spécifiques (par exemple, je veux que mes machines virtuelles SQL Server soient dirigées vers un cluster exécutant des disques plus rapides ; ou, mon ordinateur virtuel d’infrastructure n’est pas aussi critique et peut s’exécuter sur des lecteurs plus lents).

Une fois que vous avez répondu à ces questions, vous créez l’ordinateur virtuel sur le cluster dont vous avez besoin.  L’un des avantages des ensembles de clusters est que les ensembles de clusters effectuent ces vérifications pour vous et placent l’ordinateur virtuel sur le nœud le plus optimal.

Les commandes ci-dessous identifient le cluster optimal et y déploient l’ordinateur virtuel.  Dans l’exemple ci-dessous, un nouvel ordinateur virtuel est créé, spécifiant qu’au moins 4 Go de mémoire sont disponibles pour l’ordinateur virtuel et qu’il doit utiliser un processeur virtuel.

- Assurez-vous que 4 Go sont disponibles pour la machine virtuelle
- définir le processeur virtuel utilisé à 1
- Vérifiez que la machine virtuelle dispose d’au moins 10% d’UC

   ```PowerShell
   # Identify the optimal node to create a new virtual machine
   $memoryinMB=4096
   $vpcount = 1
   $targetnode = Get-ClusterSetOptimalNodeForVM -CimSession CSMASTER -VMMemory $memoryinMB -VMVirtualCoreCount $vpcount -VMCpuReservation 10
   $secure_string_pwd = convertto-securestring "<password>" -asplaintext -force
   $cred = new-object -typename System.Management.Automation.PSCredential ("<domain\account>",$secure_string_pwd)

   # Deploy the virtual machine on the optimal node
   Invoke-Command -ComputerName $targetnode.name -scriptblock { param([String]$storagepath); New-VM CSVM1 -MemoryStartupBytes 3072MB -path $storagepath -NewVHDPath CSVM.vhdx -NewVHDSizeBytes 4194304 } -ArgumentList @("\\SOFS-CLUSTER1\VOLUME1") -Credential $cred | Out-Null
   
   Start-VM CSVM1 -ComputerName $targetnode.name | Out-Null
   Get-VM CSVM1 -ComputerName $targetnode.name | fl State, ComputerName
   ```

Une fois l’opération terminée, vous recevrez les informations sur la machine virtuelle et l’endroit où elle a été placée.  Dans l’exemple ci-dessus, il s’affiche comme suit :

        State         : Running
        ComputerName  : 1-S2D2

Si vous ne disposez pas de suffisamment de mémoire, de processeur ou d’espace disque pour ajouter l’ordinateur virtuel, l’erreur suivante s’affiche :

      Get-ClusterSetOptimalNodeForVM : A cluster node is not available for this operation.  

Une fois la machine virtuelle créée, elle s’affiche dans le Gestionnaire Hyper-V sur le nœud spécifique spécifié.  Pour l’ajouter en tant qu’ordinateur virtuel de groupe de clusters et dans le cluster, la commande est indiquée ci-dessous.  

        Register-ClusterSetVM -CimSession CSMASTER -MemberName $targetnode.Member -VMName CSVM1

Une fois l’opération terminée, la sortie est la suivante :

         Id  VMName  State  MemberName  PSComputerName
         --  ------  -----  ----------  --------------
          1  CSVM1      On  CLUSTER1    CSMASTER

Si vous avez ajouté un cluster avec des machines virtuelles existantes, les machines virtuelles devront également être inscrites avec des ensembles de clusters. ainsi, inscrivez tous les ordinateurs virtuels en même temps, la commande à utiliser est la suivante :

        Get-ClusterSetMember -name CLUSTER3 -CimSession CSMASTER | Register-ClusterSetVM -RegisterAll -CimSession CSMASTER

Toutefois, le processus n’est pas terminé, car le chemin d’accès à l’ordinateur virtuel doit être ajouté à l’espace de noms de l’ensemble de clusters.

Par exemple, un cluster existant est ajouté et des ordinateurs virtuels préconfigurés se trouvent sur le Volume partagé de cluster local (CSV), le chemin d’accès du VHDX ressemble à «C:\ClusterStorage\Volume1\MYVM\Virtual Hard Disks\MYVM.vhdx.  Une migration de stockage doit être effectuée lorsque les chemins d’accès CSV sont en mode de conception locale sur un cluster à un seul membre. Par conséquent, n’est pas accessible à la machine virtuelle une fois qu’elles sont migrées en direct entre les clusters membres. 

Dans cet exemple, CLUSTER3 a été ajouté à l’ensemble de clusters à l’aide de Add-ClusterSetMember avec l’infrastructure Serveur de fichiers avec montée en puissance parallèle en tant que SOFS-CLUSTER3.  Pour déplacer la configuration et le stockage de l’ordinateur virtuel, la commande est la suivante :

        Move-VMStorage -DestinationStoragePath \\SOFS-CLUSTER3\Volume1 -Name MYVM

Une fois l’opération terminée, un message d’avertissement s’affiche :

        WARNING: There were issues updating the virtual machine configuration that may prevent the virtual machine from running.  For more information view the report file below.
        WARNING: Report file location: C:\Windows\Cluster\Reports\Update-ClusterVirtualMachineConfiguration '' on date at time.htm.

Cet avertissement peut être ignoré, car l’avertissement est « aucune modification de la configuration de stockage du rôle d’ordinateur virtuel n’a été détectée ».  La raison de l’avertissement en tant qu’emplacement physique réel ne change pas ; uniquement les chemins d’accès de configuration. 

Pour plus d’informations sur Move-VMStorage, consultez ce [lien](https://docs.microsoft.com/powershell/module/hyper-v/move-vmstorage?view=win10-ps). 

La migration dynamique d’un ordinateur virtuel entre différents clusters d’ensembles de clusters n’est pas la même que dans le passé. Dans les scénarios sans cluster, les étapes sont les suivantes :

1. Supprimez le rôle de machine virtuelle du cluster.
2. effectuer une migration dynamique de l’ordinateur virtuel vers un nœud de membre d’un autre cluster.
3. Ajoutez l’ordinateur virtuel au cluster en tant que nouveau rôle de machine virtuelle.

Avec les ensembles de clusters, ces étapes ne sont pas nécessaires et une seule commande est nécessaire.  Tout d’abord, vous devez définir tous les réseaux disponibles pour la migration à l’aide de la commande :

    Set-VMHost -UseAnyNetworkForMigration $true

Par exemple, je souhaite déplacer un ordinateur virtuel de l’ensemble de clusters de CLUSTER1 vers NODE2-CL3 sur CLUSTER3.  La commande est la suivante :

        Move-ClusterSetVM -CimSession CSMASTER -VMName CSVM1 -Node NODE2-CL3

Notez que cela ne déplace pas les fichiers de stockage ou de configuration de l’ordinateur virtuel.  Cela n’est pas nécessaire, car le chemin d’accès à la machine virtuelle reste \\SOFS-CLUSTER1\VOLUME1.  Une fois qu’un ordinateur virtuel a été inscrit avec des ensembles de clusters, le chemin d’accès du partage de serveur de fichiers d’infrastructure ne nécessite pas que les lecteurs et les ordinateurs virtuels se trouvent sur le même ordinateur que l’ordinateur virtuel.

## <a name="creating-availability-sets-fault-domains"></a>Création de domaines d’erreur de groupes à haute disponibilité

Comme décrit dans l’introduction, les domaines d’erreur de type Azure et les groupes à haute disponibilité peuvent être configurés dans un ensemble de clusters.  Cela est bénéfique pour les placements et les migrations de machines virtuelles initiaux entre les clusters.  

Dans l’exemple ci-dessous, il y a quatre clusters qui participent à l’ensemble de clusters.  Dans l’ensemble, un domaine d’erreur logique sera créé avec deux des clusters et un domaine d’erreur créé avec les deux autres clusters.  Ces deux domaines d’erreur comporteront le jeu disponibilité. 

Dans l’exemple ci-dessous, CLUSTER1 et CLUSTER2 se trouvent dans un domaine d’erreur appelé **FD1 en** , tandis que CLUSTER3 et CLUSTER4 se trouvent dans un domaine d’erreur appelé **FD2**.  Le groupe à haute disponibilité sera appelé **CSMASTER-As** et sera composé des deux domaines d’erreur.

Pour créer les domaines d’erreur, les commandes sont les suivantes :

        New-ClusterSetFaultDomain -Name FD1 -FdType Logical -CimSession CSMASTER -MemberCluster CLUSTER1,CLUSTER2 -Description "This is my first fault domain"

        New-ClusterSetFaultDomain -Name FD2 -FdType Logical -CimSession CSMASTER -MemberCluster CLUSTER3,CLUSTER4 -Description "This is my second fault domain"

Pour vous assurer qu’elles ont été correctement créées, vous pouvez exécuter la fonction ClusterSetFaultDomain avec sa sortie affichée.

        PS C:\> Get-ClusterSetFaultDomain -CimSession CSMASTER -FdName FD1 | fl *

        PSShowComputerName    : True
        FaultDomainType       : Logical
        ClusterName           : {CLUSTER1, CLUSTER2}
        Description           : This is my first fault domain
        FDName                : FD1
        Id                    : 1
        PSComputerName        : CSMASTER

Maintenant que les domaines d’erreur ont été créés, le groupe à haute disponibilité doit être créé.

        New-ClusterSetAvailabilitySet -Name CSMASTER-AS -FdType Logical -CimSession CSMASTER -ParticipantName FD1,FD2

Pour valider qu’il a été créé, utilisez :

        Get-ClusterSetAvailabilitySet -AvailabilitySetName CSMASTER-AS -CimSession CSMASTER

Lorsque vous créez des machines virtuelles, vous devez utiliser le paramètre-lesquelles dans le cadre de la détermination du nœud optimal.  Par conséquent, il ressemble à ceci :

        # Identify the optimal node to create a new virtual machine
        $memoryinMB=4096
        $vpcount = 1
        $av = Get-ClusterSetAvailabilitySet -Name CSMASTER-AS -CimSession CSMASTER
        $targetnode = Get-ClusterSetOptimalNodeForVM -CimSession CSMASTER -VMMemory $memoryinMB -VMVirtualCoreCount $vpcount -VMCpuReservation 10 -AvailabilitySet $av
        $secure_string_pwd = convertto-securestring "<password>" -asplaintext -force
        $cred = new-object -typename System.Management.Automation.PSCredential ("<domain\account>",$secure_string_pwd)

Suppression d’un cluster des ensembles de clusters en raison de différents cycles de vie. Il peut arriver qu’un cluster doive être supprimé d’un ensemble de clusters. En guise de meilleure pratique, toutes les machines virtuelles de l’ensemble de clusters doivent être déplacées hors du cluster. Pour ce faire, vous pouvez utiliser les commandes **Move-ClusterSetVM** et **Move-VMStorage** .

Toutefois, si les ordinateurs virtuels ne sont pas également déplacés, les ensembles de clusters exécutent une série d’actions pour fournir un résultat intuitif à l’administrateur.  Lorsque le cluster est supprimé du jeu, toutes les machines virtuelles d’ensemble de cluster restantes hébergées sur le cluster en cours de suppression deviennent simplement des machines virtuelles à haut niveau de disponibilité qui sont liées à ce cluster, en supposant qu’elles aient accès à leur stockage.  Les ensembles de clusters mettent également à jour automatiquement son inventaire en :

- N’effectue plus le suivi de l’intégrité du cluster maintenant supprimé et des machines virtuelles en cours d’exécution.
- Supprime de l’espace de noms de l’ensemble de clusters et toutes les références aux partages hébergés sur le cluster maintenant supprimé

Par exemple, la commande permettant de supprimer le cluster CLUSTER1 des ensembles de clusters serait la suivante :

        Remove-ClusterSetMember -ClusterName CLUSTER1 -CimSession CSMASTER

## <a name="frequently-asked-questions-faq"></a>Forum Aux Questions

**Question :** Dans mon ensemble de clusters, suis-je limité à l’utilisation de clusters hyper-convergents uniquement ? <br>
**Réponse :** º.  Vous pouvez mélanger des espaces de stockage direct avec des clusters traditionnels.

**Question :** Puis-je gérer mon ensemble de clusters via System Center Virtual Machine Manager ? <br>
**Réponse :** System Center Virtual Machine Manager ne prend actuellement pas en charge les ensembles de clusters <br><br> **Question :** Les clusters Windows Server 2012 R2 ou 2016 peuvent-ils coexister dans le même ensemble de cluster ? <br>
**Question :** Puis-je migrer des charges de travail de clusters Windows Server 2012 R2 ou 2016 en faisant simplement en sorte que ces clusters rejoignent le même ensemble de clusters ? <br>
**Réponse :** Cluster sets est une nouvelle technologie introduite dans Windows Server 2019. par conséquent, elle n’existe pas dans les versions précédentes. Les clusters de base du système d’exploitation ne peuvent pas joindre un ensemble de clusters. Toutefois, la technologie des mises à niveau propagées du système d’exploitation de cluster doit fournir la fonctionnalité de migration que vous recherchez en mettant à niveau ces clusters vers Windows Server 2019.

**Question :** Les ensembles de clusters peuvent-ils me permettre de mettre à l’échelle le stockage ou le calcul (seul) ? <br>
**Réponse :** Oui, en ajoutant simplement un cluster d’espace de stockage direct ou un cluster Hyper-V traditionnel. Avec les ensembles de clusters, il s’agit d’un changement simple du ratio de calcul à stockage même dans un ensemble de clusters hyper-convergé.

**Question :** Qu’est-ce que les outils de gestion pour les ensembles de clusters ? <br>
**Réponse :** PowerShell ou WMI dans cette version.

**Question :** Comment la migration dynamique entre clusters fonctionne-t-elle avec des processeurs de générations différentes ?  <br>
**Réponse :** Les ensembles de clusters ne contournent pas les différences de processeur et remplacent ce qu’Hyper-V prend actuellement en charge.  Par conséquent, le mode de compatibilité du processeur doit être utilisé avec les migrations rapides.  La recommandation pour les ensembles de clusters consiste à utiliser le même matériel de processeur au sein de chaque cluster individuel, ainsi que le jeu de clusters entier pour les migrations dynamiques entre les clusters.

**Question :** Mes machines virtuelles de l’ensemble de clusters peuvent-elles basculer automatiquement en cas de défaillance du cluster ?  <br>
**Réponse :** Dans cette version, les machines virtuelles de l’ensemble de clusters peuvent uniquement être migrées manuellement sur les clusters ; mais ne peut pas basculer automatiquement. 

**Question :** Comment garantir que le stockage est résilient aux défaillances de cluster ? <br>
**Réponse :** Utilisez la solution de réplica de stockage entre clusters (SR) entre les clusters membres pour tirer parti de la résilience du stockage pour les défaillances de cluster.

**Question :** J’utilise le réplica de stockage (SR) pour répliquer les clusters membres. Les chemins d’accès UNC de stockage de l’espace de noms du cluster sont-ils modifiés sur le basculement SR vers la cible de réplica espaces de stockage direct cluster ? <br>
**Réponse :** Dans cette version, la modification de la référence d’espace de noms d’un ensemble de clusters n’a pas lieu avec le basculement SR. Indiquez à Microsoft si ce scénario est essentiel pour vous et comment vous envisagez de l’utiliser.

**Question :** Est-il possible de basculer des machines virtuelles entre des domaines d’erreur dans une situation de récupération d’urgence (disons que le domaine d’erreur entier est tombé en panne) ? <br>
**Réponse :** Non, Notez que le basculement entre clusters au sein d’un domaine d’erreur logique n’est pas encore pris en charge. 

**Question :** Mon ensemble de cluster peut-il couvrir des clusters dans plusieurs sites (ou domaines DNS) ? <br> 
**réponse :** il s’agit d’un scénario non testé qui n’est pas immédiatement planifié pour la prise en charge de la production. Indiquez à Microsoft si ce scénario est essentiel pour vous et comment vous envisagez de l’utiliser.

**Question :** L’ensemble de clusters fonctionne-t-il avec IPv6 ? <br>
**Réponse :** IPv4 et IPv6 sont tous deux pris en charge avec les clusters de basculement.

**Question :** Quelles sont les exigences en matière de forêt Active Directory pour les ensembles de clusters ? <br>
**Réponse :** Tous les clusters membres doivent se trouver dans la même forêt Active Directory.

**Question :** Combien de nœuds ou de clusters peuvent faire partie d’un seul ensemble de clusters ? <br>
**Réponse :** Dans Windows Server 2019, les ensembles de clusters ont été testés et pris en charge jusqu’à 64 nœuds de cluster au total. Toutefois, l’architecture des ensembles de clusters s’adapte aux plus grandes limites et n’est pas une valeur codée en dur pour une limite. Indiquez à Microsoft si une plus grande échelle est essentielle pour vous et comment vous envisagez de l’utiliser.

**Question :** Toutes les espaces de stockage direct clusters d’un ensemble de cluster forment-ils un seul pool de stockage ? <br>
**Réponse :** º. Espaces de stockage direct technologie continue de fonctionner dans un cluster unique et non pas entre les clusters membres d’un ensemble de clusters.

**Question :** L’espace de noms de l’ensemble de clusters est-il hautement disponible ? <br>
**Réponse :** Oui, l’espace de noms de l’ensemble de clusters est fourni via un serveur d’espaces de noms SOFS de référence en continu (CA) qui s’exécute sur le cluster de gestion. Microsoft recommande de disposer d’un nombre suffisant de machines virtuelles à partir de clusters membres pour le rendre résilient aux défaillances localisées à l’ensemble du cluster. Toutefois, pour tenir compte des défaillances catastrophiques imprévues (par exemple, toutes les machines virtuelles dans le cluster de gestion sont en cours d’exécution en même temps), les informations de référence sont en outre mises en cache de manière permanente dans chaque nœud de l’ensemble de clusters, même au redémarrage.
 
**Question :** L’accès au stockage basé sur l’espace de noms de l’ensemble de clusters a-t-il ralenti les performances de stockage dans un cluster ? <br>
**Réponse :** º. L’espace de noms de l’ensemble de clusters offre un espace de noms de référence dans un ensemble de clusters, conceptuellement comme les espaces de noms système de fichiers DFS (DFSN). Et contrairement à DFSN, toutes les métadonnées de référence de l’espace de noms du jeu de clusters sont automatiquement remplies et mises à jour automatiquement sur tous les nœuds sans aucune intervention de l’administrateur. il n’y a donc pratiquement aucune surcharge de performance dans le chemin d’accès de stockage. 

**Question :** Comment sauvegarder les métadonnées du jeu de clusters ? <br>
**Réponse :** Ce guide est le même que celui du cluster de basculement. La sauvegarde de l’état du système va également sauvegarder l’état du cluster.  Par Sauvegarde Windows Server, vous pouvez effectuer des restaurations de la base de données de cluster d’un nœud (ce qui ne doit jamais être nécessaire en raison de la logique de réparation spontanée dont nous disposons) ou d’une restauration faisant autorité pour restaurer l’ensemble de la base de données de cluster sur tous les nœuds. Dans le cas des ensembles de clusters, Microsoft recommande d’abord d’exécuter une restauration faisant autorité sur le cluster membre, puis sur le cluster de gestion, le cas échéant.
