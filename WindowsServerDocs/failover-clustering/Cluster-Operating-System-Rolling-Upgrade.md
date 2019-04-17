---
title: "Mise à niveau propagée de système d’exploitation cluster"
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: get-started-article
ms.assetid: 6e102c1f-df26-4eaa-bc7a-d0d55d3b82d5
author: jasongerend
ms.author: jgerend
ms.date: 10/17/2017
ms.openlocfilehash: 8463c163294a4d2223a74b7cfeaea6ac5ae4fcfe
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="cluster-operating-system-rolling-upgrade"></a>Mise à niveau propagée du système d’exploitation de cluster

> S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Niveau propagée de cluster du système d’exploitation permet à un administrateur mettre à niveau le système d’exploitation des nœuds du cluster sans arrêter l’Hyper-V ou les charges de travail du serveur de fichiers avec montée en puissance parallèle. À l’aide de cette fonctionnalité, les pénalités de temps d’arrêt contre les contrats de niveau de Service (SLA) peuvent être évitées.

Niveau propagée de cluster du système d’exploitation offre les avantages suivants:

- Clusters de basculement exécutant des charges de travail de serveur de fichiers avec montée en puissance parallèle (SOFS) et d’ordinateur virtuel Hyper-V peuvent être mis à niveau à partir de Windows Server2012R2 (en cours d’exécution sur tous les nœuds du cluster) vers Windows Server2016 (en cours d’exécution sur tous les nœuds de cluster du cluster) sans temps d’arrêt. Autres charges de travail de cluster, telles que SQLServer, sera pas disponibles pendant le temps (généralement moins de cinq minutes) nécessaire pour le basculement vers Windows Server2016.  
- Il ne nécessite aucun matériel supplémentaire. Bien que vous pouvez ajouter d’autres nœuds de cluster temporairement pour les clusters de petite taille pour améliorer la disponibilité du cluster pendant le Cluster du système d’exploitation mise à niveau propagée traitent.  
- Le cluster n’a pas besoin être arrêté ou redémarré.  
- Un nouveau cluster n’est pas nécessaire. Le cluster existant est mis à niveau. En outre, les objets de cluster existants stockés dans ActiveDirectory sont utilisés.  
- Le processus de mise à niveau est réversible jusqu'à ce que le choisit client le «point de non-rendement», lorsque tous les nœuds de cluster exécutent Windows Server2016, et lors de l’exécution de l’applet de commande PowerShell Update-ClusterFunctionalLevel.  
- Le cluster peut prendre en charge les opérations de maintenance et de mise à jour corrective lors de l’exécution en mode mixte-système d’exploitation.  
- Il prend en charge d’automation via PowerShell et WMI.  
- La propriété publique cluster **ClusterFunctionalLevel** propriété indique l’état du cluster sur les nœuds de cluster Windows Server2016. Cette propriété peut être interrogée à l’aide de l’applet de commande PowerShell à partir d’un nœud de cluster Windows Server2016 qui appartient à un cluster de basculement:  
    ```PowerShell
    Get-Cluster | Select ClusterFunctionalLevel  
    ```  

    La valeur **8** indique que le cluster est en cours d’exécution au niveau fonctionnel de Windows Server2012R2. La valeur **9** indique que le cluster est en cours d’exécution au niveau fonctionnel Windows Server2016.  

Ce guide décrit les différentes étapes de la niveau propagée de Cluster du système d’exploitation, des processus étapes de l’installation, les limitations de fonctionnalité et Forum aux questions (FAQ) et s’applique aux scénarios suivants dans Windows Server2016 niveau propagée de Cluster du système d’exploitation:  
- Clusters Hyper-V  
- Clusters de serveurs de fichiers avec montée en puissance parallèle  

Le scénario suivant n’est pas pris en charge dans Windows Server2016:  
-  Cluster du système d’exploitation mise à niveau propagée d’invité clusters à l’aide du disque dur virtuel (.vhdx file) en tant que stockage partagé  

Niveau propagée de cluster du système d’exploitation entièrement est pris en charge par SystemCenter Virtual Machine Manager (SCVMM) 2016. Si vous utilisez SCVMM2016, voir [les clusters de la mise à niveau de Windows Server2012R2 vers Windows Server2016dans VMM](https://technet.microsoft.com/library/mt445417.aspx) pour obtenir des conseils sur la mise à niveau les clusters et en automatisant les étapes décrites dans ce document.  

## <a name="requirements"></a>Configuration requise  
Terminer la configuration requise suivante avant de commencer le processus de niveau propagée de Cluster du système d’exploitation:

- Démarrez avec un Cluster de basculement exécutant Windows Server (canal annuel un point-virgule), Windows Server2016 ou Windows Server2012R2.
- Mise à niveau d’un cluster d’espaces de stockage Direct vers Windows Server, version1709 n’est pas prise en charge.
- Si la charge de travail de cluster est ordinateurs virtuels Hyper-V ou un serveur de fichiers avec montée en puissance parallèle, vous pouvez vous attendre mise à niveau sans interruption de service.
- Vérifiez que les processeurs qui prennent en charge de Second niveau adressage Table (SLAT) à l’aide d’une des méthodes suivantes; les nœuds Hyper-V  
        -Passer en revue les [vous êtes Compatible SLAT? WP8 SDK Conseil 01](http://blogs.msdn.com/b/devfish/archive/2012/11/06/are-you-slat-compatible-wp8-sdk-tip-01.aspx) qui décrit les deux méthodes pour vérifier si un processeur prend en charge les lames  
        -Télécharger le [Coreinfo v3.31](https://technet.microsoft.com/sysinternals/cc835722) outil pour déterminer si un processeur compatible SLAT.

## <a name="cluster-transition-states-during-cluster-os-rolling-upgrade"></a>Changement d’état du cluster au cours de niveau propagée de Cluster du système d’exploitation

Cette section décrit les différents États de transition du cluster Windows Server2012R2 qui est mis à niveau vers Windows Server2016 à l’aide de niveau propagée de Cluster du système d’exploitation.  

Afin de maintenir les charges de travail de cluster en cours d’exécution pendant le processus de niveau propagée de Cluster du système d’exploitation, le déplacement d’une charge de travail de cluster à partir d’un nœud de Windows Server2012R2 vers Windows Server2016 nœud fonctionne comme si les deux nœuds ont été exécutant le système d’exploitation Windows Server2012R2. Lorsque Windows Server2016nœuds sont ajoutés au cluster, ils fonctionnent dans un mode de compatibilité de Windows Server2012R2. Un nouveau mode de cluster conceptuelle, appelé «Mode système d’exploitation», permet aux nœuds de différentes versions d’exister dans le même cluster (voir Figure1).  

![Image illustrant les trois phases d’une mise à niveau propagée du système d’exploitation de cluster: tous les nœuds de Windows Server2012R2, le mode système d’exploitation et tous les nœuds Windows Server2016](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_RollingUpgrade_Overview.png)  
**Figure1: Les transitions d’état système d’exploitation de Cluster**  

Un cluster Windows Server2012R2 en mode mixte-système d’exploitation lorsqu’un nœud de Windows Server2016 est ajouté au cluster. Le processus est entièrement réversible: Windows Server2016nœuds peuvent être supprimés du cluster et les nœuds de Windows Server2012R2 peuvent être ajoutés au cluster dans ce mode. «Point de non retour» se produit lorsque l’applet de commande PowerShell Update-ClusterFunctionalLevel est exécuté sur le cluster. Dans l’ordre de cette applet de commande réussisse, tous les nœuds doivent être Windows Server2016, et tous les nœuds doivent être en ligne.  

## <a name="transition-states-of-a-four-node-cluster-while-performing-rolling-os-upgrade"></a>États de transition d’un cluster à quatre nœuds lors de l’exécution du système d’exploitation mise à niveau propagée

Cette section illustre et décrit les différentes quatre étapes d’un cluster avec un stockage partagé dont les nœuds sont mis à niveau à partir de Windows Server2012R2 vers Windows Server2016.  

«Phase 1» est l’état initial: nous allons commencer avec un cluster Windows Server2012R2.  

![Image illustrant l’état initial: tous les nœuds de Windows Server2012R2](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage1.png)  
**La figure2: État Initial: Cluster de basculement Windows Server2012R2 (étape1)**  

Dans «étape2», les deux nœuds ont été suspendus, épuisées, éliminés, reformatés et installés avec Windows Server2016.  

![Image illustrant le cluster en mode mixte-système d’exploitation: du cluster de 4nœuds exemple, deux nœuds exécutent Windows Server2016, et deux nœuds exécutent Windows Server2012R2](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage2.png)  
**Figure3: Intermédiaire état: mode système d’exploitation: cluster de basculement Windows Server2016 et de Windows Server2012R2 (étape2)**  

À «étape3», tous les nœuds du cluster ont été mis à niveau vers Windows Server2016 et le cluster est prêt à être mis à niveau avec l’applet de commande PowerShell Update-ClusterFunctionalLevel.  

> [!NOTE]  
> À ce stade, le processus peut être inversé entièrement, et Windows Server2012R2 nœuds peuvent être ajoutées à ce cluster.  

![Illustration indiquant que le cluster a été mis à niveau vers Windows Server2016 et est prêt pour l’applet de commande Update-ClusterFunctionalLevel pour mettre le niveau fonctionnel du cluster jusqu'à Windows Server2016](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage3.png)  
**Figure4: Intermédiaire état: tous les nœuds mis à niveau vers Windows Server2016, prêt pour Update-ClusterFunctionalLevel (étape3)**  

Après que le Update-ClusterFunctionalLevelcmdlet est exécuté, le cluster saisit «Étape4», où les nouvelles fonctionnalités de cluster Windows Server2016 peuvent être utilisées.  

![Illustration indiquant que la mise à niveau du système d’exploitation propagée de cluster a réussi; tous les nœuds ont été mis à niveau vers Windows Server2016 et le cluster est en cours d’exécution au niveau fonctionnel de cluster Windows Server2016](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_Stage4.png)  
**La figure5: État Final: Cluster de basculement Windows Server2016 (étape4)**  

## <a name="cluster-os-rolling-upgrade-process"></a>Système d’exploitation de cluster propagée mise à niveau

Cette section décrit le flux de travail pour l’exécution à niveau propagée de Cluster du système d’exploitation.  

![Image illustrant le flux de travail de mise à niveau d’un cluster](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_RollingUpgrade_Workflow.png)  
**Figure6: Système d’exploitation de Cluster propagée workflow du processus de mise à niveau**  

Mise à niveau propagée du système d’exploitation des cluster comprend les étapes suivantes:  

1. Préparer le cluster de la mise à niveau du système d’exploitation comme suit:  
    1. Niveau propagée de cluster du système d’exploitation requiert la suppression d’un nœud à la fois le cluster. Vérifiez si vous disposez d’une capacité suffisante sur le cluster pour mettre à jour HA SLA lorsqu’un des nœuds de cluster est supprimé du cluster pour une mise à niveau du système d’exploitation. En d’autres termes, vous avez besoin à la possibilité de basculement des charges de travail vers un autre nœud lorsqu’un nœud est supprimé du cluster au cours du processus de niveau propagée de Cluster du système d’exploitation? Le cluster a la capacité d’exécuter les charges de travail nécessaires lorsqu’un nœud est supprimé du cluster de niveau propagée de Cluster du système d’exploitation?  
    2. Pour les charges de travail Hyper-V, vérifiez que tous les ordinateurs hôtes Windows Server2016 Hyper-V ont processeur prend en charge de la Table d’adresse de Second niveau (SLAT). Seuls les ordinateurs prenant en charge SLAT peuvent utiliser le rôle Hyper-V dans Windows Server2016.  
    3. Vérifiez que toutes les sauvegardes de charge de travail est terminé et envisagez de sauvegarder le cluster. Arrêter les opérations de sauvegarde lors de l’ajout de nœuds au cluster.  
    4. Vérifiez que tous les nœuds de cluster sont en ligne/en cours d’exécution/haut à l’aide de la [`Get-ClusterNode`](https://technet.microsoft.com/library/ee460990.aspx)applet de commande (voir Figure7).  

        ![ScreenCap illustrant les résultats de l’exécution de l’applet de commande Get-ClusterNode](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetClusterNode.png)  
        **Figure7: Déterminer l’état du nœud à l’aide d’applet de commande Get-ClusterNode**  

    5. Si vous exécutez des mises à jour prenant en charge de Cluster (CAU), vérifiez si adaptée aux clusters est en cours d’exécution à l’aide de la **mise à jour de Cluster-conscientes** l’interface utilisateur, ou le [`Get-CauRun`](https://technet.microsoft.com/library/hh847230.aspx)applet de commande (voir Figure8). Arrêter adaptée à l’aide de la [`Disable-CauClusterRole`](https://technet.microsoft.com/library/hh847219.aspx)applet de commande (voir Figure9) pour empêcher tous les nœuds à partir de la suspension et épuisée par adaptée aux clusters pendant le processus de niveau propagée de Cluster du système d’exploitation.  

        ![ScreenCap montrant la sortie de l’applet de commande Get-CauRun](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetCAU.png)  
        **Figure8: À l’aide de la [`Get-CauRun`](https://technet.microsoft.com/library/hh847230.aspx)applet de commande pour déterminer si les mises à jour adaptée aux Cluster s’exécute sur le cluster**  

        ![ScreenCap montrant la sortie de l’applet de commande Disable-CauClusterRole](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_DisableCAU.png)  
        **La figure9: La désactivation le rôle de mises à jour adaptée aux Cluster à l’aide de la [`Disable-CauClusterRole`](https://technet.microsoft.com/library/hh847219.aspx)applet de commande**  

2. Pour chaque nœud du cluster, effectuez les opérations suivantes:  
    1. À l’aide du Gestionnaire du Cluster UI, sélectionnez un nœud et utilisez le **Pause | Drainer** option de menu pour décharger le nœud (voir Figure10) ou utilisez le [`Suspend-ClusterNode`](https://technet.microsoft.com/library/ee461051.aspx)applet de commande (voir Figure11).  

        ![ScreenCap montrant comment décharger des rôles avec le Gestionnaire du Cluster UI](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_FCM_DrainRoles.png)  
        **Figure10: Drainage des rôles à partir d’un nœud à l’aide du Gestionnaire du Cluster de basculement**  

        ![ScreenCap montrant la sortie de l’applet de commande Suspend-ClusterNode](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_SuspendNode.png)  
        **Figure11: Drainage des rôles à partir d’un nœud à l’aide de la [`Suspend-ClusterNode`](https://technet.microsoft.com/library/ee461051.aspx)applet de commande**  

    2.  À l’aide du Gestionnaire du Cluster UI, **nœuds** le nœud en pause de cluster, ou utilisez le [`Remove-ClusterNode`](https://technet.microsoft.com/library/ee461001.aspx)applet de commande.  

        ![ScreenCap montrant la sortie de l’applet de commande Remove-ClusterNode](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_RemoveNode.png)  
        **Figure12: Supprimer un nœud du cluster à l’aide [`Remove-ClusterNode`](https://technet.microsoft.com/library/ee461001.aspx)applet de commande**  

    3.  Reformater le lecteur du système et d’effectuer une installation «nettoyer le système d’exploitation» de Windows Server2016 sur le nœud à l’aide de la **personnalisé: installer uniquement Windows (Avancé)** option d’installation (voir Figure13) de setup.exe. Évitez de sélectionner le **mise à niveau: Windows Installer et conserver les fichiers, les paramètres et applications** option dans la mesure où niveau propagée de Cluster du système d’exploitation n’encourager mise à niveau sur place.  

        ![ScreenCap de l’Assistant installation de Windows Server2016 montrant l’option d’installation personnalisée sélectionnée](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_InstallOption.png)  
        **Figure13: Options d’installation disponibles pour Windows Server2016**  

    4.  Ajouter le nœud au domaine ActiveDirectory approprié.  
    5.  Ajoutez les utilisateurs appropriés au groupe Administrateurs.  
    6.  À l’aide de l’applet de commande UI Gestionnaire de serveur ou PowerShell Install-WindowsFeature, installer des rôles de serveur dont vous avez besoin, telles que Hyper-V.  

        ```PowerShell
        Install-WindowsFeature -Name Hyper-V  
        ```  

    7.  À l’aide de l’applet de commande UI Gestionnaire de serveur ou PowerShell Install-WindowsFeature, installez la fonctionnalité de Clustering de basculement.  

        ```PowerShell
        Install-WindowsFeature -Name Failover-Clustering  
        ```  

    8.  Installer les fonctionnalités supplémentaires requises par vos charges de travail de cluster.  
    9. Vérifiez les paramètres de connectivité réseau et de stockage à l’aide de l’UI Gestionnaire du Cluster de basculement.  
    10. Si le pare-feu Windows est utilisé, vérifiez que les paramètres de pare-feu sont correctes pour le cluster. Par exemple, les clusters de Cluster en charge la mise à jour (adaptée aux clusters) activé peuvent nécessiter une configuration pare-feu.  
    11. Pour les charges de travail Hyper-V, utilisez l’interface utilisateur de Gestionnaire Hyper-V pour lancer la boîte de dialogue Gestionnaire de commutateur virtuel (voir Figure14).  

        Vérifiez que le nom des commutateurs virtuels utilisés sont identiques pour tous les nœuds d’ordinateur hôte Hyper-V dans le cluster.  

        ![ScreenCap indiquant l’emplacement de la boîte de dialogue Gestionnaire de commutateur virtuel Hyper-V](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_VMSwitch.png)  
        **Figure14: Gestionnaire de commutateur virtuel**  

    12. Sur un nœud de Windows Server2016 (n’utilisez pas un nœud de Windows Server2012R2), utilisez le Gestionnaire du Cluster de basculement (voir Figure15) pour se connecter au cluster.  

        ![ScreenCap montrant la boîte de dialogue Sélectionnez cluster](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_AddNode.png)  
        **Figure15: Ajout d’un nœud au cluster à l’aide du Gestionnaire du Cluster de basculement**  

    13. Utilisez soit l’UI de gestionnaire du Cluster de basculement ou le [`Add-ClusterNode`](https://technet.microsoft.com/library/ee461047.aspx)applet de commande (voir Figure16) pour ajouter le nœud au cluster.  

        ![ScreenCap montrant la sortie de l’applet de commande Add-ClusterNode](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_AddNode3.png)  
        **La figure16: Ajout d’un nœud au cluster à l’aide [`Add-ClusterNode`](https://technet.microsoft.com/library/ee461047.aspx)applet de commande**  

        > [!NOTE]  
        > Lorsque le premier nœud de Windows Server2016 rejoint le cluster, le cluster en mode «Mixte-système d’exploitation», et les principales ressources de cluster sont déplacés vers le nœud de Windows Server2016. Un cluster de mode «Mixtes-système d’exploitation» est un entièrement fonctionnel où les nouveaux nœuds exécutent dans un mode de compatibilité avec les nœuds anciens. Mode «Mixtes-système d’exploitation» est un mode transitoire pour le cluster. Il n’est pas destinée à être permanente et clients doivent mettre à jour tous les nœuds du cluster de leur dans les quatre semaines.  

    14. Une fois Windows Server2016les nœud est correctement ajoutés au cluster, vous pourrez (éventuellement) transférer certains de la charge de travail de cluster vers le nœud qui vient d’être ajouté afin de rééquilibrer les charges de travail au sein du cluster, comme suit:

        ![ScreenCap montrant la sortie de l’applet de commande Move-ClusterVirtualMachineRole](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_MoveVMRole.png)  
        **La figure17: Déplacement d’une charge de travail (rôle de machine virtuelle de cluster) de cluster à l’aide [`Move-ClusterVirtualMachineRole`](https://technet.microsoft.com/library/ee461041.aspx)applet de commande**  

        1. Utilisez **Live Migration** à partir du Gestionnaire du Cluster de basculement pour les ordinateurs virtuels ou les [`Move-ClusterVirtualMachineRole`](https://technet.microsoft.com/library/ee461041.aspx)applet de commande (voir Figure17) pour effectuer une migration dynamique des ordinateurs virtuels.  

            ```PowerShell
            Move-ClusterVirtualMachineRole -Name VM1 -Node robhind-host3  
            ```  

        2. Utilisez **déplacer** à partir du Gestionnaire du Cluster de basculement ou le [`Move-ClusterGroup`](https://technet.microsoft.com/library/ee461002.aspx)applet de commande pour les autres charges de travail de cluster.  

3. Lors de chaque nœud a été mis à niveau vers Windows Server2016 et ajouté au cluster, ou lorsque les nœuds restants de Windows Server2012R2 ont été supprimés, procédez comme suit:  

    > [!IMPORTANT]  
    > -   Une fois que vous mettez à jour le niveau fonctionnel du cluster, vous ne pouvez pas revenir au niveau fonctionnel de Windows Server2012R2 et Windows Server2012R2 nœuds ne peuvent pas être ajoutés au cluster.
    > -   Jusqu'à ce que le [`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx)applet de commande est exécutée, le processus est entièrement réversible et Windows Server2012R2 nœuds peuvent être ajoutées à ce cluster Windows Server2016nœuds peuvent être supprimés.  
    > -   Après le [`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx)applet de commande est exécutée, de nouvelles fonctionnalités seront disponibles.  

    1.  À l’aide de l’UI Gestionnaire du Cluster de basculement ou le [`Get-ClusterGroup`](https://technet.microsoft.com/library/ee461017.aspx)applet de commande, vérifiez que tous les rôles de cluster sont en cours d’exécution sur le cluster comme prévu. Dans l’exemple suivant, stockage disponible n'est pas utilisé, à la place des volumes partagés de cluster est utilisé, par conséquent, le stockage disponible affiche un **hors connexion** état (voir Figure18).  

        ![ScreenCap montrant la sortie de l’applet de commande Get-ClusterGroup](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_GetClusterGroup.png)  
        **Figure18: Vérifier que tous les clusters de groupes (rôles de cluster) sont en cours d’exécution à l’aide de la [`Get-ClusterGroup`](https://technet.microsoft.com/library/ee461017.aspx)applet de commande**  

    2.  Vérifiez que tous les nœuds de cluster sont en ligne et en cours d’exécution à l’aide de la [`Get-ClusterNode`](https://technet.microsoft.com/library/ee460990.aspx)applet de commande.  
    3.  Exécutez le [`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx)applet de commande - aucune erreur ne doit être retourné (voir Figure19).  

        ![ScreenCap montrant la sortie de l’applet de commande Update-ClusterFunctionalLevel](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_SelectFunctionalLevel.png)  
        **La figure19: Mise à jour le niveau fonctionnel d’un cluster à l’aide de PowerShell**  

    4.  Après le [`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx)applet de commande est exécutée, les nouvelles fonctionnalités sont disponibles.  

4. Windows Server2016 - reprendre des sauvegardes et des mises à jour normales de cluster:  

    1. Si vous avez précédemment exécuté adaptée aux clusters, redémarrez à l’aide de l’UI CAU ou utilisez le [`Enable-CauClusterRole`](https://technet.microsoft.com/library/hh847229.aspx)applet de commande (voir Figure20).  

        ![ScreenCap montrant la sortie de la Enable-CauClusterRole](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_EnableCAUClusterRole.png)  
        **Figure20: À l’aide de rôle Enable Cluster prenant en charge les mises à jour le [`Enable-CauClusterRole`](https://technet.microsoft.com/library/hh847229.aspx)applet de commande**  

    2. Reprendre les opérations de sauvegarde.  

5. Activer et utiliser les fonctionnalités de Windows Server2016 sur les Machines virtuelles Hyper-V.  

    1. Une fois que le cluster a été mis à niveau au niveau fonctionnel de Windows Server2016, nombreuses charges de travail comme des ordinateurs virtuels Hyper-V auront les nouvelles fonctionnalités. Pour obtenir une liste de nouvelles fonctionnalités Hyper-V. Voir [migration et mise à niveau les ordinateurs virtuels](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/migrating_vms)  

    2. Sur chaque nœud d’ordinateur hôte Hyper-V dans le cluster, utilisez le [`Get-VMHostSupportedVersion`](https://technet.microsoft.com/library/mt653838.aspx)applet de commande pour afficher les versions de configuration de machine virtuelle Hyper-V qui sont pris en charge par l’hôte.  

        ![ScreenCap montrant la sortie de l’applet de commande Get-VMHostSupportedVersion](media/Cluster-Operating-System-Rolling-Upgrade/Clustering_GetVMHostSupportVersion.png)  
        **Figure21: Afficher les versions de configuration d’ordinateur virtuel Hyper-V pris en charge par l’ordinateur hôte**  

   3.  Sur chaque nœud d’ordinateur hôte Hyper-V dans le cluster, les versions de configuration de machine virtuelle Hyper-V peuvent être mis à niveau en planifiant une fenêtre de maintenance brève avec des utilisateurs, sauvegarde, la désactivation des ordinateurs virtuels et en cours d’exécution le [`Update-VMVersion`](https://technet.microsoft.com/library/mt484146.aspx)applet de commande (voir Figure22). Cette mise à jour de la version de l’ordinateur virtuel et activer de nouvelles fonctionnalités Hyper-V, en éliminant la nécessité pour les futures mises à jour des composants d’intégration Hyper-V (IC). Cette applet de commande peut être exécuté à partir du nœud Hyper-V qui héberge l’ordinateur virtuel, ou le `-ComputerName`paramètre peut être utilisé pour mettre à jour la Version de l’ordinateur virtuel à distance. Dans cet exemple, ici nous mettre à niveau la version de configuration de VM1 5.0 7.0 afin de tirer parti de nombreuses nouvelles fonctionnalités d’Hyper-V associé à cette version de configuration de machine virtuelle tels que les points de contrôle de Production (sauvegardes cohérentes d’Application) et fichier binaire de la configuration de machine virtuelle.  

        ![ScreenCap illustrant l’applet de commande Update-VMVersion en action](media/Cluster-Operating-System-Rolling-Upgrade/Cluster_RollingUpgrade_StopVM.png)  
        **La figure22: La mise à niveau une version de l’ordinateur virtuel à l’aide de l’applet de commande PowerShell Update-VMVersion**  

4.  Pools de stockage peuvent être mis à niveau à l’aide de la [mise à jour-StoragePool](https://technet.microsoft.com/itpro/powershell/windows/storage/update-storagepool) applet de commande PowerShell - il s’agit d’une opération en ligne.  

Bien que nous ciblons scénarios de Cloud privé, en particulier Hyper-V et les clusters de serveur de fichiers avec montée en puissance parallèle, ce qui peuvent être mis à niveau sans temps d’arrêt, le processus de niveau propagée de Cluster du système d’exploitation peuvent être utilisés pour un rôle de cluster.  

## <a name="restrictions--limitations"></a>Restrictions / Limitations  
- Cette fonctionnalité fonctionne uniquement pour Windows Server2012R2 pour les versions de Windows Server2016 uniquement. Cette fonctionnalité ne peut pas mettre à niveau des versions antérieures de Windows Server, tels que Windows Server2008, Windows Server2008R2 ou Windows Server2012vers Windows Server2016.  
- Chaque nœud de Windows Server2016 doit être reformaté/nouvelle installation uniquement. «Sur place» ou «mise à niveau» type d’installation est déconseillé.  
- Un nœud de Windows Server2016 doit être utilisé pour ajouter des nœuds de Windows Server2016 pour le cluster.  
- Lorsque vous gérez un cluster en mode mixte-système d’exploitation, vous devez toujours effectuer les tâches de gestion à partir d’un nœud de niveau supérieur qui exécute Windows Server2016. Nœuds de niveau inférieur Windows Server2012R2 ne peut pas utiliser les outils de gestion ou de l’interface utilisateur par rapport à Windows Server2016.  
- Nous encourageons vivement à déplacer rapidement à travers le processus de mise à niveau du cluster, car certaines fonctionnalités de cluster ne sont pas optimisées pour le mode système d’exploitation.  
- Évitez de créer ou de redimensionnement de stockage sur les nœuds de Windows Server2016tandis que le cluster est en cours d’exécution en mode mixte-système d’exploitation en raison d’incompatibilités lors du basculement d’un nœud de Windows Server2016aux nœuds de Windows Server2012R2 de bas niveau.  

## <a name="frequently-asked-questions"></a>Forum aux questions  
**La durée pendant laquelle le cluster de basculement exécutables en mode mixte-système d’exploitation?**  
    Nous invitons les clients à effectuer la mise à niveau dans les quatre semaines. Il existe de nombreuses optimisations dans Windows Server2016. Nous avons mis à niveau Hyper-V et des clusters de serveur de fichiers avec montée en puissance parallèle sans interruption de service dans le nombre total de moins de quatre heures.  

**Vous le portage de cette fonctionnalité vers Windows Server2012, Windows Server2008R2 ou Windows Server2008?**  
    Nous n’avons pas tous les plans de portage de cette fonctionnalité sur les versions précédentes. Niveau propagée de cluster du système d’exploitation est notre vision de la mise à niveau de clusters Windows Server2012R2 à Windows Server2016 et plus.  

**Le cluster Windows Server2012R2 doit-elle toutes les mises à jour de logiciel installé avant de commencer le processus de niveau propagée de Cluster du système d’exploitation?**  
    Oui, avant de commencer le processus de niveau propagée de Cluster du système d’exploitation, vérifiez que tous les nœuds de cluster sont mis à jour avec les dernières mises à jour logicielles.  

**Puis-je exécuter la [`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx)applet de commande pendant que les nœuds sont désactivés ou en pause?**  
    Non. Tous les nœuds de cluster doivent être sur et d’appartenance active pour la [`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx)applet de commande fonctionne.  

**Niveau propagée de Cluster du système d’exploitation fonctionne pour les charges de travail de cluster? Il fonctionne pour SQLServer?**  
    Oui, niveau propagée de Cluster du système d’exploitation fonctionne pour les charges de travail de cluster. Toutefois, il est uniquement sans interruption de service pour Hyper-V et les clusters de serveurs de fichiers avec montée en puissance parallèle. La plupart des autres charges de travail subir un temps d’inactivité (en général quelques minutes) lorsqu’ils basculement et le basculement est nécessaire au moins une fois pendant le processus de niveau propagée de Cluster du système d’exploitation.  

**Puis-je automatiser ce processus à l’aide de PowerShell?**  
    Oui, nous avons conçu de Cluster du système d’exploitation mise à niveau propagée être automatisée à l’aide de PowerShell.  

**Pour un cluster volumineux qui dispose de la charge de travail supplémentaire et la capacité de basculement, puis-je mettre à niveau plusieurs nœuds simultanément?**  
    Oui. Lorsque vous retirez un nœud du cluster pour mettre à niveau le système d’exploitation, le cluster ont un seul nœud dédié pour le basculement, donc aura une capacité de basculement réduite. Pour les clusters volumineux avec suffisamment de charge de travail et la capacité de basculement, plusieurs nœuds peuvent être mis à niveau simultanément. Vous pouvez temporairement ajouter des nœuds de cluster pour le cluster pour fournir la charge de travail amélioré et la capacité de basculement pendant le processus de niveau propagée de Cluster du système d’exploitation.  

**Que se passe-t-il si je découvre un problème dans mon cluster après [`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx)a été exécuté avec succès?**  
    Si vous avez sauvegardé la base de données de cluster avec une sauvegarde de l’état du système avant d’exécuter [`Update-ClusterFunctionalLevel`](https://technet.microsoft.com/library/mt589702.aspx), vous devez être en mesure d’effectuer une faisant autorité restaurer sur un nœud de cluster Windows Server2012R2 et restaurer la base de données de cluster d’origine et la configuration.  

**Puis-je utiliser la mise à niveau sur place pour chaque nœud au lieu d’utiliser l’installation du système d’exploitation nettoyer par reformater le lecteur du système?**  
    Nous ne pas encourager l’utilisation de mise à niveau sur place de Windows Server, mais nous sommes conscients qu’il fonctionne dans certains cas où les pilotes par défaut sont utilisées. Veuillez lire attentivement tous les messages d’avertissement affichent au cours de la mise à niveau sur place d’un nœud de cluster.  

**Si j’utilise la réplication Hyper-V pour un ordinateur virtuel Hyper-V sur mon cluster Hyper-V, réplication resteront intacte pendant et après le processus de niveau propagée de Cluster du système d’exploitation?**  
    Oui, la réplication Hyper-V est conservé pendant et après le processus de niveau propagée de Cluster du système d’exploitation.  

**Puis-je utiliser SystemCenter2016 Virtual Machine Manager (SCVMM) pour automatiser le processus de niveau propagée de Cluster du système d’exploitation?**  
    Oui, vous pouvez automatiser le processus de niveau propagée de Cluster du système d’exploitation à l’aide de VMM dans SystemCenter2016.  

## <a name="see-also"></a>Voir aussi  
-   [Notes de publication: Points importants dans Windows Server2016](../get-started/Release-Notes--Important-Issues-in-Windows-Server-2016-Technical-Preview.md)  
-   [Quelles sont les nouveautés dans Windows Server2016](../get-started/What-s-New-in-windows-server-2016.md)  
-   [Nouveautés du Clustering de basculement dans Windows Server](whats-new-in-failover-clustering.md)  