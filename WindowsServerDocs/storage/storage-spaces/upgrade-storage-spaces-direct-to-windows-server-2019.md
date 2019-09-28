---
title: Mettre à niveau un cluster espaces de stockage direct vers Windows Server 2019
description: Comment mettre à niveau un cluster espaces de stockage direct vers Windows Server 2019, tout en conservant les machines virtuelles en cours d’exécution ou pendant qu’elles sont arrêtées.
author: robhindman
ms.author: robhind
manager: eldenc
ms.date: 03/06/2019
ms.topic: article
ms.prod: windows-server
ms.technology: storage-spaces
ms.openlocfilehash: 09a1116b7fc1b1d0f8bc144ba9c4cf68fc45697e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402785"
---
# <a name="upgrade-a-storage-spaces-direct-cluster-to-windows-server-2019"></a>Mettre à niveau un cluster espaces de stockage direct vers Windows Server 2019

Cette rubrique explique comment mettre à niveau un cluster espaces de stockage direct vers Windows Server 2019. Il existe quatre approches pour mettre à niveau un cluster espaces de stockage direct de Windows Server 2016 vers Windows Server 2019, à l’aide du [processus de mise à niveau propagée du système d’exploitation du cluster](../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md) , à savoir deux qui impliquent de maintenir les machines virtuelles en cours d’exécution, et deux qui impliquent l’arrêt de toutes les machines Chaque approche présente des points forts et des faiblesses différents. Sélectionnez donc celle qui répond le mieux aux besoins de votre organisation :

- **Mise à niveau sur place lorsque des machines virtuelles sont en cours d’exécution** sur chaque serveur du cluster : cette option n’entraîne aucun temps d’arrêt de la machine virtuelle, mais vous devez attendre la fin des tâches de stockage (réparation en miroir) après la mise à niveau de chaque serveur.

- **Installation Clean-OS lorsque des machines virtuelles sont en cours d’exécution** sur chaque serveur du cluster : cette option n’entraîne aucun temps d’arrêt de la machine virtuelle, mais vous devez attendre la fin des tâches de stockage (réparation en miroir) après la mise à niveau de chaque serveur et configurer chaque serveur et tous ses les applications et les rôles.

- **Mise à niveau sur place alors que les machines virtuelles sont arrêtées** sur chaque serveur du cluster : cette option entraîne un temps d’arrêt de la machine virtuelle, mais vous n’avez pas besoin d’attendre les tâches de stockage (réparation en miroir), et c’est donc plus rapide.

- **Installation Clean-OS lorsque des machines virtuelles sont arrêtées** sur chaque serveur du cluster : cette option entraîne un temps d’arrêt de la machine virtuelle, mais vous n’avez pas besoin d’attendre les tâches de stockage (réparation en miroir) pour accélérer l’opération.

## <a name="prerequisites-and-limitations"></a>Conditions préalables et limitations

Avant de procéder à une mise à niveau :

- Vérifiez que vous disposez de sauvegardes utilisables en cas de problème pendant le processus de mise à niveau.

- Vérifiez que votre fournisseur de matériel possède un BIOS, un microprogramme et des pilotes pour vos serveurs qu’il prendra en charge sur Windows Server 2019.

Le processus de mise à niveau présente certaines limitations à connaître :

- Pour permettre la espaces de stockage direct avec les versions de Windows Server 2019 antérieures à 176693,292, les clients devront peut-être contacter le support technique de Microsoft pour obtenir les clés de Registre qui activent les espaces de stockage direct et les fonctionnalités de mise en réseau définies par logiciel. Pour plus d’informations, consultez [l’article 4464776](https://support.microsoft.com/help/4464776/software-defined-data-center-and-software-defined-networking)de la base de connaissances Microsoft.

- Lors de la mise à niveau d’un cluster avec des volumes ReFS, il existe quelques limitations :

- La mise à niveau est entièrement prise en charge sur les volumes ReFS. Toutefois, les volumes mis à niveau ne tirent pas parti des améliorations apportées à ReFS dans Windows Server 2019. Ces avantages, tels que l’amélioration des performances pour la parité avec accélération miroir, nécessitent un volume Windows Server 2019 ReFS récemment créé. En d’autres termes, vous devez créer des volumes à l’aide `New-Volume` de l’applet de commande ou gestionnaire de serveur. Voici quelques-unes des améliorations apportées par les références aux nouveaux volumes :

    - **Mapper le journal-contournement**: amélioration des performances dans les références qui s’applique uniquement aux systèmes en cluster (espaces de stockage direct) et qui ne s’applique pas aux pools de stockage autonomes.

    - Améliorations de l’efficacité de **compactage** dans Windows Server 2019 qui sont spécifiques aux volumes multi-résilients.

- Avant de mettre à niveau un serveur de cluster Windows Server 2016 espaces de stockage direct, nous vous recommandons de placer le serveur en mode de maintenance du stockage. Pour plus d’informations, consultez la section Event 5120 de [Troubleshoot espaces de stockage direct](troubleshooting-storage-spaces.md). Bien que ce problème ait été résolu dans Windows Server 2016, nous vous recommandons de placer chaque espaces de stockage direct Server en mode de maintenance du stockage au cours de la mise à niveau, comme meilleure pratique.

- Il existe un problème connu avec les environnements réseau définis par logiciel qui utilisent des commutateurs SET. Ce problème concerne les migrations dynamiques des machines virtuelles Hyper-V de Windows Server 2019 vers Windows Server 2016 (migration dynamique vers un système d’exploitation antérieur). Pour garantir la réussite des migrations dynamiques, nous vous recommandons de modifier un paramètre de réseau de machines virtuelles sur les machines virtuelles en cours de migration de Windows Server 2019 vers Windows Server 2016. Ce problème est résolu pour Windows Server 2019 dans le package de correctifs de correctifs 2019-01D, également appelé Build 17763,292. Pour plus d’informations, consultez [l’article 4476976](https://support.microsoft.com/help/4476976/windows-10-update-kb4476976)de la base de connaissances Microsoft.

En raison des problèmes connus ci-dessus, certains clients peuvent envisager de créer un nouveau cluster Windows Server 2019 et de copier des données à partir de l’ancien cluster, au lieu de mettre à niveau leurs clusters Windows Server 2016 à l’aide de l’un des quatre processus décrits ci-dessous.

## <a name="performing-an-in-place-upgrade-while-vms-are-running"></a>Exécution d’une mise à niveau sur place pendant l’exécution de machines virtuelles

Cette option n’entraîne aucun temps d’arrêt de la machine virtuelle, mais vous devez attendre la fin des tâches de stockage (réparation en miroir) après la mise à niveau de chaque serveur. Bien que des serveurs individuels soient redémarrés séquentiellement pendant le processus de mise à niveau, les autres serveurs du cluster, ainsi que toutes les machines virtuelles, restent en cours d’exécution.

1. Vérifiez que tous les serveurs du cluster ont installé les dernières mises à jour de Windows. Pour plus d’informations, consultez [l’historique des mises à jour Windows 10 et Windows Server 2016](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history). Au minimum, installez [l’article 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) de la base de connaissances Microsoft (19 février, 2019). Le numéro de build ( `ver` voir commande) doit être 14393,2828 ou une version ultérieure.

2. Si vous utilisez la mise en réseau définie par logiciel avec les commutateurs SET, ouvrez une session PowerShell avec élévation de privilèges et exécutez la commande suivante pour désactiver les contrôles de vérification de la migration dynamique de machine virtuelle sur toutes les machines virtuelles sur le cluster :

   ```PowerShell
   Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" |
   Set-ClusterParameter -Create SkipMigrationDestinationCheck -Value 1
   ```

3. Effectuez les étapes suivantes sur un serveur de cluster à la fois :

   1. Utilisez la migration dynamique des machines virtuelles Hyper-V pour déplacer les machines virtuelles en cours d’exécution sur le serveur que vous allez mettre à niveau.

   2. Suspendez le serveur de cluster à l’aide de la commande PowerShell suivante : Notez que certains groupes internes sont masqués. Nous vous recommandons d’effectuer cette étape avec prudence : Si vous n’avez pas encore migré les machines virtuelles à partir du serveur, cette applet de commande le fera pour vous, vous pouvez donc ignorer l’étape précédente si vous préférez.

        ```PowerShell
       Suspend-ClusterNode -Drain
       ```

   3. Placez le serveur en mode maintenance du stockage en exécutant les commandes PowerShell suivantes :

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Enable-StorageMaintenanceMode
       ```

   4. Exécutez l’applet de commande suivante pour vérifier que la valeur **OperationalStatus** est **en mode maintenance**:

       ```PowerShell
       Get-PhysicalDisk
       ```

   5. Effectuez une installation de mise à niveau de Windows Server 2019 sur le serveur en exécutant le **fichier Setup. exe** et en utilisant l’option « conserver les fichiers et les applications personnels ». Une fois l’installation terminée, le serveur reste dans le cluster et le service de cluster démarre automatiquement.

   6. Vérifiez que le serveur qui vient d’être mis à niveau dispose des dernières mises à jour de Windows Server 2019. Pour plus d’informations, consultez [l’historique des mises à jour Windows 10 et Windows Server 2019](https://support.microsoft.com/help/4464619/windows-10-update-history). Le numéro de build ( `ver` voir commande) doit être 17763,292 ou une version ultérieure.

   7. Supprimez le serveur du mode de maintenance du stockage à l’aide de la commande PowerShell suivante :

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Disable-StorageMaintenanceMode
       ```

   8. Reprenez le serveur à l’aide de la commande PowerShell suivante :

       ```PowerShell
       Resume-ClusterNode
       ```

   9. Attendez la fin des tâches de réparation de stockage et pour que tous les disques reviennent à un état sain. Cela peut prendre beaucoup de temps en fonction du nombre de machines virtuelles en cours d’exécution pendant la mise à niveau du serveur. Voici les commandes à exécuter :

       ```PowerShell
       Get-StorageJob
       Get-VirtualDisk
       ```

4. Mettez à niveau le serveur suivant du cluster.

5. Une fois que tous les serveurs ont été mis à niveau vers Windows Server 2019, utilisez l’applet de commande PowerShell suivante pour mettre à jour le niveau fonctionnel du cluster.

   ```PowerShell
   Update-ClusterFunctionalLevel
   ```

   > [!NOTE]
   > Nous vous recommandons de mettre à jour le niveau fonctionnel du cluster dès que possible, bien que techniquement vous ayez jusqu’à quatre semaines pour le faire.

6. Une fois le niveau fonctionnel du cluster mis à jour, utilisez l’applet de commande suivante pour mettre à jour le pool de stockage. À ce stade, de nouvelles applets de `Get-ClusterPerf` commande, telles que, sont entièrement opérationnelles sur n’importe quel serveur du cluster.

   ```PowerShell
   Update-StoragePool
   ```

7. Vous pouvez éventuellement mettre à niveau des niveaux de configuration de machine virtuelle `Update-VMVersion` en arrêtant chaque machine virtuelle, en utilisant l’applet de commande, puis en redémarrant les machines virtuelles.

8. Si vous utilisez la mise en réseau définie par logiciel avec des commutateurs SET et des contrôles de migration dynamique de machine virtuelle désactivés comme indiqué ci-dessus, utilisez l’applet de commande suivante pour réactiver les contrôles de vérification dynamique de machine virtuelle :

   ```PowerShell
   Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" |
   Set-ClusterParameter  SkipMigrationDestinationCheck -Value 0
   ```

9. Vérifiez que le cluster mis à niveau fonctionne comme prévu. Les rôles doivent basculer correctement et si la migration dynamique de machine virtuelle est utilisée sur le cluster, les machines virtuelles doivent réussir la migration.

10. Validez le cluster en exécutant la validation`Test-Cluster`de cluster () et en examinant le rapport de validation de cluster.

## <a name="performing-a-clean-os-installation-while-vms-are-running"></a>Exécution d’une installation propre du système d’exploitation pendant l’exécution des machines virtuelles

Cette option n’entraîne aucun temps d’arrêt de la machine virtuelle, mais vous devez attendre la fin des tâches de stockage (réparation en miroir) après la mise à niveau de chaque serveur. Bien que des serveurs individuels soient redémarrés séquentiellement pendant le processus de mise à niveau, les autres serveurs du cluster, ainsi que toutes les machines virtuelles, restent en cours d’exécution.

1. Vérifiez que tous les serveurs du cluster exécutent les dernières mises à jour. Pour plus d’informations, consultez [l’historique des mises à jour Windows 10 et Windows Server 2016](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history). Au minimum, installez [l’article 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) de la base de connaissances Microsoft (19 février, 2019). Le numéro de build ( `ver` voir commande) doit être 14393,2828 ou une version ultérieure.

2. Si vous utilisez la mise en réseau définie par logiciel avec les commutateurs SET, ouvrez une session PowerShell avec élévation de privilèges et exécutez la commande suivante pour désactiver les contrôles de vérification de la migration dynamique de machine virtuelle sur toutes les machines virtuelles sur le cluster :

   ```PowerShell
   Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" |
   Set-ClusterParameter -Create SkipMigrationDestinationCheck -Value 1
   ```

3. Effectuez les étapes suivantes sur un serveur de cluster à la fois :

   1. Utilisez la migration dynamique des machines virtuelles Hyper-V pour déplacer les machines virtuelles en cours d’exécution sur le serveur que vous allez mettre à niveau.

   2. Suspendez le serveur de cluster à l’aide de la commande PowerShell suivante : Notez que certains groupes internes sont masqués. Nous vous recommandons d’effectuer cette étape avec prudence : Si vous n’avez pas encore migré les machines virtuelles à partir du serveur, cette applet de commande le fera pour vous, vous pouvez donc ignorer l’étape précédente si vous préférez.

        ```PowerShell
       Suspend-ClusterNode -Drain
       ```

   3. Placez le serveur en mode maintenance du stockage en exécutant les commandes PowerShell suivantes :

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Enable-StorageMaintenanceMode
       ```

   4. Exécutez l’applet de commande suivante pour vérifier que la valeur **OperationalStatus** est **en mode maintenance**:

       ```PowerShell
       Get-PhysicalDisk
       ```

   3.  Supprimez le serveur du cluster en exécutant la commande PowerShell suivante :  

       ```PowerShell
       Remove-ClusterNode <ServerName>
       ```

   4. Effectuez une nouvelle installation de Windows Server 2019 sur le serveur : formatez le lecteur système, exécutez **Setup. exe** et utilisez l’option « Nothing ». Vous devez configurer l’identité, les rôles, les fonctionnalités et les applications du serveur une fois l’installation terminée et le serveur redémarré.

   5. Installez le rôle Hyper-V et la fonctionnalité de clustering de basculement sur le serveur (vous `Install-WindowsFeature` pouvez utiliser l’applet de commande).

   6. Installez les derniers pilotes de réseau et de stockage pour votre matériel qui sont approuvés par le fabricant de votre serveur pour une utilisation avec espaces de stockage direct.

   7. Vérifiez que le serveur qui vient d’être mis à niveau dispose des dernières mises à jour de Windows Server 2019. Pour plus d’informations, consultez [l’historique des mises à jour Windows 10 et Windows Server 2019](https://support.microsoft.com/help/4464619/windows-10-update-history). Le numéro de build ( `ver` voir commande) doit être 17763,292 ou une version ultérieure.

   8. Rattachez le serveur au cluster à l’aide de la commande PowerShell suivante :

       ```PowerShell
       Add-ClusterNode
       ```

   9. Supprimez le serveur du mode de maintenance du stockage à l’aide des commandes PowerShell suivantes :

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Disable-StorageMaintenanceMode
       ```

   10. Attendez la fin des tâches de réparation de stockage et pour que tous les disques reviennent à un état sain. Cela peut prendre beaucoup de temps en fonction du nombre de machines virtuelles en cours d’exécution pendant la mise à niveau du serveur. Voici les commandes à exécuter :

        ```PowerShell
        Get-StorageJob
        Get-VirtualDisk
        ```

4. Mettez à niveau le serveur suivant du cluster.

5. Une fois que tous les serveurs ont été mis à niveau vers Windows Server 2019, utilisez l’applet de commande PowerShell suivante pour mettre à jour le niveau fonctionnel du cluster.
    
   ```PowerShell
   Update-ClusterFunctionalLevel
   ```

   > [!NOTE]
   > Nous vous recommandons de mettre à jour le niveau fonctionnel du cluster dès que possible, bien que techniquement vous ayez jusqu’à quatre semaines pour le faire.

6. Une fois le niveau fonctionnel du cluster mis à jour, utilisez l’applet de commande suivante pour mettre à jour le pool de stockage. À ce stade, de nouvelles applets de `Get-ClusterPerf` commande, telles que, sont entièrement opérationnelles sur n’importe quel serveur du cluster.

   ```PowerShell
   Update-StoragePool
   ```

7. Vous pouvez éventuellement mettre à niveau des niveaux de configuration de machine virtuelle `Update-VMVersion` en arrêtant chaque machine virtuelle et en utilisant l’applet de commande, puis en redémarrant les machines virtuelles.

8. Si vous utilisez la mise en réseau définie par logiciel avec des commutateurs SET et des contrôles de migration dynamique de machine virtuelle désactivés comme indiqué ci-dessus, utilisez l’applet de commande suivante pour réactiver les contrôles de vérification dynamique de machine virtuelle :

   ```PowerShell
   Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" | 
   Set-ClusterParameter SkipMigrationDestinationCheck -Value 0
   ```

9. Vérifiez que le cluster mis à niveau fonctionne comme prévu. Les rôles doivent basculer correctement et si la migration dynamique de machine virtuelle est utilisée sur le cluster, les machines virtuelles doivent réussir la migration.

10. Validez le cluster en exécutant la validation`Test-Cluster`de cluster () et en examinant le rapport de validation de cluster.

## <a name="performing-an-in-place-upgrade-while-vms-are-stopped"></a>Exécution d’une mise à niveau sur place alors que les machines virtuelles sont arrêtées

Cette option entraîne un temps d’arrêt de la machine virtuelle, mais elle peut prendre moins de temps que si vous avez conservé les machines virtuelles en cours d’exécution pendant la mise à niveau, car vous n’avez pas besoin d’attendre la fin des tâches de stockage (réparation en miroir) après la mise à niveau de chaque serveur. Bien que les serveurs individuels soient redémarrés séquentiellement pendant le processus de mise à niveau, les autres serveurs du cluster restent en cours d’exécution.

1. Vérifiez que tous les serveurs du cluster exécutent les dernières mises à jour. Pour plus d’informations, consultez [l’historique des mises à jour Windows 10 et Windows Server 2016](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history). Au minimum, installez [l’article 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) de la base de connaissances Microsoft (19 février, 2019). Le numéro de build ( `ver` voir commande) doit être 14393,2828 ou une version ultérieure.

2. Arrêtez les machines virtuelles en cours d’exécution sur le cluster.

3. Effectuez les étapes suivantes sur un serveur de cluster à la fois :

   1. Suspendez le serveur de cluster à l’aide de la commande PowerShell suivante : Notez que certains groupes internes sont masqués. Nous vous recommandons d’effectuer cette étape avec prudence.

        ```PowerShell
       Suspend-ClusterNode -Drain
       ```

   2. Placez le serveur en mode maintenance du stockage en exécutant les commandes PowerShell suivantes :

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Enable-StorageMaintenanceMode
       ```

   3. Exécutez l’applet de commande suivante pour vérifier que la valeur **OperationalStatus** est **en mode maintenance**:

       ```PowerShell
       Get-PhysicalDisk
       ```

   4. Effectuez une installation de mise à niveau de Windows Server 2019 sur le serveur en exécutant le **fichier Setup. exe** et en utilisant l’option « conserver les fichiers et les applications personnels ».  
   Une fois l’installation terminée, le serveur reste dans le cluster et le service de cluster démarre automatiquement.

   5.  Vérifiez que le serveur qui vient d’être mis à niveau dispose des dernières mises à jour de Windows Server 2019.  
   Pour plus d’informations, consultez [l’historique des mises à jour Windows 10 et Windows Server 2019](https://support.microsoft.com/help/4464619/windows-10-update-history).
   Le numéro de build ( `ver` voir commande) doit être 17763,292 ou une version ultérieure.

   6.  Supprimez le serveur du mode de maintenance du stockage à l’aide des commandes PowerShell suivantes :

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Disable-StorageMaintenanceMode
       ```

   7.  Reprenez le serveur à l’aide de la commande PowerShell suivante :

       ```PowerShell
       Resume-ClusterNode
       ```

   8.  Attendez la fin des tâches de réparation de stockage et pour que tous les disques reviennent à un état sain.  
   Cela doit être relativement rapide, puisque les machines virtuelles ne sont pas en cours d’exécution. Voici les commandes à exécuter :

       ```PowerShell
       Get-StorageJob
       Get-VirtualDisk
       ```

4. Mettez à niveau le serveur suivant du cluster.
5. Une fois que tous les serveurs ont été mis à niveau vers Windows Server 2019, utilisez l’applet de commande PowerShell suivante pour mettre à jour le niveau fonctionnel du cluster.
    
   ```PowerShell
   Update-ClusterFunctionalLevel
   ```

   > [!NOTE]
   >   Nous vous recommandons de mettre à jour le niveau fonctionnel du cluster dès que possible, bien que techniquement vous ayez jusqu’à quatre semaines pour le faire.

6. Une fois le niveau fonctionnel du cluster mis à jour, utilisez l’applet de commande suivante pour mettre à jour le pool de stockage.  
   À ce stade, de nouvelles applets de `Get-ClusterPerf` commande, telles que, sont entièrement opérationnelles sur n’importe quel serveur du cluster.

   ```PowerShell
   Update-StoragePool
   ```

7. Démarrez les machines virtuelles sur le cluster et assurez-vous qu’elles fonctionnent correctement.

8. Vous pouvez éventuellement mettre à niveau des niveaux de configuration de machine virtuelle `Update-VMVersion` en arrêtant chaque machine virtuelle et en utilisant l’applet de commande, puis en redémarrant les machines virtuelles.

9. Vérifiez que le cluster mis à niveau fonctionne comme prévu.  
   Les rôles doivent basculer correctement et si la migration dynamique de machine virtuelle est utilisée sur le cluster, les machines virtuelles doivent réussir la migration.

10. Validez le cluster en exécutant la validation`Test-Cluster`de cluster () et en examinant le rapport de validation de cluster.

## <a name="performing-a-clean-os-installation-while-vms-are-stopped"></a>Exécution d’une installation propre du système d’exploitation pendant l’arrêt des machines virtuelles

Cette option entraîne un temps d’arrêt de la machine virtuelle, mais elle peut prendre moins de temps que si vous avez conservé les machines virtuelles en cours d’exécution pendant la mise à niveau, car vous n’avez pas besoin d’attendre la fin des tâches de stockage (réparation en miroir) après la mise à niveau de chaque serveur. Bien que les serveurs individuels soient redémarrés séquentiellement pendant le processus de mise à niveau, les autres serveurs du cluster restent en cours d’exécution.

1. Vérifiez que tous les serveurs du cluster exécutent les dernières mises à jour.  
   Pour plus d’informations, consultez [l’historique des mises à jour Windows 10 et Windows Server 2016](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history).
   Au minimum, installez [l’article 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) de la base de connaissances Microsoft (19 février, 2019). Le numéro de build ( `ver` voir commande) doit être 14393,2828 ou une version ultérieure.

2. Arrêtez les machines virtuelles en cours d’exécution sur le cluster.

3. Effectuez les étapes suivantes sur un serveur de cluster à la fois :

   2. Suspendez le serveur de cluster à l’aide de la commande PowerShell suivante : Notez que certains groupes internes sont masqués.  
      Nous vous recommandons d’effectuer cette étape avec prudence.

       ```PowerShell
      Suspend-ClusterNode -Drain
      ```

   3. Placez le serveur en mode maintenance du stockage en exécutant les commandes PowerShell suivantes :

      ```PowerShell
      Get-StorageFaultDomain -type StorageScaleUnit | 
      Where FriendlyName -Eq <ServerName> | 
      Enable-StorageMaintenanceMode
      ```

   4. Exécutez l’applet de commande suivante pour vérifier que la valeur **OperationalStatus** est **en mode maintenance**:

      ```PowerShell
      Get-PhysicalDisk
      ```

   5. Supprimez le serveur du cluster en exécutant la commande PowerShell suivante :  
    
      ```PowerShell
      Remove-ClusterNode <ServerName>
      ```

   6. Effectuez une nouvelle installation de Windows Server 2019 sur le serveur : formatez le lecteur système, exécutez **Setup. exe** et utilisez l’option « Nothing ».  
      Vous devez configurer l’identité, les rôles, les fonctionnalités et les applications du serveur une fois l’installation terminée et le serveur redémarré.

   7. Installez le rôle Hyper-V et la fonctionnalité de clustering de basculement sur le serveur (vous `Install-WindowsFeature` pouvez utiliser l’applet de commande).

   8. Installez les derniers pilotes de réseau et de stockage pour votre matériel qui sont approuvés par le fabricant de votre serveur pour une utilisation avec espaces de stockage direct.

   9. Vérifiez que le serveur qui vient d’être mis à niveau dispose des dernières mises à jour de Windows Server 2019.  
      Pour plus d’informations, consultez [l’historique des mises à jour Windows 10 et Windows Server 2019](https://support.microsoft.com/help/4464619/windows-10-update-history).
      Le numéro de build ( `ver` voir commande) doit être 17763,292 ou une version ultérieure.

   10. Rattachez le serveur au cluster à l’aide de la commande PowerShell suivante :

      ```PowerShell
      Add-ClusterNode
      ```

   11. Supprimez le serveur du mode de maintenance du stockage à l’aide de la commande PowerShell suivante :

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Disable-StorageMaintenanceMode
       ```

   12. Attendez la fin des tâches de réparation de stockage et pour que tous les disques reviennent à un état sain.  
       Cela peut prendre beaucoup de temps en fonction du nombre de machines virtuelles en cours d’exécution pendant la mise à niveau du serveur. Voici les commandes à exécuter :

       ```PowerShell
       Get-StorageJob
       Get-VirtualDisk
       ```

4. Mettez à niveau le serveur suivant du cluster.

5. Une fois que tous les serveurs ont été mis à niveau vers Windows Server 2019, utilisez l’applet de commande PowerShell suivante pour mettre à jour le niveau fonctionnel du cluster.
    
   ```PowerShell
   Update-ClusterFunctionalLevel
   ```

   > [!NOTE]
   >   Nous vous recommandons de mettre à jour le niveau fonctionnel du cluster dès que possible, bien que techniquement vous ayez jusqu’à quatre semaines pour le faire.

6. Une fois le niveau fonctionnel du cluster mis à jour, utilisez l’applet de commande suivante pour mettre à jour le pool de stockage.  
   À ce stade, de nouvelles applets de `Get-ClusterPerf` commande, telles que, sont entièrement opérationnelles sur n’importe quel serveur du cluster.

   ```PowerShell
   Update-StoragePool
   ```

7. Démarrez les machines virtuelles sur le cluster et assurez-vous qu’elles fonctionnent correctement.

8. Vous pouvez éventuellement mettre à niveau des niveaux de configuration de machine virtuelle `Update-VMVersion` en arrêtant chaque machine virtuelle et en utilisant l’applet de commande, puis en redémarrant les machines virtuelles.

9. Vérifiez que le cluster mis à niveau fonctionne comme prévu.  
   Les rôles doivent basculer correctement et si la migration dynamique de machine virtuelle est utilisée sur le cluster, les machines virtuelles doivent réussir la migration.

10. Validez le cluster en exécutant la validation`Test-Cluster`de cluster () et en examinant le rapport de validation de cluster.
