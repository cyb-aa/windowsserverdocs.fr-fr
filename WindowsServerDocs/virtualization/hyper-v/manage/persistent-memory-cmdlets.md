---
title: Applets de commande pour la configuration des périphériques de mémoire persistante pour les machines virtuelles Hyper-V
description: Comment configurer des appareils de mémoire persistants pour les machines virtuelles Hyper-V
ms.prod: windows-server
ms.service: na
manager: jasgroce
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b5715c02-a90f-4de9-a71e-0fc08039ba1d
author: coreyp-at-msft
ms.author: coreyp
ms.openlocfilehash: ecae1fe96bc5088fa840c6e2e24a75bb72a9e8f3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392535"
---
# <a name="cmdlets-for-configuring-persistent-memory-devices-for-hyper-v-vms"></a>Applets de commande pour la configuration des périphériques de mémoire persistante pour les machines virtuelles Hyper-V

>S'applique à : Windows Server 2019

Cet article fournit aux administrateurs système et aux professionnels de l’informatique des informations sur la configuration des machines virtuelles Hyper-V avec une mémoire persistante (également appelée mémoire de classe de stockage ou NVDIMM). Les périphériques de mémoire persistante NVDIMM-N compatibles JDEC sont pris en charge dans Windows Server 2016 et Windows 10, et fournissent un accès au niveau des octets à des appareils non volatiles à faible latence. Les périphériques de mémoire persistante de machine virtuelle sont pris en charge dans Windows Server 2019. 

## <a name="create-a-persistent-memory-device-for-a-vm"></a>Créer un périphérique de mémoire persistante pour une machine virtuelle

Utilisez l’applet de commande **[New-VHD](https://docs.microsoft.com/powershell/module/hyper-v/new-vhd?view=win10-ps)** pour créer un périphérique de mémoire persistante pour une machine virtuelle. L’appareil doit être créé sur un volume DAX NTFS existant.  La nouvelle extension de nom de fichier (. vhdpmem) est utilisée pour spécifier que l’appareil est un périphérique de mémoire persistante. Seul le format de fichier VHD fixe est pris en charge.

**Exemple :** `New-VHD d:\VMPMEMDevice1.vhdpmem -Fixed -SizeBytes 4GB`

## <a name="create-a-vm-with-a-persistent-memory-controller"></a>Créer une machine virtuelle avec un contrôleur de mémoire persistante



Utilisez l' **applet de commande New-VM** pour créer une machine virtuelle de génération 2 avec la taille de mémoire et le chemin d’accès spécifiés à une image VHDX. Ensuite, utilisez **Add-VMPmemController** pour ajouter un contrôleur de mémoire persistante à une machine virtuelle.

**Tels** 
    
    New-VM -Name "ProductionVM1" -MemoryStartupBytes 1GB -VHDPath c:\vhd\BaseImage.vhdx

    Add-VMPmemController ProductionVM1x

## <a name="attach-a-persistent-memory-device-to-a-vm"></a>Attacher un appareil de mémoire persistante à une machine virtuelle

Utiliser **[Add-VMHardDiskDrive](https://docs.microsoft.com/powershell/module/hyper-v/add-vmharddiskdrive?view=win10-ps)** pour attacher un appareil de mémoire persistante à une machine virtuelle

**Exemple :** `Add-VMHardDiskDrive ProductionVM1 PMEM -ControllerLocation 1 -Path D:\VPMEMDevice1.vhdpmem`

Les périphériques de mémoire persistants au sein d’une machine virtuelle Hyper-V apparaissent sous la forme d’un périphérique de mémoire persistante à consommer et à gérer par le système d’exploitation invité. Les systèmes d’exploitation invités peuvent utiliser l’appareil comme un bloc ou un volume DAX. Lorsque des appareils de mémoire persistants au sein d’une machine virtuelle sont utilisés comme volume DAX, ils tirent parti de la faible latence des adresses de niveau octet du périphérique hôte (aucune virtualisation d’e/s sur le chemin de code). 

>[!NOTE] 
>La mémoire persistante est uniquement prise en charge pour les machines virtuelles Hyper-V Gen. Les Migration dynamique et la migration du stockage ne sont pas prises en charge pour les machines virtuelles avec une mémoire persistante. Les points de contrôle de production des machines virtuelles n’incluent pas l’état de la mémoire persistante. 