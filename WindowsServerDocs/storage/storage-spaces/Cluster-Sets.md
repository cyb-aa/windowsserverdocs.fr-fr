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
ms.openlocfilehash: 2deeb6968f910e80bacb2354ad2e575060a7797a
ms.sourcegitcommit: 07aefbdbb0eedb42aaed3d195c2429310c761da0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/31/2019
ms.locfileid: "9041005"
---
# Jeux de clusters

> S’applique à: Windows Server Insider Preview-build 17650 version ultérieure

Jeux de cluster est une nouvelle technologie dans cette version d’évaluation qui augmente le nombre de nœuds de cluster dans un seul cloud de centre de données défini logiciel (SDDC) en ordre de grandeur avec montée en cloud. Un ensemble de cluster est un regroupement abandonnent de plusieurs Clusters de basculement: calcul, stockage, voire hyperconvergée. Cluster définit permet de technologie fluidité de machine virtuelle sur les clusters de membre au sein d’un ensemble de cluster et un espace de noms de stockage unifié sur l’ensemble de la prise en charge intégrée fluidité de machine virtuelle. 

Tandis que la gestion de Cluster de basculement existante conservation des expériences sur les clusters de membre, une instance de jeu de cluster offre en outre cas d’utilisation de clés autour de gestion du cycle de vie au total. Ce Guide d’évaluation de Windows Server version d’évaluation scénario vous fournit les informations nécessaires en arrière-plan, ainsi que des instructions pas à pas pour évaluer la technologie de jeux de cluster à l’aide de PowerShell. 

## Présentation de la technologie

Technologie de jeux de cluster est développée pour répondre aux demandes de clients spécifiques d’exploitation nuages Datacenter de défini par logiciel (SDDC) à l’échelle. Proposition de valeur de jeux de cluster dans cette version d’évaluation peut se résumer comme suit:  

- Augmenter considérablement l’échelle du cloud SDDC pris en charge pour l’exécution de machines virtuelles hautement disponibles en combinant plusieurs clusters plus petits en un grand fabric unique, voire tout en conservant de la limite de tolérance des logiciels à un seul cluster
- Gérer l’ensemble du cycle de vie du Cluster de basculement, y compris d’intégration et de retrait des clusters, sans impact sur la disponibilité de la machine virtuelle locataire, via la migration de façon fluide machines virtuelles sur ce grand fabric
- Facilement changer le taux de calcul vers le stockage dans votre hyperconvergé j’ai
- Bénéficier de [domaines d’erreur similaires Azure et de disponibilité définit](htttps://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) sur les clusters de placement de la machine virtuelle initiale et la migration des machines virtuelles ultérieures
- Et combinaisons de différentes générations de matériel de processeur, dans le même cluster définir fabric, même conservant domaines d’erreur individuels homogène pour une efficacité optimale.  Veuillez noter que la recommandation du même matériel est toujours présente dans chaque cluster individuels, ainsi que l’ensemble de l’ensemble du cluster.

À partir d’un affichage de haut niveau, il s’agit de quel cluster jeux peuvent ressembler.

![Cluster définit solution vue](media\Cluster-sets-Overview\Cluster-sets-solution-View.png)

Ce qui suit fournit un résumé rapide de chacun des éléments dans l’image ci-dessus:

**Cluster de gestion**

Cluster de gestion dans un ensemble de cluster est un Cluster de basculement qui héberge le plan de gestion hautement disponibles de l’ensemble de l’ensemble du cluster et la référence d’espace de noms (Cluster définir Namespace) stockage unifié avec montée en fichier de serveur en puissance parallèle. Un cluster de gestion est logiquement découplé à partir de clusters membre qui s’exécutent les charges de travail de machine virtuelle. Cela rend le cluster défini plan gestion résilient sur les échecs de cluster à l’échelle du localisées, par exemple, une perte de puissance d’un cluster de membre.   

**Cluster de membre**

Un membre cluster dans un ensemble de cluster est en règle générale, un cluster hyperconvergé traditionnel des charges de travail espaces de stockage Direct et de la machine virtuelle en cours d’exécution. Plusieurs clusters membre participent à un déploiement de jeu seul cluster, formant l’infrastructure de cloud SDDC plus grande. Diffèrent des clusters de membre d’un cluster de gestion dans deux principaux aspects: clusters membre participent au domaine d’erreur et disponibilité définie constructions de clusters membre sont également ajustées à machine virtuelle hôte et les charges de travail espaces de stockage Direct. Cluster ensemble des machines virtuelles qui se déplacent au-delà des limites de cluster dans un ensemble de cluster ne doivent pas être hébergés sur le cluster de gestion pour cette raison.

**Référence d’espace de noms en puissance de jeu de clusters**

Une référence d’espace de noms cluster ensemble (Cluster définir Namespace) en puissance est un serveur de fichiers avec montée en où chaque partage SMB sur la définir en puissance de Namespace Cluster est un partage de référence – de type «SimpleReferral» qui vient d’être introduite dans cette version d’évaluation.  Cette référence permet d’accéder à la cible de partage SMB hébergée sur le cluster de membre en puissance clients Server Message Block (SMB). Le jeu de clusters référence d’espace de noms en puissance est un mécanisme de redirection légère et par conséquent, n’est pas inclus dans le chemin d’accès e/s. Les références de SMB sont mises en cache permanence sur chacun des nœuds du client et l’espace de noms de jeux de cluster dynamiquement met à jour automatiquement ces références en fonction des besoins.

**Cluster maître des jeux**

Dans un ensemble de cluster, la communication entre les clusters de membre est faiblement couplée et est coordonnée par une nouvelle ressource de cluster appelée «Cluster définir de base» (CS-Master). Comme toute autre ressource de cluster, CS-Master est hautement disponible et résilient à des échecs de cluster de membres individuels et/ou les défaillances de nœud de cluster de gestion. Par le biais d’un nouveau fournisseur WMI de définir des clusters, CS-maître fournit le point de terminaison de gestion pour toutes les interactions de facilité de gestion de Cluster défini.

**Travailleur de jeu de cluster**

Dans un déploiement de Cluster définie, le masque de CS interagit avec une nouvelle ressource de cluster sur le membre Clusters appelés «Cluster définir un travail» (CS-travailleur). CS-travailleur joue de la liaison uniquement sur le cluster pour orchestrer les interactions de cluster local comme demandé par le CS-maître. Les exemples de ces interactions de placement de la machine virtuelle et l’inventaire des ressources de cluster locaux. Il en existe qu’une seule instance CS-travailleur pour chacune du membre clusters dans un ensemble de cluster. 

**Domaine d’erreur**

Un domaine d’erreur est le regroupement de logiciels et les artefacts de matériel qui détermine l’administrateur peuvent échouer ensemble lorsqu’un échec se produit.  Alors que l’administrateur peut désigner un ou plusieurs clusters ensemble comme un domaine d’erreur, chaque nœud peut participer à un domaine d’erreur dans un jeu de disponibilité. Cluster définit la décision de détermination de limite de domaine tolérance en feuilles de conception à l’administrateur qui est expérimenté avec les considérations relatives à la topologie données centre: par exemple, PDU, mise en réseau – partagent des clusters de membre. 

**Ensemble de disponibilité**

Un ensemble de disponibilité permet à l’administrateur de configurer la redondance souhaitée de charges de travail en cluster entre les domaines d’erreur, à organiser les dans un ensemble de la disponibilité et le déploiement de charges de travail dans ce jeu de disponibilité. Supposons que si vous déployez une application à deux niveaux, nous vous recommandons de configurer au moins deux machines virtuelles dans une disponibilité définie pour chaque niveau qui permet de garantir que lorsqu’un domaine d’erreur dans ce jeu de disponibilité tombe en panne, votre application au moins a une machine virtuelle dans chaque niveau hébergé sur un domaine d’erreur différentes de ce même jeu de disponibilité.

## Pourquoi utiliser des ensembles de cluster

Jeux de cluster offre l’avantage d’échelle sans pour autant sacrifier la résilience.  

Jeux de cluster permet plusieurs clusters d’ensemble pour créer une infrastructure de grande taille, tandis que chaque cluster reste indépendant pour la résilience.  Par exemple, vous avez un HCI 4 nœuds plusieurs clusters de machines virtuelles en cours d’exécution.  Chaque cluster fournit la résilience nécessaire pour lui-même.  Si la mémoire ou le stockage commence à remplir, montée en charge est l’étape suivante.  À l’échelle, il existe certaines options et les considérations.

1. Ajouter un stockage plus au cluster en cours.  Avec les espaces de stockage Direct, cela peut être difficile comme exactement les mêmes lecteurs de microprogramme/modèle ne peuvent pas être disponibles.  Vous devez également prendre en considération l’examen des temps de reconstruction.
2. Ajoutez de la mémoire.  Que se passe-t-il si vous sont atteinte sur la mémoire, que les ordinateurs peuvent gérer?  Que se passe-t-il si tous les emplacements de mémoire disponible sont complets?
3. Ajouter des nœuds de calcul supplémentaires utilisant des disques dans le cluster en cours.  Cela fait nous revenir à l’Option 1 avoir besoin d’être considéré comme.
4. Acheter un nouveau cluster

Il s’agit dans lequel les jeux de cluster offre l’avantage de mise à l’échelle.  Si j’ajoute mon clusters dans un ensemble de cluster, j’ai puis-je tirer parti de la mémoire peut-être être disponible sur un autre cluster sans les achats supplémentaires ou de stockage.  Du point de vue de la résilience, ajout de nœuds supplémentaires à un espaces de stockage Direct ne va pas de fournir des voix supplémentaires pour le quorum.  Comme indiqué [ici](drive-symmetry-considerations.md), un Cluster d’espaces de stockage Direct peut surmonter la perte de 2 nœuds avant d’aller vers le bas.  Si vous disposez d’un cluster HCI à 4 nœuds, 3 nœuds cesser prendra la totalité du cluster vers le bas.  Si vous disposez d’un cluster de 8 nœuds, 3 nœuds cesser prendra la totalité du cluster vers le bas.  Avec des ensembles de Cluster qui a deux clusters HCI 4 nœuds dans le jeu, 2 nœuds dans un seul HCI aller vers le bas et 1 nœud dans l’autres HCI aller vers le bas, les deux clusters restent.  Est-il mieux pour créer un cluster d’espaces de stockage Direct 16 nœuds grand ou décomposer en quatre nœuds 4 et utiliser des ensembles de cluster?  Avoir quatre clusters 4 nœuds avec cluster jeux donne la même échelle, mais une meilleure résilience dans la mesure où plusieurs nœuds de calcul peuvent aller vers le bas (de façon inattendue ou pour la maintenance) et reste en production.

## Considérations relatives au déploiement des ensembles de cluster

Lorsque vous envisagez si les jeux de cluster est quelque chose que vous devez utiliser, posez-vous les questions:

- Avez-vous besoin accéder aux limites actuel HCI calcul et stockage à l’échelle?
- Sont tous les calcul et stockage pas identique les mêmes?
- Dynamiques migrer les machines virtuelles entre les clusters?
- Vous souhaiteriez domaines d’erreur et les jeux de disponibilité d’un ordinateur Azure similaires sur plusieurs clusters?
- Avez-vous besoin de prendre le temps d’examiner toutes vos clusters afin de déterminer où toutes les nouvelles machines virtuelles doivent être placés?

Si votre réponse est Oui, les jeux de cluster est ce que vous avez besoin.

Il existe quelques autres éléments à prendre en compte dans lequel une plus grande SDDC sont susceptibles de changer vos stratégies de centre de données globale.  SQL Server est un bon exemple.  Déplacement machines virtuelles de SQL Server entre les clusters nécessite-t-elle une licence SQL à s’exécuter sur des nœuds supplémentaires?  

## Serveur de fichiers avec montée en et ensembles de cluster

Dans Windows Server 2019, il existe un nouveau rôle de serveur de fichiers avec montée en appelée Infrastructure avec montée en fichier de serveur en puissance parallèle. 

Les considérations suivantes s’appliquent à un rôle en puissance Infrastructure:

1.  Il peut être au plus qu’un seul rôle de cluster en puissance Infrastructure sur un Cluster de basculement. Rôle d’infrastructure en puissance est créé en spécifiant la «**-Infrastructure**«basculer le paramètre à l’applet de commande **Add-ClusterScaleOutFileServerRole** .  Exemple :

        Add-ClusterScaleoutFileServerRole -Name "my_infra_sofs_name" -Infrastructure

2.  Chaque volume CSV créé dans le basculement automatique déclenche la création d’un partage SMB avec un nom généré automatiquement en fonction du nom de volume CSV. Un administrateur ne peut pas directement créer ou modifier les partages SMB sous un rôle en puissance, autres que via les opérations de création/modification de volume CSV.

3.  Dans les configurations hyperconvergées, une en puissance Infrastructure permet à un client SMB (hôte Hyper-V) pour communiquer avec garantie Continuous disponibilité (CA) sur le serveur de l’Infrastructure en puissance SMB. Cette boucle SMB hyperconvergé autorité de certification s’effectue par le biais des machines virtuelles accéder à leurs fichiers de disque virtuel (VHDx) où l’identité propriétaire de la machine virtuelle est transférée entre le client et serveur. Le transfert de cette identité permet ACL binaire VHDx fichiers comme dans les configurations de cluster hyperconvergé standard qu’auparavant.

Une fois qu’un ensemble de cluster est créé, l’espace de noms de jeu de cluster s’appuie sur une en puissance Infrastructure sur chacun des clusters membre et en outre une en Infrastructure puissance du cluster de gestion.

Au moment où un cluster de membre est ajouté à un ensemble de cluster, l’administrateur spécifie le nom d’un en puissance Infrastructure sur ce cluster s’il en existe déjà. Si l’en puissance Infrastructure n’existe pas, un nouveau rôle en puissance Infrastructure sur le nouveau cluster membre est créé par cette opération. Si un rôle en puissance Infrastructure existe déjà sur le cluster de membre, l’opération d’ajout implicitement renomme le nom spécifié en fonction des besoins. N’importe quel singleton SMB, ou des serveurs existants rôles en puissance - Infrastructure sur le membre clusters restent inutilisés par le jeu de cluster. 

Au moment de la création de l’ensemble du cluster, l’administrateur a la possibilité d’utiliser un objet existe déjà d’un ordinateur AD comme racine de l’espace de noms sur le cluster de gestion. Opérations de création de jeu de cluster créer le rôle de cluster en puissance Infrastructure sur le cluster de gestion ou renomme le rôle en puissance Infrastructure existant simplement comme décrit précédemment pour les clusters de membre. L’en puissance Infrastructure sur le cluster de gestion est utilisé comme le cluster de définie des références d’espace de noms (Cluster définir Namespace) en puissance. Cela signifie simplement que chaque partage SMB sur le cluster de définir espace de noms en puissance est un partage de référence – de type «SimpleReferral» - nouvellement introduit dans cette version d’évaluation.  Cette référence permet de SMB aux clients d’accéder à la cible de partage SMB hébergement sur le cluster de membre en puissance. Le jeu de clusters référence d’espace de noms en puissance est un mécanisme de redirection légère et par conséquent, n’est pas inclus dans le chemin d’accès e/s. Les références de SMB sont mises en cache permanence sur chacun des nœuds du client et l’espace de noms de jeux de cluster dynamiquement met à jour automatiquement ces références en fonction des besoins

## Création d’un jeu de Cluster

### Conditions préalables

Lors de la valeur de la création d’un cluster, vous suivant les conditions préalables sont recommandées:

1. Configurer un client de gestion de la dernière version de Windows Server Insider en cours d’exécution.
2. Installer les outils de Cluster de basculement sur ce serveur d’administration.
3. Créer des membres du cluster (au moins deux clusters au moins deux Volumes partagés de Cluster sur chaque cluster)
4. Créer un cluster de gestion (physique ou invité) qui superpose les clusters de membre.  Cette approche garantit que les jeux de Cluster plan gestion continue d’être disponibles malgré les échecs de cluster membre possible.

### Étapes

1. Créer un nouveau cluster définir à partir de trois clusters comme défini dans les conditions préalables.  Le sous graphique donne un exemple de créer des clusters.  Le nom du cluster défini dans cet exemple sera **CSMASTER**.

   | Nom du cluster               | Nom en puissance de l’infrastructure pour une utilisation ultérieure | 
   |----------------------------|-------------------------------------------|
   | SET-CLUSTER                | EN PUISSANCE-CLUSTERSET                           |
   | CLUSTER1                   | EN PUISSANCE-CLUSTER1                             |
   | CLUSTER2                   | EN PUISSANCE-CLUSTER2                             |

2. Une fois que tous les clusters ont été créés, utilisez les commandes suivantes pour créer le cluster maître du jeu.

        New-ClusterSet -Name CSMASTER -NamespaceRoot SOFS-CLUSTERSET -CimSession SET-CLUSTER

3. Pour ajouter un serveur de Cluster à l’ensemble du cluster, le vous trouverez ci-dessous une option peut être utilisée.

        Add-ClusterSetMember -ClusterName CLUSTER1 -CimSession CSMASTER -InfraSOFSName SOFS-CLUSTER1
        Add-ClusterSetMember -ClusterName CLUSTER2 -CimSession CSMASTER -InfraSOFSName SOFS-CLUSTER2

   > [!NOTE]
   > Si vous utilisez un schéma d’adresse IP statique, vous devez inclure *x.x.x.x - StaticAddress* sur la commande **New-ClusterSet** .

4. Une fois que vous avez créé le cluster définie en dehors de membres du cluster, vous pouvez répertorier l’ensemble de nœuds et ses propriétés.  Pour énumérer tous les clusters de membres dans le jeu de cluster:

        Get-ClusterSetMember -CimSession CSMASTER

5. Pour énumérer tous les clusters de membre du cluster défini, y compris les nœuds de cluster de gestion:

        Get-ClusterSet -CimSession CSMASTER | Get-Cluster | Get-ClusterNode

6. Pour répertorier tous les nœuds de clusters membre:

        Get-ClusterSetNode -CimSession CSMASTER

7. Pour répertorier tous les groupes de ressource dans l’ensemble du cluster:

        Get-ClusterSet -CimSession CSMASTER | Get-Cluster | Get-ClusterGroup 

8. Pour vérifier le cluster de définie le processus de création créé un partage SMB (identifié par Volume1 ou quel que soit le dossier CSV porte le ScopeName est le nom de serveur de fichiers Infrastructure et le chemin d’accès en tant que les deux) sur l’en puissance Infrastructure pour chaque membre cluster Volume du volume partagé de cluster:

        Get-SmbShare -CimSession CSMASTER

8. Jeux de cluster a qui peuvent être collectés pour passer en revue les journaux de débogage.  Définie le cluster et les journaux de débogage de cluster peuvent être collectées pour tous les membres et le cluster de gestion.

        Get-ClusterSetLog -ClusterSetCimSession CSMASTER -IncludeClusterLog -IncludeManagementClusterLog -DestinationFolderPath <path>

9. Configurer Kerberos [délégation contrainte](https://blogs.technet.microsoft.com/virtualization/2017/02/01/live-migration-via-constrained-delegation-with-kerberos-in-windows-server-2016/) entre tous les membres du jeu de cluster.

10. Configurer le type d’authentification d’ordinateur virtuel inter-cluster migration dynamique à Kerberos sur chaque nœud dans le jeu de Cluster.

        foreach($h in $hosts){ Set-VMHost -VirtualMachineMigrationAuthenticationType Kerberos -ComputerName $h }

11. Ajoutez le cluster de gestion au groupe Administrateurs local sur chaque nœud dans le jeu de cluster.

        foreach($h in $hosts){ Invoke-Command -ComputerName $h -ScriptBlock {Net localgroup administrators /add <management_cluster_name>$} }

## Création de nouvelles machines virtuelles et en ajoutant aux jeux de cluster

Une fois définie de la création du cluster, l’étape suivante consiste à créer de nouvelles machines virtuelles.  En règle générale, lorsqu’il est temps de créer des machines virtuelles et les ajouter à un cluster, vous devez effectuer des vérifications sur les clusters pour voir laquelle il peut être préférable de s’exécuter sur.  Ces contrôles peuvent inclure:

- La quantité de mémoire est disponible sur les nœuds de cluster?
- La quantité d’espace disque est disponible sur les nœuds de cluster?
- La machine virtuelle nécessite des exigences de stockage spécifique (autrement dit, je veux mes machines virtuelles de SQL Server pour accéder à un cluster de disques plus rapides; en cours d’exécution ou ma machine virtuelle d’infrastructure n’est pas aussi critique et peut s’exécuter sur les disques plus lents).

Une fois ces questions sont traitées, vous créez la machine virtuelle sur le cluster que vous avez besoin d’utiliser.  Un des avantages des jeux de cluster est que les jeux de cluster accomplir ces contrôles pour vous et place la machine virtuelle sur le nœud plus performants.

Les commandes ci-dessous s’identifier le cluster optimale et déployer la machine virtuelle sur celle-ci.  Dans l’exemple indiqué ci-dessous, une machine virtuelle est créée en spécifiant qu’au moins 4 Go de mémoire est disponible pour la machine virtuelle et qu’il faudra utiliser 1 processeur virtuel.

- Assurez-vous que les 4 Go est disponible pour la machine virtuelle
- définir le processeur virtuel utilisé à 1
- Vérifiez qu’il existe au moins 10 % UC disponible pour la machine virtuelle

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

Lorsqu’elle est terminée, vous pourrez les informations sur la machine virtuelle et où il a été placé.  Dans l’exemple ci-dessus, il s’affichera comme:

        State         : Running
        ComputerName  : 1-S2D2

Si vous n’a ne pas suffisamment de mémoire, processeur ou d’espace disque pour ajouter la machine virtuelle, vous recevez l’erreur:

      Get-ClusterSetOptimalNodeForVM : A cluster node is not available for this operation.  

Une fois que la machine virtuelle a été créée, il s’affiche dans le Gestionnaire Hyper-V sur le nœud spécifique spécifié.  Pour l’ajouter comme un cluster de définie la machine virtuelle et dans le cluster, la commande est fourni ci-dessous.  

        Register-ClusterSetVM -CimSession CSMASTER -MemberName $targetnode.Member -VMName CSVM1

Lorsqu’elle est terminée, le résultat sera:

         Id  VMName  State  MemberName  PSComputerName
         --  ------  -----  ----------  --------------
          1  CSVM1      On  CLUSTER1    CSMASTER

Si vous avez ajouté un cluster avec des machines virtuelles existantes, les machines virtuelles devrez également être inscrit auprès de Cluster jeux inscrire tous les ordinateurs virtuels en même temps, la commande à utiliser est:

        Get-ClusterSetMember -name CLUSTER3 -CimSession CSMASTER | Register-ClusterSetVM -RegisterAll -CimSession CSMASTER

Toutefois, le processus n’est pas terminé car le chemin d’accès à la machine virtuelle doit être ajouté à l’espace de noms de jeu de cluster.

Par conséquent, par exemple, un cluster existant est ajouté et qui présente des machines virtuelles préconfigurés le résident sur le local Volume (partagé de Cluster), le chemin d’accès pour le VHDX serait semblable à «C:\ClusterStorage\Volume1\MYVM\Virtual dur Disks\MYVM.vhdx.  Migration d’un stockage doivent être accompli comme chemins d’accès de volume partagé de cluster sont par conception localement dans un cluster seul membre. Par conséquent, ne seront pas accessibles à la machine virtuelle une fois qu’ils sont dynamiques migrés entre les clusters de membre. 

Dans cet exemple, CLUSTER3 a été ajouté au cluster à l’aide de Add-ClusterSetMember avec le serveur de fichiers avec montée en Infrastructure en tant qu’en puissance-CLUSTER3.  Pour déplacer la configuration de machine virtuelle et le stockage, la commande est le suivant:

        Move-VMStorage -DestinationStoragePath \\SOFS-CLUSTER3\Volume1 -Name MYVM

Une fois que c’est terminé, vous recevrez un avertissement:

        WARNING: There were issues updating the virtual machine configuration that may prevent the virtual machine from running.  For more information view the report file below.
        WARNING: Report file location: C:\Windows\Cluster\Reports\Update-ClusterVirtualMachineConfiguration '' on date at time.htm.

Cet avertissement peut être ignoré en l’état de l’avertissement «aucune modification dans la configuration de stockage de rôle de machine virtuelle ont été détectées».  La cause de l’avertissement en tant que l’emplacement physique réel ne change pas; uniquement les chemins de configuration. 

Pour plus d’informations sur le déplacement-VMStorage, passez en revue ce [lien](https://docs.microsoft.com/powershell/module/hyper-v/move-vmstorage?view=win10-ps). 

Live Migration d’une machine virtuelle entre les clusters de jeu autre cluster n'est pas la même manière que dans le passé. Dans les scénarios de jeu non cluster, les étapes serait:

1. supprimer le rôle de machine virtuelle du Cluster.
2. migrer dynamiquement la machine virtuelle à un nœud membre d’un autre cluster.
3. ajouter la machine virtuelle dans le cluster en tant qu’un nouveau rôle de machine virtuelle.

Dans les jeux de Cluster, ces étapes ne sont pas nécessaires et qu’une seule commande est nécessaire.  Par exemple, je souhaite atteindre une machine virtuelle Cluster définie à partir de CLUSTER1 nœud 2-CL3 sur CLUSTER3.  La seule commande serait:

        Move-ClusterSetVM -CimSession CSMASTER -VMName CSVM1 -Node NODE2-CL3

Veuillez noter que cela ne déplace pas les fichiers de configuration ou de stockage de machine virtuelle.  Cela n’est pas nécessaire que le chemin d’accès à la machine virtuelle reste en tant que \\SOFS-CLUSTER1\VOLUME1.  Une fois qu’une machine virtuelle a été enregistrée avec des jeux de cluster possède le chemin d’accès du partage de serveur de fichiers d’Infrastructure, la machine virtuelle et les lecteurs ne nécessitent pas en cours sur le même ordinateur que la machine virtuelle.

## Disponibilité de création de jeux de domaines d’erreur

Comme décrit dans l’introduction, domaines d’erreur de type Azure et des ensembles de disponibilité peuvent être configurés dans un ensemble de cluster.  Cela est utile pour les emplacements de la machine virtuelle initiale et les migrations entre les clusters.  

Dans l’exemple ci-dessous, il existe quatre clusters participant dans l’ensemble du cluster.  Dans le jeu, un domaine d’erreur logique est créé avec les deux clusters et un domaine d’erreur créés avec les deux clusters.  Ces domaines de deux erreur seront compris dans l’ensemble de Availabiilty. 

Dans l’exemple ci-dessous, CLUSTER1 et CLUSTER2 sera dans un domaine d’erreur appelé **FD1** bien que CLUSTER3 et CLUSTER4 soit dans un domaine d’erreur appelé **FD2 Répartiteurs**.  L’ensemble de disponibilité sera appelée **CSMASTER-AS** et composée des domaines de deux erreur.

Pour créer les domaines d’erreur, les commandes sont:

        New-ClusterSetFaultDomain -Name FD1 -FdType Logical -CimSession CSMASTER -MemberCluster CLUSTER1,CLUSTER2 -Description "This is my first fault domain"

        New-ClusterSetFaultDomain -Name FD2 -FdType Logical -CimSession CSMASTER -MemberCluster CLUSTER3,CLUSTER4 -Description "This is my second fault domain"

Pour vous assurer qu’ils ont été créés avec succès, Get-ClusterSetFaultDomain peut être exécuté avec sa sortie indiqué.

        PS C:\> Get-ClusterSetFaultDomain -CimSession CSMASTER -FdName FD1 | fl *

        PSShowComputerName    : True
        FaultDomainType       : Logical
        ClusterName           : {CLUSTER1, CLUSTER2}
        Description           : This is my first fault domain
        FDName                : FD1
        Id                    : 1
        PSComputerName        : CSMASTER

Maintenant que les domaines d’erreur ont été créés, la disponibilité défini doit être créée.

        New-ClusterSetAvailabilitySet -Name CSMASTER-AS -FdType Logical -CimSession CSMASTER -ParticipantName FD1,FD2

Pour valider qu’il a été créé, puis utilisez:

        Get-ClusterSetAvailabilitySet -AvailabilitySetName CSMASTER-AS -CimSession CSMASTER

Lors de la création de nouvelles machines virtuelles, vous devez alors utiliser le paramètre - AvailabilitySet dans le cadre de la détermination du nœud optimal.  Par conséquent, il serait alors ressembler à ceci:

        # Identify the optimal node to create a new virtual machine
        $memoryinMB=4096
        $vpcount = 1
        $av = Get-ClusterSetAvailabilitySet -Name CSMASTER-AS -CimSession CSMASTER
        $targetnode = Get-ClusterSetOptimalNodeForVM -CimSession CSMASTER -VMMemory $memoryinMB -VMVirtualCoreCount $vpcount -VMCpuReservation 10 -AvailabilitySet $av
        $secure_string_pwd = convertto-securestring "<password>" -asplaintext -force
        $cred = new-object -typename System.Management.Automation.PSCredential ("<domain\account>",$secure_string_pwd)

Supprimer un cluster de cluster définit en raison des différents cycles de vie. Il est parfois quand un cluster doit être supprimé à partir d’un ensemble de cluster. Comme meilleure pratique, toutes les machines virtuelles de jeu de cluster doit être déplacés en dehors du cluster. Cela peut se faire en utilisant les commandes de **Déplacement-ClusterSetVM** et **Move-VMStorage** .

Toutefois, si les ordinateurs virtuels ne seront pas également déplacés, ensembles de cluster s’exécute une série d’actions pour fournir un résultat intuitif à l’administrateur.  Lorsque le cluster est supprimé de l’ensemble, toutes les autres cluster ensemble machines virtuelles hébergées sur le cluster en cours de suppression sera simplement des ordinateurs virtuels hautement disponibles liés à ce cluster, en supposant qu’elles ont accès à leur stockage.  Jeux de cluster met également automatiquement à jour son inventaire par:

- Suivi n’est plus l’intégrité du cluster maintenant-supprimé et les machines virtuelles en cours d’exécution sur ce dernier
- Supprime de l’espace de noms de cluster ensemble et toutes les références aux partages hébergés sur le cluster maintenant-supprimé

Par exemple, la commande pour supprimer le cluster CLUSTER1 d’ensembles de cluster serait:

        Remove-ClusterSetMember -ClusterName CLUSTER1 -CimSession CSMASTER

## Forum Aux Questions (FAQ)

**Question:** Dans mon jeu de cluster, ai j’ai limité à uniquement à l’aide de clusters hyperconvergés? <br>
**Réponses:** Non.  Vous pouvez combiner les espaces de stockage Direct avec des clusters traditionnels.

**Question:** Puis-je gérer mon Cluster défini par le biais de System Center Virtual Machine Manager? <br>
**Réponses:** System Center Virtual Machine Manager ne prend pas en charge les jeux de Cluster <br><br> **Question:** Peuvent Windows Server 2012 R2 ou clusters 2016 coexister dans le même ensemble de cluster? <br>
**Question:** Puis-je migrer des charges de travail désactiver Windows Server 2012 R2 ou des 2016 clusters par le simple fait d’avoir ces clusters joindre le même ensemble de Cluster? <br>
**Réponses:** Jeux de cluster est une nouvelle technologie introduits dans Windows Server Preview builds, par conséquent, par conséquent, n’existe pas dans les versions précédentes. Clusters basée sur le système d’exploitation de niveau inférieur ne peut pas joindre un ensemble de cluster. Toutefois, technologie de mises à niveau propagée de système d’exploitation de Cluster doit fournir la fonctionnalité de migration que vous recherchez en mettant à niveau ces clusters pour Windows Server 2019.

**Question:** Jeux de Cluster me à l’échelle de stockage ou de calcul (seul) peut autoriser? <br>
**Réponses:** Oui, en ajoutant simplement un espace de stockage Direct ou clusters Hyper-V traditionnels. Jeux de cluster, il est un simple changement de taux de calcul-stockage même dans un ensemble de cluster hyperconvergé.

**Question:** Nouveautés, les outils de gestion pour les jeux de cluster <br>
**Réponses:** PowerShell ou WMI dans cette version.

**Question:** Comment fonctionne la migration dynamique inter-cluster avec les processeurs de différentes générations?  <br>
**Réponses:** Jeux de cluster n’a pas été contourner les différences de processeur et remplacer ce que Hyper-V prend en charge.  Par conséquent, le mode de compatibilité du processeur doit être utilisé avec les migrations rapides.  La recommandation pour les jeux de Cluster consiste à utiliser le même matériel du processeur dans chaque Cluster individuels ainsi que le jeu de Cluster entier pour les migrations en direct entre les clusters se produire.

**Question:** Peut mon jeu de cluster virtuel des ordinateurs automatiquement sur la défaillance d’un cluster de basculement?  <br>
**Réponses:** Dans cette version, cluster ensemble ordinateurs virtuels peuvent uniquement être manuellement migrés entre les clusters; mais ne peut pas automatiquement de basculement. 

**Question:** Comment nous assurer que le stockage est résilient à des échecs de cluster? <br>
**Réponses:** Utilisez solution de réplica de stockage (SR) inter-cluster sur les clusters de membre de réaliser la résilience de stockage pour les échecs de cluster.

**Question:** Utiliser le réplica de stockage (SR) pour répliquer sur les clusters de membre. Stockage en cluster ensemble espace de noms modification des chemins d’accès UNC lors du basculement SR au cluster espaces de stockage Direct cible du réplica du système? <br>
**Réponses:** Dans cette version, une telle cluster ensemble espace de noms référence modification n’a pas lieu avec basculement SR. Veuillez informer Microsoft si ce scénario est essentiel pour vous et de la façon dont vous souhaitez l’utiliser.

**Question:** Est-il possible de machines virtuelles de basculement entre les domaines d’erreur dans une situation de récupération d’urgence (par exemple, le domaine d’erreur entier s’est arrêté)? <br>
**Réponses:** Non, notez que le basculement inter-cluster au sein d’une erreur de logique domaine n'est pas encore pris en charge. 

**Question:** Mon cluster peut définir des clusters span dans plusieurs sites (ou domaines DNS)? <br> 
**Réponses:** Cela est un scénario non testé et pas immédiatement prévue pour la prise en charge de production. Veuillez informer Microsoft si ce scénario est essentiel pour vous et de la façon dont vous souhaitez l’utiliser.

**Question:** Cluster définit des tâches avec IPv6? <br>
**Réponses:** IPv4 et IPv6 sont prises en charge dans les jeux de cluster à l’instar des Clusters de basculement.

**Question:** Quelles sont les exigences de forêt Active Directory pour cluster définit <br>
**Réponses:** Tous les clusters de membre doivent être dans la même forêt AD.

**Question:** Le nombre de clusters ou de nœuds peut être partie d’un seul cluster défini? <br>
**Réponses:** Dans l’aperçu, les jeux de cluster été testées et prises en charge jusqu'à 64 nœuds de cluster total. Toutefois, cluster définit architecture mise à l’échelle des limites beaucoup plus grande et n’est pas quelque chose qui est codé en dur pour une limite. Veuillez informer Microsoft si plus grande échelle est essentielle pour vous et de la façon dont vous souhaitez l’utiliser.

**Question:** Tous les clusters d’espaces de stockage Direct dans un ensemble de cluster forment un pool de stockage unique? <br>
**Réponses:** Non. La technologie Direct d’espaces de stockage fonctionne toujours au sein d’un seul cluster et pas sur les clusters de membre dans un ensemble de cluster.

**Question:** Est-ce que le cluster défini hautement disponible de l’espace de noms? <br>
**Réponses:** Oui, l’espace de noms de jeu de cluster est fournie via un serveur d’espace de noms en permanence disponibles (CA) référence en puissance en cours d’exécution sur le cluster de gestion. Microsoft recommande de disposer de suffisamment nombre de machines virtuelles à partir de clusters membre afin de faciliter résilient localisées défaillances à l’échelle du cluster. Toutefois, pour prendre en compte pour les défaillances catastrophiques imprévus: par exemple, toutes les machines virtuelles du cluster de gestion en panne simultanément les informations de référence sont en outre permanence mis en cache dans chaque nœud du cluster ensemble, même au fil des redémarrages.
 
**Question:** Le cluster définit un accès de stockage basée sur l’espace de noms ralentir les performances de stockage dans un ensemble de cluster? <br>
**Réponses:** Non. Espace de noms de cluster ensemble propose un espace de noms de référence de superposition au sein d’un ensemble de cluster – le point de vue conceptuel comme distribuées fichier système des espaces de noms (DFSN). Et à la différence DFSN, toutes les métadonnées de référence d’espace de noms du ensemble cluster sont remplie-automatique et de mise à jour automatique sur tous les nœuds sans aucune intervention de l’administrateur, donc il n’existe pratiquement aucune baisse des performances dans le chemin d’accès de stockage. 

**Question:** Comment puis-je sauvegarde des métadonnées du jeu de cluster? <br>
**Réponses:** Ce guide est identique à celui du Cluster de basculement. La sauvegarde d’état système sauvegardera également l’état du cluster.  Par le biais de sauvegarde Windows Server, vous pouvez effectuer des restaurations de simplement d’un nœud cluster de base de données (qui ne doit jamais être nécessaire en raison d’une série de logique de réparation automatique, nous avons) ou effectuez une restauration forcée pour restaurer la base de données de l’ensemble du cluster sur tous les nœuds. Dans le cas des jeux de cluster, Microsoft recommande telle une restauration forcée tout d’abord sur le cluster de membre, puis sur le cluster de gestion si nécessaire. 
