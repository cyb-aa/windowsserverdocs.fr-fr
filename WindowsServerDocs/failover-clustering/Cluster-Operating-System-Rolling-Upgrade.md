---
title: Mise à niveau propagée de système d’exploitation de cluster
ms.prod: windows-server
ms.technology: storage-failover-clustering
ms.topic: get-started-article
ms.assetid: 6e102c1f-df26-4eaa-bc7a-d0d55d3b82d5
author: jasongerend
ms.author: jgerend
manager: lizross
ms.date: 03/27/2018
ms.openlocfilehash: 8b2ea665542d57b12899a5993a62973c446485a7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80828352"
---
# <a name="cluster-operating-system-rolling-upgrade"></a>Mise à niveau propagée du système d’exploitation du cluster

> S’applique à : Windows Server 2019, Windows Server 2016

La mise à niveau propagée de système d’exploitation de cluster permet à un administrateur de mettre à niveau le système d’exploitation des nœuds de cluster sans arrêter les charges de travail Hyper-V ou Serveur de fichiers avec montée en puissance parallèle. Avec cette fonctionnalité, les pénalités de temps d’arrêt sur les contrats de niveau de service peuvent être évitées.

La mise à niveau propagée de système d’exploitation de cluster offre les avantages suivants :

- Les clusters de basculement exécutant des charges de travail de machine virtuelle Hyper-V et de serveur de fichiers avec montée en puissance parallèle (SOFS) peuvent être mis à niveau à partir de Windows Server 2012 R2 (exécuté sur tous les nœuds du cluster) vers Windows Server 2016 (exécuté sur tous les nœuds de cluster du cluster) sans temps d’arrêt. D’autres charges de travail de cluster, telles que SQL Server, seront indisponibles pendant le temps (généralement moins de cinq minutes) qu’il faut pour basculer vers Windows Server 2016.  
- Il ne nécessite pas de matériel supplémentaire. Bien que vous puissiez ajouter temporairement des nœuds de cluster supplémentaires aux petits clusters pour améliorer la disponibilité du cluster pendant le processus de mise à niveau propagée du système d’exploitation du cluster.  
- Le cluster n’a pas besoin d’être arrêté ou redémarré.  
- Un nouveau cluster n’est pas requis. Le cluster existant est mis à niveau. En outre, les objets de cluster existants stockés dans Active Directory sont utilisés.  
- Le processus de mise à niveau est réversible jusqu’à ce que le client choisisse « point-of-No-Return », lorsque tous les nœuds de cluster exécutent Windows Server 2016 et lorsque l’applet de commande PowerShell Update-ClusterFunctionalLevel est exécutée.  
- Le cluster peut prendre en charge les mises à jour correctives et les opérations de maintenance lors de l’exécution en mode mixte du système d’exploitation.  
- Il prend en charge l’automatisation via PowerShell et WMI.  
- La propriété **ClusterFunctionalLevel** de la propriété publique du cluster indique l’état du cluster sur les nœuds de cluster Windows Server 2016. Cette propriété peut être interrogée à l’aide de l’applet de commande PowerShell d’un nœud de cluster Windows Server 2016 qui appartient à un cluster de basculement :  
    ```PowerShell
    Get-Cluster | Select ClusterFunctionalLevel  
    ```  

    La valeur **8** indique que le cluster s’exécute au niveau fonctionnel de Windows Server 2012 R2. La valeur **9** indique que le cluster s’exécute au niveau fonctionnel de Windows Server 2016.  

Ce guide décrit les différentes étapes du processus de mise à niveau propagée de système d’exploitation de cluster, les étapes d’installation, les limitations de fonctionnalités et les questions fréquentes (FAQ), et s’applique aux scénarios de mise à niveau propagée de système d’exploitation de cluster suivants dans Windows Server 2016 :  
- Clusters Hyper-V  
- Clusters de Serveur de fichiers avec montée en puissance parallèle  

Le scénario suivant n’est pas pris en charge dans Windows Server 2016 :  
-  Mise à niveau propagée du système d’exploitation du cluster des clusters invités à l’aide d’un disque dur virtuel (fichier. vhdx) en tant que stockage partagé  

La mise à niveau propagée de système d’exploitation de cluster est entièrement prise en charge par System Center Virtual Machine Manager (SCVMM) 2016. Si vous utilisez SCVMM 2016, consultez [effectuer une mise à niveau propagée d’un cluster hôte Hyper-V vers Windows Server 2016 dans VMM](https://docs.microsoft.com/system-center/vmm/hyper-v-rolling-upgrade?view=sc-vmm-1807) pour obtenir des conseils sur la mise à niveau des clusters et sur l’automatisation des étapes décrites dans ce document.  

## <a name="requirements"></a>Configuration requise  
Avant de commencer le processus de mise à niveau propagée du système d’exploitation du cluster, complétez les conditions suivantes :

- Démarrez avec un cluster de basculement exécutant Windows Server (canal semi-annuel), Windows Server 2016 ou Windows Server 2012 R2.
- La mise à niveau d’un cluster espaces de stockage direct vers Windows Server version 1709 n’est pas prise en charge.
- Si la charge de travail du cluster est des machines virtuelles Hyper-V ou Serveur de fichiers avec montée en puissance parallèle, vous pouvez vous attendre à une mise à niveau sans temps mort.
- Vérifiez que les nœuds Hyper-V ont des processeurs qui prennent en charge la table d’adressage de second niveau (SLAT) à l’aide de l’une des méthodes suivantes :  
        -Passez en revue la [compatibilité SLAT ? Article du kit de développement logiciel (SDK) WP8, tip 01](https://blogs.msdn.com/b/devfish/archive/2012/11/06/are-you-slat-compatible-wp8-sdk-tip-01.aspx) , qui décrit deux méthodes pour vérifier si un processeur prend en charge les SLAT  
        -Téléchargez l’outil [Coreinfo v 3.31](https://technet.microsoft.com/sysinternals/cc835722) pour déterminer si un processeur prend en charge SLAT.

## <a name="cluster-transition-states-during-cluster-os-rolling-upgrade"></a>États de transition de cluster pendant la mise à niveau propagée du système d’exploitation

Cette section décrit les différents États de transition du cluster Windows Server 2012 R2 en cours de mise à niveau vers Windows Server 2016 à l’aide de la mise à niveau propagée du système d’exploitation du cluster.  

Pour que les charges de travail de cluster s’exécutent pendant le processus de mise à niveau propagée du système d’exploitation du cluster, le déplacement d’une charge de travail de cluster à partir d’un nœud Windows Server 2012 R2 vers le nœud Windows Server 2016 fonctionne comme si les deux nœuds exécutaient le système d’exploitation Windows Server 2012 R2. Quand des nœuds Windows Server 2016 sont ajoutés au cluster, ils fonctionnent dans un mode de compatibilité Windows Server 2012 R2. Un nouveau mode de cluster conceptuel, appelé « mode de système d’exploitation mixte », permet à des nœuds de différentes versions d’exister dans le même cluster (voir la figure 1).  

![illustration illustrant les trois étapes d’une mise à niveau propagée de système d’exploitation de cluster : tous les nœuds Windows Server 2012 R2, mode mixte du système d’exploitation et tous les nœuds Windows Server 2016](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_RollingUpgrade_Overview.png)  
**Figure 1 : transitions d’État du système d’exploitation du cluster**  

Un cluster Windows Server 2012 R2 passe en mode mixte lorsqu’un nœud Windows Server 2016 est ajouté au cluster. Le processus est entièrement réversible-les nœuds Windows Server 2016 peuvent être supprimés du cluster et les nœuds Windows Server 2012 R2 peuvent être ajoutés au cluster dans ce mode. Le « point de non retour » se produit lorsque l’applet de commande PowerShell Update-ClusterFunctionalLevel est exécutée sur le cluster. Pour que cette applet de commande aboutisse, tous les nœuds doivent être Windows Server 2016, et tous les nœuds doivent être en ligne.  

## <a name="transition-states-of-a-four-node-cluster-while-performing-rolling-os-upgrade"></a>États de transition d’un cluster à quatre nœuds pendant l’exécution de la mise à niveau propagée du système d’exploitation

Cette section illustre et décrit les quatre étapes différentes d’un cluster avec un stockage partagé dont les nœuds sont mis à niveau de Windows Server 2012 R2 vers Windows Server 2016.  

« Étape 1 » est l’état initial : nous commençons par un cluster Windows Server 2012 R2.  

![illustration montrant l’état initial : tous les nœuds Windows Server 2012 R2](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage1.png)  
**Figure 2 : état initial : cluster de basculement Windows Server 2012 R2 (étape 1)**  

Dans « étape 2 », deux nœuds ont été suspendus, vidés, supprimés, reformatés et installés avec Windows Server 2016.  

![illustration illustrant le cluster en mode de système d’exploitation mixte : en dehors de l’exemple de cluster à 4 nœuds, deux nœuds exécutent Windows Server 2016 et deux nœuds exécutent Windows Server 2012 R2](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage2.png)  
**Figure 3 : état intermédiaire : mode mixte du système d’exploitation : cluster de basculement Windows Server 2012 R2 et Windows Server 2016 (étape 2)**  

À l’étape 3, tous les nœuds du cluster ont été mis à niveau vers Windows Server 2016 et le cluster est prêt à être mis à niveau avec l’applet de commande PowerShell Update-ClusterFunctionalLevel.  

> [!NOTE]  
> À ce niveau, le processus peut être complètement inversé et les nœuds Windows Server 2012 R2 peuvent être ajoutés à ce cluster.  

![illustration montrant que le cluster a été entièrement mis à niveau vers Windows Server 2016 et que l’applet de commande Update-ClusterFunctionalLevel est prête pour ramener le niveau fonctionnel du cluster à Windows Server 2016](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage3.png)  
**Figure 4 : état intermédiaire : tous les nœuds mis à niveau vers Windows Server 2016, prêts pour la mise à jour-ClusterFunctionalLevel (étape 3)**  

Une fois la mise à jour-ClusterFunctionalLevelcmdlet exécutée, le cluster passe à « Stage 4 », ce qui permet d’utiliser les nouvelles fonctionnalités de cluster Windows Server 2016.  

![illustration montrant que la mise à niveau propagée du système d’exploitation a été correctement effectuée ; tous les nœuds ont été mis à niveau vers Windows Server 2016 et le cluster s’exécute au niveau fonctionnel du cluster Windows Server 2016](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage4.png)  
**Figure 5 : état final : cluster de basculement Windows Server 2016 (étape 4)**  

## <a name="cluster-os-rolling-upgrade-process"></a>Processus de mise à niveau propagée du système d’exploitation

Cette section décrit le flux de travail pour effectuer une mise à niveau propagée du système d’exploitation du cluster.  

![illustration montrant le flux de travail pour la mise à niveau d’un cluster](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_RollingUpgrade_Workflow.png)  
**Figure 6 : workflow du processus de mise à niveau propagée du système d’exploitation du cluster**  

La mise à niveau propagée de système d’exploitation de cluster comprend les étapes suivantes :  

1. Préparez le cluster pour la mise à niveau du système d’exploitation comme suit :  
    1. La mise à niveau propagée de système d’exploitation de cluster nécessite la suppression d’un nœud à la fois du cluster. Vérifiez que vous disposez d’une capacité suffisante sur le cluster pour maintenir les SLA de haute disponibilité quand l’un des nœuds de cluster est supprimé du cluster pour une mise à niveau du système d’exploitation. En d’autres termes, avez-vous besoin de la possibilité de basculer des charges de travail vers un autre nœud lorsqu’un nœud est supprimé du cluster pendant le processus de mise à niveau propagée du système d’exploitation du cluster ? Le cluster a-t-il la capacité d’exécuter les charges de travail requises lorsqu’un nœud est supprimé du cluster pour la mise à niveau propagée du système d’exploitation du cluster ?  
    2. Pour les charges de travail Hyper-V, vérifiez que tous les ordinateurs hôtes Windows Server 2016 Hyper-V prennent en charge l’utilisation d’une table d’adresses de second niveau (SLAT). Seuls les ordinateurs à capacité SLAT peuvent utiliser le rôle Hyper-V dans Windows Server 2016.  
    3. Vérifiez que toutes les sauvegardes de charge de travail sont terminées, puis envisagez de sauvegarder le cluster. Arrêtez les opérations de sauvegarde lors de l’ajout de nœuds au cluster.  
    4. Vérifiez que tous les nœuds de cluster sont en ligne/Running/up à l’aide de l’applet de commande [`Get-ClusterNode`](https://docs.microsoft.com/powershell/module/failoverclusters/Get-ClusterNode?view=win10-ps) (voir la figure 7).  

        ![ScreenCap présentant les résultats de l’exécution de l’applet de commande de la commande obtenir-ClusterNode](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetClusterNode.png)  
        **Figure 7 : détermination de l’état du nœud à l’aide de l’applet de commande**  

    5. Si vous exécutez la mise à jour adaptée aux clusters, vérifiez si la mise à jour adaptée aux clusters est en cours d’exécution à l’aide de l’interface utilisateur de **mise à jour adaptée aux clusters** ou de l’applet de commande [`Get-CauRun`](https://docs.microsoft.com/powershell/module/clusterawareupdating/Get-CauRun?view=win10-ps) (voir figure 8). Arrêtez la mise à jour adaptée aux clusters à l’aide de l’applet de commande [`Disable-CauClusterRole`](https://docs.microsoft.com/powershell/module/clusterawareupdating/Disable-CauClusterRole?view=win10-ps) (voir figure 9) pour empêcher la mise à jour propagée de tous les nœuds au cours du processus de mise à niveau propagée du système d’exploitation.  

        ![ScreenCap présentant la sortie de l’applet de commande CauRun](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetCAU.png)  
        **Figure 8 : utilisation de l’applet de commande [`Get-CauRun`](https://docs.microsoft.com/powershell/module/clusterawareupdating/Get-CauRun?view=win10-ps) pour déterminer si les mises à jour adaptées aux clusters sont en cours d’exécution sur le cluster**  

        ![ScreenCap présentant la sortie de l’applet de commande Disable-CauClusterRole](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_DisableCAU.png)  
        **Figure 9 : désactivation du rôle mises à jour prenant en charge les clusters à l’aide de l’applet de commande [`Disable-CauClusterRole`](https://docs.microsoft.com/powershell/module/clusterawareupdating/Disable-CauClusterRole?view=win10-ps)**  

2. Pour chaque nœud du cluster, procédez comme suit :  
    1. À l’aide de l’interface utilisateur du gestionnaire de cluster, sélectionnez un nœud et utilisez la **Pause |** L’option de menu drainer permet de purger le nœud (voir figure 10) ou d’utiliser l’applet de commande [`Suspend-ClusterNode`](https://docs.microsoft.com/powershell/module/failoverclusters/Suspend-ClusterNode?view=win10-ps) (voir figure 11).  

        ![ScreenCap qui montre comment drainer des rôles à l’aide de l’interface utilisateur du gestionnaire de cluster](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_FCM_DrainRoles.png)  
        **Figure 10 : purge des rôles d’un nœud à l’aide de Gestionnaire du cluster de basculement**  

        ![ScreenCap présentant la sortie de l’applet de commande suspend-ClusterNode](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_SuspendNode.png)  
        **Figure 11 : purge des rôles d’un nœud à l’aide de l’applet de commande [`Suspend-ClusterNode`](https://docs.microsoft.com/powershell/module/failoverclusters/Suspend-ClusterNode?view=win10-ps)**  

    2.  À l’aide de l’interface utilisateur du gestionnaire de cluster, **supprimez** le nœud suspendu du cluster ou utilisez l’applet de commande [`Remove-ClusterNode`](https://docs.microsoft.com/powershell/module/failoverclusters/Remove-ClusterNode?view=win10-ps) .  

        ![ScreenCap présentant la sortie de l’applet de commande Remove-ClusterNode](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_RemoveNode.png)  
        **Figure 12 : supprimer un nœud du cluster à l’aide de l’applet de commande [`Remove-ClusterNode`](https://docs.microsoft.com/powershell/module/failoverclusters/Remove-ClusterNode?view=win10-ps)**  

    3.  Reformatez le lecteur système et effectuez une nouvelle installation du système d’exploitation de Windows Server 2016 sur le nœud à l’aide de l’option **personnalisée : installer uniquement Windows (** voir figure 13) dans Setup. exe. Évitez de sélectionner l’option **mettre à niveau : installer Windows et conserver les fichiers, les paramètres et les applications** , car la mise à niveau propagée du système d’exploitation du cluster n’encourage pas la mise à niveau sur place.  

        ![ScreenCap de l’Assistant Installation de Windows Server 2016 avec l’option d’installation personnalisée sélectionnée](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_InstallOption.png)  
        **Figure 13 : options d’installation disponibles pour Windows Server 2016**  

    4.  Ajoutez le nœud au domaine de Active Directory approprié.  
    5.  Ajoutez les utilisateurs appropriés au groupe administrateurs.  
    6.  À l’aide de l’interface utilisateur Gestionnaire de serveur ou de l’applet de commande PowerShell install-WindowsFeature, installez tous les rôles de serveur dont vous avez besoin, tels que Hyper-V.  

        ```PowerShell
        Install-WindowsFeature -Name Hyper-V  
        ```  

    7.  À l’aide de l’interface utilisateur Gestionnaire de serveur ou de l’applet de commande PowerShell install-WindowsFeature, installez la fonctionnalité de clustering de basculement.  

        ```PowerShell
        Install-WindowsFeature -Name Failover-Clustering  
        ```  

    8.  Installez les fonctionnalités supplémentaires requises par vos charges de travail de cluster.  
    9. Vérifiez les paramètres de connectivité réseau et de stockage à l’aide de l’interface utilisateur Gestionnaire du cluster de basculement.  
    10. Si le pare-feu Windows est utilisé, vérifiez que les paramètres du pare-feu sont corrects pour le cluster. Par exemple, les clusters compatibles avec la mise à jour adaptée aux clusters peuvent nécessiter une configuration de pare-feu.  
    11. Pour les charges de travail Hyper-V, utilisez l’interface utilisateur du Gestionnaire Hyper-V pour lancer la boîte de dialogue Gestionnaire de commutateur virtuel (voir figure 14).  

        Vérifiez que le nom du ou des commutateurs virtuels utilisés est identique pour tous les nœuds hôtes Hyper-V du cluster.  

        ![ScreenCap présentant l’emplacement de la boîte de dialogue Gestionnaire de commutateur virtuel Hyper-V](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_VMSwitch.png)  
        **Figure 14 : gestionnaire de commutateur virtuel**  

    12. Sur un nœud Windows Server 2016 (n’utilisez pas de nœud Windows Server 2012 R2), utilisez le Gestionnaire du cluster de basculement (voir figure 15) pour vous connecter au cluster.  

        ![ScreenCap présentant la boîte de dialogue Sélectionner un cluster](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_AddNode.png)  
        **Figure 15 : ajout d’un nœud au cluster à l’aide de Gestionnaire du cluster de basculement**  

    13. Utilisez l’interface utilisateur Gestionnaire du cluster de basculement ou l’applet de commande [`Add-ClusterNode`](https://docs.microsoft.com/powershell/module/failoverclusters/Add-ClusterNode?view=win10-ps) (voir figure 16) pour ajouter le nœud au cluster.  

        ![ScreenCap présentant la sortie de l’applet de commande Add-ClusterNode](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_AddNode3.png)  
        **Figure 16 : ajout d’un nœud au cluster à l’aide de l’applet de commande [`Add-ClusterNode`](https://docs.microsoft.com/powershell/module/failoverclusters/Add-ClusterNode?view=win10-ps)**  

        > [!NOTE]  
        > Lorsque le premier nœud Windows Server 2016 rejoint le cluster, le cluster passe en mode « système d’exploitation mixte » et les principales ressources du cluster sont déplacées vers le nœud Windows Server 2016. Un cluster en mode de « système d’exploitation mixte » est un cluster entièrement fonctionnel dans lequel les nouveaux nœuds s’exécutent dans un mode de compatibilité avec les anciens nœuds. Le mode « mixte-système d’exploitation » est un mode transitoire pour le cluster. Elle n’est pas destinée à être permanente et les clients sont censés mettre à jour tous les nœuds de leur cluster dans un délai de quatre semaines.  

    14. Une fois le nœud Windows Server 2016 correctement ajouté au cluster, vous pouvez (si vous le souhaitez) déplacer une partie de la charge de travail du cluster vers le nœud qui vient d’être ajouté pour rééquilibrer la charge de travail sur le cluster comme suit :

        ![ScreenCap présentant la sortie de l’applet de commande Move-ClusterVirtualMachineRole](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_MoveVMRole.png)  
        **Figure 17 : déplacement d’une charge de travail de cluster (rôle de machine virtuelle du cluster) à l’aide de [`Move-ClusterVirtualMachineRole`](https://docs.microsoft.com/powershell/module/failoverclusters/Move-ClusterVirtualMachineRole?view=win10-ps) applet de commande**  

        1. Utilisez **migration dynamique** de la gestionnaire du cluster de basculement pour les machines virtuelles ou l’applet de commande [`Move-ClusterVirtualMachineRole`](https://docs.microsoft.com/powershell/module/failoverclusters/Move-ClusterVirtualMachineRole?view=win10-ps) (voir figure 17) pour effectuer une migration dynamique des machines virtuelles.  

            ```PowerShell
            Move-ClusterVirtualMachineRole -Name VM1 -Node robhind-host3  
            ```  

        2. Utilisez **Move** à partir du gestionnaire du cluster de basculement ou de l’applet de commande [`Move-ClusterGroup`](https://docs.microsoft.com/powershell/module/failoverclusters/Move-ClusterGroup?view=win10-ps) pour les autres charges de travail de cluster.  

3. Lorsque tous les nœuds ont été mis à niveau vers Windows Server 2016 et rajoutés au cluster, ou lorsque des nœuds Windows Server 2012 R2 restants ont été supprimés, procédez comme suit :  

    > [!IMPORTANT]  
    > -   Après avoir mis à jour le niveau fonctionnel du cluster, vous ne pouvez pas revenir au niveau fonctionnel de Windows Server 2012 R2 et les nœuds Windows Server 2012 R2 ne peuvent pas être ajoutés au cluster.
    > -   Tant que l’applet de commande [`Update-ClusterFunctionalLevel`](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) n’est pas exécutée, le processus est entièrement réversible et les nœuds windows server 2012 R2 peuvent être ajoutés à ce cluster et les nœuds windows server 2016 peuvent être supprimés.  
    > -   Une fois l’applet de commande [`Update-ClusterFunctionalLevel`](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) exécutée, de nouvelles fonctionnalités sont disponibles.  

    1.  À l’aide de l’interface utilisateur Gestionnaire du cluster de basculement ou de l’applet de commande [`Get-ClusterGroup`](https://docs.microsoft.com/powershell/module/failoverclusters/Get-ClusterGroup?view=win10-ps) , vérifiez que tous les rôles de cluster s’exécutent sur le cluster comme prévu. Dans l’exemple suivant, le stockage disponible n’est pas utilisé. au lieu de cela, le volume partagé de cluster est utilisé, par conséquent, le stockage disponible affiche un état **hors connexion** (voir la figure 18).  

        ![ScreenCap présentant la sortie de l’applet de commande ClusterGroup](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetClusterGroup.png)  
        **Figure 18 : vérification de l’exécution de tous les groupes de cluster (rôles de cluster) à l’aide de l’applet de commande [`Get-ClusterGroup`](https://docs.microsoft.com/powershell/module/failoverclusters/Get-ClusterGroup?view=win10-ps)**  

    2.  Vérifiez que tous les nœuds de cluster sont en ligne et en cours d’exécution à l’aide de l’applet de commande [`Get-ClusterNode`](https://docs.microsoft.com/powershell/module/failoverclusters/Get-ClusterNode?view=win10-ps) .  
    3.  Exécutez l’applet de commande [`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx) -aucune erreur ne doit être retournée (voir figure 19).  

        ![ScreenCap présentant la sortie de l’applet de commande Update-ClusterFunctionalLevel](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_SelectFunctionalLevel.png)  
        **Figure 19 : mise à jour du niveau fonctionnel d’un cluster à l’aide de PowerShell**  

    4.  Une fois l’applet de commande [`Update-ClusterFunctionalLevel`](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) exécutée, de nouvelles fonctionnalités sont disponibles.  

4. Windows Server 2016-reprendre les mises à jour et les sauvegardes de cluster normales :  

    1. Si vous exécutez la mise à jour adaptée aux clusters, redémarrez-la à l’aide de l’interface utilisateur de la mise à jour adaptée aux clusters ou utilisez l’applet de commande [`Enable-CauClusterRole`](https://docs.microsoft.com/powershell/module/clusterawareupdating/Enable-CauClusterRole?view=win10-ps)  

        ![ScreenCap présentant la sortie de la](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_EnableCAUClusterRole.png) Enable-CauClusterRole  
        **Figure 20 : activer le rôle mises à jour prenant en charge les clusters à l’aide de l’applet de commande [`Enable-CauClusterRole`](https://docs.microsoft.com/powershell/module/clusterawareupdating/Enable-CauClusterRole?view=win10-ps)**  

    2. Reprendre les opérations de sauvegarde.  

5. Activez et utilisez les fonctionnalités de Windows Server 2016 sur les machines virtuelles Hyper-V.  

    1. Une fois que le cluster a été mis à niveau vers le niveau fonctionnel de Windows Server 2016, de nombreuses charges de travail comme les machines virtuelles Hyper-V auront de nouvelles fonctionnalités. Pour obtenir la liste des nouvelles fonctionnalités Hyper-V. Voir [migrer et mettre à niveau des machines virtuelles](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/migrating_vms)  

    2. Sur chaque nœud hôte Hyper-V du cluster, utilisez l’applet de commande [`Get-VMHostSupportedVersion`](https://docs.microsoft.com/powershell/module/hyper-v/Get-VMHostSupportedVersion?view=win10-ps) pour afficher les versions de configuration de machine virtuelle Hyper-v prises en charge par l’hôte.  

        ![ScreenCap présentant la sortie de l’applet de commande VMHostSupportedVersion](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_GetVMHostSupportVersion.png)  
        **Figure 21 : affichage des versions de configuration des machines virtuelles Hyper-V prises en charge par l’hôte**  

   3. Sur chaque nœud hôte Hyper-V du cluster, les versions de configuration de machine virtuelle Hyper-V peuvent être mises à niveau en planifiant une courte fenêtre de maintenance avec les utilisateurs, en sauvegardant, en désactivant les machines virtuelles et en exécutant l’applet de commande [`Update-VMVersion`](https://docs.microsoft.com/powershell/module/hyper-v/Update-VMVersion?view=win10-ps) (voir figure 22). Cette opération met à jour la version de la machine virtuelle et active les nouvelles fonctionnalités Hyper-V, ce qui évite d’avoir à effectuer de futures mises à jour du composant d’intégration Hyper-V. Cette applet de commande peut être exécutée à partir du nœud Hyper-V qui héberge la machine virtuelle, ou le paramètre `-ComputerName` peut être utilisé pour mettre à jour la version de la machine virtuelle à distance. Dans cet exemple, nous mettons à niveau la version de configuration de VM1 de 5,0 à 7,0 pour tirer parti de nombreuses nouvelles fonctionnalités Hyper-V associées à cette version de configuration de machine virtuelle, telles que les points de contrôle de production (sauvegardes de cohérence d’application) et le fichier de configuration de machine virtuelle binaire.  

       ![ScreenCap présentant l’applet de commande Update-VMVersion en action](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_StopVM.png)  
       **Figure 22 : mise à niveau d’une version de machine virtuelle à l’aide de l’applet de commande PowerShell Update-VMVersion**  

6. Les pools de stockage peuvent être mis à niveau à l’aide de l’applet de commande PowerShell [Update-StoragePool](https://docs.microsoft.com/powershell/module/storage/Update-StoragePool?view=win10-ps) -il s’agit d’une opération en ligne.  

Bien que nous ciblions des scénarios de cloud privé, en particulier les clusters de serveurs de fichiers Hyper-V et avec montée en puissance parallèle, qui peuvent être mis à niveau sans temps d’arrêt, le processus de mise à niveau propagée de système d’exploitation de cluster peut être utilisé pour n’importe quel rôle de cluster.  

## <a name="restrictions--limitations"></a>Restrictions/limitations  
- Cette fonctionnalité fonctionne uniquement pour les versions de Windows Server 2012 R2 vers Windows Server 2016 uniquement. Cette fonctionnalité ne peut pas mettre à niveau des versions antérieures de Windows Server, telles que Windows Server 2008, Windows Server 2008 R2 ou Windows Server 2012 vers Windows Server 2016.  
- Chaque nœud Windows Server 2016 doit être reformaté/nouvelle installation uniquement. Le type d’installation « sur place » ou « mise à niveau » est déconseillé.  
- Un nœud Windows Server 2016 doit être utilisé pour ajouter des nœuds Windows Server 2016 au cluster.  
- Lors de la gestion d’un cluster en mode mixte, effectuez toujours les tâches de gestion à partir d’un nœud de niveau supérieur exécutant Windows Server 2016. Les nœuds Windows Server 2012 R2 de niveau inférieur ne peuvent pas utiliser l’interface utilisateur ou les outils de gestion sur Windows Server 2016.  
- Nous encourageons les clients à parcourir rapidement le processus de mise à niveau du cluster, car certaines fonctionnalités du cluster ne sont pas optimisées pour le mode mixte du système d’exploitation.  
- Évitez de créer ou de redimensionner le stockage sur des nœuds Windows Server 2016 alors que le cluster s’exécute en mode mixte en raison d’éventuelles incompatibilités lors du basculement d’un nœud Windows Server 2016 vers des nœuds Windows Server 2012 R2 de niveau supérieur.  

## <a name="frequently-asked-questions"></a>Forum aux questions  
**Combien de temps le cluster de basculement peut-il s’exécuter en mode mixte ?**  
    Nous encourageons les clients à effectuer la mise à niveau dans un délai de quatre semaines. Il existe de nombreuses optimisations dans Windows Server 2016. Nous avons réussi à mettre à niveau les clusters de serveurs de fichiers Hyper-V et avec montée en puissance parallèle sans temps d’arrêt en moins de quatre heures au total.  

**Reportez-vous cette fonctionnalité sur Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008 ?**  
    Nous n’avons pas de plan pour replacer cette fonctionnalité dans les versions précédentes. La mise à niveau propagée de système d’exploitation de cluster est notre vision de la mise à niveau des clusters Windows Server 2012 R2 vers Windows Server 2016 et ultérieur.  

**Est-ce que toutes les mises à jour logicielles doivent être installées sur le cluster Windows Server 2012 R2 avant de commencer le processus de mise à niveau propagée du système d’exploitation du cluster ?**  
    Oui, avant de commencer le processus de mise à niveau propagée du système d’exploitation du cluster, vérifiez que tous les nœuds de cluster sont mis à jour avec les dernières mises à jour logicielles.  

**Puis-je exécuter l’applet de commande [`Update-ClusterFunctionalLevel`](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) alors que les nœuds sont désactivés ou suspendus ?**  
    Non. Tous les nœuds de cluster doivent être activés et dans l’appartenance active pour que l’applet de commande [`Update-ClusterFunctionalLevel`](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) fonctionne.  

**La mise à niveau propagée du système d’exploitation du cluster fonctionne-t-elle pour toute charge de travail Fonctionne-t-il pour SQL Server ?**  
    Oui, la mise à niveau propagée de système d’exploitation de cluster fonctionne pour toute charge de travail de cluster. Toutefois, il ne s’agit que d’un temps d’arrêt nul pour les clusters de serveurs de fichiers avec montée en puissance parallèle et Hyper-V. La plupart des autres charges de travail entraînent des temps d’arrêt (généralement quelques minutes) lors du basculement, et le basculement est requis au moins une fois pendant le processus de mise à niveau propagée du système d’exploitation du cluster.  

**Puis-je automatiser ce processus à l’aide de PowerShell ?**  
    Oui, nous avons conçu la mise à niveau propagée du système d’exploitation de cluster pour qu’elle soit automatisée avec PowerShell.  

**Puis-je mettre à niveau plusieurs nœuds simultanément pour un cluster de grande taille qui dispose d’une charge de travail et d’une capacité de basculement supplémentaires ?**  
    Oui. Lorsqu’un nœud est supprimé du cluster pour mettre à niveau le système d’exploitation, le cluster possède un nœud moins pour le basculement, ce qui réduit la capacité de basculement. Pour les clusters de grande taille avec une charge de travail et une capacité de basculement suffisantes, plusieurs nœuds peuvent être mis à niveau simultanément. Vous pouvez ajouter temporairement des nœuds de cluster au cluster pour améliorer la capacité de charge de travail et de basculement pendant le processus de mise à niveau propagée du système d’exploitation du cluster.  

**Que se passe-t-il si je découvre un problème dans mon cluster après que [`Update-ClusterFunctionalLevel`](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) a été exécuté avec succès ?**  
    Si vous avez sauvegardé la base de données de cluster avec une sauvegarde de l’état du système avant d’exécuter [`Update-ClusterFunctionalLevel`](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps), vous devez être en mesure d’effectuer une restauration faisant autorité sur un nœud de cluster Windows Server 2012 R2 et de restaurer la base de données et la configuration de cluster d’origine.  

**Puis-je utiliser la mise à niveau sur place pour chaque nœud au lieu d’utiliser l’installation Clean-OS en reformatant le lecteur système ?**  
    Nous n’encourageons pas l’utilisation de la mise à niveau sur place de Windows Server, mais nous sommes conscients qu’elle fonctionne dans certains cas où les pilotes par défaut sont utilisés. Veuillez lire attentivement tous les messages d’avertissement affichés pendant la mise à niveau sur place d’un nœud de cluster.  

**Si j’utilise la réplication Hyper-V pour une machine virtuelle Hyper-v sur mon cluster Hyper-V, la réplication reste-t-elle intacte pendant et après le processus de mise à niveau propagée du système d’exploitation du cluster ?**  
    Oui, la réplication Hyper-V reste intacte pendant et après le processus de mise à niveau propagée du système d’exploitation du cluster.  

**Puis-je utiliser System Center 2016 Virtual Machine Manager (SCVMM) pour automatiser le processus de mise à niveau propagée du système d’exploitation du cluster ?**  
    Oui, vous pouvez automatiser le processus de mise à niveau propagée du système d’exploitation du cluster à l’aide de VMM dans System Center 2016.  

## <a name="see-also"></a>Voir aussi  
-   [Notes de publication : problèmes importants dans Windows Server 2016](../get-started/Release-Notes--Important-Issues-in-Windows-Server-2016-Technical-Preview.md)  
-   [Nouveautés de Windows Server 2016](../get-started/What-s-New-in-windows-server-2016.md)  
-   [Nouveautés du clustering de basculement dans Windows Server](whats-new-in-failover-clustering.md)  
