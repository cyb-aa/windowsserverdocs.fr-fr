---
title: PowerShell sur Nano Server
description: Différences dans l’ensemble réduit de fonctionnalités PowerShell sur Nano Server
ms.prod: windows-server-threshold
ms.service: na
manager: DonGill
ms.technology: server-nano
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9b25b939-1e2c-4bed-a8d3-2a8e8e46b53d
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: 8a19082121e2d859bc4694fd3f7332e9d0d0b3b9
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/22/2018
ms.locfileid: "2082073"
---
# <a name="powershell-on-nano-server"></a>PowerShell sur Nano Server

>S’applique à WindowsServer2016
  
> [!IMPORTANT]
> À compter de WindowsServer, version1709, NanoServer sera uniquement disponible sous forme d’[image du système d’exploitation de base du conteneur](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image). Consultez [Modifications apportées à NanoServer](nano-in-semi-annual-channel.md) pour en savoir plus. 
  
## <a name="powershell-editions"></a>Éditions de PowerShell   
  
À compter de la version5.1, PowerShell est disponible dans plusieurs éditions, dont les ensembles de fonctionnalités et la compatibilité de plateforme diffèrent.  
  
- **Édition Desktop:** repose sur .NET Framework et offre une compatibilité avec les scripts et les modules ciblant les versions de PowerShell exécutées sur les éditions complètes de Windows, telles que Server Core et le Bureau Windows.  
- **Édition Core:** repose sur .NET Core et offre une compatibilité avec les scripts et les modules ciblant les versions de PowerShell exécutées sur les éditions à encombrement réduit de Windows, telles que Nano Server et Windows IoT.  
  
L’édition de PowerShell exécutée est indiquée dans la propriété PSEdition de $PSVersionTable.  
```powershell  
$PSVersionTable  
  
Name                           Value  
----                           -----  
PSVersion                      5.1.14300.1000  
PSEdition                      Desktop  
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0...}  
CLRVersion                     4.0.30319.42000  
BuildVersion                   10.0.14300.1000  
WSManStackVersion              3.0  
PSRemotingProtocolVersion      2.3  
SerializationVersion           1.1.0.1  
```  
  
Les auteurs de modules peuvent déclarer la compatibilité de leurs modules avec une ou plusieurs éditions de PowerShell à l’aide de la clé de manifeste de module CompatiblePSEditions. Cette clé est uniquement prise en charge sur PowerShell5.1 ou version ultérieure.  
```powershell  
New-ModuleManifest -Path .\TestModuleWithEdition.psd1 -CompatiblePSEditions Desktop,Core -PowerShellVersion 5.1  
$moduleInfo = Test-ModuleManifest -Path \TestModuleWithEdition.psd1  
$moduleInfo.CompatiblePSEditions  
Desktop  
Core  
  
$moduleInfo | Get-Member CompatiblePSEditions  
  
   TypeName: System.Management.Automation.PSModuleInfo  
  
Name                 MemberType Definition  
----                 ---------- ----------  
CompatiblePSEditions Property   System.Collections.Generic.IEnumerable[string] CompatiblePSEditions {get;}  
  
```  
Lors de l’obtention de la liste des modules disponibles, vous pouvez filtrer cette liste par édition de PowerShell.  
```powershell  
Get-Module -ListAvailable | ? CompatiblePSEditions -Contains "Desktop"  
  
    Directory: C:\Program Files\WindowsPowerShell\Modules  
  
  
ModuleType Version    Name                                ExportedCommands  
---------- -------    ----                                ----------------  
Manifest   1.0        ModuleWithPSEditions  
  
Get-Module -ListAvailable | ? CompatiblePSEditions -Contains "Core" | % CompatiblePSEditions  
Desktop  
Core  
  
```  
Les auteurs de scripts peuvent empêcher un script de s’exécuter sur les éditions non compatibles de PowerShell en utilisant le paramètre PSEdition dans une instruction #requires.  
```powershell  
Set-Content C:\script.ps1 -Value "#requires -PSEdition Core  
Get-Process -Name PowerShell"  
Get-Content C:\script.ps1  
#requires -PSEdition Core  
Get-Process -Name PowerShell  
  
C:\script.ps1  
C:\script.ps1 : The script 'script.ps1' cannot be run because it contained a "#requires" statement for PowerShell editions 'Core'. The edition of PowerShell that is required by the script does not match the currently running PowerShell Desktop edition.  
At line:1 char:1  
+ C:\script.ps1  
+ ~~~~~~~~~~~~~  
    + CategoryInfo          : NotSpecified: (script.ps1:String) [], RuntimeException  
    + FullyQualifiedErrorId : ScriptRequiresUnmatchedPSEdition  
```  
  
## <a name="differences-in-powershell-on-nano-server"></a>Différences de PowerShell sur Nano Server  
Nano Server inclut PowerShell Core par défaut dans toutes les installations. PowerShell Core est une édition à encombrement réduit de PowerShell qui repose sur .NET Core et s’exécute sur des éditions à encombrement réduit de Windows, telles que Nano Server et Windows IoT Core. PowerShell Core fonctionne de la même manière que les autres éditions de PowerShell, par exemple Windows PowerShell exécuté sur WindowsServer2016. Toutefois, l’encombrement réduit de Nano Server signifie que certaines fonctionnalités PowerShell de WindowsServer2016 ne sont pas disponibles dans PowerShell Core sur Nano Server.  
  
  
**Fonctionnalités Windows PowerShell non disponibles dans Nano Server**  
* Adaptateurs de type ADSI, ADO et WMI   
* Enable-PSRemoting, Disable-PSRemoting (la communication à distance PowerShell est activée par défaut; voir la section «Utilisation de la communication à distance Windows PowerShell» dans [Installer Nano Server](Getting-Started-with-Nano-Server.md)).  
* Tâches planifiées et module PSScheduledJob   
* Applets de commande Computer pour la jonction à un domaine {Add | Remove} (pour connaître les autres méthodes pour joindre Nano Server à un domaine, voir la section «Jonction de Nano Server à un domaine» dans [Installer Nano Server](Getting-Started-with-Nano-Server.md)).  
* Reset-ComputerMachinePassword, Test-ComputerSecureChannel   
* Profils (vous pouvez ajouter un script de démarrage pour les connexions à distance entrantes avec `Set-PSSessionConfiguration`)  
* Applets de commande de Presse-papiers   
* Applets de commande EventLog {Clear | Get | Limit | New | Remove | Show | Write} (utilisez les applets de commande New-WinEvent et Get-WinEvent à la place)   
* Applet de commande Get-PfxCertificate   
* Applets de commande TraceSource {Get | Set}   
* Applets de commande Counter {Get | Export | Import}   
* Certaines applets de commande liées au web {New-WebServiceProxy, Send-MailMessage, ConvertTo-Html}  
* Journalisation et suivi à l’aide du module PSDiagnostics    
* Get-HotFix (pour obtenir et gérer les mises à jour sur Nano Server, consultez [Gérer Nano Server](Manage-Nano-Server.md))  
* Applets de commande de communication à distance implicite {Export-PSSession | Import-PSSession}   
* New-PSTransportOption   
* Transactions PowerShell et applets de commande Transaction {Complete | Get | Start | Undo | Use}   
* Infrastructure, modules et applets de commande PowerShell Workflow   
* Out-Printer   
* Update-List   
* Applets de commande WMI v1: Get-WmiObject, Invoke-WmiMethod, Register-WmiEvent, Remove-WmiObject, Set-WmiInstance (utilisez le module CimCmdlets à la place)   
  
## <a name="using-windows-powershell-desired-state-configuration-with-nano-server"></a>Utilisation de la configuration d’état souhaité Windows PowerShell avec Nano Server  
  
Vous pouvez gérer Nano Server en tant que nœuds cibles avec la configuration d’état souhaité Windows PowerShell (DSC). Actuellement, vous pouvez gérer les nœuds exécutant Nano Server avec DSC en mode par envoi uniquement. Certaines fonctionnalités DSC ne sont pas compatibles avec Nano Server.  
  
Pour plus d’informations, consultez [Utilisation de DSC sur Nano Server](https://msdn.microsoft.com/powershell/dsc/nanoDsc).  
  
  


