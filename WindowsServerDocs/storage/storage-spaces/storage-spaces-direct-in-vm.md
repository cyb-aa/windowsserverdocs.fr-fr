---
title: Utilisation de espaces de stockage direct sur une machine virtuelle
ms.prod: windows-server
ms.author: eldenc
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 10/25/2017
description: Comment déployer des espaces de stockage direct dans un cluster invité d’ordinateur virtuel, par exemple, dans Microsoft Azure.
ms.localizationpriority: medium
ms.openlocfilehash: ab0ce792c5a948e763a48493a78ccdac7a6fe74c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366047"
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

> [!TIP]
> Les modèles Azure configurent automatiquement les éléments ci-dessous pour vous et sont la solution recommandée lors du déploiement dans des machines virtuelles IaaS Azure.

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

-   Désactivez les fonctionnalités de remplacement automatique de lecteur dans le Service de contrôle d’intégrité en exécutant l’applet de commande PowerShell suivante :

    ```powershell
    Get-storagesubsystem clus* | set-storagehealthsetting -name “System.Storage.PhysicalDisk.AutoReplace.Enabled” -value “False”
    ```

-   Non pris en charge : Instantané/restauration du disque virtuel au niveau de l’hôte

    Au lieu de cela, utilisez les solutions de sauvegarde de niveau invité traditionnelles pour sauvegarder et restaurer les données sur les volumes de espaces de stockage direct.

-   Pour offrir une plus grande résilience à la latence de stockage VHD/VHDX/VMDK possible dans les clusters invités, augmentez la valeur du délai d’attente d’e/s des espaces de stockage :

    `HKEY_LOCAL_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\spaceport\\Parameters\\HwTimeout`

    `dword: 00007530`

    L’équivalent décimal de la valeur hexadécimale 7530 est 30000, ce qui correspond à 30 secondes. Notez que la valeur par défaut est 1770 hexadécimale, ou 6000 décimal, soit 6 secondes.

## <a name="see-also"></a>Voir aussi

[Autres modèles de machine virtuelle IaaS Azure pour le déploiement de espaces de stockage direct, de vidéos et de guides pas à pas](https://techcommunity.microsoft.com/t5/Failover-Clustering/Deploying-IaaS-VM-Guest-Clusters-in-Microsoft-Azure/ba-p/372126).

[Présentation des espaces de stockage direct supplémentaires](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview)
