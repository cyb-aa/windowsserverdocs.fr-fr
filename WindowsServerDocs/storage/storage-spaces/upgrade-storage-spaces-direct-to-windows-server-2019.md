---
title: Mise à niveau d’un cluster d’espaces de stockage Direct vers Windows Server 2019
description: La mise à niveau un cluster d’espaces de stockage Direct Windows Server 2019-soit tout en conservant les machines virtuelles en cours d’exécution alors que ceux-ci sont arrêtés.
author: robhindman
ms.author: robhind
manager: eldenc
ms.date: 03/06/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage-spaces
ms.openlocfilehash: 9db92aa33cde9b2beed11149dae06bb3af2b5a03
ms.sourcegitcommit: fe621b72d45d0259bac1d5b9031deed3dcbed29d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2019
ms.locfileid: "66455412"
---
# <a name="upgrade-a-storage-spaces-direct-cluster-to-windows-server-2019"></a>Mise à niveau d’un cluster d’espaces de stockage Direct vers Windows Server 2019

Cette rubrique décrit comment mettre à niveau un cluster d’espaces de stockage Direct à Windows Server 2019. Il existe quatre approches pour la mise à niveau d’un cluster d’espaces de stockage Direct à partir de Windows Server 2016 pour Windows Server 2019, à l’aide de la [du système d’exploitation, les processus de mise à niveau propagée de cluster](../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md) : deux qui nécessitent de conserver les machines virtuelles en cours d’exécution et deux qui impliquent arrêt de toutes les machines virtuelles. Chaque approche présente des forces et faiblesses, donc en sélectionner qu’un mieux adaptée aux besoins de votre organisation :

- **Niveau sur place pendant l’exécutant des machines virtuelles** sur chaque serveur dans le cluster, cette option n’implique aucun temps d’arrêt de la machine virtuelle, mais vous devrez attendre des tâches de stockage (réparation de miroir) à effectuer après la mise à niveau chaque serveur.

- **Installation de système d’exploitation nettoyer pendant l’exécutant des machines virtuelles** sur chaque serveur dans le cluster, cette option n’implique aucun temps d’arrêt de la machine virtuelle, mais vous devrez attendre des tâches de stockage (réparation de miroir) effectuer une fois que chaque serveur est mis à niveau, et vous devrez définir chaque serveur et tous les ses applications et les rôles à nouveau.

- **Niveau sur place alors que les machines virtuelles sont arrêtées** sur chaque serveur dans le cluster, cette option implique des temps d’arrêt de la machine virtuelle, mais vous n’avez pas besoin d’attendre le stockage des travaux (réparation de miroir), par conséquent, il est plus rapide.

- **Système d’exploitation nettoyer installer pendant l’arrêt des machines virtuelles** sur chaque serveur dans le cluster, cette option implique des temps d’arrêt de la machine virtuelle, mais vous n’avez pas besoin d’attendre pour le stockage des travaux (réparation de miroir). il est plus rapide.

## <a name="prerequisites-and-limitations"></a>Conditions préalables et limitations

Avant de procéder à une mise à niveau :

- Vérifiez que vous disposez des sauvegardes utilisables dans le cas où il existe des problèmes pendant le processus de mise à niveau.

- Vérifiez que votre fournisseur de matériel a un BIOS, des microprogrammes et des pilotes pour vos serveurs seront pris en charge sur Windows Server 2019.

Il existe certaines limitations avec la mise à niveau à connaître :

- Pour activer les espaces de stockage Direct avec Windows Server 2019 builds antérieures à 176693.292, les clients peuvent doivent contacter le support Microsoft pour les clés de Registre qui activent les fonctionnalités Sdn et les espaces de stockage Direct. Pour plus d’informations, consultez la Base de connaissances Microsoft [article 4464776](https://support.microsoft.com/help/4464776/software-defined-data-center-and-software-defined-networking).

- Lors de la mise à niveau un cluster avec des volumes ReFS, il existe quelques limitations :

- La mise à niveau est entièrement pris en charge sur des volumes ReFS, toutefois, les volumes mis à niveau ne sont pas en bénéficier ReFS améliorations dans Windows Server 2019. Ces avantages, tels que de meilleures performances pour la parité avec accélération miroir, nécessitent un volume de Windows Server 2019 ReFS nouvellement créé. En d’autres termes, vous seriez obligé de créer des volumes à l’aide de la `New-Volume` applet de commande ou le Gestionnaire de serveur. Voici quelques-unes des améliorations ReFS nouveaux volumes obtiendriez :

    - **CARTE journal-contournement**: une amélioration des performances dans les références qui seulement s’applique aux systèmes (espaces de stockage Direct) en cluster et ne s’applique pas aux pools de stockage autonome.

    - **Compactage** des améliorations de l’efficacité dans Windows Server 2019 qui sont spécifiques à des volumes multiples résilientes.

- Avant la mise à niveau un serveur de cluster espaces de stockage Windows Server 2016 Direct, nous vous recommandons de mettre le serveur en mode de maintenance de stockage. Pour plus d’informations, consultez la section événements 5120 de [résoudre les espaces de stockage Direct](troubleshooting-storage-spaces.md). Bien que ce problème a été résolu dans Windows Server 2016, nous vous recommandons de placer chaque serveur d’espaces de stockage Direct en mode de maintenance de stockage pendant la mise à niveau est recommandé.

- Il existe un problème connu avec les environnements Sdn qui utilisent le jeu de commutateurs. Ce problème implique des migrations dynamiques machine virtuelle Hyper-V à partir de Windows Server 2019 vers Windows Server 2016 (migration dynamique à un système d’exploitation antérieur). Pour garantir la réussite migrations en direct, nous vous recommandons de modifier un paramètre de réseau de machine virtuelle sur des machines virtuelles qui sont migrés à partir de Windows Server 2019 vers Windows Server 2016. Ce problème est résolu pour Windows Server 2019 dans le package de correctif cumulatif 2019-01D, également appelé génération 17763.292. Pour plus d’informations, consultez la Base de connaissances Microsoft [article 4476976](https://support.microsoft.com/help/4476976/windows-10-update-kb4476976).

En raison de problèmes connus ci-dessus, certains clients peuvent prendre en compte la création d’un cluster Windows Server 2019 et copier des données à partir de l’ancien cluster, au lieu de la mise à niveau leurs clusters Windows Server 2016 à l’aide d’un des quatre processus décrits ci-dessous.

## <a name="performing-an-in-place-upgrade-while-vms-are-running"></a>Effectuer une mise à niveau sur place pendant l’exécutant des machines virtuelles

Cette option n’implique aucun temps d’arrêt de la machine virtuelle, mais vous devrez attendre des tâches de stockage (réparation de miroir) à effectuer après la mise à niveau chaque serveur. Bien que des serveurs individuels doit être redémarrés de manière séquentielle pendant le processus de mise à niveau, les serveurs restants dans le cluster, ainsi que toutes les machines virtuelles, reste en cours d’exécution.

1. Vérifiez que tous les serveurs du cluster ont installé les dernières mises à jour de Windows. Pour plus d’informations, consultez [l’historique de mise à jour de Windows 10 et Windows Server 2016](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history). Au minimum, installez la Base de connaissances Microsoft [article 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) (le 19 février 2019). Le numéro de build (voir `ver` commande) doit être 14393.2828 ou une version ultérieure.

2. Si vous utilisez Sdn avec jeu de commutateurs, ouvrez une session PowerShell avec élévation de privilèges et exécutez la commande suivante pour désactiver les contrôles de vérification de la migration dynamique de machine virtuelle sur toutes les machines virtuelles sur le cluster :

   ```PowerShell
   Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" |
   Set-ClusterParameter -Create SkipMigrationDestinationCheck -Value 1
   ```

3. Procédez comme suit sur un serveur de cluster à la fois :

   1. Utiliser la migration dynamique de machine virtuelle Hyper-V pour déplacer des machines virtuelles en cours d’exécution sur le serveur que vous êtes sur le point de mise à niveau.

   2. Suspendez le serveur de cluster à l’aide de la commande PowerShell suivante : Notez que certains groupes internes sont masqués. Nous vous recommandons de cette étape pour être prudent : Si vous n’avez pas déjà actives migrer des machines virtuelles du serveur de cette applet de commande fera que pour vous, par conséquent, vous pourriez ignorer l’étape précédente si vous préférez.

        ```PowerShell
       Suspend-ClusterNode -Drain
       ```

   3. Placer le serveur en mode de maintenance de stockage en exécutant les commandes PowerShell suivantes :

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Enable-StorageMaintenanceMode
       ```

   4. Exécutez l’applet de commande suivante pour vérifier que qui le **OperationalStatus** valeur est **en Mode de Maintenance**:

       ```PowerShell
       Get-PhysicalDisk
       ```

   5. Effectuez une mise à niveau vers Windows Server 2019 sur le serveur en exécutant **setup.exe** et à l’aide de l’option « Conserver les fichiers personnels et des applications ». Une fois l’installation terminée, le serveur reste dans le cluster et le cluster service démarre automatiquement.

   6. Vérifiez que le serveur qui vient d’être mis à niveau sont les dernières mises à jour de Windows Server 2019. Pour plus d’informations, consultez [l’historique de mise à jour de Windows 10 et Windows Server 2019](https://support.microsoft.com/help/4464619/windows-10-update-history). Le numéro de build (voir `ver` commande) doit être 17763.292 ou une version ultérieure.

   7. Supprimer le serveur de stockage mode de maintenance à l’aide de la commande PowerShell suivante :

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Disable-StorageMaintenanceMode
       ```

   8. Reprendre le serveur à l’aide de la commande PowerShell suivante :

       ```PowerShell
       Resume-ClusterNode
       ```

   9. Attente de fin des tâches de réparation de stockage et pour tous les disques revenir à un état sain. Cette opération peut prendre beaucoup de temps en fonction du nombre de machines virtuelles en cours d’exécution pendant la mise à niveau du serveur. Voici les commandes à exécuter :

       ```PowerShell
       Get-StorageJob
       Get-VirtualDisk
       ```

4. Mettre à niveau le serveur suivant dans le cluster.

5. Une fois que tous les serveurs ont été mis à niveau vers Windows Server 2019, utilisez l’applet de commande PowerShell suivante pour mettre à jour le niveau fonctionnel du cluster.

   ```PowerShell
   Update-ClusterFunctionalLevel
   ```

   > [!NOTE]
   > Nous vous recommandons de la mise à jour le niveau fonctionnel du cluster dès que possible, bien que techniquement, vous avez quatre semaines au maximum à le faire.

6. Une fois que le niveau fonctionnel du cluster a été mis à jour, utilisez l’applet de commande suivante pour mettre à jour le pool de stockage. À ce stade, nouvelles applets de commande telles que `Get-ClusterPerf` soit entièrement opérationnel sur n’importe quel serveur dans le cluster.

   ```PowerShell
   Update-StoragePool
   ```

7. Si vous le souhaitez mettre à niveau des niveaux de configuration de machine virtuelle en arrêtant chaque machine virtuelle, à l’aide de la `Update-VMVersion` applet de commande, puis de redémarrer les machines virtuelles.

8. Si vous utilisez Sdn avec des commutateurs de jeu et désactivées vérifications de migration dynamique de machine virtuelle en suivant les instructions ci-dessus, pour utilisent l’applet de commande suivante pour réactiver Live de la machine virtuelle aux contrôles de vérification :

   ```PowerShell
   Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" |
   Set-ClusterParameter  SkipMigrationDestinationCheck -Value 0
   ```

9. Vérifiez que le cluster mis à niveau fonctionne comme prévu. Rôles doivent basculer correctement et si la migration dynamique de machine virtuelle est utilisée sur le cluster, les machines virtuelles doivent migrer correctement dynamiquement.

10. Valider le cluster en exécutant la Validation du Cluster (`Test-Cluster`) et en examinant le rapport de validation de cluster.

## <a name="performing-a-clean-os-installation-while-vms-are-running"></a>Effectuez une nouvelle installation de système d’exploitation pendant l’exécutant des machines virtuelles

Cette option n’implique aucun temps d’arrêt de la machine virtuelle, mais vous devrez attendre des tâches de stockage (réparation de miroir) à effectuer après la mise à niveau chaque serveur. Bien que des serveurs individuels doit être redémarrés de manière séquentielle pendant le processus de mise à niveau, les serveurs restants dans le cluster, ainsi que toutes les machines virtuelles, reste en cours d’exécution.

1. Vérifiez que tous les serveurs du cluster exécutent les dernières mises à jour. Pour plus d’informations, consultez [l’historique de mise à jour de Windows 10 et Windows Server 2016](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history). Au minimum, installez la Base de connaissances Microsoft [article 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) (le 19 février 2019). Le numéro de build (voir `ver` commande) doit être 14393.2828 ou une version ultérieure.

2. Si vous utilisez Sdn avec jeu de commutateurs, ouvrez une session PowerShell avec élévation de privilèges et exécutez la commande suivante pour désactiver les contrôles de vérification de la migration dynamique de machine virtuelle sur toutes les machines virtuelles sur le cluster :

   ```PowerShell
   Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" |
   Set-ClusterParameter -Create SkipMigrationDestinationCheck -Value 1
   ```

3. Procédez comme suit sur un serveur de cluster à la fois :

   1. Utiliser la migration dynamique de machine virtuelle Hyper-V pour déplacer des machines virtuelles en cours d’exécution sur le serveur que vous êtes sur le point de mise à niveau.

   2. Suspendez le serveur de cluster à l’aide de la commande PowerShell suivante : Notez que certains groupes internes sont masqués. Nous vous recommandons de cette étape pour être prudent : Si vous n’avez pas déjà actives migrer des machines virtuelles du serveur de cette applet de commande fera que pour vous, par conséquent, vous pourriez ignorer l’étape précédente si vous préférez.

        ```PowerShell
       Suspend-ClusterNode -Drain
       ```

   3. Placer le serveur en mode de maintenance de stockage en exécutant les commandes PowerShell suivantes :

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Enable-StorageMaintenanceMode
       ```

   4. Exécutez l’applet de commande suivante pour vérifier que qui le **OperationalStatus** valeur est **en Mode de Maintenance**:

       ```PowerShell
       Get-PhysicalDisk
       ```

   3.  Supprimez le serveur à partir du cluster en exécutant la commande PowerShell suivante :  

       ```PowerShell
       Remove-ClusterNode <ServerName>
       ```

   4. Effectuez une nouvelle installation de Windows Server 2019 sur le serveur : format le lecteur du système, exécutez **setup.exe** et utiliser le « Nothing » option. Vous devrez configurer l’identité du serveur, rôles, fonctionnalités et applications une fois le programme d’installation est terminée et le serveur a redémarré.

   5. Installez le rôle Hyper-V et la fonctionnalité de Clustering de basculement sur le serveur (vous pouvez utiliser le `Install-WindowsFeature` applet de commande).

   6. Installer les pilotes réseau et de stockage plus récente pour votre matériel qui sont approuvées par le fabricant de votre serveur pour une utilisation avec les espaces de stockage Direct.

   7. Vérifiez que le serveur qui vient d’être mis à niveau sont les dernières mises à jour de Windows Server 2019. Pour plus d’informations, consultez [l’historique de mise à jour de Windows 10 et Windows Server 2019](https://support.microsoft.com/help/4464619/windows-10-update-history). Le numéro de build (voir `ver` commande) doit être 17763.292 ou une version ultérieure.

   8. Rejoindre le serveur au cluster à l’aide de la commande PowerShell suivante :

       ```PowerShell
       Add-ClusterNode
       ```

   9. Supprimer le serveur de stockage mode de maintenance à l’aide des commandes PowerShell suivantes :

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Disable-StorageMaintenanceMode
       ```

   10. Attente de fin des tâches de réparation de stockage et pour tous les disques revenir à un état sain. Cette opération peut prendre beaucoup de temps en fonction du nombre de machines virtuelles en cours d’exécution pendant la mise à niveau du serveur. Voici les commandes à exécuter :

        ```PowerShell
        Get-StorageJob
        Get-VirtualDisk
        ```

4. Mettre à niveau le serveur suivant dans le cluster.

5. Une fois que tous les serveurs ont été mis à niveau vers Windows Server 2019, utilisez l’applet de commande PowerShell suivante pour mettre à jour le niveau fonctionnel du cluster.
    
   ```PowerShell
   Update-ClusterFunctionalLevel
   ```

   > [!NOTE]
   > Nous vous recommandons de la mise à jour le niveau fonctionnel du cluster dès que possible, bien que techniquement, vous avez quatre semaines au maximum à le faire.

6. Une fois que le niveau fonctionnel du cluster a été mis à jour, utilisez l’applet de commande suivante pour mettre à jour le pool de stockage. À ce stade, nouvelles applets de commande telles que `Get-ClusterPerf` soit entièrement opérationnel sur n’importe quel serveur dans le cluster.

   ```PowerShell
   Update-StoragePool
   ```

7. Si vous le souhaitez mettre à niveau des niveaux de configuration de machine virtuelle par l’arrêt de chaque machine virtuelle et à l’aide de la `Update-VMVersion` applet de commande, puis de redémarrer les machines virtuelles.

8. Si vous utilisez Sdn avec des commutateurs de jeu et désactivées vérifications de migration dynamique de machine virtuelle en suivant les instructions ci-dessus, pour utilisent l’applet de commande suivante pour réactiver Live de la machine virtuelle aux contrôles de vérification :

   ```PowerShell
   Get-ClusterResourceType -Cluster {clusterName} -Name "Virtual Machine" | 
   Set-ClusterParameter SkipMigrationDestinationCheck -Value 0
   ```

9. Vérifiez que le cluster mis à niveau fonctionne comme prévu. Rôles doivent basculer correctement et si la migration dynamique de machine virtuelle est utilisée sur le cluster, les machines virtuelles doivent migrer correctement dynamiquement.

10. Valider le cluster en exécutant la Validation du Cluster (`Test-Cluster`) et en examinant le rapport de validation de cluster.

## <a name="performing-an-in-place-upgrade-while-vms-are-stopped"></a>Effectuer une mise à niveau sur place pendant l’arrêt des machines virtuelles

Cette option implique des temps d’arrêt de la machine virtuelle, mais peut prendre moins de temps que si vous avez conservé les machines virtuelles en cours d’exécution pendant la mise à niveau, car vous n’avez pas besoin d’attendre des tâches de stockage (réparation de miroir) à effectuer après la mise à niveau chaque serveur. Bien que des serveurs individuels doit être redémarrés de manière séquentielle pendant le processus de mise à niveau, les serveurs restants du cluster restent en cours d’exécution.

1. Vérifiez que tous les serveurs du cluster exécutent les dernières mises à jour. Pour plus d’informations, consultez [l’historique de mise à jour de Windows 10 et Windows Server 2016](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history). Au minimum, installez la Base de connaissances Microsoft [article 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) (le 19 février 2019). Le numéro de build (voir `ver` commande) doit être 14393.2828 ou une version ultérieure.

2. Arrêter les machines virtuelles en cours d’exécution sur le cluster.

3. Procédez comme suit sur un serveur de cluster à la fois :

   1. Suspendez le serveur de cluster à l’aide de la commande PowerShell suivante : Notez que certains groupes internes sont masqués. Nous recommandons cette étape pour être prudent.

        ```PowerShell
       Suspend-ClusterNode -Drain
       ```

   2. Placer le serveur en mode de maintenance de stockage en exécutant les commandes PowerShell suivantes :

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Enable-StorageMaintenanceMode
       ```

   3. Exécutez l’applet de commande suivante pour vérifier que qui le **OperationalStatus** valeur est **en Mode de Maintenance**:

       ```PowerShell
       Get-PhysicalDisk
       ```

   4. Effectuez une mise à niveau vers Windows Server 2019 sur le serveur en exécutant **setup.exe** et à l’aide de l’option « Conserver les fichiers personnels et des applications ».  
   Une fois l’installation terminée, le serveur reste dans le cluster et le cluster service démarre automatiquement.

   5.  Vérifiez que le serveur qui vient d’être mis à niveau sont les dernières mises à jour de Windows Server 2019.  
   Pour plus d’informations, consultez [l’historique de mise à jour de Windows 10 et Windows Server 2019](https://support.microsoft.com/help/4464619/windows-10-update-history).
   Le numéro de build (voir `ver` commande) doit être 17763.292 ou une version ultérieure.

   6.  Supprimer le serveur de stockage mode de maintenance à l’aide des commandes PowerShell suivantes :

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Disable-StorageMaintenanceMode
       ```

   7.  Reprendre le serveur à l’aide de la commande PowerShell suivante :

       ```PowerShell
       Resume-ClusterNode
       ```

   8.  Attente de fin des tâches de réparation de stockage et pour tous les disques revenir à un état sain.  
   Cela doit être relativement rapide, étant donné que les machines virtuelles ne sont pas en cours d’exécution. Voici les commandes à exécuter :

       ```PowerShell
       Get-StorageJob
       Get-VirtualDisk
       ```

4. Mettre à niveau le serveur suivant dans le cluster.
5. Une fois que tous les serveurs ont été mis à niveau vers Windows Server 2019, utilisez l’applet de commande PowerShell suivante pour mettre à jour le niveau fonctionnel du cluster.
    
   ```PowerShell
   Update-ClusterFunctionalLevel
   ```

   > [!NOTE]
   >   Nous vous recommandons de la mise à jour le niveau fonctionnel du cluster dès que possible, bien que techniquement, vous avez quatre semaines au maximum à le faire.

6. Une fois que le niveau fonctionnel du cluster a été mis à jour, utilisez l’applet de commande suivante pour mettre à jour le pool de stockage.  
   À ce stade, nouvelles applets de commande telles que `Get-ClusterPerf` soit entièrement opérationnel sur n’importe quel serveur dans le cluster.

   ```PowerShell
   Update-StoragePool
   ```

7. Démarrer les machines virtuelles sur le cluster et vérifiez qu’elles fonctionnent correctement.

8. Si vous le souhaitez mettre à niveau des niveaux de configuration de machine virtuelle par l’arrêt de chaque machine virtuelle et à l’aide de la `Update-VMVersion` applet de commande, puis de redémarrer les machines virtuelles.

9. Vérifiez que le cluster mis à niveau fonctionne comme prévu.  
   Rôles doivent basculer correctement et si la migration dynamique de machine virtuelle est utilisée sur le cluster, les machines virtuelles doivent migrer correctement dynamiquement.

10. Valider le cluster en exécutant la Validation du Cluster (`Test-Cluster`) et en examinant le rapport de validation de cluster.

## <a name="performing-a-clean-os-installation-while-vms-are-stopped"></a>Effectuez une nouvelle installation de système d’exploitation pendant l’arrêt des machines virtuelles

Cette option implique des temps d’arrêt de la machine virtuelle, mais peut prendre moins de temps que si vous avez conservé les machines virtuelles en cours d’exécution pendant la mise à niveau, car vous n’avez pas besoin d’attendre des tâches de stockage (réparation de miroir) à effectuer après la mise à niveau chaque serveur. Bien que des serveurs individuels doit être redémarrés de manière séquentielle pendant le processus de mise à niveau, les serveurs restants du cluster restent en cours d’exécution.

1. Vérifiez que tous les serveurs du cluster exécutent les dernières mises à jour.  
   Pour plus d’informations, consultez [l’historique de mise à jour de Windows 10 et Windows Server 2016](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history).
   Au minimum, installez la Base de connaissances Microsoft [article 4487006](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) (le 19 février 2019). Le numéro de build (voir `ver` commande) doit être 14393.2828 ou une version ultérieure.

2. Arrêter les machines virtuelles en cours d’exécution sur le cluster.

3. Procédez comme suit sur un serveur de cluster à la fois :

   2. Suspendez le serveur de cluster à l’aide de la commande PowerShell suivante : Notez que certains groupes internes sont masqués.  
      Nous recommandons cette étape pour être prudent.

       ```PowerShell
      Suspend-ClusterNode -Drain
      ```

   3. Placer le serveur en mode de maintenance de stockage en exécutant les commandes PowerShell suivantes :

      ```PowerShell
      Get-StorageFaultDomain -type StorageScaleUnit | 
      Where FriendlyName -Eq <ServerName> | 
      Enable-StorageMaintenanceMode
      ```

   4. Exécutez l’applet de commande suivante pour vérifier que qui le **OperationalStatus** valeur est **en Mode de Maintenance**:

      ```PowerShell
      Get-PhysicalDisk
      ```

   5. Supprimez le serveur à partir du cluster en exécutant la commande PowerShell suivante :  
    
      ```PowerShell
      Remove-ClusterNode <ServerName>
      ```

   6. Effectuez une nouvelle installation de Windows Server 2019 sur le serveur : format le lecteur du système, exécutez **setup.exe** et utiliser le « Nothing » option.  
      Vous devrez configurer l’identité du serveur, rôles, fonctionnalités et applications une fois le programme d’installation est terminée et le serveur a redémarré.

   7. Installez le rôle Hyper-V et la fonctionnalité de Clustering de basculement sur le serveur (vous pouvez utiliser le `Install-WindowsFeature` applet de commande).

   8. Installer les pilotes réseau et de stockage plus récente pour votre matériel qui sont approuvées par le fabricant de votre serveur pour une utilisation avec les espaces de stockage Direct.

   9. Vérifiez que le serveur qui vient d’être mis à niveau sont les dernières mises à jour de Windows Server 2019.  
      Pour plus d’informations, consultez [l’historique de mise à jour de Windows 10 et Windows Server 2019](https://support.microsoft.com/help/4464619/windows-10-update-history).
      Le numéro de build (voir `ver` commande) doit être 17763.292 ou une version ultérieure.

   10. Rejoindre le serveur au cluster à l’aide de la commande PowerShell suivante :

      ```PowerShell
      Add-ClusterNode
      ```

   11. Supprimer le serveur de stockage mode de maintenance à l’aide de la commande PowerShell suivante :

       ```PowerShell
       Get-StorageFaultDomain -type StorageScaleUnit | 
       Where FriendlyName -Eq <ServerName> | 
       Disable-StorageMaintenanceMode
       ```

   12. Attente de fin des tâches de réparation de stockage et pour tous les disques revenir à un état sain.  
       Cette opération peut prendre beaucoup de temps en fonction du nombre de machines virtuelles en cours d’exécution pendant la mise à niveau du serveur. Voici les commandes à exécuter :

       ```PowerShell
       Get-StorageJob
       Get-VirtualDisk
       ```

4. Mettre à niveau le serveur suivant dans le cluster.

5. Une fois que tous les serveurs ont été mis à niveau vers Windows Server 2019, utilisez l’applet de commande PowerShell suivante pour mettre à jour le niveau fonctionnel du cluster.
    
   ```PowerShell
   Update-ClusterFunctionalLevel
   ```

   > [!NOTE]
   >   Nous vous recommandons de la mise à jour le niveau fonctionnel du cluster dès que possible, bien que techniquement, vous avez quatre semaines au maximum à le faire.

6. Une fois que le niveau fonctionnel du cluster a été mis à jour, utilisez l’applet de commande suivante pour mettre à jour le pool de stockage.  
   À ce stade, nouvelles applets de commande telles que `Get-ClusterPerf` soit entièrement opérationnel sur n’importe quel serveur dans le cluster.

   ```PowerShell
   Update-StoragePool
   ```

7. Démarrer les machines virtuelles sur le cluster et vérifiez qu’elles fonctionnent correctement.

8. Si vous le souhaitez mettre à niveau des niveaux de configuration de machine virtuelle par l’arrêt de chaque machine virtuelle et à l’aide de la `Update-VMVersion` applet de commande, puis de redémarrer les machines virtuelles.

9. Vérifiez que le cluster mis à niveau fonctionne comme prévu.  
   Rôles doivent basculer correctement et si la migration dynamique de machine virtuelle est utilisée sur le cluster, les machines virtuelles doivent migrer correctement dynamiquement.

10. Valider le cluster en exécutant la Validation du Cluster (`Test-Cluster`) et en examinant le rapport de validation de cluster.
