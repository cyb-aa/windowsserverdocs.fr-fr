---
title: L’utilisation d’espaces de stockage Direct dans une machine virtuelle
ms.prod: windows-server-threshold
ms.author: eldenc
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 10/25/2017
description: Découvrez comment déployer des espaces de stockage Direct dans un cluster invité de machine virtuelle, par exemple, dans Microsoft Azure.
ms.localizationpriority: medium
ms.openlocfilehash: a741475d09ab7f7ab29f158ef55378ca9a37beac
ms.sourcegitcommit: 52883945fd6e6bb5c24bd81944ca4bc0c5e6a216
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2018
ms.locfileid: "8964611"
---
# L’utilisation d’espaces de stockage Direct dans des clusters de machine virtuelle invité

> S’applique à: Windows Server 2019, Windows Server 2016

Vous pouvez déployer les espaces de stockage Direct sur un cluster de serveurs physiques, ou sur les clusters d’invité de machine virtuelle comme indiqué dans cette rubrique. Ce type de déploiement offre un stockage partagé virtuel sur un ensemble d’ordinateurs virtuels sur un cloud privé ou public afin que les solutions de haute disponibilité d’application peuvent être utilisées pour accroître la disponibilité des applications.

![](media/storage-spaces-direct-in-vm/storage-spaces-direct-in-vm.png)

## Déploiement de clusters invités de machine virtuelle de Azure Iaas

[Modèles Azure](https://github.com/robotechredmond/301-storage-spaces-direct-md) ont été publiée diminuer la complexité, configurer les meilleures pratiques et la vitesse de vos déploiements d’espaces de stockage Direct dans une machine virtuelle Iaas Azure. Il s’agit de la solution recommandée pour le déploiement dans Azure.

<iframe src="https://channel9.msdn.com/Series/Microsoft-Hybrid-Cloud-Best-Practices-for-IT-Pros/Step-by-Step-Deploy-Windows-Server-2016-Storage-Spaces-Direct-S2D-Cluster-in-Microsoft-Azure/player" width="960" height="540" allowfullscreen></iframe>

## Spécifications

Les considérations suivantes s’appliquent lors du déploiement d’espaces de stockage Direct dans un environnement virtualisé.

> [!TIP]
> Modèles Azure va configurer automatiquement le ci-dessous considérations pour vous et sont la solution recommandée lors du déploiement de machines virtuelles Azure IaaS.

-   2 nœuds au minimum et maximum de 3 nœuds

-   les déploiements de 2 nœuds doivent configurer un témoin (témoin de Cloud ou un témoin de partage de fichiers)

-   les déploiements de 3 nœuds peuvent tolérer 1 nœud vers le bas et la perte de 1 ou plusieurs disques sur un autre nœud.  Si 2 nœuds sont arrêtés alors les disques virtuels nous en hors connexion jusqu'à ce qu’un des nœuds retourne.  

-   Configurer les ordinateurs virtuels à être déployé sur les domaines d’erreur

    -   Azure – configurer la disponibilité

    -   Hyper-V – AntiAffinityClassNames configurer sur les ordinateurs virtuels de séparer les machines virtuelles entre les nœuds

    -   VMware – règle de configurer une machine virtuelle-autre anti affinité en créant une Rule DRS de type «des Machines virtuelles séparées» pour séparer les machines virtuelles sur les hôtes ESX. Disques présentés pour une utilisation avec les espaces de stockage Direct doivent utiliser l’adaptateur SCSI Paravirtual (PVSCSI). Pour un support PVSCSI avec Windows Server, consultez https://kb.vmware.com/s/article/1010398.

-   Tirez parti de faible latence / de gestion du stockage de haute performance - le stockage Azure Premium disques sont nécessaires

-   Déployer une conception de stockage plate avec aucun appareil de la mise en cache configuré

-   Minimum de 2 disques de données virtuel présentées à chaque ordinateur virtuel (VHD / VHDX / VMDK)

    Ce nombre est différent de celui les déploiements complets dans la mesure où les disques virtuels peuvent être implémentés sous forme de fichiers qui ne sont pas susceptibles de subir des échecs physiques.

-   Désactiver les fonctionnalités de remplacement automatique des lecteurs dans le Service d’intégrité en exécutant l’applet de commande PowerShell suivante:

    ```powershell
    Get-storagesubsystem clus* | set-storagehealthsetting -name “System.Storage.PhysicalDisk.AutoReplace.Enabled” -value “False”
    ```

-   Non pris en charge: héberger le disque virtuel niveau instantané/restauration

    Au lieu de cela, utilisez des solutions de sauvegarde au niveau invité classique pour sauvegarder et restaurer les données sur les volumes d’espaces de stockage Direct.

-   Pour donner une plus grande résilience à possible VHD / VHDX / latence de stockage VMDK dans des clusters invités, augmentez la valeur de délai d’expiration d’e/s des espaces de stockage:

    `HKEY_LOCAL_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\spaceport\\Parameters\\HwTimeout`

    `dword: 00007530`

    L’équivalent décimal 7530 hexadécimal est 30000, qui est de 30 secondes. Notez que la valeur par défaut est 1770 en notation hexadécimale ou 6000 décimal, ce qui est de 6 secondes.

## Voir aussi

[Modèles supplémentaires Azure Iaas VM pour les espaces de stockage déploiement Direct, des vidéos et des guides pas à pas](https://blogs.msdn.microsoft.com/clustering/2017/02/14/deploying-an-iaas-vm-guest-clusters-in-microsoft-azure/).

[Les espaces de stockage supplémentaire Direct vue d’ensemble] (https://docs.microsoft.com/en-us/windows-server/storage/storage-spaces/storage-spaces-direct-overview)
