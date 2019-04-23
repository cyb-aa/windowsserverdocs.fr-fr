---
title: À l’aide d’espaces de stockage Direct dans une machine virtuelle
ms.prod: windows-server-threshold
ms.author: eldenc
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 10/25/2017
description: Comment déployer des espaces de stockage Direct dans un cluster invité de machine virtuelle, par exemple, dans Microsoft Azure.
ms.localizationpriority: medium
ms.openlocfilehash: a741475d09ab7f7ab29f158ef55378ca9a37beac
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841020"
---
# <a name="using-storage-spaces-direct-in-guest-virtual-machine-clusters"></a>À l’aide d’espaces de stockage Direct dans les clusters invités de machine virtuelle

> S’applique à : Windows Server 2019, Windows Server 2016

Vous pouvez déployer les espaces de stockage Direct sur un cluster de serveurs physiques, ou sur des clusters invités de machine virtuelle comme indiqué dans cette rubrique. Ce type de déploiement offre un stockage partagé virtuel avec un ensemble de machines virtuelles sur un cloud privé ou public afin que les solutions de haute disponibilité d’application peuvent être utilisées pour augmenter la disponibilité des applications.

![](media/storage-spaces-direct-in-vm/storage-spaces-direct-in-vm.png)

## <a name="deploying-in-azure-iaas-vm-guest-clusters"></a>Déploiement de clusters invités de machine virtuelle Azure Iaas

[Les modèles Azure](https://github.com/robotechredmond/301-storage-spaces-direct-md) ont été publiée diminuer la complexité, configurez les meilleures pratiques et la vitesse de vos déploiements d’espaces de stockage Direct dans une machine virtuelle Azure Iaas. Il s’agit de la solution recommandée pour le déploiement dans Azure.

<iframe src="https://channel9.msdn.com/Series/Microsoft-Hybrid-Cloud-Best-Practices-for-IT-Pros/Step-by-Step-Deploy-Windows-Server-2016-Storage-Spaces-Direct-S2D-Cluster-in-Microsoft-Azure/player" width="960" height="540" allowfullscreen></iframe>

## <a name="requirements"></a>Configuration requise

Les considérations suivantes s’appliquent lors du déploiement d’espaces de stockage Direct dans un environnement virtualisé.

> [!TIP]
> Les modèles Azure configure automatiquement le ci-dessous les considérations pour vous et sont la solution recommandée lors du déploiement de machines virtuelles IaaS Azure.

-   Minimum de 2 nœuds et maximum de 3 nœuds

-   déploiements de 2 nœuds doivent configurer un témoin (témoin de Cloud ou témoin de partage de fichiers)

-   déploiements de 3 nœuds peuvent tolérer 1 nœud vers le bas et la perte de 1 ou plusieurs disques sur un autre nœud.  Si les 2 nœuds sont arrêtées ensuite les disques virtuels nous être hors connexion jusqu'à ce qu’un des nœuds retourne.  

-   Configurer les machines virtuelles puissent être déployées dans des domaines d’erreur

    -   Azure : configurer la haute disponibilité

    -   Hyper-V – AntiAffinityClassNames configurer sur les machines virtuelles pour séparer les machines virtuelles entre les nœuds

    -   VMware – règle configurer anti-affinité des machines virtuelles de la machine virtuelle en créant un type de Rule DRS » des Machines virtuelles distinctes » pour séparer les machines virtuelles sur les hôtes ESX. Disques présentés pour une utilisation avec les espaces de stockage Direct doivent utiliser l’adaptateur Paravirtual SCSI (PVSCSI). Pour un support PVSCSI avec Windows Server, consultez https://kb.vmware.com/s/article/1010398.

-   Tirer parti d’une latence faible / stockage hautes performances - stockage Premium Azure gérée disques sont nécessaires

-   Déployer une conception de stockage plat avec aucun appareil de mise en cache configuré

-   Minimum de 2 disques de données virtuel présentés à chaque machine virtuelle (VHD / VHDX / VMDK)

    Ce nombre est différent de celui des déploiements de systèmes nus, car les disques virtuels peuvent être implémentées en tant que fichiers qui ne sont pas vulnérables aux défaillances physiques.

-   Désactiver les fonctionnalités de remplacement automatique des lecteurs dans le Service de contrôle d’intégrité en exécutant l’applet de commande PowerShell suivante :

    ```powershell
    Get-storagesubsystem clus* | set-storagehealthsetting -name “System.Storage.PhysicalDisk.AutoReplace.Enabled” -value “False”
    ```

-   Non pris en charge : Capture instantanée de disque virtuel au niveau hôte/restauration

    Au lieu de cela, utilisez les solutions de sauvegarde au niveau invité traditionnel pour sauvegarder et restaurer les données sur les volumes d’espaces de stockage Direct.

-   Pour donner une résilience accrue au disque dur virtuel possible / VHDX / latence de stockage VMDK dans les clusters invités, augmentez la valeur de délai d’attente d’e/s de stockage espaces :

    `HKEY_LOCAL_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\spaceport\\Parameters\\HwTimeout`

    `dword: 00007530`

    L’équivalent décimal de 7530 hexadécimal est 30000, qui est de 30 secondes. Notez que la valeur par défaut est 1770 hexadécimal ou 6000 décimal, ce qui est de 6 secondes.

## <a name="see-also"></a>Voir aussi

[Autres modèles de machine virtuelle Azure Iaas pour déployer des espaces de stockage Direct, des vidéos et des guides étape par étape](https://blogs.msdn.microsoft.com/clustering/2017/02/14/deploying-an-iaas-vm-guest-clusters-in-microsoft-azure/).

[Vue d’ensemble de diriger les espaces de stockage supplémentaires] (https://docs.microsoft.com/en-us/windows-server/storage/storage-spaces/storage-spaces-direct-overview)
