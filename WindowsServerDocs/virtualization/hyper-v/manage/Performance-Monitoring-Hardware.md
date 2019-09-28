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
ms.openlocfilehash: 6938739d7c8efdf60c859d2d5ea5bc63246ae4fe
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364101"
---
# <a name="enable-intel-performance-monitoring-hardware-in-a-hyper-v-virtual-machine"></a>Activer le matériel de surveillance des performances Intel sur une machine virtuelle Hyper-V

Les processeurs Intel contiennent des fonctionnalités collectivement appelées matériel de surveillance des performances (par exemple, PMU, PEBS, LBR). Ces fonctionnalités sont utilisées par les logiciels de réglage des performances tels que l’amplificateur Intel VTune pour analyser les performances des logiciels.  Avant Windows Server 2019 et Windows 10 version 1809, ni le système d’exploitation hôte ni les machines virtuelles invitées Hyper-V ne pouvaient utiliser le matériel de surveillance des performances lors de l’activation d’Hyper-V.  À compter de Windows Server 2019 et Windows 10 version 1809, le système d’exploitation hôte a accès au matériel de surveillance des performances par défaut.  Les machines virtuelles invitées Hyper-V n’ont pas accès par défaut, mais les administrateurs Hyper-V peuvent choisir d’accorder l’accès à une ou plusieurs machines virtuelles invitées.  Ce document décrit les étapes requises pour exposer le matériel de surveillance des performances aux machines virtuelles invitées.

## <a name="requirements"></a>Configuration requise

Pour activer le matériel de surveillance des performances sur une machine virtuelle, vous avez besoin des éléments suivants :

- Un processeur Intel avec matériel de surveillance des performances (par exemple, PMU, PEBS, IPT)
- Windows Server 2019 ou Windows 10 version 1809 (mise à jour d’octobre 2018) ou version ultérieure
- Un ordinateur virtuel Hyper-V _sans_ [virtualisation imbriquée](https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization) qui est également à l’état arrêté
 
## <a name="enabling-performance-monitoring-components-in-a-virtual-machine"></a>Activation des composants d’analyse des performances dans une machine virtuelle

Pour activer différents composants d’analyse des performances pour une machine virtuelle invitée spécifique `Set-VMProcessor` , utilisez l’applet de commande PowerShell :
 
``` Powershell
# Enable all components
Set-VMProcessor MyVMName -Perfmon @("pmu", "lbr", "pebs")
```
 
``` Powershell
# Enable a specific component
Set-VMProcessor MyVMName -Perfmon @("pmu")
```
 
``` Powershell
# Disable all components
Set-VMProcessor MyVMName -Perfmon @()
```
> [!NOTE]
> Lorsque vous activez les composants d’analyse des `"pebs"` performances, si est `"pmu"` spécifié, doit être spécifié.  En outre, l’activation d’un composant qui n’est pas pris en charge par les processeurs physiques de l’ordinateur hôte entraîne un échec de démarrage de la machine virtuelle.
 
## <a name="effects-of-enabling-performance-monitoring-hardware-on-saverestore-export-and-live-migration"></a>Effets de l’activation du matériel de surveillance des performances lors de l’enregistrement, de la restauration, de l’exportation et de la migration dynamique
 
Microsoft ne recommande pas de migrer dynamiquement ou d’enregistrer/de restaurer des machines virtuelles avec un matériel de surveillance des performances entre les systèmes dotés de matériel Intel différent. Le comportement spécifique du matériel de surveillance des performances est souvent non architectural et les modifications entre les systèmes matériels Intel.  Le déplacement d’un ordinateur virtuel en cours d’exécution entre différents systèmes peut entraîner un comportement imprévisible des compteurs non architecturaux.