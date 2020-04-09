---
title: Utilisation de espaces de stockage direct sur une machine virtuelle
ms.prod: windows-server
ms.author: eldenc
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 10/25/2017
description: Comment déployer des espaces de stockage direct dans un cluster invité d’ordinateur virtuel, par exemple, dans Microsoft Azure.
ms.localizationpriority: medium
ms.openlocfilehash: 74b1b90a780a0b238a356e942f8348e2a483d94a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856112"
---
# <a name="using-storage-spaces-direct-in-guest-virtual-machine-clusters"></a>Utilisation de espaces de stockage direct dans les clusters d’ordinateurs virtuels invités

> S’applique à : Windows Server 2019, Windows Server 2016

Vous pouvez déployer des espaces de stockage direct sur un cluster de serveurs physiques ou sur des clusters invités d’ordinateurs virtuels comme indiqué dans cette rubrique. Ce type de déploiement fournit un stockage partagé virtuel sur un ensemble de machines virtuelles sur un Cloud privé ou public afin que les solutions de haute disponibilité de l’application puissent être utilisées pour augmenter la disponibilité des applications.

![](media/storage-spaces-direct-in-vm/storage-spaces-direct-in-vm.png)

## <a name="deploying-in-azure-iaas-vm-guest-clusters"></a>Déploiement dans des clusters invités de machine virtuelle Azure IaaS

Les [modèles Azure](https://github.com/robotechredmond/301-storage-spaces-direct-md) ont été publiés réduisent la complexité, configurent les meilleures pratiques et accélèrent vos déploiements espaces de stockage direct sur une machine virtuelle Azure IaaS. Il s’agit de la solution recommandée pour le déploiement dans Azure.

<iframe src="https://channel9.msdn.com/Series/Microsoft-Hybrid-Cloud-Best-Practices-for-IT-Pros/Step-by-Step-Deploy-Windows-Server-2016-Storage-Spaces-Direct-S2D-Cluster-in-Microsoft-Azure/player" width="960" height="540" allowfullscreen></iframe>

## <a name="requirements"></a>Configuration requise

Les considérations suivantes s’appliquent lors du déploiement d’espaces de stockage direct dans un environnement virtualisé.
       
>        !TIP]
>        zure templates will automatically configure the below considerations for you and are the recommended solution when deploying in Azure IaaS VMs.

-   Au moins 2 nœuds et 3 nœuds au maximum

-   les déploiements à 2 nœuds doivent configurer un témoin (témoin de Cloud ou témoin de partage de fichiers)

-   les déploiements à 3 nœuds peuvent tolérer 1 nœud en panne et la perte de 1 ou plusieurs disques sur un autre nœud.  Si deux nœuds sont arrêtés, les disques virtuels sont hors connexion jusqu’à ce que l’un des nœuds retourne.  

-   Configurer les ordinateurs virtuels à déployer dans les domaines d’erreur

    -   Azure – configurer un groupe à haute disponibilité

    -   Hyper-V : configurer AntiAffinityClassNames sur les machines virtuelles pour séparer les machines virtuelles sur les nœuds

    -   VMware : configurer la règle d’anti-affinité des machines virtuelles en créant une règle DRS de type « machines virtuelles distinctes » pour séparer les machines virtuelles sur les hôtes ESX. Les disques présentés pour une utilisation avec espaces de stockage direct doivent utiliser l’adaptateur PVSCSI (paravirtual SCSI). Pour la prise en charge de PVSCSI avec Windows Server, consultez https://kb.vmware.com/s/article/1010398.

-   Tirez parti du stockage à faible latence/hautes performances-les disques managés Azure Premium Storage sont requis

-   Déployer une conception de stockage plat sans aucun appareil de mise en cache configuré

-   Au moins deux disques de données virtuels présentés à chaque machine virtuelle (VHD/VHDX/VMDK)

    Ce nombre est différent des déploiements nus, car les disques virtuels peuvent être implémentés en tant que fichiers qui ne sont pas vulnérables aux défaillances physiques.

-   Désactivez la LITIES « APAB » de remplacement automatique de lecteur dans le Service de contrôle d’intégrité en exécutant l’applet de commande PowerShell suivante :

    ```powershell
          Get-storagesubsystem clus* | set-storagehealthsetting -name "System.Storage.PhysicalDisk.AutoReplace.Enabled" -value "False"
          ```

-   To give greater resiliency to possible VHD / VHDX / VMDK storage latency in guest clusters, increase the Storage Spaces I/O timeout value:

    `HKEY_LOCAL_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\spaceport\\Parameters\\HwTimeout`

    `dword: 00007530`

    The decimal equivalent of Hexadecimal 7530 is 30000, which is 30 seconds. Note that the default value is 1770 Hexadecimal, or 6000 Decimal, which is 6 seconds.

## Not supported

-   Host level virtual disk snapshot/restore

    Instead use traditional guest level backup solutions to backup and restore the data on the Storage Spaces Direct volumes.

-   Host level virtual disk size change

    The virtual disks exposed through the virtual machine must retain the same size and characteristics. Adding more capacity to the storage pool can be accomplished by adding more virtual disks to each of the virtual machines and adding them to the pool. It's highly recommended to use virtual disks of the same size and characteristics as the current virtual disks.

## See also

[Additional Azure Iaas VM templates for deploying Storage Spaces Direct, videos, and step-by-step guides](https://techcommunity.microsoft.com/t5/Failover-Clustering/Deploying-IaaS-VM-Guest-Clusters-in-Microsoft-Azure/ba-p/372126).

[Additional Storage Spaces Direct Overview](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview)
""""""''''                                                                                                                                                                        """"""''''                                                                                                                                                                        