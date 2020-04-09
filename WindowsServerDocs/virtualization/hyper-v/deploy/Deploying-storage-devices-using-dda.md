---
title: Déployer les appareils de stockage NVMe à l’aide de l’affectation discrète des appareils
description: Découvrez comment utiliser DDA pour déployer des dispositifs de stockage
ms.prod: windows-server
ms.technology: hyper-v
ms.topic: article
author: chrishuybregts
ms.author: chrihu
ms.assetid: 1c36107e-78c9-4ec0-a313-6ed557ac0ffc
ms.openlocfilehash: 2b92b175a6e914b62b069f76f92255cb99d55d74
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860902"
---
# <a name="deploy-nvme-storage-devices-using-discrete-device-assignment"></a>Déployer les appareils de stockage NVMe à l’aide de l’affectation discrète des appareils

>S’applique à : Microsoft Hyper-V Server 2016, Windows Server 2016

À compter de Windows Server 2016, vous pouvez utiliser l’affectation discrète des appareils, ou DDA, pour transmettre un appareil PCIe entier à une machine virtuelle.  Cela permet un accès très performant aux appareils tels que le stockage NVMe ou les cartes graphiques à partir d’une machine virtuelle tout en étant en mesure de tirer parti des pilotes natifs des appareils.  Pour plus d’informations sur les appareils qui fonctionnent, sur les implications sur la sécurité, consultez le [plan de déploiement d’appareils à l’aide de l’affectation discrète](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md) d’appareils, etc. L’utilisation d’un appareil avec DDA se présente en trois étapes :
-   Configurer la machine virtuelle pour DDA
-   Démonter l’appareil de la partition hôte
-   Attribution de l’appareil à la machine virtuelle invitée

Toutes les commandes peuvent être exécutées sur l’hôte d’une console Windows PowerShell en tant qu’administrateur.

## <a name="configure-the-vm-for-dda"></a>Configurer la machine virtuelle pour DDA
L’affectation discrète des appareils impose des restrictions aux machines virtuelles et l’étape suivante doit être effectuée.

1.  Configurez l’action d’arrêt automatique d’une machine virtuelle sur TurnOff en exécutant

```
Set-VM -Name VMName -AutomaticStopAction TurnOff
```

## <a name="dismount-the-device-from-the-host-partition"></a>Démonter l’appareil de la partition hôte

### <a name="locating-the-devices-location-path"></a>Recherche du chemin d’accès à l’emplacement de l’appareil
Le chemin d’accès à l’emplacement PCI est requis pour démonter et monter l’appareil à partir de l’ordinateur hôte.  Un exemple de chemin d’accès à l’emplacement ressemble à ce qui suit : `"PCIROOT(20)#PCI(0300)#PCI(0000)#PCI(0800)#PCI(0000)"`.   Pour plus d’informations sur le chemin d’accès de l’emplacement, cliquez ici : [planifier le déploiement des appareils à l’aide de l’attribution discrète des appareils](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md).

### <a name="disable-the-device"></a>Désactiver l’appareil
À l’aide de Device Manager ou de PowerShell, assurez-vous que l’appareil est « désactivé ».  

### <a name="dismount-the-device"></a>Démonter l’appareil
```
Dismount-VMHostAssignableDevice -LocationPath $locationPath
```

## <a name="assigning-the-device-to-the-guest-vm"></a>Attribution de l’appareil à la machine virtuelle invitée
La dernière étape consiste à dire à Hyper-V qu’une machine virtuelle doit avoir accès à l’appareil.  En plus du chemin d’accès d’emplacement indiqué ci-dessus, vous devez connaître le nom de la machine virtuelle.

```
Add-VMAssignableDevice -LocationPath $locationPath -VMName VMName
```

## <a name="whats-next"></a>Étape suivante
Une fois qu’un appareil est correctement monté sur une machine virtuelle, vous pouvez maintenant démarrer cette machine virtuelle et interagir avec l’appareil comme vous le feriez normalement si vous étiez en train d’exécuter sur un système nu.  Vous pouvez le vérifier en ouvrant le gestionnaire de périphériques sur la machine virtuelle invitée et en vérifiant que le matériel s’affiche à présent.

## <a name="removing-a-device-and-returning-it-to-the-host"></a>Suppression d’un appareil et retour à l’ordinateur hôte
Si vous souhaitez rétablir l’appareil à son état d’origine, vous devez arrêter la machine virtuelle et émettre les informations suivantes :
```
#Remove the device from the VM
Remove-VMAssignableDevice -LocationPath $locationPath -VMName VMName
#Mount the device back in the host
Mount-VMHostAssignableDevice -LocationPath $locationPath
```
Vous pouvez ensuite réactiver l’appareil dans le gestionnaire de périphériques et le système d’exploitation hôte pourra à nouveau interagir avec l’appareil.
