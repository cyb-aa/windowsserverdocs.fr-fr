---
title: Collecter les données de diagnostic avec des espaces de stockage Direct
description: Présentation de stockage espaces Direct Data Collection Tools, avec des exemples spécifiques de l’exécution et de les utiliser.
keywords: Espaces de stockage, la collecte de données, résolution des problèmes, chaînes d’événements, Get-SDDCDiagnosticInfo
ms.assetid: ''
ms.prod: windows-server-threshold
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 10/24/2018
ms.localizationpriority: ''
ms.openlocfilehash: eaa7d92fe6f77697614cacf1405a25e5a42e14b7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880270"
---
# <a name="collect-diagnostic-data-with-storage-spaces-direct"></a>Collecter les données de diagnostic avec des espaces de stockage Direct

> S’applique à : Windows Server 2019, Windows Server 2016

Il existe différents outils de diagnostic qui peuvent être utilisés pour collecter les données nécessaires pour résoudre les espaces de stockage Direct et Cluster de basculement. Dans cet article, nous allons nous concentrer sur **Get-SDDCDiagnosticInfo** -un outil une pression tactile qui rassemble toutes les informations pertinentes pour vous aider à diagnostiquer votre cluster.

<!-- The health summary report is a great start to understanding the status of your system to start diagnosing an issue. -->

Étant donné que les journaux et autres informations qui **Get-SDDCDiagnosticInfo** sont dense, les informations sur la résolution des problèmes présentés ci-dessous vous sera utiles pour le dépannage avancé des problèmes qui ont été réaffectés et qui peut nécessitent des données à envoyer à Microsoft pour le triage.

<!--
## Collecting live dumps

Windows will trigger the collection of a ``` LiveDump ``` when there are known resources that are hanging in kernel calls. ``` RHS ``` will trigger ```LiveDump``` collection if both the resource type and cluster ``` DumpPolicy ``` are set to 1. For physical disk it is set out of the box
-->

## <a name="installing-get-sddcdiagnosticinfo"></a>L’installation de Get-SDDCDiagnosticInfo

Le **Get-SDDCDiagnosticInfo** applet de commande PowerShell (également appelé) **Get-PCStorageDiagnosticInfo**, précédemment appelé **Test-StorageHealth**) peut être utilisé pour collecter des journaux et effectuer des contrôles d’intégrité pour le Clustering de basculement (Cluster, ressources, réseaux, nœuds), (des espaces de stockage Disques physiques, des boîtiers, les disques virtuels), Cluster partagés Volumes, partages de fichiers SMB et la déduplication. 

Il existe deux méthodes d’installation de ce script, qui sont tous deux décrit ci-dessous.

### <a name="powershell-gallery"></a>PowerShell Gallery

Le [PowerShell Gallery](https://www.powershellgallery.com/packages/PrivateCloud.DiagnosticInfo) est un instantané du référentiel GitHub. Notez que l’installation des éléments à partir de PowerShell Gallery nécessite la dernière version du module PowerShellGet, qui est disponible dans Windows 10, Windows Management Framework (WMF) 5.0 ou dans le programme d’installation basé sur MSI (pour PowerShell 3 et 4).

Vous pouvez installer le module en exécutant la commande suivante dans PowerShell avec des privilèges d’administrateur :

``` PowerShell
Install-PackageProvider NuGet -Force
Install-Module PrivateCloud.DiagnosticInfo -Force
Import-Module PrivateCloud.DiagnosticInfo -Force
```

Pour mettre à jour le module, exécutez la commande suivante dans PowerShell :

``` PowerShell
Update-Module PrivateCloud.DiagnosticInfo
```

### <a name="github"></a>GitHub

Le [référentiel GitHub](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/) est la version la plus récente du module, dans la mesure où nous sommes en permanence effectuant une itération ici. Pour installer le module à partir de GitHub, téléchargez le dernier module à partir de la [archive](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/archive/master.zip) et extraire le répertoire PrivateCloud.DiagnosticInfo référencée par le chemin d’accès correct du modules PowerShell ```$env:PSModulePath```

``` PowerShell
# Allowing Tls12 and Tls11 -- e.g. github now requires Tls12
# If this is not set, the Invoke-WebRequest fails with "The request was aborted: Could not create SSL/TLS secure channel."
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
$module = 'PrivateCloud.DiagnosticInfo'
Invoke-WebRequest -Uri https://github.com/PowerShell/$module/archive/master.zip -OutFile $env:TEMP\master.zip
Expand-Archive -Path $env:TEMP\master.zip -DestinationPath $env:TEMP -Force
if (Test-Path $env:SystemRoot\System32\WindowsPowerShell\v1.0\Modules\$module) {
    rm -Recurse $env:SystemRoot\System32\WindowsPowerShell\v1.0\Modules\$module -ErrorAction Stop
    Remove-Module $module -ErrorAction SilentlyContinue
} else {
    Import-Module $module -ErrorAction SilentlyContinue
} 
if (-not ($m = Get-Module $module -ErrorAction SilentlyContinue)) {
    $md = "$env:ProgramFiles\WindowsPowerShell\Modules"
} else {
    $md = (gi $m.ModuleBase -ErrorAction SilentlyContinue).PsParentPath
    Remove-Module $module -ErrorAction SilentlyContinue
    rm -Recurse $m.ModuleBase -ErrorAction Stop
}
cp -Recurse $env:TEMP\$module-master\$module $md -Force -ErrorAction Stop
rm -Recurse $env:TEMP\$module-master,$env:TEMP\master.zip
Import-Module $module -Force

``` 

Si vous avez besoin obtenir ce module sur un cluster en mode hors connexion, téléchargez le fichier zip, déplacez-le vers le nœud de votre serveur cible et installez le module.

## <a name="gathering-logs"></a>Collecte des journaux

Une fois que vous avez activé des canaux d’événement et le processus d’installation terminé, vous pouvez utiliser l’applet de commande Get-SDDCDiagnosticInfo PowerShell dans le module pour obtenir :

- Rapports sur l’intégrité du stockage, ainsi que des détails sur les composants défectueux
- Rapports de la capacité de stockage par pool, volume et volume dédupliqué
- Journaux des événements à partir de tous les nœuds de cluster et un rapport de résumé des erreurs

Supposons que votre cluster de stockage porte le nom *« CLUS01 ».*

À exécuter sur un cluster de stockage à distance :

``` PowerShell
Get-SDDCDiagnosticInfo -ClusterName CLUS01
```

Pour exécuter localement sur le nœud de stockage en cluster :

``` PowerShell
Get-SDDCDiagnosticInfo
```

Pour enregistrer les résultats dans un dossier spécifié :

``` PowerShell
Get-SDDCDiagnosticInfo -WriteToPath D:\Folder 
```

Voici un exemple à quoi cela ressemble sur un cluster réel :

``` PowerShell
New-Item -Name SDDCDiagTemp -Path d:\ -ItemType Directory -Force
Get-SddcDiagnosticInfo -ClusterName S2D-Cluster -WriteToPath d:\SDDCDiagTemp
```

Comme vous pouvez le voir, script effectuent également une validation de l’état actuel du cluster

![capture d’écran de données collection powershell](media/data-collection/CollectData.png)

Comme vous pouvez le voir, toutes les données sont écrits dans le dossier de SDDCDiagTemp

![données dans la capture d’écran de l’Explorateur de fichiers](media/data-collection/CollectDataFolder.png)

Une fois le script se termine, ZIP est créé dans votre répertoire d’utilisateurs

![fichier zip de données dans la capture d’écran de powershell](media/data-collection/CollectDataResult.png)

Nous allons générer un rapport dans un fichier texte

```PowerShell
#find the latest diagnostic zip in UserProfile
    $DiagZip=(get-childitem $env:USERPROFILE | where Name -like HealthTest*.zip)
    $LatestDiagPath=($DiagZip | sort lastwritetime | select -First 1).FullName
#expand to temp directory
    New-Item -Name SDDCDiagTemp -Path d:\ -ItemType Directory -Force
    Expand-Archive -Path $LatestDiagPath -DestinationPath D:\SDDCDiagTemp -Force
#generate report and save to text file
    $report=Show-SddcDiagnosticReport -Path D:\SDDCDiagTemp
    $report | out-file d:\SDDCReport.txt
    
```

Pour référence, Voici un lien vers le [exemple de rapport](https://github.com/Microsoft/WSLab/blob/dev/Scenarios/S2D%20Tools/Get-SDDCDiagnosticInfo/SDDCReport.txt) et [exemple zip](https://github.com/Microsoft/WSLab/blob/dev/Scenarios/S2D%20Tools/Get-SDDCDiagnosticInfo/HealthTest-S2D-Cluster-20180522-1546.ZIP).

Pour afficher ceci dans Windows Admin Center (version 1812 et versions ultérieures), accédez à la *Diagnostics* onglet. Comme vous le voir dans la capture d’écran ci-dessous, vous pouvez : 

- Installer les outils de diagnostic
- Mettez-les à jour (si elles sont obsolètes), 
- Planifier des exécutions de diagnostics quotidiennes (ces messages ont un faible impact sur votre système, prennent généralement < 5 minutes en arrière-plan et ne prennent plus de 500 Mo sur votre cluster)
- Vue précédemment collectées des informations de diagnostic que si vous devez lui donner à prendre en charge ou de les analyser vous-même.

![capture d’écran de diagnostics wac](media/data-collection/Wac.png)

## <a name="get-sddcdiagnosticinfo-output"></a>Sortie de Get-SDDCDiagnosticInfo

Voici les fichiers inclus dans la sortie compressée de Get-SDDCDiagnosticInfo.

### <a name="health-summary-report"></a>Rapport de synthèse de contrôle d’intégrité

Le rapport de synthèse de contrôle d’intégrité est enregistré sous :
- 0_CloudHealthSummary.log

Ce fichier est généré après que l’analyse de toutes les données collectées et est destinée à fournir un résumé rapide de votre système. Il contient :

- Informations système
- Vue d’ensemble du contrôle d’intégrité de stockage (nombre de nœuds, des ressources en ligne, volumes partagés de cluster en ligne, les composants défectueux, etc.).
- Pour plus d’informations sur les composants défectueux (ressources de cluster sont en ligne en attente, échec ou hors connexion)
- Informations de pilote et de microprogramme
- Pool, disque physique et les détails de volume
- Performances de stockage (collecte des compteurs de performance)

Ce rapport est en cours continuellement mis à jour pour inclure des informations plus utiles. Pour plus d’informations, consultez le [GitHub README](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/edit/master/README.md).

### <a name="logs-and-xml-files"></a>Journaux et les fichiers XML

Le script s’exécute diverses journal collecte des scripts et enregistre la sortie en tant que fichiers xml. Nous collectons des journaux de cluster et l’intégrité, informations système (MSInfo32), journaux des événements non filtrées (clustering de basculement, les diagnostics dis, hyper-v, espaces de stockage, etc.) et les informations de diagnostic de stockage (journaux des opérations). Pour plus d’informations sur les informations collectées, consultez le [README GitHub (ce que nous collectons)](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/blob/master/README.md#what-does-the-cmdlet-output-include).

<!--
## Enabling event channels

When Windows Server is installed, many event channels are enabled by default. But sometimes when diagnosing an issue, we want to be able to enable some of these event channels since it will help in triaging and diagnosing system issues.

You could enable additional event channels on each server node in your cluster as needed; however, this approach presents two problems:

1. You need to remember to enable the same event channels on every new server node that you add to your cluster.
2. When diagnosing, it can be tedious to enable specific event channels, reproduce the error, and repeat this process until you root cause.

To avoid these issues, you can enable event channels on cluster startup. The list of enabled event channels on your cluster can be configured using the public property **EnabledEventLogs**. By default, the following event channels are enabled:

```powershell
PS C:\Windows\system32> (get-cluster).EnabledEventLogs
```

Here's an example of the output:
```
Microsoft-Windows-Hyper-V-VmSwitch-Diagnostic,4,0xFFFFFFFD
Microsoft-Windows-SMBDirect/Debug,4
Microsoft-Windows-SMBServer/Analytic
Microsoft-Windows-Kernel-LiveDump/Analytic
```

The **EnabledEventLogs** property is a multistring, where each string is in the form: **channel-name, log-level, keyword-mask**. The **keyword-mask** can be a hexadecimal (prefix 0x), octal (prefix 0), or decimal number (no prefix) number that each event contains (so you can filter by it). For instance, to add a new event channel to the list and to configure both **log-level** and **keyword-mask** you can run:

```powershell
(get-cluster).EnabledEventLogs += "Microsoft-Windows-WinINet/Analytic,2,321"
```

If you want to set the **log-level** but keep the **keyword-mask** at its default value, you can use either of the following commands:

```powershell
(get-cluster).EnabledEventLogs += "Microsoft-Windows-WinINet/Analytic,2"
(get-cluster).EnabledEventLogs += "Microsoft-Windows-WinINet/Analytic,2,"
```

If you want to keep the **log-level** at its default value, but set the **keyword-mask** you can run the following command:

```powershell
(get-cluster).EnabledEventLogs += "Microsoft-Windows-WinINet/Analytic,,0xf1"
```

If you want to keep both the **log-level** and the **keyword-mask** at their default values, you can run any of the following commands:

```powershell
(get-cluster).EnabledEventLogs += "Microsoft-Windows-WinINet/Analytic"
(get-cluster).EnabledEventLogs += "Microsoft-Windows-WinINet/Analytic,"
(get-cluster).EnabledEventLogs += "Microsoft-Windows-WinINet/Analytic,,"
```

These event channels will be enabled on every cluster node when the cluster service starts or whenever the **EnabledEventLogs** property is changed.
-->

## <a name="how-to-consume-the-xml-files-from-get-pcstoragediagnosticinfo"></a>Comment utiliser les fichiers XML à partir de Get-PCStorageDiagnosticInfo
Vous pouvez utiliser les données à partir des fichiers XML fournis dans les données collectées par le **Get-PCStorageDiagnosticInfo** applet de commande. Ces fichiers contiennent des informations sur le serveur virtuel disques, les disques physiques, les informations sur le cluster de base et les autres PowerShell associées sorties. 

Pour afficher les résultats de ces sorties, ouvrez une fenêtre PowerShell et exécutez les étapes suivantes. 

```PowerShell
ipmo storage
$d = import-clixml <filename> 
$d
```

## <a name="what-to-expect-next"></a>À quoi s’attendre ensuite ?
Un grand nombre d’améliorations et nouvelles applets de commande pour analyser l’intégrité du système SDDC.
Fournir des commentaires sur ce que vous aimeriez voir en soumettant des problèmes [ici](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/issues). En outre, n’hésitez pas à fournir des modifications utiles pour le script, en soumettant un [demande de tirage](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/pulls).
