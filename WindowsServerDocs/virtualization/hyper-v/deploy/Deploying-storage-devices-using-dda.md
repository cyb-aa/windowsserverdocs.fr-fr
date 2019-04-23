---
title: Déployer des dispositifs de stockage NVMe à l’aide d’attribution discrète d’appareil
description: Découvrez comment utiliser DDA pour déployer des périphériques de stockage
ms.prod: windows-server-threshold
ms.service: na
ms.technology: hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: chrishuybregts
ms.author: chrihu
ms.assetid: 1c36107e-78c9-4ec0-a313-6ed557ac0ffc
ms.openlocfilehash: d6fe54789d37386d5dc782ef8a2ca26b47adc69e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841360"
---
# <a name="deploy-nvme-storage-devices-using-discrete-device-assignment"></a>Déployer des dispositifs de stockage NVMe à l’aide d’attribution discrète d’appareil

>S'applique à : Microsoft Hyper-V Server 2016, Windows Server 2016

À compter de Windows Server 2016, vous pouvez utiliser affectation discrète d’appareils ou DDA, pour transmettre l’intégralité d’un appareil de PCIe dans une machine virtuelle.  Cela autorise l’accès hautes performances pour les périphériques de stockage de NVMe cartes graphiques à partir d’une machine virtuelle tout en étant en mesure d’utiliser les pilotes de périphériques natives.  Visitez le [planifier pour les appareils de déploiement à l’aide d’affectation d’appareils discrètes](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md) pour plus d’informations sur les appareils de travail, quelles sont les implications en matière de sécurité possibles, etc. Il existe à l’aide d’un appareil avec DDA en trois étapes :
-   Configurer la machine virtuelle pour DDA
-   Démonter l’appareil à partir de la Partition hôte
-   Affectation de l’appareil à la machine virtuelle invitée

Tout (commande) peuvent être exécutée sur l’ordinateur hôte sur une console Windows PowerShell en tant qu’administrateur.

## <a name="configure-the-vm-for-dda"></a>Configurer la machine virtuelle pour DDA
Affectation d’appareils discrètes impose certaines restrictions sur les machines virtuelles et l’étape suivante doit être prise.

1.  Configurez le « arrêter Action automatique » d’une machine virtuelle à l’arrêt en exécutant

```
Set-VM -Name VMName -AutomaticStopAction TurnOff
```

## <a name="dismount-the-device-from-the-host-partition"></a>Démonter l’appareil à partir de la Partition hôte

### <a name="locating-the-devices-location-path"></a>Recherche de chemin d’accès de l’appareil
Le chemin d’accès de l’emplacement de la norme PCI est requise pour démonter et monter l’appareil à partir de l’hôte.  Un chemin de localisation exemple ressemble à ceci : `"PCIROOT(20)#PCI(0300)#PCI(0000)#PCI(0800)#PCI(0000)"`.   Plus d’informations sur trouve le chemin d’accès d’emplacement peut trouver ici : [Planifier le déploiement des appareils à l’aide d’attribution discrète d’appareil](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md).

### <a name="disable-the-device"></a>Désactiver l’appareil
En utilisant le Gestionnaire de périphériques ou de PowerShell, assurez-vous de l’appareil est « disabled ».  

### <a name="dismount-the-device"></a>Démonter l’appareil
```
Dismount-VMHostAssignableDevice -LocationPath $locationPath
```

## <a name="assigning-the-device-to-the-guest-vm"></a>Affectation de l’appareil à la machine virtuelle invitée
L’étape finale consiste à indiquer à Hyper-V qu’une machine virtuelle doit avoir accès à l’appareil.  Outre le chemin d’accès d’emplacement identifié précédemment, vous devez connaître le nom de la machine virtuelle.

```
Add-VMAssignableDevice -LocationPath $locationPath -VMName VMName
```

## <a name="whats-next"></a>Quelle est la suite
Une fois un appareil est correctement monté dans une machine virtuelle, vous êtes maintenant en mesure de démarrer cette machine virtuelle et interagir avec l’appareil, comme vous le feriez normalement si vous exécutiez sur un système de métal nu.  Vous pouvez le vérifier en ouvrant le Gestionnaire de périphériques dans la machine virtuelle invitée et de voir que le matériel affiche maintenant.

## <a name="removing-a-device-and-returning-it-to-the-host"></a>Suppression d’un périphérique et retourner à l’hôte
Si vous souhaitez qu’il retourne un appareil à son état d’origine, vous devez arrêter la machine virtuelle et émettre les éléments suivants :
```
#Remove the device from the VM
Remove-VMAssignableDevice -LocationPath $locationPath -VMName VMName
#Mount the device back in the host
Mount-VMHostAssignableDevice -LocationPath $locationPath
```
Vous pouvez réactiver ensuite l’appareil dans le Gestionnaire de périphériques et le système d’exploitation hôte sera en mesure d’interagir avec l’appareil à nouveau.
