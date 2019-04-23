---
title: Applets de commande pour la configuration des périphériques de mémoire persistante pour les machines virtuelles Hyper-V
description: Comment configurer des dispositifs de mémoire persistant pour machines virtuelles Hyper-V
ms.prod: windows-server-threshold
ms.service: na
manager: jasgroce
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b5715c02-a90f-4de9-a71e-0fc08039ba1d
author: coreyp-at-msft
ms.author: coreyp
ms.openlocfilehash: fd1b04ce74f0b8d490529d2a7f65091f5847d0f4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878180"
---
# <a name="cmdlets-for-configuring-persistent-memory-devices-for-hyper-v-vms"></a>Applets de commande pour la configuration des périphériques de mémoire persistante pour les machines virtuelles Hyper-V

>S'applique à : Windows Server 2019

Cet article fournit aux administrateurs système et les professionnels de l’informatique avec les informations sur la configuration des machines virtuelles Hyper-V avec mémoire persistante (également appelé mémoire de classe de stockage ou NVDIMM). Dispositifs de mémoire persistante NVDIMM-N compatibles JDEC sont pris en charge dans Windows Server 2016 et Windows 10 et fournissent un accès au niveau des octets sur des appareils non volatile à très faible latence. Les dispositifs de mémoire persistants de machine virtuelle sont prises en charge dans Windows Server 2019. 

## <a name="create-a-persistent-memory-device-for-a-vm"></a>Créer un périphérique de mémoire persistante pour une machine virtuelle

Utilisez le **[New-VHD](https://docs.microsoft.com/powershell/module/hyper-v/new-vhd?view=win10-ps)** applet de commande pour créer un périphérique de mémoire persistante pour une machine virtuelle. L’appareil doit être créé sur un volume NTFS DAX existant.  La nouvelle extension de nom de fichier (.vhdpmem) est utilisée pour spécifier que l’appareil est un périphérique de mémoire persistante. Seul le format de fichier de disque dur virtuel fixe est pris en charge.

**Exemple :** `New-VHD d:\VMPMEMDevice1.vhdpmem -Fixed -SizeBytes 4GB`

## <a name="create-a-vm-with-a-persistent-memory-controller"></a>Créez une machine virtuelle avec un contrôleur de mémoire persistante



Utilisez le **applet de commande New-VM** pour créer une machine virtuelle 2 de génération avec la taille de mémoire spécifiée et le chemin d’accès à une image VHDX. Ensuite, utilisez **Add-VMPmemController** pour ajouter un contrôleur de mémoire persistante à une machine virtuelle.

**Exemple :** 
    
    New-VM -Name "ProductionVM1" -MemoryStartupBytes 1GB -VHDPath c:\vhd\BaseImage.vhdx

    Add-VMPmemController ProductionVM1x

## <a name="attach-a-persistent-memory-device-to-a-vm"></a>Attachez un appareil de mémoire persistante à une machine virtuelle

Utilisez **[Add-VMHardDiskDrive](https://docs.microsoft.com/powershell/module/hyper-v/add-vmharddiskdrive?view=win10-ps)** pour attacher un dispositif de mémoire persistante à une machine virtuelle

**Exemple :** `Add-VMHardDiskDrive ProductionVM1 PMEM -ControllerLocation 1 -Path D:\VPMEMDevice1.vhdpmem`

Dispositifs de mémoire persistante au sein d’une machine virtuelle Hyper-V apparaissent sous la forme d’un périphérique de mémoire persistante à être consommés et gérés par le système d’exploitation invité. Systèmes d’exploitation invités peuvent utiliser l’appareil en tant que bloc ou volume DAX. Lorsque les dispositifs de mémoire persistante au sein d’une machine virtuelle sont utilisés comme un volume DAX, ils bénéficient de faible latence au niveau des octets adresse-capacité de l’appareil hôte (aucune virtualisation d’e/s sur le chemin d’accès du code). 

>[!NOTE] 
>Mémoire persistante est uniquement pris en charge pour les machines virtuelles de génération 2 Hyper-V. Migration en direct et la Migration de stockage ne sont pas pris en charge pour les machines virtuelles avec mémoire persistante. Points de contrôle de production de machines virtuelles n’incluent pas l’état de la mémoire persistante. 