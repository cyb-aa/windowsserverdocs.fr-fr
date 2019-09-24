---
title: Exposition du matériel de surveillance des performances Intel à une machine virtuelle Hyper-V
description: Décrit comment exposer le matériel Monitorning de performances d’Intel à un ordinateur Hyper-V. Aborde également le fonctionnement de la migration dynamique des effets d’activation.
ms.prod: windows-server-threshold
ms.reviewer: ifufondu
author: ifeomaufondu-ms
ms.author: ifufondu
manager: chhuybre
ms.topic: article
ms.date: 09/20/2019
ms.openlocfilehash: 1bc821cc46a3a402778e1c76f4a2d8feb862fd75
ms.sourcegitcommit: 45415ba58907d650cfda45f4c57f6ddf1255dcbf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71213615"
---
# <a name="exposing-intel-performance-monitoring-hardware-to-a-hyper-v-virtual-machine"></a>Exposition du matériel de surveillance des performances Intel à une machine virtuelle Hyper-V
 
## <a name="background"></a>Présentation
Les processeurs Intel contiennent des fonctionnalités collectivement appelées matériel de surveillance des performances (par exemple, PMU, PEBS, LBR). Ces fonctionnalités sont utilisées par les logiciels de réglage des performances tels que l’amplificateur Intel VTune pour analyser les performances des logiciels.  Avant Windows Server 2019 et Windows 10 version 1809, ni le système d’exploitation hôte ni les machines virtuelles invitées Hyper-V ne pouvaient utiliser le matériel de surveillance des performances lors de l’activation d’Hyper-V.  À compter de Windows Server 2019 et Windows 10 version 1809, le système d’exploitation hôte a accès au matériel de surveillance des performances par défaut.  Les machines virtuelles invitées Hyper-V n’ont pas accès par défaut, mais les administrateurs Hyper-V peuvent choisir d’accorder l’accès à une ou plusieurs machines virtuelles invitées.  Ce document décrit les étapes requises pour exposer le matériel de surveillance des performances aux machines virtuelles invitées.
 
## <a name="requirements"></a>Configuration requise 
Pour exposer le matériel de surveillance des performances à une machine virtuelle, vous avez besoin des éléments suivants :
- Un processeur Intel avec matériel de surveillance des performances (par exemple, PMU, PEBS, IPT)
- Windows Server 2019 ou Windows 10 version 1809 (mise à jour d’octobre 2018) ou version ultérieure
- Un ordinateur virtuel Hyper-V _sans_ [virtualisation imbriquée](https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization) qui est à l’état arrêté
 
## <a name="exposing-the-pmu-capabilities-pmu-lbr-pebs-to-virtual-machines-via-powershells-set-vmprocessor-cmdlet"></a>Exposition des fonctionnalités PMU (PMU, LBR, PEBS) aux machines virtuelles par le biais de l’applet de commande Set-VMProcessor de PowerShell
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
 
>Remarque : Lorsque vous activez les composants Perfmon, `"pebs"` si est spécifié `"pmu"` , doit être spécifié.  En outre, l’activation d’un composant Perfmon qui n’est pas pris en charge par les processeurs physiques de l’ordinateur hôte entraîne un échec de démarrage de la machine virtuelle.
 
## <a name="effect-of-pmu-enabelement-on-saverestore-export-and-live-migration"></a>Effet de l’enPMU de mise à niveau de l’enregistrement, de la restauration, de l’exportation et de la migration dynamique
 
Microsoft ne recommande pas de migrer dynamiquement ou d’enregistrer/de restaurer des machines virtuelles avec un matériel de surveillance des performances entre les systèmes dotés de matériel Intel différent. Le comportement spécifique du matériel de surveillance des performances est souvent non architectural et les modifications entre les systèmes matériels Intel.  Le déplacement d’un ordinateur virtuel en cours d’exécution entre différents systèmes peut entraîner un comportement imprévisible des compteurs non architecturaux.