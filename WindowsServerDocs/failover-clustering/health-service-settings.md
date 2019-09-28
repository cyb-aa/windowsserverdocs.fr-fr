---
title: Paramètres de Service de contrôle d’intégrité
ms.prod: windows-server
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: ''
author: cosmosdarwin
ms.date: 08/14/2017
ms.openlocfilehash: 1b10d7c5432b10262e8df85d427f17f402d4f64a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361041"
---
# <a name="health-service-settings"></a>Paramètres de Service de contrôle d’intégrité
> S’applique à : Windows Server 2019, Windows Server 2016

Le Service de contrôle d’intégrité est une nouvelle fonctionnalité de Windows Server 2016 qui améliore la surveillance quotidienne et l’expérience opérationnelle pour les clusters exécutant espaces de stockage direct.

Un grand nombre des paramètres qui régissent le comportement du Service de contrôle d’intégrité sont exposés en tant que paramètres. Vous pouvez les modifier pour ajuster l’intensité des erreurs ou des actions, activer ou désactiver certains comportements, et bien plus encore.

Utilisez l’applet de commande PowerShell suivante pour définir ou modifier des paramètres.

### <a name="usage"></a>Utilisation

```PowerShell
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name <SettingName> -Value <Value>  
```

#### <a name="example"></a>Exemple

```PowerShell
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name "System.Storage.Volume.CapacityThreshold.Warning" -Value 70
```

### <a name="common-settings"></a>Paramètres communs

Certains paramètres couramment modifiés sont répertoriés ci-dessous, ainsi que leurs valeurs par défaut.

#### <a name="volume-capacity-threshold"></a>Seuil de capacité du volume

```
"System.Storage.Volume.CapacityThreshold.Enabled"  = True
"System.Storage.Volume.CapacityThreshold.Warning"  = 80
"System.Storage.Volume.CapacityThreshold.Critical" = 90
```

#### <a name="pool-reserve-capacity-threshold"></a>Seuil de capacité de réserve de pool

```
"System.Storage.StoragePool.CheckPoolReserveCapacity.Enabled" = True
```

#### <a name="physical-disk-lifecycle"></a>Cycle de vie du disque physique

```
"System.Storage.PhysicalDisk.AutoPool.Enabled"                             = True
"System.Storage.PhysicalDisk.AutoRetire.OnLostCommunication.Enabled"       = True
"System.Storage.PhysicalDisk.AutoRetire.OnUnresponsive.Enabled"            = True
"System.Storage.PhysicalDisk.AutoRetire.DelayMs"                           = 900000 (i.e. 15 minutes)
"System.Storage.PhysicalDisk.Unresponsive.Reset.CountResetIntervalSeconds" = 360 (i.e. 60 minutes)
"System.Storage.PhysicalDisk.Unresponsive.Reset.CountAllowed"              = 3
```

#### <a name="supported-components-document"></a>Document sur les composants pris en charge

Consultez la section précédente.

#### <a name="firmware-rollout"></a>Déploiement du microprogramme

```
"System.Storage.PhysicalDisk.AutoFirmwareUpdate.SingleDrive.Enabled"       = True
"System.Storage.PhysicalDisk.AutoFirmwareUpdate.RollOut.Enabled"           = True
"System.Storage.PhysicalDisk.AutoFirmwareUpdate.RollOut.LongDelaySeconds"  = 604800 (i.e. 7 days)
"System.Storage.PhysicalDisk.AutoFirmwareUpdate.RollOut.ShortDelaySeconds" = 86400 (i.e. 1 day)
"System.Storage.PhysicalDisk.AutoFirmwareUpdate.RollOut.LongDelayCount"    = 1
"System.Storage.PhysicalDisk.AutoFirmwareUpdate.RollOut.FailureTolerance"  = 3
```

#### <a name="platform--quiescence"></a>Plateforme/fichier

```
"Platform.Quiescence.MinDelaySeconds" = 120 (i.e. 2 minutes)
"Platform.Quiescence.MaxDelaySeconds" = 420 (i.e. 7 minutes)
```

#### <a name="metrics"></a>metrics

```
"System.Reports.ReportingPeriodSeconds" = 1
```

#### <a name="debugging"></a>Débogage

```
"System.LogLevel" = 4
```

## <a name="see-also"></a>Voir aussi

- [Service de contrôle d’intégrité dans Windows Server 2016](health-service-overview.md)
- [espaces de stockage direct dans Windows Server 2016](../storage/storage-spaces/storage-spaces-direct-overview.md)
