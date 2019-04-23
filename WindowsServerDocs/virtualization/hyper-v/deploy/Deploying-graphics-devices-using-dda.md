---
title: Déployer des périphériques de graphiques à l’aide d’attribution discrète d’appareil
description: Découvrez comment utiliser DDA pour déployer des périphériques graphiques dans Windows Server
ms.prod: windows-server-threshold
ms.service: na
ms.technology: hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: chrishuybregts
ms.author: chrihu
ms.assetid: 67a01889-fa36-4bc6-841d-363d76df6a66
ms.openlocfilehash: 9e9a36df39c7bd7a96cc8c5681e83bf263ee5f8e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833870"
---
# <a name="deploy-graphics-devices-using-discrete-device-assignment"></a>Déployer des périphériques de graphiques à l’aide d’attribution discrète d’appareil

>S'applique à : Microsoft Hyper-V Server 2016, Windows Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019  

À compter de Windows Server 2016, vous pouvez utiliser affectation discrète d’appareils ou DDA, pour transmettre l’intégralité d’un appareil de PCIe dans une machine virtuelle.  Cela autorise l’accès hautes performances sur des appareils tels que [NVMe stockage](./Deploying-storage-devices-using-dda.md) ou cartes graphiques à partir d’une machine virtuelle tout en étant en mesure d’utiliser les pilotes de périphériques natives.  Visitez le [planifier pour les appareils de déploiement à l’aide d’affectation d’appareils discrètes](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md) pour plus d’informations sur les appareils de travail, quelles sont les implications en matière de sécurité possibles, etc.

Il existe trois étapes à l’utilisation d’un appareil avec attribution discrète d’appareil :
-   Configurer la machine virtuelle pour l’attribution discrète d’appareils
-   Démonter l’appareil à partir de la Partition hôte
-   Affectation de l’appareil à la machine virtuelle invitée

Tout (commande) peuvent être exécutée sur l’ordinateur hôte sur une console Windows PowerShell en tant qu’administrateur.

## <a name="configure-the-vm-for-dda"></a>Configurer la machine virtuelle pour DDA
Affectation d’appareils discrètes impose certaines restrictions sur les machines virtuelles et l’étape suivante doit être prise.

1.  Configurez le « arrêter Action automatique » d’une machine virtuelle à l’arrêt en exécutant

```
Set-VM -Name VMName -AutomaticStopAction TurnOff
```

### <a name="some-additional-vm-preparation-is-required-for-graphics-devices"></a>Certaines tâches de préparation de machine virtuelle supplémentaire est requis pour les périphériques graphiques

Certains matériels fonctionne mieux si la machine virtuelle dans configuré d’une certaine manière.  Pour plus d’informations sur ou non, vous devez les configurations suivantes pour votre matériel, veuillez contacter le fournisseur de matériel. Vous trouverez des détails supplémentaires sur [planifier pour les appareils de déploiement à l’aide d’affectation d’appareils discrètes](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md) et sur ce [billet de blog.](https://blogs.technet.microsoft.com/virtualization/2015/11/23/discrete-device-assignment-gpus/)

1.  Activer la combinaison d’écriture sur l’UC
```
Set-VM -GuestControlledCacheTypes $true -VMName VMName
```
2.  Configurer l’espace MMIO 32 bits
```
Set-VM -LowMemoryMappedIoSpace 3Gb -VMName VMName
```
3.  Configurer supérieur à 32 bits MMIO espace
```
Set-VM -HighMemoryMappedIoSpace 33280Mb -VMName VMName
```
Notez les valeurs d’espace MMIO ci-dessus sont des valeurs raisonnables à définir pour l’expérimentation avec une seule unité GPU.  Si après le démarrage de la machine virtuelle, l’appareil signale une erreur liée à des ressources insuffisantes, vous devrez probablement modifier ces valeurs.  En outre, si vous affectez plusieurs GPU, vous devrez augmenter ces valeurs ainsi.

## <a name="dismount-the-device-from-the-host-partition"></a>Démonter l’appareil à partir de la Partition hôte
### <a name="optional---install-the-partitioning-driver"></a>Facultatif : installez le pilote de partitionnement
Attribution discrète d’appareil permettent de vendeurs de matériel pour fournir un pilote d’atténuation de sécurité avec leurs appareils.  Notez que ce pilote n’est pas le même que le pilote de périphérique sera installé dans la machine virtuelle invitée.  Il a jusqu'à la discrétion du fabricant de matériel pour fournir ce pilote, toutefois, si elles ne fournissent pas, veuillez l’installer avant de démontage de l’appareil à partir de la partition hôte.  Veuillez contacter le fournisseur de matériel pour plus d’informations si un pilote d’atténuation
> Si aucun pilote de partitionnement n’est fournie, lors du démontage vous devez utiliser le `-force` option pour ignorer l’avertissement de sécurité. Veuillez en savoir plus sur les implications en matière de sécurité de sur [planifier pour les appareils de déploiement à l’aide d’affectation d’appareils discrètes](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md).

### <a name="locating-the-devices-location-path"></a>Recherche de chemin d’accès de l’appareil
Le chemin d’accès de l’emplacement de la norme PCI est requise pour démonter et monter l’appareil à partir de l’hôte.  Un chemin de localisation exemple ressemble à ceci : `"PCIROOT(20)#PCI(0300)#PCI(0000)#PCI(0800)#PCI(0000)"`.  Plus d’informations sur trouve le chemin d’accès d’emplacement peut trouver ici : [Planifier le déploiement des appareils à l’aide d’attribution discrète d’appareil](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md).

### <a name="disable-the-device"></a>Désactiver l’appareil
En utilisant le Gestionnaire de périphériques ou de PowerShell, assurez-vous de l’appareil est « disabled ».  

### <a name="dismount-the-device"></a>Démonter l’appareil
Selon si le fournisseur a fourni un pilote d’atténuation, vous soit devrez utiliser le «-force » option ou non.
-   Si un pilote d’atténuation a été installé.
```
Dismount-VMHostAssignableDevice -LocationPath $locationPath
```
-   Si un pilote d’atténuation n’a pas été installé.
```
Dismount-VMHostAssignableDevice -force -LocationPath $locationPath
```

## <a name="assigning-the-device-to-the-guest-vm"></a>Affectation de l’appareil à la machine virtuelle invitée
L’étape finale consiste à indiquer à Hyper-V qu’une machine virtuelle doit avoir accès à l’appareil.  Outre le chemin d’accès d’emplacement identifié précédemment, vous devez connaître le nom de la machine virtuelle.

```
Add-VMAssignableDevice -LocationPath $locationPath -VMName VMName
```

## <a name="whats-next"></a>Quelle est la suite
Une fois un appareil est correctement monté dans une machine virtuelle, vous êtes maintenant en mesure de démarrer cette machine virtuelle et interagir avec l’appareil, comme vous le feriez normalement si vous exécutiez sur un système de métal nu.  Cela signifie que vous êtes maintenant en mesure d’installer les pilotes du fabricant de matériel dans la machine virtuelle et applications seront en mesure de voir ce matériel présent.  Vous pouvez le vérifier en ouvrant le Gestionnaire de périphériques dans la machine virtuelle invitée et de voir que le matériel affiche maintenant.

## <a name="removing-a-device-and-returning-it-to-the-host"></a>Suppression d’un périphérique et retourner à l’hôte
Si vous souhaitez qu’il retourne un appareil à son état d’origine, vous devez arrêter la machine virtuelle et émettre les éléments suivants :
```
#Remove the device from the VM
Remove-VMAssignableDevice -LocationPath $locationPath -VMName VMName
#Mount the device back in the host
Mount-VMHostAssignableDevice -LocationPath $locationPath
```
Vous pouvez réactiver ensuite l’appareil dans le Gestionnaire de périphériques et le système d’exploitation hôte sera en mesure d’interagir avec l’appareil à nouveau.

## <a name="examples"></a>Exemples

### <a name="mounting-a-gpu-to-a-vm"></a>Le montage d’un GPU à une machine virtuelle
Dans cet exemple, nous utilisons PowerShell pour configurer une machine virtuelle nommée « ddatest1 » pour prendre le premier GPU disponible par le fabricant de NVIDIA et l’affecter à la machine virtuelle.  
```
#Configure the VM for a Discrete Device Assignment
$vm =   "ddatest1"
#Set automatic stop action to TurnOff
Set-VM -Name $vm -AutomaticStopAction TurnOff
#Enable Write-Combining on the CPU
Set-VM -GuestControlledCacheTypes $true -VMName $vm
#Configure 32 bit MMIO space
Set-VM -LowMemoryMappedIoSpace 3Gb -VMName $vm
#Configure Greater than 32 bit MMIO space
Set-VM -HighMemoryMappedIoSpace 33280Mb -VMName $vm

#Find the Location Path and disable the Device
#Enumerate all PNP Devices on the system
$pnpdevs = Get-PnpDevice -presentOnly
#Select only those devices that are Display devices manufactured by NVIDIA
$gpudevs = $pnpdevs |where-object {$_.Class -like "Display" -and $_.Manufacturer -like "NVIDIA"}
#Select the location path of the first device that's available to be dismounted by the host.
$locationPath = ($gpudevs | Get-PnpDeviceProperty DEVPKEY_Device_LocationPaths).data[0]
#Disable the PNP Device
Disable-PnpDevice  -InstanceId $gpudevs[0].InstanceId

#Dismount the Device from the Host
Dismount-VMHostAssignableDevice -force -LocationPath $locationPath

#Assign the device to the guest VM.
Add-VMAssignableDevice -LocationPath $locationPath -VMName $vm
```
