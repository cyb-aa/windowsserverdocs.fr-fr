---
title: Collecter les données de diagnostic avec espaces de stockage direct
description: Comprendre espaces de stockage direct outils de collecte de données, avec des exemples spécifiques sur la manière de les exécuter et de les utiliser.
keywords: Espaces de stockage, collecte de données, dépannage, canaux d’événements, SDDCDiagnosticInfo
ms.assetid: ''
ms.prod: windows-server
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 10/24/2018
ms.localizationpriority: ''
ms.openlocfilehash: 67f35e3afa8e9eafabe7b22eb60cc85c7be6cb23
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402873"
---
# <a name="collect-diagnostic-data-with-storage-spaces-direct"></a>Collecter les données de diagnostic avec espaces de stockage direct

> S’applique à : Windows Server 2019, Windows Server 2016

Plusieurs outils de diagnostic peuvent être utilisés pour collecter les données nécessaires pour résoudre les problèmes de espaces de stockage direct et de cluster de basculement. Dans cet article, nous allons nous concentrer sur l’outil **SDDCDiagnosticInfo** -a Touch qui rassemble toutes les informations pertinentes pour vous aider à diagnostiquer votre cluster.

Étant donné que les journaux et autres informations que **reçoit-SDDCDiagnosticInfo** sont denses, les informations sur la résolution des problèmes présentées ci-dessous sont utiles pour la résolution des problèmes avancés qui ont été remontés et qui peuvent nécessiter l’envoi de données à Microsoft pour le triage.

## <a name="installing-get-sddcdiagnosticinfo"></a>Installation de la SDDCDiagnosticInfo

Applet de commande PowerShell **SDDCDiagnosticInfo** (également appelée Vous pouvez utiliser la fonction **PCStorageDiagnosticInfo**, précédemment appelée **test-StorageHealth**) pour rassembler des journaux et effectuer des vérifications d’intégrité pour le clustering de basculement (cluster, ressources, réseaux, nœuds), espaces de stockage (disques physiques, boîtiers, Disques virtuels), volumes partagés de cluster, partages de fichiers SMB et déduplication. 

Il existe deux méthodes d’installation du script : les deux sont des contours ci-dessous.

### <a name="powershell-gallery"></a>PowerShell Gallery

Le [PowerShell Gallery](https://www.powershellgallery.com/packages/PrivateCloud.DiagnosticInfo) est un instantané du référentiel github. Notez que l’installation d’éléments à partir du PowerShell Gallery nécessite la version la plus récente du module PowerShellGet, qui est disponible dans Windows 10, dans Windows Management Framework (WMF) 5,0 ou dans le programme d’installation MSI (pour PowerShell 3 et 4).

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

Le [GitHub référentiel](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/) est la version la plus à jour du module, puisque nous continuons continuellement ici. Pour installer le module à partir de GitHub, téléchargez le module le plus récent à partir de l' [Archive](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/archive/master.zip) et extrayez le répertoire PrivateCloud. DiagnosticInfo vers le chemin des modules PowerShell correct pointé par ```$env:PSModulePath```

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

Si vous avez besoin d’accéder à ce module sur un cluster hors connexion, téléchargez-le, déplacez-le vers le nœud de votre serveur cible, puis installez le module.

## <a name="gathering-logs"></a>Collecte des journaux

Une fois que vous avez activé les canaux d’événements et terminé le processus d’installation, vous pouvez utiliser l’applet de commande PowerShell SDDCDiagnosticInfo dans le module pour obtenir :

- Rapports sur l’intégrité du stockage et détails sur les composants défectueux
- Rapports de la capacité de stockage par pool, volume et volume dédupliqué
- Journaux des événements de tous les nœuds de cluster et rapport d’erreurs Résumé

Supposons que votre cluster de stockage porte le nom *« CLUS01 ».*

Pour exécuter sur un cluster de stockage distant :

``` PowerShell
Get-SDDCDiagnosticInfo -ClusterName CLUS01
```

Pour exécuter localement sur un nœud de stockage en cluster :

``` PowerShell
Get-SDDCDiagnosticInfo
```

Pour enregistrer les résultats dans un dossier spécifié :

``` PowerShell
Get-SDDCDiagnosticInfo -WriteToPath D:\Folder 
```

Voici un exemple de la façon dont cela se passe sur un cluster réel :

``` PowerShell
New-Item -Name SDDCDiagTemp -Path d:\ -ItemType Directory -Force
Get-SddcDiagnosticInfo -ClusterName S2D-Cluster -WriteToPath d:\SDDCDiagTemp
```

Comme vous pouvez le voir, le script effectue également la validation de l’état actuel du cluster

![capture d’écran PowerShell de collecte de données](media/data-collection/CollectData.png)

Comme vous pouvez le voir, toutes les données sont écrites dans le dossier SDDCDiagTemp

![capture d’écran des données dans l’Explorateur de fichiers](media/data-collection/CollectDataFolder.png)

Une fois le script terminé, le fichier ZIP est créé dans le répertoire de vos utilisateurs

![capture d’écran des données zip dans PowerShell](media/data-collection/CollectDataResult.png)

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

Pour référence, voici un lien vers l' [exemple de rapport](https://github.com/Microsoft/WSLab/blob/dev/Scenarios/S2D%20Tools/Get-SDDCDiagnosticInfo/SDDCReport.txt) et un exemple de fichier [zip](https://github.com/Microsoft/WSLab/blob/dev/Scenarios/S2D%20Tools/Get-SDDCDiagnosticInfo/HealthTest-S2D-Cluster-20180522-1546.ZIP).

Pour l’afficher dans le centre d’administration Windows (version 1812 et versions ultérieures), accédez à l’onglet *Diagnostics* . Comme vous pouvez le voir dans la capture d’écran ci-dessous, vous pouvez 

- Installer les outils de diagnostic
- Les mettre à jour (si elles sont obsolètes), 
- Planifier des exécutions de diagnostics quotidiennes (celles-ci ont un impact faible sur votre système, prennent généralement < 5 minutes en arrière-plan et n’occupent pas plus de 500 Mo sur votre cluster)
- Affichez les informations de diagnostic précédemment collectées si vous devez leur permettre de prendre en charge ou de les analyser vous-même.

![capture d’écran Diagnostics WAC](media/data-collection/Wac.png)

## <a name="get-sddcdiagnosticinfo-output"></a>Sortie de la SDDCDiagnosticInfo

Voici les fichiers inclus dans la sortie compressée de la commande SDDCDiagnosticInfo.

### <a name="health-summary-report"></a>Rapport récapitulatif de l’intégrité

Le rapport de synthèse d’intégrité est enregistré sous la forme :
- 0_CloudHealthSummary. log

Ce fichier est généré après l’analyse de toutes les données collectées et est destiné à fournir un résumé rapide de votre système. Il contient :

- Informations système
- Vue d’ensemble de l’intégrité du stockage (nombre de nœuds, ressources en ligne, volumes partagés de cluster en ligne, composants défectueux, etc.)
- Détails sur les composants défectueux (ressources de cluster hors connexion, en échec ou en attente en ligne)
- Informations sur le microprogramme et le pilote
- Détails sur le pool, le disque physique et le volume
- Performances de stockage (les compteurs de performance sont collectés)

Ce rapport est continuellement mis à jour pour inclure des informations plus utiles. Pour obtenir les informations les plus récentes, consultez le [fichier Lisez-moi GitHub](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/edit/master/README.md).

### <a name="logs-and-xml-files"></a>Journaux et fichiers XML

Le script exécute différents scripts de collecte des journaux et enregistre la sortie sous forme de fichiers XML. Nous collectons les journaux de cluster et d’intégrité, les informations système (MSInfo32), les journaux des événements non filtrés (clustering de basculement, diagnostics dis, Hyper-v, les espaces de stockage, etc.) et les informations de diagnostics de stockage (journaux des opérations). Pour obtenir les informations les plus récentes sur les informations collectées, consultez le [fichier Lisez-moi GitHub (ce que nous recueillons)](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/blob/master/README.md#what-does-the-cmdlet-output-include).

## <a name="how-to-consume-the-xml-files-from-get-pcstoragediagnosticinfo"></a>Utilisation des fichiers XML à partir de la méthode PCStorageDiagnosticInfo
Vous pouvez utiliser les données des fichiers XML fournis dans les données collectées par l’applet de commande **PCStorageDiagnosticInfo** . Ces fichiers contiennent des informations sur les disques virtuels, les disques physiques, les informations de cluster de base et d’autres sorties associées à PowerShell. 

Pour afficher les résultats de ces sorties, ouvrez une fenêtre PowerShell et exécutez les étapes suivantes. 

```PowerShell
ipmo storage
$d = import-clixml <filename> 
$d
```

## <a name="what-to-expect-next"></a>Que dois-je faire ensuite ?
Un grand nombre d’améliorations et de nouvelles applets de commande pour analyser l’intégrité du système SDDC.
Fournissez des commentaires sur ce que vous aimeriez voir dans les problèmes de profilage [ici](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/issues). En outre, n’hésitez pas à apporter des modifications utiles au script en soumettant une [demande de tirage (pull Request](https://github.com/PowerShell/PrivateCloud.DiagnosticInfo/pulls)).
