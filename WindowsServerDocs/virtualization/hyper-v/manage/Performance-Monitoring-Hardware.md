---
title: Activer le matériel de surveillance des performances Intel sur une machine virtuelle Hyper-V
description: Comment activer le matériel de surveillance des performances d’Intel sur un ordinateur Hyper-V. Explique également comment activer la migration dynamique des effets matériels de surveillance des performances.
ms.prod: windows-server
ms.reviewer: ifufondu
author: ifeomaufondu-ms
ms.author: ifufondu
manager: chhuybre
ms.topic: article
ms.date: 09/20/2019
ms.openlocfilehash: 515831df6b97271b52c4a715fd979f2afff4a3a1
ms.sourcegitcommit: f73662069329b1abf6aa950c2a826bc113718857
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/31/2019
ms.locfileid: "73240344"
---
# <a name="enable-intel-performance-monitoring-hardware-in-a-hyper-v-virtual-machine"></a>Activer le matériel de surveillance des performances Intel sur une machine virtuelle Hyper-V

Les processeurs Intel contiennent des fonctionnalités collectivement appelées matériel de surveillance des performances (par exemple, PMU, PEBS, LBR). Ces fonctionnalités sont utilisées par les logiciels de réglage des performances tels que l’amplificateur Intel VTune pour analyser les performances des logiciels.  Avant Windows Server 2019 et Windows 10 version 1809, ni le système d’exploitation hôte ni les machines virtuelles invitées Hyper-V ne pouvaient utiliser le matériel de surveillance des performances lors de l’activation d’Hyper-V.  À compter de Windows Server 2019 et Windows 10 version 1809, le système d’exploitation hôte a accès au matériel de surveillance des performances par défaut.  Les machines virtuelles invitées Hyper-V n’ont pas accès par défaut, mais les administrateurs Hyper-V peuvent choisir d’accorder l’accès à une ou plusieurs machines virtuelles invitées.  Ce document décrit les étapes requises pour exposer le matériel de surveillance des performances aux machines virtuelles invitées.

## <a name="requirements"></a>Conditions préalables

Pour activer le matériel de surveillance des performances sur une machine virtuelle, vous avez besoin des éléments suivants :

- Un processeur Intel avec matériel de surveillance des performances (par exemple, PMU, PEBS, LBR).  Reportez-vous à [ce document]( https://software.intel.com/en-us/vtune-amplifier-cookbook-configuring-a-hyper-v-virtual-machine-for-hardware-based-hotspots-analysis) d’Intel pour déterminer le matériel de surveillance des performances que votre système prend en charge.
- Windows Server 2019 ou Windows 10 version 1809 (mise à jour d’octobre 2018) ou version ultérieure
- Un ordinateur virtuel Hyper-V _sans_ [virtualisation imbriquée](https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization) qui est également à l’état arrêté

Pour activer le matériel de surveillance des performances IPT (suivi du processeur Intel) à venir sur une machine virtuelle, vous avez besoin des éléments suivants :

- Un processeur Intel qui prend en charge IPT et la fonctionnalité PT2GPA.  Reportez-vous à [ce document]( https://software.intel.com/en-us/vtune-amplifier-cookbook-configuring-a-hyper-v-virtual-machine-for-hardware-based-hotspots-analysis) d’Intel pour déterminer le matériel de surveillance des performances que votre système prend en charge.
- Windows Server version 1903 (SAC) ou Windows 10 version 1903 (mise à jour de mai 2019) ou version ultérieure
- Un ordinateur virtuel Hyper-V _sans_ [virtualisation imbriquée](https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization) qui est également à l’état arrêté

## <a name="enabling-performance-monitoring-components-in-a-virtual-machine"></a>Activation des composants d’analyse des performances dans une machine virtuelle

Pour activer différents composants d’analyse des performances pour une machine virtuelle invitée spécifique, utilisez l’applet de commande `Set-VMProcessor` PowerShell lors de l’exécution en tant qu’administrateur :

``` Powershell
# Enable all components except IPT
Set-VMProcessor MyVMName -Perfmon @("pmu", "lbr", "pebs")
```

``` Powershell
# Enable a specific component
Set-VMProcessor MyVMName -Perfmon @("pmu")
```

``` Powershell
# Enable IPT 
Set-VMProcessor MyVMName -Perfmon @("ipt")
```

``` Powershell
# Disable all components
Set-VMProcessor MyVMName -Perfmon @()
```
> [!NOTE]
> Lorsque vous activez les composants d’analyse des performances, si `"pebs"` est spécifié, `"pmu"` doit également être spécifié. PEBS est uniquement pris en charge sur le matériel doté d’une version PMU > = 4. L’activation d’un composant qui n’est pas pris en charge par les processeurs physiques de l’ordinateur hôte entraîne l’échec du démarrage de la machine virtuelle.

## <a name="effects-of-enabling-performance-monitoring-hardware-on-saverestore-export-and-live-migration"></a>Effets de l’activation du matériel de surveillance des performances lors de l’enregistrement, de la restauration, de l’exportation et de la migration dynamique

Microsoft ne recommande pas de migrer dynamiquement ou d’enregistrer/de restaurer des machines virtuelles avec un matériel de surveillance des performances entre les systèmes dotés de matériel Intel différent. Le comportement spécifique du matériel de surveillance des performances est souvent non architectural et les modifications entre les systèmes matériels Intel.  Le déplacement d’un ordinateur virtuel en cours d’exécution entre différents systèmes peut entraîner un comportement imprévisible des compteurs non architecturaux.

