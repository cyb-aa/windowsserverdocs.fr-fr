---
title: Jeux de clusters
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: johnmarlin-msft
ms.date: 01/30/2019
description: Cet article décrit le scénario de jeux de Cluster
ms.localizationpriority: medium
ms.openlocfilehash: 349b69835ae68c626e886cad30f4d5a89d358372
ms.sourcegitcommit: a3958dba4c2318eaf2e89c7532e36c78b1a76644
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66719708"
---
# <a name="cluster-sets"></a>Jeux de clusters

> S’applique à : Windows Server 2019

Ensembles de cluster est la nouvelle technologie de montée en puissance de cloud dans la version de Windows Server 2019 qui augmente le nombre de nœuds de cluster dans un seul cloud logiciel défini Data Center (SDDC) Privilégiant par ordre de grandeur. Un ensemble de cluster est un regroupement faiblement couplés de plusieurs Clusters de basculement : calcul, le stockage ou hyperconvergé. Cluster à haute technologie permet fluidité de machine virtuelle sur des clusters membre au sein d’un ensemble de cluster et un espace de noms de stockage unifié sur l’ensemble pour prendre en charge la fluidité de machine virtuelle.

Alors que la préservation gestion du Cluster de basculement existante des expériences sur des clusters de membre, une instance de cluster offre en outre des principaux cas d’usage dans la gestion du cycle de vie à l’agrégat. Ce Guide d’évaluation de Windows Server 2019 scénario fournit les informations nécessaires en arrière-plan, ainsi que des instructions pas à pas pour évaluer la technologie de jeux de cluster à l’aide de PowerShell.

## <a name="technology-introduction"></a>Présentation de technologie

Technologie de jeux de cluster est développée pour répondre aux demandes des clients spécifiques d’exploitation de clouds de logiciels définie par centre de données (SDDC) à grande échelle. Proposition de valeur de jeux de cluster peut être résumée comme suit :  

- Augmenter considérablement l’échelle du cloud SDDC pris en charge pour l’exécution de machines virtuelles hautement disponibles en combinant plusieurs clusters plus petits dans une seule infrastructure de grande taille, même tout en conservant de la limite de pannes logicielles à un seul cluster
- Gérer l’intégralité de cycle de vie du Cluster de basculement, y compris l’intégration et la mise hors service des clusters, sans affecter la disponibilité de machine virtuelle de locataire, par le biais de migration avec fluidité les machines virtuelles dans cette structure volumineux
- Facilement modifier le taux de calcul vers le stockage dans votre hyperconvergé je
- Tirer parti de [jeux de domaines d’erreur semblable à Azure et disponibilité](htttps://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) sur des clusters dans la sélection élective de machine virtuelle initiale et de migration d’ordinateurs virtuels ultérieurs
- Mélanger et différentes générations de matériel de l’UC dans le même cluster définir fabric, même lors de la conservation des domaines d’erreur individuels homogène pour une efficacité maximum.  Veuillez noter que la recommandation du même matériel est encore présente dans chaque cluster individuel, ainsi que l’ensemble de l’ensemble du cluster.

À partir d’une vue de haut niveau, il s’agit de quel cluster jeux peuvent ressembler.

![Cluster définit la vue de solution](media/Cluster-sets-Overview/Cluster-sets-solution-View.png)

Ce qui suit fournit un résumé rapide de chacun des éléments dans l’image ci-dessus :

**Cluster de gestion**

Cluster de gestion dans un ensemble de cluster est un Cluster de basculement qui héberge le plan de haut niveau de gestion de l’ensemble de l’ensemble du cluster et de la référence (Cluster définir Namespace) de l’espace de noms de stockage unifié serveur de fichiers avec montée en puissance (SOFS). Un cluster de gestion est logiquement découplé à partir de clusters de membre qui s’exécutent les charges de travail de machine virtuelle. Cela rend le cluster à haute plan de gestion résiliente à n’importe quel cluster à l’échelle les défaillances localisées, par exemple, une perte d’alimentation d’un cluster de membre.   

**Cluster de membre**

Un cluster de membre dans un ensemble de cluster est généralement un cluster hyperconvergé traditionnel en cours d’exécution des charges de travail espaces de stockage Direct et de la machine virtuelle. Plusieurs clusters membre participent à un déploiement de jeu de cluster unique, qui forment la structure du cloud SDDC supérieure. Les clusters de membre diffèrent d’un cluster de gestion dans les deux principaux aspects : clusters de membre participent au domaine d’erreur et constructions à haute disponibilité, et les clusters de membre sont également ajustées à des charges de travail espaces de stockage Direct et de la machine virtuelle hôte. Cluster machines virtuelles qui déplacent au-delà des limites de cluster dans un ensemble de cluster ne doit pas être hébergés sur le cluster de gestion pour cette raison.

**Cluster de définie la référence de l’espace de noms SOFS**

Une référence d’espace de noms cluster ensemble (Cluster définir Namespace) SOFS est un serveur de fichiers de montée en puissance dans laquelle chaque partage SMB sur le serveur SOFS Cluster définir Namespace est un partage de référence – de type « SimpleReferral » qui vient d’être introduite dans Windows Server 2019. Cette référence permet d’accéder à la cible de partage SMB hébergée sur le cluster de membre SOFS clients de Server Message Block (SMB). Le jeu de clusters référence d’espace de noms SOFS est un mécanisme de redirection de légers et par conséquent, n’est pas inclus dans le chemin d’accès d’e/s. Les références de SMB sont mis en cache permanente sur chacun des nœuds client et l’espace de noms de jeux de cluster met dynamiquement à jour automatiquement ces références en fonction des besoins.

**Ensemble principal de cluster**

Dans un ensemble de cluster, la communication entre les clusters de membre est faiblement couplée et est coordonnée par une nouvelle ressource de cluster appelée « Cluster définir Master » (CS-Master). Comme toute autre ressource de cluster, CS-Master est hautement disponible et résistante aux échecs de cluster individuels membre et/ou les échecs de nœud de cluster de gestion. Via un nouveau fournisseur WMI de Cluster défini, CS-Master fournit le point de terminaison de gestion pour toutes les interactions de la facilité de gestion de Cluster défini.

**Travail d’ensemble de cluster**

Dans un déploiement de Cluster défini, le maître de CS interagit avec une nouvelle ressource de cluster sur le membre Clusters appelés « Cluster définir Worker » (CS-Worker). CS-Worker agit comme la liaison uniquement sur le cluster pour orchestrer les interactions de cluster local, comme demandé par le maître de CS. Les exemples de ces interactions de sélection élective de machine virtuelle et l’inventaire des ressources de cluster local. Il en existe qu’une seule instance de CS-Worker pour chaque membre de clusters dans un ensemble de cluster. 

**domaine d’erreur**

Un domaine d’erreur est le regroupement des logiciels et des artefacts de matériel qui détermine l’administrateur peut échouer ensemble lorsqu’une défaillance se produit.  Alors que l’administrateur peut désigner un ou plusieurs clusters ensemble comme un domaine d’erreur, chaque nœud peut participer à un domaine d’erreur dans un groupe à haute disponibilité. Cluster définit par conception laisse le choix de la détermination de limites de domaine d’erreur à l’administrateur qui est familiarisé avec les considérations de topologie de centre de données – par exemple, les PDU, mise en réseau : partagent des clusters de membre. 

**Groupe à haute disponibilité**

Un groupe à haute disponibilité permet à l’administrateur de configurer une redondance souhaitée des charges de travail en cluster entre les domaines d’erreur, en organiser ces informations dans un groupe à haute disponibilité et en déployant des charges de travail dans ce groupe à haute disponibilité. Supposons que si vous déployez une application à deux niveaux, nous vous recommandons de configurer au moins deux machines virtuelles dans un groupe de disponibilité pour chaque niveau afin de vous assurer que lorsqu’un domaine d’erreur dans ce groupe à haute disponibilité tombe en panne, votre application dispose au moins une machine virtuelle dans chaque niveau hébergé sur un domaine d’erreur différents de ce même groupe à haute disponibilité.

## <a name="why-use-cluster-sets"></a>Pourquoi utiliser des ensembles de cluster

Ensembles de cluster offre une mise à l’échelle sans pour autant sacrifier la résilience.  

Ensembles de cluster permet une pour le clustering de plusieurs clusters ensemble pour créer une structure de grande taille, tandis que chaque cluster reste indépendant pour assurer la résilience.  Par exemple, vous avez un HCL de 4 nœuds plusieurs clusters de machines virtuelles en cours d’exécution.  Chaque cluster fournit la résilience nécessaire pour lui-même.  Si la mémoire ou le stockage commence à augmenter, est l’étape suivante.  Avec montée en puissance, il existe quelques options et les considérations.

1. Ajouter du stockage au cluster en cours.  Espaces de stockage Direct, cela peut être difficile car les mêmes lecteurs de modèle/microprogramme n’est peut-être pas disponibles.  Vous devez également prendre en compte la prise en compte des délais de régénération.
2. Ajoutez davantage de mémoire.  Que se passe-t-il si vous sont atteint sa limite de la mémoire, que les machines peuvent gérer ?  Que se passe-t-il si tous les emplacements de mémoire disponible sont saturées ?
3. Ajouter des nœuds de calcul supplémentaires avec des lecteurs dans le cluster actuel.  Revenez nous Option 1 avoir à prendre en considération.
4. Acheter un nouveau cluster

Il s’agit où les jeux de cluster offre une mise à l’échelle.  Si j’ajoute mes clusters en un ensemble de cluster, je peux profiter de stockage ou de la mémoire qui peut-être être disponible sur un autre cluster sans les achats supplémentaires.  Du point de vue de la résilience, ajouter des nœuds à un espace de stockage Direct ne va pas à fournir des votes supplémentaires pour le quorum.  Comme mentionné [ici](drive-symmetry-considerations.md), un Cluster à espaces de stockage Direct peut survivre à la perte de 2 nœuds avant d’aller vers le bas.  Si vous disposez d’un cluster HCL 4, 3 nœuds arrêtent prendra l’ensemble du cluster vers le bas.  Si vous avez un cluster 8 nœuds, 3 nœuds arrêtent prendra l’ensemble du cluster vers le bas.  Avec les jeux de Cluster qui a deux clusters de HCL de 4 nœuds dans le jeu, 2 nœuds dans un seul HCL tombent en panne et 1 nœud dans l’autres HCL tombent en panne, les deux clusters restent en fonction.  Est-il préférable de créer un grand cluster espaces de stockage Direct de 16 nœuds ou la décomposer en quatre clusters de 4 nœuds et utiliser des ensembles de cluster ?  Avoir quatre clusters de 4 nœuds avec cluster jeux donne la même échelle, mais une meilleure résilience dans la mesure où plusieurs nœuds de calcul peuvent descendre (ou de façon inattendue pour la maintenance) et reste en production.

## <a name="considerations-for-deploying-cluster-sets"></a>Considérations relatives au déploiement des ensembles de cluster

Lorsque vous envisagez si les ensembles de cluster est quelque chose que vous devez utiliser, posez-vous ces questions :

- Avez-vous besoin d’aller au-delà des HCL calcul et stockage à l’échelle limites actuelles ?
- Diffèrent calcul et stockage identique ?
- En direct migrer les ordinateurs virtuels entre les clusters ?
- Voulez-vous que les groupes à haute disponibilité Azure de type ordinateur et les domaines d’erreur sur plusieurs clusters ?
- Vous devez prendre le temps d’examiner tous vos clusters pour déterminer où les nouvelles machines virtuelles doivent être placées ?

Si votre réponse est Oui, les ensembles de cluster est ce dont vous avez besoin.

Il existe quelques autres éléments à prendre en compte dans lequel une plus grande SDDC peut changer vos stratégies de centre de données globale.  SQL Server est un bon exemple.  Déplacement machines virtuelles de SQL Server entre des clusters requiert-il SQL à exécuter sur des nœuds supplémentaires de gestion de licences ?  

## <a name="scale-out-file-server-and-cluster-sets"></a>Serveur de fichiers avec montée en puissance et de jeux de cluster

Dans Windows Server 2019, il est appelé serveur de fichiers avec montée en puissance Infrastructure (SOFS) un nouveau rôle de serveur de fichiers de montée en puissance. 

Les considérations suivantes s’appliquent à un rôle d’Infrastructure SOFS :

1.  Il peut y avoir qu’un seul rôle de cluster SOFS de l’Infrastructure sur un Cluster de basculement. Rôle d’infrastructure SOFS est créée en spécifiant le « **-Infrastructure**» changez le paramètre à la **Add-ClusterScaleOutFileServerRole** applet de commande.  Exemple :

        Add-ClusterScaleoutFileServerRole -Name "my_infra_sofs_name" -Infrastructure

2.  Chaque volume partagé de cluster créé automatiquement dans le basculement déclenche la création d’un partage SMB avec un nom généré automatiquement en fonction du nom de volume CSV. Un administrateur ne peut pas créer ou modifier directement des partages SMB dans un rôle de serveur SOFS, autres que via des opérations de création/modification de volume CSV.

3.  Dans les configurations hyperconvergées, un serveur SOFS Infrastructure permet à un client SMB (hôte Hyper-V) pour communiquer avec garantie continue disponibilité (CA) sur le serveur d’Infrastructure SOFS SMB. Cette boucle SMB hyperconvergé autorité de certification est obtenue via l’accès à leurs fichiers de disque virtuel (VHDx) où l’identité de machine virtuelle propriétaire est transférée entre le client et le serveur des machines virtuelles. Ce transfert d’identité permet aux fichiers de liste ACL en intégrant VHDx comme dans les configurations de cluster hyperconvergé standard comme avant.

Une fois un ensemble de cluster est créé, l’espace de noms de jeu de cluster s’appuie sur un serveur d’Infrastructure SOFS sur chacun des clusters de membre et, en outre, un serveur SOFS Infrastructure dans le cluster de gestion.

Au moment où un cluster de membre est ajouté à un ensemble de cluster, l’administrateur spécifie le nom d’un serveur SOFS Infrastructure sur ce cluster s’il en existe déjà. Si le serveur d’Infrastructure SOFS n’existe pas, un nouveau rôle d’Infrastructure SOFS sur le nouveau cluster de membre est créé par cette opération. Si un rôle d’Infrastructure SOFS existe déjà sur le cluster de membre, l’opération d’ajout implicitement renomme le nom spécifié en fonction des besoins. Les serveurs SMB singleton existants, ni les rôles de serveur SOFS - Infrastructure sur le membre clusters sont laissés inutilisés par l’ensemble du cluster. 

Au moment de l’ensemble du cluster est créé, l’administrateur a la possibilité d’utiliser un objet d’ordinateur Active Directory existante en tant que l’espace de noms racine sur le cluster de gestion. Opérations de création de jeu de cluster créer le rôle de cluster SOFS de l’Infrastructure sur le cluster de gestion ou renomme le rôle d’Infrastructure SOFS existant juste comme décrit précédemment pour les clusters de membre. Le serveur d’Infrastructure SOFS sur le cluster de gestion est utilisé comme référence de l’espace de noms (Cluster définir Namespace) SOFS le cluster. Cela signifie simplement que chaque partage SMB sur le cluster définie d’espace de noms que SOFS est un partage de référence – de type « SimpleReferral » - qui vient d’être introduite dans Windows Server 2019.  Cette référence permet à SMB aux clients d’accéder à la cible de partage SMB hébergé sur le cluster SOFS de membre. Le jeu de clusters référence d’espace de noms SOFS est un mécanisme de redirection de légers et par conséquent, n’est pas inclus dans le chemin d’accès d’e/s. Les références de SMB sont mis en cache permanente sur chacun des nœuds client et l’espace de noms de jeux de cluster met dynamiquement à jour automatiquement ces références en fonction des besoins

## <a name="creating-a-cluster-set"></a>Création d’un ensemble de Cluster

### <a name="prerequisites"></a>Prérequis

Lorsque la valeur de la création d’un cluster, vous les prérequis suivants sont recommandés :

1. Configurer un client de gestion exécutant Windows Server 2019.
2. Installer les outils de Cluster de basculement sur ce serveur d’administration.
3. Créer des membres du cluster (au moins deux clusters au moins deux Volumes partagés de Cluster sur chaque cluster)
4. Créer un cluster de gestion (physique ou invité) qui rapproche les clusters de membre.  Cette approche garantit que les ensembles de Cluster plan de gestion continue d’être disponibles malgré les échecs de cluster de membres possibles.

### <a name="steps"></a>Étapes

1. Créer un cluster défini à partir de trois clusters, comme défini dans les conditions préalables.  Le sous graphique donne un exemple de clusters à créer.  Le nom du cluster défini dans cet exemple sera **CSMASTER**.

   | Nom du cluster               | Nom du serveur SOFS infrastructure pour une utilisation ultérieure | 
   |----------------------------|-------------------------------------------|
   | CLUSTER DE JEU                | CLUSTERSET SOFS                           |
   | CLUSTER1                   | SOFS-CLUSTER1                             |
   | CLUSTER2                   | SOFS-CLUSTER2                             |

2. Une fois que tous les clusters ont été créées, utilisez les commandes suivantes pour créer le cluster maître du jeu.

        New-ClusterSet -Name CSMASTER -NamespaceRoot SOFS-CLUSTERSET -CimSession SET-CLUSTER

3. Pour ajouter un serveur de Cluster à l’ensemble du cluster, le ci-dessous serait utilisé.

        Add-ClusterSetMember -ClusterName CLUSTER1 -CimSession CSMASTER -InfraSOFSName SOFS-CLUSTER1
        Add-ClusterSetMember -ClusterName CLUSTER2 -CimSession CSMASTER -InfraSOFSName SOFS-CLUSTER2

   > [!NOTE]
   > Si vous utilisez un schéma d’adresse IP statique, vous devez inclure *- StaticAddress x.x.x.x* sur le **New-ClusterSet** commande.

4. Une fois que vous avez créé le cluster définie en dehors des membres du cluster, vous pouvez répertorier l’ensemble de nœuds et de ses propriétés.  Pour énumérer tous les clusters de membre dans l’ensemble du cluster :

        Get-ClusterSetMember -CimSession CSMASTER

5. Pour énumérer tous les clusters de membre dans le cluster haute y compris les nœuds de cluster de gestion :

        Get-ClusterSet -CimSession CSMASTER | Get-Cluster | Get-ClusterNode

6. Pour répertorier tous les nœuds à partir des clusters de membre :

        Get-ClusterSetNode -CimSession CSMASTER

7. Pour répertorier tous les groupes de ressources sur l’ensemble du cluster :

        Get-ClusterSet -CimSession CSMASTER | Get-Cluster | Get-ClusterGroup 

8. Pour vérifier le cluster de définie le processus de création créé un partage SMB (identifié comme Volume1 ou quel que soit le dossier CSV est étiqueté avec le ScopeName étant le nom du serveur de fichiers l’Infrastructure et le chemin d’accès à la fois comme) sur le serveur d’Infrastructure SOFS pour chaque membre de cluster Volume partagé de cluster :

        Get-SmbShare -CimSession CSMASTER

8. Ensembles de cluster a des journaux de débogage qui peuvent être collectées pour révision.  Définie le cluster et les journaux de débogage de cluster peuvent être rassemblées pour tous les membres et le cluster de gestion.

        Get-ClusterSetLog -ClusterSetCimSession CSMASTER -IncludeClusterLog -IncludeManagementClusterLog -DestinationFolderPath <path>

9. Configurer Kerberos [la délégation contrainte](https://blogs.technet.microsoft.com/virtualization/2017/02/01/live-migration-via-constrained-delegation-with-kerberos-in-windows-server-2016/) entre cluster tous les membres du jeu.

10. Configurer le type d’authentification de migration dynamique entre clusters virtual machine pour Kerberos sur chaque nœud dans l’ensemble du Cluster.

        foreach($h in $hosts){ Set-VMHost -VirtualMachineMigrationAuthenticationType Kerberos -ComputerName $h }

11. Ajoutez le cluster de gestion au groupe Administrateurs local sur chaque nœud dans l’ensemble du cluster.

        foreach($h in $hosts){ Invoke-Command -ComputerName $h -ScriptBlock {Net localgroup administrators /add <management_cluster_name>$} }

## <a name="creating-new-virtual-machines-and-adding-to-cluster-sets"></a>Création d’ordinateurs virtuels et l’ajout aux jeux de cluster

Une fois que la valeur de la création du cluster, l’étape suivante consiste à créer des machines virtuelles.  Normalement, lorsqu’il est temps pour créer des machines virtuelles et les ajouter à un cluster, vous devez effectuer quelques vérifications sur les clusters pour voir lequel il peut être préférable d’exécuter.  Ces vérifications sont les suivantes :

- La quantité de mémoire est disponible sur les nœuds de cluster ?
- La quantité d’espace disque est disponible sur les nœuds de cluster ?
- La machine virtuelle nécessite des exigences de stockage spécifique (par exemple, je veux mes machines virtuelles de SQL Server pour accéder à un cluster en cours d’exécution de disques rapides ; ou, ma machine virtuelle d’infrastructure n’est pas aussi critique et peut s’exécuter sur des lecteurs plus lents).

Une fois ce questions sont traitées, vous créez la machine virtuelle sur le cluster que vous avez besoin d’utiliser.  Un des avantages des jeux de cluster est que les ensembles de cluster effectuer ces vérifications pour vous et placer l’ordinateur virtuel sur le nœud plus optimal.

Les commandes ci-dessous à la fois identifie le cluster optimal et déployer la machine virtuelle sur celui-ci.  Dans l’exemple ci-dessous, une nouvelle machine virtuelle est créée en spécifiant qu’au moins 4 gigaoctets de mémoire est disponible pour la machine virtuelle et qu’il est nécessaire d’utiliser 1 processeur virtuel.

- Assurez-vous que les 4 Go est disponible pour la machine virtuelle
- définir le processeur virtuel utilisé à 1
- Assurez-vous que moins de 10 % UC disponible pour la machine virtuelle

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

Lorsqu’elle est terminée, nous vous fournirons les informations sur la machine virtuelle et où il a été placé.  Dans l’exemple ci-dessus, il s’affichera comme :

        State         : Running
        ComputerName  : 1-S2D2

Si vous deviez n'avoir pas assez de mémoire, UC ou d’espace disque pour ajouter l’ordinateur virtuel, vous recevrez l’erreur :

      Get-ClusterSetOptimalNodeForVM : A cluster node is not available for this operation.  

Une fois que la machine virtuelle a été créée, il s’affichera dans le Gestionnaire Hyper-V sur le nœud spécifique spécifié.  Pour l’ajouter comme un cluster de machine virtuelle et dans le cluster, la commande figure ci-dessous.  

        Register-ClusterSetVM -CimSession CSMASTER -MemberName $targetnode.Member -VMName CSVM1

Lorsqu’elle est terminée, la sortie sera :

         Id  VMName  State  MemberName  PSComputerName
         --  ------  -----  ----------  --------------
          1  CSVM1      On  CLUSTER1    CSMASTER

Si vous avez ajouté un cluster avec des machines virtuelles existantes, les machines virtuelles amené à inscrire avec Cluster jeux inscrire, toutes les machines virtuelles à la fois, la commande à utiliser est :

        Get-ClusterSetMember -name CLUSTER3 -CimSession CSMASTER | Register-ClusterSetVM -RegisterAll -CimSession CSMASTER

Toutefois, le processus n’est pas terminé comme le chemin d’accès à la machine virtuelle doit être ajouté à l’espace de noms de jeu de cluster.

Par exemple, un cluster existant est ajouté et il a des machines virtuelles préconfigurées le se trouvent sur le local Cluster Volume partagé (CSV), le chemin d’accès pour le VHDX serait quelque chose de similaire à « C:\ClusterStorage\Volume1\MYVM\Virtual dur Disks\MYVM.vhdx.  Une migration de stockage serait doivent être effectuées que les chemins d’accès du volume partagé de cluster sont en local à un cluster de membre unique de la conception. Par conséquent, pas seront accessibles à la machine virtuelle une fois qu’ils live migration entre clusters de membre. 

Dans cet exemple, CLUSTER3 a été ajouté au cluster à l’aide de Add-ClusterSetMember avec le serveur de fichiers de montée en puissance Infrastructure en tant que SOFS-CLUSTER3.  Pour déplacer la configuration de machine virtuelle et le stockage, la commande est :

        Move-VMStorage -DestinationStoragePath \\SOFS-CLUSTER3\Volume1 -Name MYVM

Une fois qu’elle est terminée, vous recevrez un avertissement :

        WARNING: There were issues updating the virtual machine configuration that may prevent the virtual machine from running.  For more information view the report file below.
        WARNING: Report file location: C:\Windows\Cluster\Reports\Update-ClusterVirtualMachineConfiguration '' on date at time.htm.

Cet avertissement peut être ignoré car l’avertissement est « aucune modification dans la configuration de stockage du rôle machine virtuelle ont été détectées ».  La raison de l’avertissement en tant que l’emplacement physique réel ne change pas ; uniquement les chemins de la configuration. 

Pour plus d’informations sur Move-VMStorage, reportez-vous à cette [lien](https://docs.microsoft.com/powershell/module/hyper-v/move-vmstorage?view=win10-ps). 

Live Migration d’un ordinateur virtuel entre des clusters d’ensemble de cluster différent n'est pas le même que dans le passé. Dans les scénarios de l’ensemble de non cluster, la procédure serait :

1. supprimer le rôle de machine virtuelle du Cluster.
2. migrer dynamiquement l’ordinateur virtuel à un nœud de membre d’un autre cluster.
3. Ajoutez la machine virtuelle dans le cluster comme un nouveau rôle de machine virtuelle.

Avec des jeux de Cluster, ces étapes ne sont pas nécessaires et qu’une seule commande est nécessaire.  Tout d’abord, vous devez définir tous les réseaux doivent être disponibles pour la migration avec la commande :

    Set-VMHost -UseAnyNetworkMigration $true

Par exemple, je souhaite déplacer une machine virtuelle de Cluster défini à partir de CLUSTER1 à NODE2-CL3 sur CLUSTER3.  La commande serait :

        Move-ClusterSetVM -CimSession CSMASTER -VMName CSVM1 -Node NODE2-CL3

Veuillez noter que cela ne déplace pas les fichiers de stockage ou de configuration de machine virtuelle.  Ce n’est pas nécessaire, car le chemin d’accès à la machine virtuelle reste en tant que \\SOFS-CLUSTER1\VOLUME1.  Une fois qu’un ordinateur virtuel a été enregistré avec des jeux de cluster a le chemin d’accès du partage de serveur de fichiers d’Infrastructure, la machine virtuelle et les lecteurs ne nécessitent pas en cours sur le même ordinateur que la machine virtuelle.

## <a name="creating-availability-sets-fault-domains"></a>Disponibilité de création d’ensembles de domaines d’erreur

Comme décrit dans l’introduction, les domaines d’erreur semblable à Azure et les groupes à haute disponibilité peuvent être configurés dans un ensemble de cluster.  Cela est utile pour les emplacements de machine virtuelle initiale et les migrations entre clusters.  

Dans l’exemple ci-dessous, il existe quatre clusters participant à l’ensemble du cluster.  Dans le jeu, un domaine d’erreur logique sera créé avec deux des clusters et un domaine d’erreur créé avec les deux autres clusters.  Ces deux domaines d’erreur comprend l’ensemble de disponibilité. 

Dans l’exemple ci-dessous, CLUSTER1 et CLUSTER2 sera dans un domaine d’erreur appelé **FD1** alors que CLUSTER3 et CLUSTER4 seront dans un domaine d’erreur appelé **FD2 Répartiteurs**.  Le groupe à haute disponibilité sera appelée **CSMASTER-AS** et comprendre les deux domaines d’erreur.

Pour créer les domaines d’erreur, les commandes sont :

        New-ClusterSetFaultDomain -Name FD1 -FdType Logical -CimSession CSMASTER -MemberCluster CLUSTER1,CLUSTER2 -Description "This is my first fault domain"

        New-ClusterSetFaultDomain -Name FD2 -FdType Logical -CimSession CSMASTER -MemberCluster CLUSTER3,CLUSTER4 -Description "This is my second fault domain"

Pour vous assurer qu’ils ont été créés avec succès, vous pouvez exécuter Get-ClusterSetFaultDomain avec sa sortie indiqué.

        PS C:\> Get-ClusterSetFaultDomain -CimSession CSMASTER -FdName FD1 | fl *

        PSShowComputerName    : True
        FaultDomainType       : Logical
        ClusterName           : {CLUSTER1, CLUSTER2}
        Description           : This is my first fault domain
        FDName                : FD1
        Id                    : 1
        PSComputerName        : CSMASTER

Maintenant que les domaines d’erreur ont été créées, la disponibilité définie doit être créée.

        New-ClusterSetAvailabilitySet -Name CSMASTER-AS -FdType Logical -CimSession CSMASTER -ParticipantName FD1,FD2

Pour valider qu’il a été créé, puis utiliser :

        Get-ClusterSetAvailabilitySet -AvailabilitySetName CSMASTER-AS -CimSession CSMASTER

Lorsque vous créez de nouvelles machines virtuelles, vous devez alors utiliser le paramètre - groupe à haute disponibilité dans le cadre de la détermination du nœud optimal.  Par conséquent, elle ressemblerait quelque chose comme ceci :

        # Identify the optimal node to create a new virtual machine
        $memoryinMB=4096
        $vpcount = 1
        $av = Get-ClusterSetAvailabilitySet -Name CSMASTER-AS -CimSession CSMASTER
        $targetnode = Get-ClusterSetOptimalNodeForVM -CimSession CSMASTER -VMMemory $memoryinMB -VMVirtualCoreCount $vpcount -VMCpuReservation 10 -AvailabilitySet $av
        $secure_string_pwd = convertto-securestring "<password>" -asplaintext -force
        $cred = new-object -typename System.Management.Automation.PSCredential ("<domain\account>",$secure_string_pwd)

Suppression d’un cluster cluster définit en raison des différents cycles de vie. Il est parfois lorsqu’un cluster doit être supprimé à partir d’un ensemble de cluster. Comme meilleure pratique, toutes les machines virtuelles cluster doit être déplacés hors du cluster. Cela peut être accompli à l’aide de la **Move-ClusterSetVM** et **Move-VMStorage** commandes.

Toutefois, si les machines virtuelles ne seront pas déplacés ainsi, les jeux de cluster s’exécute une série d’actions pour fournir un résultat intuitif à l’administrateur.  Lorsque le cluster est supprimé de l’ensemble, tous les autres cluster machines virtuelles hébergées sur le cluster en cours de suppression devient simplement des machines virtuelles hautement disponibles, liés à ce cluster, en supposant qu’ils ont accès à leur stockage.  Ensembles de cluster met également automatiquement à jour son inventaire par :

- N’est plus le suivi l’intégrité du cluster supprimé de maintenant et les machines virtuelles en cours d’exécution sur celui-ci
- Supprime de l’espace de noms de jeu de cluster et toutes les références aux partages hébergés sur le cluster maintenant-supprimé

Par exemple, la commande pour supprimer le cluster CLUSTER1 à partir de jeux de cluster serait :

        Remove-ClusterSetMember -ClusterName CLUSTER1 -CimSession CSMASTER

## <a name="frequently-asked-questions-faq"></a>Forum Aux Questions (FAQ)

**Question :** Dans mon ensemble de cluster, suis-je limité à uniquement à l’aide de clusters hyperconvergés ? <br>
**Réponse :** Non.  Vous pouvez combiner des espaces de stockage Direct avec les clusters traditionnels.

**Question :** Puis-je gérer mon Cluster définie par le biais de System Center Virtual Machine Manager ? <br>
**Réponse :** System Center Virtual Machine Manager ne prend pas en charge les ensembles de Cluster <br><br> **Question :** Peuvent Windows Server 2012 R2 ou 2016 clusters coexister dans le même ensemble de cluster ? <br>
**Question :** Puis-je migrer des charges de travail hors tension de Windows Server 2012 R2 ou 2016 clusters en demandant simplement ces clusters joindre le même ensemble de Cluster ? <br>
**Réponse :** Ensembles de cluster est une nouvelle technologie introduite dans Windows Server 2019, par conséquent, en tant que tel, il n’existe pas dans les versions précédentes. Clusters basée sur le système d’exploitation de bas niveau ne peut pas joindre un ensemble de cluster. Toutefois, technologie de mises à niveau propagée de système d’exploitation de Cluster doit fournir la fonctionnalité de migration que vous cherchez en mettant à niveau ces clusters pour Windows Server 2019.

**Question :** Pouvant ensembles de Cluster me permettre de mettre à l’échelle de stockage ou de calcul (seul) ? <br>
**Réponse :** Oui, en ajoutant simplement un espace de stockage Direct ou cluster Hyper-V traditionnel. Avec des jeux de cluster, il est une simple modification du taux de calcul-stockage même dans un ensemble de cluster hyperconvergé.

**Question :** Nouveautés, les outils de gestion pour les jeux de cluster <br>
**Réponse :** PowerShell ou WMI dans cette version.

**Question :** La migration dynamique entre clusters le fonctionnement de processeurs de différentes générations ?  <br>
**Réponse :** Ensembles de cluster ne puissent pas contourner les différences de processeur et les remplacer ce que Hyper-V prend actuellement en charge.  Par conséquent, mode de compatibilité de processeur doit être utilisé avec des migrations rapides.  La recommandation pour les jeux de Cluster consiste à utiliser le même matériel de processeur dans chaque Cluster individuels, ainsi que l’ensemble de Cluster entière pour les migrations dynamiques entre les clusters se produise.

**Question :** Pouvez mon ensemble de cluster virtuel machines automatiquement sur la défaillance d’un cluster de basculement ?  <br>
**Réponse :** Dans cette version, machines virtuelles cluster peut uniquement être manuellement migrés sur des clusters ; mais ne peut pas le basculement automatique. 

**Question :** Comment assurer stockage résiste aux défaillances de cluster ? <br>
**Réponse :** Utilisez solution de réplica de stockage (SR) entre clusters sur des clusters de membre de réaliser la résilience du stockage pour les échecs de cluster.

**Question :** J’utilise (Réplica) de stockage pour répliquer sur des clusters de membre. Mettez en cluster stockage espace de noms de jeu de modification des chemins d’accès UNC lors du basculement de SR vers le cluster espaces de stockage Direct de réplica cible ? <br>
**Réponse :** Dans cette version, une cluster ensemble espace de noms référence modification de ce type n’a pas lieu avec SR basculement. Veuillez informer Microsoft si ce scénario est essentiel pour vous et comment vous souhaitez les utiliser.

**Question :** Il est possible de basculer des machines virtuelles entre les domaines d’erreur dans une situation de récupération d’urgence (par exemple le domaine d’erreur entier s’est arrêté) ? <br>
**Réponse :** Non, notez que le basculement entre clusters au sein d’une erreur logique domaine n’est pas encore possible. 

**Question :** Mon cluster peut définir des clusters span dans plusieurs sites (ou les domaines DNS) ? <br> 
**Réponse :** Ceci est un scénario non testé et pas immédiatement planifié pour la prise en charge de production. Veuillez informer Microsoft si ce scénario est essentiel pour vous et comment vous souhaitez les utiliser.

**Question :** Cluster attribuer de travail avec IPv6 ? <br>
**Réponse :** À la fois IPv4 et IPv6 sont prises en charge avec des jeux de cluster comme avec les Clusters de basculement.

**Question :** Quelles sont les exigences de la forêt Active Directory pour le cluster définit <br>
**Réponse :** Tous les clusters de membre doivent être dans la même forêt Active Directory.

**Question :** Le nombre de nœuds ou de clusters peut être partie d’un cluster unique définie ? <br>
**Réponse :** Ensembles de cluster dans Windows Server 2019, testé et pris en charge jusqu'à 64 nœuds totale du cluster. Toutefois, cluster définit les échelles de l’architecture des limites beaucoup plus volumineux et n’est pas quelque chose qui est codé en dur pour une limite. Veuillez informer Microsoft si plus grande échelle est essentiel pour vous et comment vous souhaitez les utiliser.

**Question :** Tous les clusters d’espaces de stockage Direct dans un ensemble de cluster constitueront un seul pool de stockage ? <br>
**Réponse :** Non. Technologie Direct des espaces de stockage fonctionne toujours dans un seul cluster et non entre les clusters de membre dans un ensemble de cluster.

**Question :** Est le jeu de clusters à haute disponibilité de l’espace de noms ? <br>
**Réponse :** Oui, l’espace de noms de jeu de cluster est fourni via un serveur d’espace de noms en permanence disponibles (CA) référence SOFS en cours d’exécution sur le cluster de gestion. Microsoft recommande d’avoir un nombre suffisant de machines virtuelles à partir de clusters de membre pour la rendre résiliente pour les défaillances localisées à l’échelle du cluster. Toutefois, pour prendre en compte les défaillances graves imprévus – par exemple, toutes les machines virtuelles dans le cluster de gestion en panne en même temps les informations de référence sont en outre de manière permanente mis en cache dans chaque nœud de cluster l’ensemble, même pour tous les redémarrages.
 
**Question :** Le cluster définie l’accès de stockage basé sur l’espace de noms de ralentir les performances de stockage dans un ensemble de cluster ? <br>
**Réponse :** Non. Espace de noms de cluster ensemble offre un espace de noms de référence de superposition au sein d’un ensemble de cluster – conceptuellement comme distribuées fichier système espaces de noms (DFSN). Et contrairement DFSN, toutes les métadonnées de référence espace de noms ensemble cluster sont rempli automatiquement et la mise à jour automatique sur tous les nœuds sans aucune intervention de l’administrateur, il n’est presque aucune surcharge de performances dans le chemin d’accès de stockage. 

**Question :** Comment puis-je sauvegarder des métadonnées du jeu de cluster ? <br>
**Réponse :** Ce guide est identique à celui du Cluster de basculement. La sauvegarde d’état système sauvegarde l’état du cluster.  Via la sauvegarde de Windows Server, vous pouvez effectuer des restaurations de simplement d’un nœud cluster de base de données (qui ne doit jamais être nécessaire en raison d’un ensemble de la logique d’auto-adaptation nous avons) ou effectuer une restauration faisant autorité pour restaurer la base de données de l’ensemble du cluster sur tous les nœuds. Dans le cas d’ensembles de cluster, Microsoft vous recommande de procéder à ce type d’une restauration faisant autorité tout d’abord le cluster de membre, puis sur le cluster de gestion si nécessaire.
