---
title: Mise à niveau propagée de système d’exploitation de cluster
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: get-started-article
ms.assetid: 6e102c1f-df26-4eaa-bc7a-d0d55d3b82d5
author: jasongerend
ms.author: jgerend
ms.date: 03/27/2018
ms.openlocfilehash: 60dacf63f1a355b961f84169060dbd7122a6fd32
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842730"
---
# <a name="cluster-operating-system-rolling-upgrade"></a>Mise à niveau propagée du système d’exploitation de cluster

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Niveau propagée de cluster du système d’exploitation permet à un administrateur mettre à niveau le système d’exploitation des nœuds du cluster sans arrêter les charges de travail du serveur de fichiers avec montée en puissance parallèle ni Hyper-V. Avec cette fonctionnalité, les pénalités de temps d’arrêt sur les contrats de niveau de service peuvent être évitées.

Niveau propagée de cluster du système d’exploitation offre les avantages suivants :

- Clusters de basculement exécutant des charges de travail de serveur de fichiers avec montée en puissance (SOFS) et de la machine virtuelle Hyper-V peuvent être mis à niveau à partir de Windows Server 2012 R2 (en cours d’exécution sur tous les nœuds du cluster) vers Windows Server 2016 (en cours d’exécution sur tous les nœuds de cluster du cluster) sans temps d’arrêt. Autres charges de travail de cluster, telles que SQL Server, ne seront pas disponibles pendant la durée (en général moins de cinq minutes) pour basculer vers Windows Server 2016.  
- Il ne nécessite pas de matériel supplémentaire. Bien que vous pouvez ajouter d’autres nœuds de cluster temporairement pour les clusters de petite taille pour améliorer la disponibilité du cluster pendant le Cluster du système d’exploitation mise à niveau propagée traitement.  
- Le cluster n’a pas besoin être arrêté ou redémarré.  
- Un nouveau cluster n’est pas nécessaire. Le cluster existant est mis à niveau. En outre, les objets de cluster existants stockés dans Active Directory sont utilisées.  
- Le processus de mise à niveau est réversible jusqu'à ce que le choisit de client le « point-de-sans retour », lorsque tous les nœuds du cluster exécutent Windows Server 2016, et lors de l’exécution de l’applet de commande Update-ClusterFunctionalLevel PowerShell.  
- Le cluster peut prendre en charge les opérations de mise à jour corrective et maintenance lors de l’exécution en mode mixte-du système d’exploitation.  
- Il prend en charge automation via PowerShell et WMI.  
- La propriété publique de cluster **ClusterFunctionalLevel** propriété indique l’état du cluster sur les nœuds de cluster Windows Server 2016. Cette propriété peut être interrogée à l’aide de l’applet de commande PowerShell à partir d’un nœud de cluster Windows Server 2016 qui appartient à un cluster de basculement :  
    ```PowerShell
    Get-Cluster | Select ClusterFunctionalLevel  
    ```  

    La valeur **8** indique que le cluster est en cours d’exécution au niveau fonctionnel Windows Server 2012 R2. La valeur **9** indique que le cluster est en cours d’exécution au niveau fonctionnel Windows Server 2016.  

Ce guide décrit les diverses étapes du processus niveau propagée de Cluster du système d’exploitation, étapes d’installation, limitations des fonctionnalités, les questions fréquemment posées (FAQ) et s’applique aux scénarios suivants dans Windows Server 2016 Upgrade propagée de Cluster du système d’exploitation :  
- Clusters Hyper-V  
- Clusters de serveur de fichiers avec montée en puissance  

Le scénario suivant n’est pas prise en charge dans Windows Server 2016 :  
-  Système d’exploitation mise à niveau propagée de clusters invités à l’aide de disque dur virtuel (fichier .vhdx) comme stockage partagé de cluster  

Niveau propagée de cluster du système d’exploitation complet est pris en charge par System Center Virtual Machine Manager (SCVMM) 2016. Si vous utilisez SCVMM 2016, consultez [effectuer une mise à niveau propagée d’un cluster d’hôte Hyper-V vers Windows Server 2016 dans VMM](https://docs.microsoft.com/system-center/vmm/hyper-v-rolling-upgrade?view=sc-vmm-1807) pour obtenir des conseils sur la mise à niveau les clusters et en automatisant les étapes décrites dans ce document.  

## <a name="requirements"></a>Configuration requise  
Terminez la configuration suivante avant de commencer le processus de niveau propagée de Cluster du système d’exploitation :

- Démarrez avec un Cluster de basculement exécutant Windows Server (canal semi-annuel), Windows Server 2016 ou Windows Server 2012 R2.
- Mise à niveau un cluster d’espaces de stockage Direct vers Windows Server, version 1709 n’est pas prise en charge.
- Si la charge de travail de cluster est machines virtuelles Hyper-V ou un serveur de fichiers avec montée en puissance, vous pouvez attendre la mise à niveau sans interruption de service.
- Vérifiez que les nœuds Hyper-V ont des processeurs qui prennent en charge de Second niveau Addressing Table (SLAT) à l’aide d’une des méthodes suivantes :  
        -Passer en revue le [êtes-vous Compatible SLAT ? WP8 SDK Conseil 01](http://blogs.msdn.com/b/devfish/archive/2012/11/06/are-you-slat-compatible-wp8-sdk-tip-01.aspx) article qui décrit deux méthodes pour vérifier si un processeur prend en charge LATTES  
        -Téléchargez le [Coreinfo v3.31](https://technet.microsoft.com/sysinternals/cc835722) outil pour déterminer si un processeur prend en charge SLAT.

## <a name="cluster-transition-states-during-cluster-os-rolling-upgrade"></a>États de transition de cluster au cours de niveau propagée de Cluster du système d’exploitation

Cette section décrit les différents États de transition du cluster Windows Server 2012 R2 qui est mis à niveau vers Windows Server 2016 à l’aide du niveau propagée de Cluster du système d’exploitation.  

Afin de conserver les charges de travail de cluster en cours d’exécution pendant le processus de niveau propagée de Cluster du système d’exploitation, déplacement d’une charge de travail du cluster à partir d’un nœud Windows Server 2012 R2 vers Windows Server 2016 nœud fonctionne comme si les deux nœuds ont été exécutant le système d’exploitation Windows Server 2012 R2. Lorsque les nœuds Windows Server 2016 sont ajoutés au cluster, elles opèrent dans un mode de compatibilité de Windows Server 2012 R2. Un nouveau mode de cluster conceptuel, appelé « Mode système d’exploitation », permet aux nœuds de différentes versions d’exister dans le même cluster (voir Figure 1).  

![Illustration montrant les trois phases d’une mise à niveau propagée de cluster du système d’exploitation : tous les nœuds Windows Server 2012 R2, le mode de-système d’exploitation et tous les nœuds Windows Server 2016](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_RollingUpgrade_Overview.png)  
**Figure 1 : Transitions d’état de système d’exploitation de cluster**  

Un cluster Windows Server 2012 R2 en mode mixte-du système d’exploitation lorsqu’un nœud Windows Server 2016 est ajouté au cluster. Le processus est entièrement réversible : les nœuds Windows Server 2016 peuvent être supprimés du cluster et les nœuds de Windows Server 2012 R2 peuvent être ajoutés au cluster dans ce mode. Le « point de non retour » se produit lorsque l’applet de commande Update-ClusterFunctionalLevel PowerShell est exécuté sur le cluster. Afin que cette applet de commande réussisse, tous les nœuds doivent être Windows Server 2016, et tous les nœuds doivent être en ligne.  

## <a name="transition-states-of-a-four-node-cluster-while-performing-rolling-os-upgrade"></a>États de transition d’un cluster à quatre nœuds lors de l’exécution du système d’exploitation mise à niveau propagée

Cette section illustre et décrit les quatre différentes étapes d’un cluster avec un stockage partagé dont les nœuds sont mis à niveau à partir de Windows Server 2012 R2 vers Windows Server 2016.  

« Étape 1 » est l’état initial : nous commençons avec un cluster Windows Server 2012 R2.  

![Illustration montrant l’état initial : tous les nœuds Windows Server 2012 R2](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage1.png)  
**Figure 2 : État initial : Cluster de basculement Windows Server 2012 R2 (étape 1)**  

Dans la « phase 2 », les deux nœuds ont été suspendues, purgés, supprimées, reformatés et installés avec Windows Server 2016.  

![Illustration montrant le cluster en mode de système d’exploitation mixtes : du cluster de 4 nœuds exemple, deux nœuds sont en cours d’exécution Windows Server 2016, et deux nœuds exécutent Windows Server 2012 R2](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage2.png)  
**Figure 3 : État de niveau intermédiaire : Mode de-système d’exploitation : Cluster Windows Server 2012 R2 et de basculement Windows Server 2016 (étape 2)**  

À « étape 3 », tous les nœuds du cluster ont été mis à niveau vers Windows Server 2016 et le cluster est prêt à être mis à niveau avec l’applet de commande Update-ClusterFunctionalLevel PowerShell.  

> [!NOTE]  
> À ce stade, le processus peut être entièrement inversé et les nœuds de Windows Server 2012 R2 peuvent être ajoutés à ce cluster.  

![Illustration montrant que le cluster a été mis à niveau vers Windows Server 2016 et est prêt pour l’applet de commande Update-ClusterFunctionalLevel pour afficher le niveau fonctionnel du cluster jusqu'à Windows Server 2016](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage3.png)  
**Figure 4 : État de niveau intermédiaire : Tous les nœuds mis à niveau vers Windows Server 2016, prêt pour la mise à jour-ClusterFunctionalLevel (étape 3)**  

Après que la mise à jour-ClusterFunctionalLevelcmdlet est exécuté, le cluster entre dans « Étape 4 », où les nouvelles fonctionnalités de cluster Windows Server 2016 peuvent être utilisées.  

![Illustration montrant que la mise à niveau du système d’exploitation propagée de cluster est correctement terminé ; tous les nœuds ont été mis à niveau vers Windows Server 2016 et le cluster est en cours d’exécution au niveau fonctionnel de cluster Windows Server 2016](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage4.png)  
**Figure 5 : État final : Cluster de basculement Windows Server 2016 (étape 4)**  

## <a name="cluster-os-rolling-upgrade-process"></a>Système d’exploitation de cluster des processus de mise à niveau propagée

Cette section décrit le flux de travail pour l’exécution à niveau propagée de Cluster du système d’exploitation.  

![Illustration montrant le flux de travail de mise à niveau d’un cluster](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_RollingUpgrade_Workflow.png)  
**Figure 6 : Système d’exploitation de cluster propagée des flux de travail de mise à niveau**  

Mise à niveau propagée du système d’exploitation des cluster comprend les étapes suivantes :  

1. Préparez le cluster pour la mise à niveau du système d’exploitation comme suit :  
    1. Niveau propagée de cluster du système d’exploitation, vous devez supprimer un nœud à la fois à partir du cluster. Vérifiez si vous disposez d’une capacité suffisante sur le cluster pour maintenir la haute disponibilité SLA lorsqu’un des nœuds du cluster est supprimé du cluster pour une mise à niveau du système d’exploitation. En d’autres termes, vous avez besoin à la possibilité de charges de travail de basculement vers un autre nœud quand un nœud est supprimé du cluster pendant le processus de niveau propagée de Cluster du système d’exploitation ? Le cluster a la capacité d’exécuter les charges de travail requis lorsqu’un nœud est supprimé du cluster de niveau propagée de Cluster du système d’exploitation ?  
    2. Pour les charges de travail Hyper-V, vérifiez que tous les hôtes Windows Server 2016 Hyper-V ont processeur prend en charge de la Table d’adresse de Second niveau (SLAT). Seules les machines prenant en charge de SLAT peuvent utiliser le rôle Hyper-V dans Windows Server 2016.  
    3. Vérifiez que les sauvegardes de la charge de travail est terminé et envisagez de sauvegarder le cluster. Arrêter des opérations de sauvegarde lors de l’ajout de nœuds au cluster.  
    4. Vérifiez que tous les nœuds de cluster sont en ligne/en cours d’exécution/accès à l’aide de la [ `Get-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Get-ClusterNode?view=win10-ps) applet de commande (voir la Figure 7).  

        ![Capture d’écran affichant les résultats de l’exécution de l’applet de commande Get-ClusterNode](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetClusterNode.png)  
        **Figure 7 : Déterminer l’état du nœud à l’aide d’applet de commande Get-ClusterNode**  

    5. Si vous exécutez des mises à jour (adaptée aux clusters), vérifiez si cette dernière est en cours d’exécution à l’aide de la **la mise à jour de Cluster-conscientes** l’interface utilisateur, ou le [ `Get-CauRun` ](https://docs.microsoft.com/powershell/module/clusterawareupdating/Get-CauRun?view=win10-ps) applet de commande (voir Figure 8). Arrêter adaptée aux clusters à l’aide de la [ `Disable-CauClusterRole` ](https://docs.microsoft.com/powershell/module/clusterawareupdating/Disable-CauClusterRole?view=win10-ps) applet de commande (voir Figure 9) pour empêcher tous les nœuds à partir de la suspension et purgées par adaptée aux clusters pendant le processus de niveau propagée de Cluster du système d’exploitation.  

        ![Capture d’écran montrant la sortie de l’applet de commande Get-CauRun](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetCAU.png)  
        **Figure 8 : À l’aide de la [ `Get-CauRun` ](https://docs.microsoft.com/powershell/module/clusterawareupdating/Get-CauRun?view=win10-ps) cmdlet afin de déterminer si prenant en charge les mises à jour de Cluster est en cours d’exécution sur le cluster**  

        ![Capture d’écran montrant la sortie de l’applet de commande Disable-CauClusterRole](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_DisableCAU.png)  
        **Figure 9 : Désactivation du rôle prenant en charge les mises à jour de Cluster à l’aide du [ `Disable-CauClusterRole` ](https://docs.microsoft.com/powershell/module/clusterawareupdating/Disable-CauClusterRole?view=win10-ps) applet de commande**  

2. Pour chaque nœud du cluster, procédez comme suit :  
    1. À l’aide du Gestionnaire du Cluster UI, sélectionnez un nœud et utiliser le **Pause | Vider** option de menu pour drainer le nœud (voir Figure 10) ou utiliser le [ `Suspend-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Suspend-ClusterNode?view=win10-ps) applet de commande (voir Figure 11).  

        ![Capture d’écran montrant comment vider les rôles avec le Gestionnaire du Cluster UI](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_FCM_DrainRoles.png)  
        **Figure 10 : Drainage des rôles à partir d’un nœud à l’aide du Gestionnaire du Cluster de basculement**  

        ![Capture d’écran montrant la sortie de l’applet de commande Suspend-ClusterNode](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_SuspendNode.png)  
        **Figure 11 : Drainage des rôles à partir d’un nœud à l’aide de la [ `Suspend-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Suspend-ClusterNode?view=win10-ps) applet de commande**  

    2.  À l’aide du Gestionnaire du Cluster UI, **supprimer** le nœud de cluster, ou utilisez suspendu le [ `Remove-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Remove-ClusterNode?view=win10-ps) applet de commande.  

        ![Capture d’écran montrant la sortie de l’applet de commande Remove-ClusterNode](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_RemoveNode.png)  
        **Figure 12 : Supprimer un nœud du cluster à l’aide [ `Remove-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Remove-ClusterNode?view=win10-ps) applet de commande**  

    3.  Reformater le lecteur du système et effectuer une installation « nettoyer le système d’exploitation » de Windows Server 2016 sur le nœud à l’aide de la **personnalisé : Installer Windows uniquement (Avancé)** option d’installation (voir Figure 13) de setup.exe. Évitez de sélectionner le **mise à niveau : Installer Windows et de conserver les fichiers, les paramètres et les applications** option étant donné que le niveau propagée de Cluster du système d’exploitation n’encourager mise à niveau sur place.  

        ![Capture d’écran de l’Assistant d’installation de Windows Server 2016 montrant l’option d’installation personnalisée sélectionnée](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_InstallOption.png)  
        **Figure 13 : Options d’installation disponibles pour Windows Server 2016**  

    4.  Ajoutez le nœud au domaine Active Directory approprié.  
    5.  Ajoutez les utilisateurs appropriés au groupe Administrateurs.  
    6.  À l’aide de l’applet de commande Install-WindowsFeature PowerShell ou de gestionnaire de serveur UI, installer tous les rôles de serveur dont vous avez besoin, telles que Hyper-V.  

        ```PowerShell
        Install-WindowsFeature -Name Hyper-V  
        ```  

    7.  À l’aide de l’applet de commande Install-WindowsFeature PowerShell ou de gestionnaire de serveur UI, installez la fonctionnalité Clustering avec basculement.  

        ```PowerShell
        Install-WindowsFeature -Name Failover-Clustering  
        ```  

    8.  Installer les fonctionnalités supplémentaires requises par vos charges de travail du cluster.  
    9. Vérifiez les paramètres de connectivité réseau et de stockage à l’aide de l’UI Gestionnaire du Cluster de basculement.  
    10. Si le pare-feu Windows est utilisé, vérifiez que les paramètres de pare-feu sont corrects pour le cluster. Par exemple, clusters de la mise à jour (adaptée aux clusters) activé peuvent nécessiter de configuration du pare-feu.  
    11. Pour les charges de travail Hyper-V, utilisez l’interface utilisateur de Gestionnaire Hyper-V pour lancer la boîte de dialogue Gestionnaire de commutateur virtuel (voir Figure 14).  

        Vérifiez que le nom de la Switch(s) virtuel utilisés sont identiques pour tous les nœuds d’hôte Hyper-V dans le cluster.  

        ![Capture d’écran montrant l’emplacement de la boîte de dialogue Gestionnaire de commutateur virtuel Hyper-V](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_VMSwitch.png)  
        **Figure 14 : Gestionnaire de commutateur virtuel**  

    12. Sur un nœud Windows Server 2016 (n’utilisez pas un nœud Windows Server 2012 R2), utilisez le Gestionnaire de Cluster de basculement (voir Figure 15) pour se connecter au cluster.  

        ![Capture d’écran montrant la boîte de dialogue Sélectionnez cluster](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_AddNode.png)  
        **Figure 15 : Ajout d’un nœud au cluster à l’aide du Gestionnaire du Cluster de basculement**  

    13. Utilisez soit l’UI du Gestionnaire du Cluster de basculement ou le [ `Add-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Add-ClusterNode?view=win10-ps) applet de commande (voir Figure 16) pour ajouter le nœud au cluster.  

        ![Capture d’écran montrant la sortie de l’applet de commande Add-ClusterNode](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_AddNode3.png)  
        **Figure 16 : Ajout d’un nœud au cluster en utilisant [ `Add-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Add-ClusterNode?view=win10-ps) applet de commande**  

        > [!NOTE]  
        > Lorsque le premier nœud Windows Server 2016 rejoint le cluster, le cluster en mode « Mixed-système d’exploitation », et les ressources principales du cluster sont déplacés vers le nœud Windows Server 2016. Un cluster du mode « Mixed-système d’exploitation » est un cluster entièrement fonctionnel, les nouveaux nœuds s’exécuter dans un mode de compatibilité avec les anciens nœuds. « OS mixte » est un mode temporaire pour le cluster. Il n’est pas destinée à être permanentes et les clients doivent mettre à jour tous les nœuds de leur cluster au sein des quatre dernières semaines.  

    14. Après Windows Server 2016 les nœud est correctement ajouté au cluster, vous pouvez (éventuellement) transférer certains de la charge de travail de cluster vers le nœud qui vient d’être ajouté pour rééquilibrer la charge de travail au sein du cluster comme suit :

        ![Capture d’écran montrant la sortie de l’applet de commande Move-ClusterVirtualMachineRole](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_MoveVMRole.png)  
        **Figure 17 : Déplacement d’une charge de travail (rôle de machine virtuelle de cluster) de cluster à l’aide [ `Move-ClusterVirtualMachineRole` ](https://docs.microsoft.com/powershell/module/failoverclusters/Move-ClusterVirtualMachineRole?view=win10-ps) applet de commande**  

        1. Utilisez **Live Migration** pour les machines virtuelles à partir du Gestionnaire de Cluster de basculement ou le [ `Move-ClusterVirtualMachineRole` ](https://docs.microsoft.com/powershell/module/failoverclusters/Move-ClusterVirtualMachineRole?view=win10-ps) applet de commande (voir Figure 17) pour effectuer une migration dynamique des machines virtuelles.  

            ```PowerShell
            Move-ClusterVirtualMachineRole -Name VM1 -Node robhind-host3  
            ```  

        2. Utilisez **déplacer** à partir du Gestionnaire du Cluster de basculement ou le [ `Move-ClusterGroup` ](https://docs.microsoft.com/powershell/module/failoverclusters/Move-ClusterGroup?view=win10-ps) applet de commande pour les autres charges de travail du cluster.  

3. Lorsque chaque nœud a été mis à niveau vers Windows Server 2016 et ajouté au cluster, ou lorsque les nœuds restants de Windows Server 2012 R2 ont été supprimés, procédez comme suit :  

    > [!IMPORTANT]  
    > -   Une fois que vous mettez à jour le niveau fonctionnel du cluster, vous ne pouvez pas revenir au niveau fonctionnel de Windows Server 2012 R2 et Windows Server 2012 R2 nœuds ne peuvent pas être ajoutés au cluster.
    > -   Jusqu'à ce que le [ `Update-ClusterFunctionalLevel` ](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) applet de commande est exécutée, le processus est entièrement réversible et les nœuds de Windows Server 2012 R2 peuvent être ajoutés à ce cluster et les nœuds Windows Server 2016 peuvent être supprimés.  
    > -   Après le [ `Update-ClusterFunctionalLevel` ](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) applet de commande est exécutée, de nouvelles fonctionnalités seront disponibles.  

    1.  À l’aide de l’UI Gestionnaire du Cluster de basculement ou le [ `Get-ClusterGroup` ](https://docs.microsoft.com/powershell/module/failoverclusters/Get-ClusterGroup?view=win10-ps) applet de commande, vérifiez que tous les rôles de cluster sont en cours d’exécution sur le cluster comme prévu. Dans l’exemple suivant, le stockage disponible n’est pas en cours utilisé, au lieu de cela CSV est utilisé, par conséquent, le stockage disponible affiche un **hors connexion** état (voir Figure 18).  

        ![Capture d’écran montrant la sortie de l’applet de commande Get-ClusterGroup](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetClusterGroup.png)  
        **Figure 18 : Vérification que tous les groupes (rôles de cluster) de cluster sont en cours d’exécution à l’aide de la [ `Get-ClusterGroup` ](https://docs.microsoft.com/powershell/module/failoverclusters/Get-ClusterGroup?view=win10-ps) applet de commande**  

    2.  Vérifiez que tous les nœuds de cluster sont en ligne et en cours d’exécution à l’aide de la [ `Get-ClusterNode` ](https://docs.microsoft.com/powershell/module/failoverclusters/Get-ClusterNode?view=win10-ps) applet de commande.  
    3.  Exécutez le [ `Update-ClusterFunctionalLevel` ](https://technet.microsoft.com/library/mt589702.aspx) applet de commande - aucun erreurs ne doivent être retournées (voir Figure 19).  

        ![Capture d’écran montrant la sortie de l’applet de commande Update-ClusterFunctionalLevel](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_SelectFunctionalLevel.png)  
        **Figure 19 : La mise à jour le niveau fonctionnel d’un cluster à l’aide de PowerShell**  

    4.  Après le [ `Update-ClusterFunctionalLevel` ](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) applet de commande est exécutée, de nouvelles fonctionnalités sont disponibles.  

4. Windows Server 2016 - reprendre les sauvegardes et les mises à jour de cluster normal :  

    1. Si vous avez précédemment exécuté adaptée aux clusters, redémarrez-le à l’aide de l’UI CAU ou utiliser le [ `Enable-CauClusterRole` ](https://docs.microsoft.com/powershell/module/clusterawareupdating/Enable-CauClusterRole?view=win10-ps) applet de commande (voir la Figure 20).  

        ![Capture d’écran montrant la sortie de l’Enable-CauClusterRole](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_EnableCAUClusterRole.png)  
        **Figure 20 : Activation de rôle prenant en charge les mises à jour de Cluster à l’aide du [ `Enable-CauClusterRole` ](https://docs.microsoft.com/powershell/module/clusterawareupdating/Enable-CauClusterRole?view=win10-ps) applet de commande**  

    2. Reprendre les opérations de sauvegarde.  

5. Activer et utiliser les fonctionnalités de Windows Server 2016 sur des Machines virtuelles Hyper-V.  

    1. Une fois que le cluster a été mis à niveau vers le niveau fonctionnel Windows Server 2016, nombreuses charges de travail comme les machines virtuelles Hyper-V auront les nouvelles fonctionnalités. Pour obtenir la liste des nouvelles fonctionnalités Hyper-V. consultez [migration et mise à niveau des machines virtuelles](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/migrating_vms)  

    2. Sur chaque nœud d’hôte Hyper-V dans le cluster, utilisez le [ `Get-VMHostSupportedVersion` ](https://docs.microsoft.com/powershell/module/hyper-v/Get-VMHostSupportedVersion?view=win10-ps) applet de commande pour afficher les versions de configuration de machine virtuelle Hyper-V qui sont pris en charge par l’hôte.  

        ![Capture d’écran montrant la sortie de l’applet de commande Get-VMHostSupportedVersion](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_GetVMHostSupportVersion.png)  
        **Figure 21 : Afficher les versions de configuration de machine virtuelle Hyper-V pris en charge par l’hôte**  

   3.  Sur chaque nœud d’hôte Hyper-V dans le cluster, les versions de configuration de machine virtuelle Hyper-V peuvent être mis à niveau par planifier une fenêtre de maintenance brève avec des utilisateurs, sauvegarde, la désactivation de machines virtuelles et en cours d’exécution le [ `Update-VMVersion` ](https://docs.microsoft.com/powershell/module/hyper-v/Update-VMVersion?view=win10-ps) (voir la section de l’applet de commande Figure 22). Cela mettre à jour la version de la machine virtuelle et activer les nouvelles fonctionnalités de Hyper-V, éliminant la nécessité pour les futures mises à jour de composant d’intégration Hyper-V (IC). Cette applet de commande peut être exécuté à partir du nœud Hyper-V qui héberge la machine virtuelle, ou le `-ComputerName` paramètre peut être utilisé pour mettre à jour la Version de la machine virtuelle à distance. Dans cet exemple, ici nous mettre à niveau la version de configuration de VM1 5.0 vers 7.0 pour tirer parti des nombreuses nouvelles fonctionnalités d’Hyper-V associé à cette version de configuration de machine virtuelle telles que les points de contrôle de Production (sauvegardes cohérentes) et la machine virtuelle binaire fichier de configuration.  

        ![Capture d’écran montrant l’applet de commande Update-VMVersion en action](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_StopVM.png)  
        **Figure 22 : La mise à niveau une version de la machine virtuelle à l’aide de l’applet de commande PowerShell de mise à jour-VMVersion**  

4.  Pools de stockage peuvent être mis à niveau à l’aide de la [mise à jour-StoragePool](https://docs.microsoft.com/powershell/module/storage/Update-StoragePool?view=win10-ps) applet de commande PowerShell - il s’agit d’une opération en ligne.  

Bien que nous ciblons les scénarios de Cloud privé, en particulier Hyper-V et les clusters de serveur de fichiers avec montée en puissance, qui peuvent être mis à niveau sans temps d’arrêt, le processus de niveau propagée de Cluster du système d’exploitation peuvent être utilisés pour tous les rôles de cluster.  

## <a name="restrictions--limitations"></a>Restrictions / Limitations  
- Cette fonctionnalité fonctionne uniquement pour Windows Server 2012 R2 aux versions de Windows Server 2016 uniquement. Cette fonctionnalité ne peut pas mettre à niveau les versions antérieures de Windows Server, tels que Windows Server 2008, Windows Server 2008 R2 ou Windows Server 2012 vers Windows Server 2016.  
- Chaque nœud Windows Server 2016 doit être reformaté/nouvelle installation uniquement. « Sur place » ou « mise à niveau » type d’installation est déconseillé.  
- Un nœud Windows Server 2016 doit être utilisé pour ajouter des nœuds de Windows Server 2016 au cluster.  
- Lorsque vous gérez un cluster en mode mixte-du système d’exploitation, vous devez toujours effectuer les tâches de gestion à partir d’un nœud de niveau supérieur qui exécute Windows Server 2016. Les nœuds de niveau inférieur Windows Server 2012 R2 ne peut pas utiliser les outils de l’interface utilisateur ou de gestion par rapport à Windows Server 2016.  
- Nous encourageons les clients à passer rapidement au long du processus de mise à niveau de cluster, car certaines fonctionnalités de cluster ne sont pas optimisées pour le mode système d’exploitation.  
- Évitez de créer ou de redimensionnement de stockage sur les nœuds Windows Server 2016 pendant que le cluster s’exécute en mode de-système d’exploitation en raison d’incompatibilités lors du basculement à partir d’un nœud Windows Server 2016 aux nœuds de bas niveau Windows Server 2012 R2.  

## <a name="frequently-asked-questions"></a>Forum Aux Questions  
**La durée du cluster de basculement d’exécution en mode de-système d’exploitation ?**  
    Nous encourageons les clients pour effectuer la mise à niveau au sein des quatre dernières semaines. Il existe de nombreuses optimisations dans Windows Server 2016. Nous avons mis à niveau Hyper-V et des clusters de serveur de fichiers avec montée en puissance sans interruption de service en moins de quatre heures de total.  

**Sera port cette fonctionnalité à Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008 ?**  
    Nous n’avons pas tous les plans de porter cette fonctionnalité vers les versions précédentes. Niveau propagée de cluster du système d’exploitation est notre vision de la mise à niveau des clusters Windows Server 2012 R2 vers Windows Server 2016 et au-delà.  

**Le cluster Windows Server 2012 R2 n’a besoin d’avoir toutes les mises à jour de logiciel installés avant de commencer le processus de niveau propagée de Cluster du système d’exploitation ?**  
    Oui, avant de commencer le processus de niveau propagée de Cluster du système d’exploitation, vérifiez que tous les nœuds de cluster sont mis à jour avec les dernières mises à jour logicielles.  

**Puis-je exécuter le [ `Update-ClusterFunctionalLevel` ](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) applet de commande alors que les nœuds sont désactivés ou en pause ?**  
    Non. Tous les nœuds de cluster doivent être sur et dans le membre actif pour le [ `Update-ClusterFunctionalLevel` ](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) applet de commande fonctionne.  

**Niveau propagée de Cluster du système d’exploitation fonctionne pour toute charge de travail de cluster ? Il fonctionne pour SQL Server ?**  
    Oui, niveau propagée de Cluster du système d’exploitation fonctionne pour toute charge de travail du cluster. Toutefois, il est uniquement sans interruption de service pour Hyper-V et les clusters de serveur de fichiers avec montée en puissance. La plupart des autres charges de travail entraînent un temps d’arrêt (généralement quelques minutes) lorsqu’ils basculement et le basculement est nécessaire au moins une fois pendant le processus de niveau propagée de Cluster du système d’exploitation.  

**Puis-je automatiser ce processus à l’aide de PowerShell ?**  
    Oui, nous avons conçu le Cluster du système d’exploitation mise à niveau propagée pour être automatisée à l’aide de PowerShell.  

**Pour un grand cluster qui a une charge de travail supplémentaire et la capacité de basculement, puis-je passer plusieurs nœuds simultanément ?**  
    Oui. Lorsqu’un nœud est supprimé du cluster pour mettre à niveau le système d’exploitation, le cluster auront un seul nœud inférieur pour le basculement, par conséquent, auront une capacité de basculement réduit. Pour les clusters de grande taille avec suffisamment de capacité de basculement et la charge de travail, plusieurs nœuds peuvent être mis à niveau simultanément. Vous pouvez ajouter temporairement des nœuds de cluster au cluster pour fournir la charge de travail amélioré et de capacité de basculement pendant le processus de niveau propagée de Cluster du système d’exploitation.  

**Que se passe-t-il si je découvre un problème dans mon cluster après [ `Update-ClusterFunctionalLevel` ](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps) a été exécuté avec succès ?**  
    Si vous avez sauvegardé la base de données de cluster avec une sauvegarde de l’état du système avant d’exécuter [ `Update-ClusterFunctionalLevel` ](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps), vous pourrez effectuer une faisant autorité restaurer sur un nœud de cluster Windows Server 2012 R2 et de restaurer le cluster d’origine base de données et la configuration.  

**Puis-je utiliser mise à niveau sur place pour chaque nœud au lieu d’utiliser l’installation de système d’exploitation nettoyer en reformatant le lecteur du système ?**  
    Nous ne pas encourager l’utilisation de mise à niveau sur place de Windows Server, mais nous sommes conscients qu’elle fonctionne dans certains cas où les pilotes par défaut sont utilisés. Veuillez lire attentivement tous les messages d’avertissement est affiché pendant la mise à niveau sur place d’un nœud de cluster.  

**Si j’utilise la réplication Hyper-V pour une machine virtuelle Hyper-V sur mon cluster Hyper-V, réplication demeureront intacte pendant et après le processus de niveau propagée de Cluster du système d’exploitation ?**  
    Oui, Hyper-V replica reste intacte pendant et après le processus de niveau propagée de Cluster du système d’exploitation.  

**Puis-je utiliser System Center 2016 Virtual Machine Manager (SCVMM) pour automatiser le processus de niveau propagée de Cluster du système d’exploitation ?**  
    Oui, vous pouvez automatiser le processus de niveau propagée de Cluster du système d’exploitation à l’aide de VMM dans System Center 2016.  

## <a name="see-also"></a>Voir aussi  
-   [Notes de publication : Problèmes importants dans Windows Server 2016](../get-started/Release-Notes--Important-Issues-in-Windows-Server-2016-Technical-Preview.md)  
-   [Quelles sont les nouveautés dans Windows Server 2016](../get-started/What-s-New-in-windows-server-2016.md)  
-   [Nouveautés du Clustering de basculement dans Windows Server](whats-new-in-failover-clustering.md)  
