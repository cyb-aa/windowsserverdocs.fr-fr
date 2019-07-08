---
title: Développement d’applets de commande PowerShell pour Nano Server
description: 'portage CIM, applets de commande .NET, C++ '
ms.prod: windows-server-threshold
ms.service: na
manager: DonGill
ms.technology: server-nano
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7b4267f0-1c91-4a40-9262-5daf4659f686
author: jaimeo
ms.author: jaimeo
ms.date: 09/06/2017
ms.localizationpriority: medium
ms.openlocfilehash: c3376d03a2e9f02b20aba608de0228efd7dfddea
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/17/2019
ms.locfileid: "66443621"
---
# <a name="developing-powershell-cmdlets-for-nano-server"></a>Développement d’applets de commande PowerShell pour Nano Server

>S'applique à : Windows Server 2016

> [!IMPORTANT]
> À partir de Windows Server version 1709, Nano Server sera uniquement disponible sous forme d'[image de système d'exploitation de base du conteneur](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image). Pour en savoir plus, consultez [Modifications apportées à Nano Server](nano-in-semi-annual-channel.md). 
  
## <a name="overview"></a>Vue d’ensemble  
Nano Server inclut PowerShell Core par défaut dans toutes les installations. PowerShell Core est une édition à encombrement réduit de PowerShell qui repose sur .NET Core et s’exécute sur des éditions à encombrement réduit de Windows, telles que Nano Server et Windows IoT Core. PowerShell Core fonctionne de la même manière que les autres éditions de PowerShell, par exemple Windows PowerShell exécuté sur Windows Server 2016. Toutefois, l’encombrement réduit de Nano Server signifie que certaines fonctionnalités PowerShell de Windows Server 2016 ne sont pas disponibles dans PowerShell Core sur Nano Server.  
  
Si vous disposez déjà d’applets de commande PowerShell que vous souhaiteriez exécuter sur Nano Server, ou si vous en développez de nouvelles à cette fin, les conseils et suggestions de cette rubrique simplifieront votre travail.  

## <a name="powershell-editions"></a>Éditions de PowerShell  
  
  
À compter de la version 5.1, PowerShell est disponible dans plusieurs éditions, dont les ensembles de fonctionnalités et la compatibilité de plateforme diffèrent.  
  
- **Édition Desktop :** repose sur .NET Framework et offre une compatibilité avec les scripts et les modules ciblant les versions de PowerShell exécutées sur les éditions complètes de Windows, telles que Server Core et le Bureau Windows.  
- **Édition Core :** repose sur .NET Core et offre une compatibilité avec les scripts et les modules ciblant les versions de PowerShell exécutées sur les éditions à encombrement réduit de Windows, telles que Nano Server et Windows IoT.  
  
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
  
Les auteurs de modules peuvent déclarer la compatibilité de leurs modules avec une ou plusieurs éditions de PowerShell à l’aide de la clé de manifeste de module CompatiblePSEditions. Cette clé est uniquement prise en charge sur PowerShell 5.1 ou version ultérieure.  
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
  
  
## <a name="installing-nano-server"></a>Installation de Nano Server  
La rubrique parente [Installer Nano Server](Getting-Started-with-Nano-Server.md) propose un démarrage rapide ainsi qu’une procédure détaillée pour l’installation de Nano Server sur des ordinateurs physiques ou virtuels.  
  
> [!NOTE]  
> Pour un travail de développement sur Nano Server, il peut vous être utile d’installer Nano Server à l’aide du paramètre -Development de New-NanoServerImage. Cela permettra l’installation de pilotes non signés, la copie des fichiers binaires du débogueur, l’ouverture d’un port pour le débogage, la signature de test et l’installation des packages AppX sans licence de développeur. Par exemple :  
>  
>`New-NanoServerImage -DeploymentType Guest -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -Development`  
  
## <a name="determining-the-type-of-cmdlet-implementation"></a>Détermination du type d’implémentation d’applet de commande  
PowerShell prend en charge plusieurs types d’implémentation pour les applets de commande. Celui que vous utilisez détermine le processus et les outils nécessaires à leur création ou à leur portage en vue d’un fonctionnement sur Nano Server. Les types d’implémentation pris en charge sont les suivants :  
* CIM : inclut plusieurs fichiers CDXML superposés sur des fournisseurs CIM (WMIv2)   
* .NET : inclut des assemblys .NET qui implémentent des interfaces d'applet de commande managées, généralement écrites en C#   
* Script PowerShell : constitué de modules de script (.psm1) ou de scripts (.ps1) écrits en langage PowerShell   
  
Si vous n’êtes pas certain de l’implémentation que vous avez utilisée pour les applets de commande existantes que vous souhaitez porter, installez votre produit ou fonctionnalité, puis recherchez le dossier des modules PowerShell dans l’un des emplacements suivants :   
  
* %windir%\system32\WindowsPowerShell\v1.0\Modules   
* %ProgramFiles%\WindowsPowerShell\Modules   
* %UserProfile%\Documents\WindowsPowerShell\Modules   
* \<Emplacement d'installation de votre produit>   
    
  Vérifiez les détails suivants dans ces emplacements :  
  * Les applets de commande CIM présentent des extensions de fichier .cdxml.  
  * Les applets de commande .NET présentent des extensions de fichier .dll ou ont des assemblys installés dans le GAC, répertoriés dans les champs RootModule, ModuleToProcess ou NestedModules du fichier .psd1.  
* Les applets de commande de script PowerShell présentent des extensions de fichier .psm1 ou .ps1.   
  
## <a name="porting-cim-cmdlets"></a>Portage des applets de commande CIM  
En règle générale, ces applets de commande doivent pouvoir fonctionner dans Nano Server sans conversion. Toutefois, si ce n’est déjà fait, vous devez porter le fournisseur WMI v2 sous-jacent de sorte qu’il s’exécute sur Nano Server.  
  
### <a name="building-c-for-nano-server"></a>Génération de code C++ pour Nano Server  
Pour faire fonctionner des DLL C++ sur Nano Server, compilez-les pour Nano Server plutôt que pour une édition spécifique.  
  
Pour obtenir une procédure pas à pas montrant comment développer en C++ sur Nano Server et connaître les conditions préalables, voir [Developing Native Apps on Nano Server](http://blogs.technet.com/b/nanoserver/archive/2016/04/27/developing-native-apps-on-nano-server.aspx) (Développement d’applications natives sur Nano Server).  
  
  
## <a name="porting-net-cmdlets"></a>Portage des applets de commande .NET  
La plupart du code C# est pris en charge sur Nano Server. Vous pouvez utiliser [ApiPort](https://github.com/Microsoft/dotnet-apiport) pour rechercher les API incompatibles.  
  
### <a name="powershell-core-sdk"></a>Kit de développement logiciel (SDK) PowerShell Core  
Le module « Microsoft.PowerShell.NanoServer.SDK » est disponible dans [PowerShell Gallery](https://www.powershellgallery.com/packages/Microsoft.PowerShell.NanoServer.SDK/). Il simplifie le développement des applets de commande .NET à l’aide de Visual Studio 2015 Update 2, ciblant les versions de CoreCLR et PowerShell Core disponibles dans Nano Server. Vous pouvez installer le module en utilisant PowerShellGet avec cette commande :  
  
`Find-Module Microsoft.PowerShell.NanoServer.SDK -Repository PSGallery | Install-Module -Scope <scope>`  
  
Le module SDK PowerShell Core expose les applets de commande pour configurer les assemblys de référence CoreCLR et PowerShell Core corrects, créer un projet C# dans Visual Studio 2015 ciblant ces assemblys de référence et configurer le débogueur distant sur un ordinateur Nano Server de sorte que les développeurs puissent déboguer leurs applets de commande .NET exécutées sur Nano Server à distance dans Visual Studio 2015.  
  
Le module SDK PowerShell Core requiert Visual Studio 2015 Update 2. Si vous ne disposez pas d’une installation de Visual Studio 2015, vous pouvez installer [Visual Studio Community 2015](https://www.visualstudio.com/en-us/products/visual-studio-community-vs.aspx).  
  
Le module SDK dépend également de l’installation de la fonctionnalité suivante dans Visual Studio 2015 :  
  
- Développement d’applications Windows et web -> Outils de développement d’applications Windows universelles -> Outils (1.3.1) et SDK Windows 10  
  
Vérifiez votre installation de Visual Studio avant d’utiliser le module SDK pour vous assurer que ces conditions préalables sont remplies. Assurez-vous de choisir l’installation de la fonctionnalité ci-dessus pendant l’installation de Visual Studio, ou modifiez votre installation existante de Visual Studio 2015 pour l’installer.  
  
Le module SDK PowerShell Core inclut des applets de commande suivantes :  
- New-NanoCSharpProject : crée un projet Visual Studio C# ciblant CoreCLR et PowerShell Core inclus dans la version Windows Server 2016 de Nano Server.  
- Show-SdkSetupReadMe : ouvre le dossier racine du kit de développement logiciel (SDK) dans l'Explorateur de fichiers, et ouvre le fichier README.txt en vue d'une installation manuelle.  
- Install-RemoteDebugger : installe et configure le débogueur distant Visual Studio sur un ordinateur Nano Server.  
- Start-RemoteDebugger : démarre le débogueur distant sur un ordinateur distant exécutant Nano Server.  
- Stop-RemoteDebugger : arrête le débogueur distant sur un ordinateur distant exécutant Nano Server.  
  
Pour plus d’informations sur l’utilisation de ces applets de commande, exécutez Get-Help sur chaque applet de commande après l’installation et l’importation du module comme suit :  
  
`Get-Command -Module Microsoft.PowerShell.NanoServer.SDK | Get-Help -Full`   
  
  
### <a name="searching-for-compatible-apis"></a>Recherche d’API compatibles  
  
Vous pouvez rechercher .NET Core dans le catalogue d’API ou désassembler des assemblys de référence CLR de base. Pour plus d’informations sur la portabilité de plateforme des API .NET, consultez [Platform Portability](https://github.com/Microsoft/dotnet-apiport/blob/master/docs/HowTo/PlatformPortability.md) (Portabilité de plateforme).  
  
### <a name="pinvoke"></a>PInvoke  
Dans le CLR de base utilisé par Nano Server, certaines DLL fondamentales telles que kernel32.dll et advapi32.dll ont été divisées en plusieurs ensembles d’API. Vous devez donc vous assurer que vos PInvokes font référence à l’API appropriée. Toute incompatibilité se manifestera par une erreur d’exécution.  
  
Pour obtenir une liste des API natives prises en charge par Nano Server, voir [Nano Server APIs](https://msdn.microsoft.com/library/mt588480(v=vs.85).aspx) (API Nano Server).  
  
### <a name="building-c-for-nano-server"></a>Génération de projet C# pour Nano Server  
  
Une fois un projet C# créé dans Visual Studio 2015 à l’aide de `New-NanoCSharpProject`, vous pouvez le générer simplement dans Visual Studio en cliquant sur le menu **Générer** et en sélectionnant **Générer un projet** ou **Générer la solution**. Les assemblys générés cibleront les versions de CoreCLR et PowerShell Core correctes fournies dans Nano Server. Vous pourrez alors simplement copier les assemblys sur un ordinateur exécutant Nano Server et les utiliser.  
  
### <a name="building-managed-c-cppcli-for-nano-server"></a>Génération de code C++ managé (CPP/CLI) pour Nano Server  
Le langage C++ managé n’est pas pris en charge pour CoreCLR. Lors du portage vers CoreCLR, réécrivez le code C++ managé en C# et effectuez tous les appels natifs par le biais de PInvoke.  
  
## <a name="porting-powershell-script-cmdlets"></a>Portage des applets de commande de script PowerShell  
  
PowerShell Core offre une parité de langage complète avec d’autres éditions de PowerShell, y compris l’édition s’exécutant sur Windows Server 2016 et Windows 10. Toutefois, lors du portage d’applets de commande de script PowerShell vers Nano Server, tenez compte des facteurs suivants :  
* Existe-t-il des dépendances vis-à-vis d’autres applets de commande ? Dans ce cas, ces applets de commande sont-elles disponibles sur Nano Server ? Voir [PowerShell sur Nano Server](PowerShell-on-Nano-Server.md) pour plus d’informations sur les éléments non disponibles.  
* S’il existe des dépendances vis-à-vis des assemblys chargés au moment de l’exécution, continueront-ils de fonctionner ?   
* Comment pouvez-vous déboguer le script à distance ?   
* Comment pouvez vous migrer de WMI .Net vers MI .Net ?  
  
### <a name="dependency-on-built-in-cmdlets"></a>Dépendance vis-à-vis des applets de commande intégrées  
Certaines applets de commande de Windows Server 2016 ne sont pas disponibles sur Nano Server (voir [PowerShell sur Nano Server](PowerShell-on-Nano-Server.md)). La meilleure approche consiste à configurer une machine virtuelle Nano Server et à déterminer si les applets de commande dont vous avez besoin sont disponibles. Pour ce faire, exécutez `Enter-PSSession` pour vous connecter au serveur Nano Server cible, puis exécutez `Get-Command -CommandType Cmdlet, Function` pour obtenir la liste des applets de commande disponibles.  
  
### <a name="consider-using-powershell-classes"></a>Utilisation de classes PowerShell  
Add-Type est pris en charge par Nano Server pour la compilation de code C# en ligne. Si vous écrivez un nouveau code ou portez du code existant, vous pouvez également envisager l’utilisation de classes PowerShell pour définir des types personnalisés. Vous pouvez utiliser des classes PowerShell pour les scénarios basés sur des conteneurs de propriétés ainsi que pour les enums. Si vous devez effectuer un PInvoke, utilisez C# avec Add-Type ou un assembly précompilé.  
Voici un exemple illustrant l’utilisation d’Add-Type :  
  
```  
Add-Type -ReferencedAssemblies ([Microsoft.Management.Infrastructure.Ciminstance].Assembly.Location) -TypeDefinition @'  
public class TestNetConnectionResult  
{  
   // The compute name  
   public string ComputerName = null;  
   // The Remote IP address used for connectivity  
   public System.Net.IPAddress RemoteAddress = null;  
}  
'@  
# Create object and set properties  
$result = New-Object TestNetConnectionResult  
$result.ComputerName = "Foo"  
$result.RemoteAddress = 1.1.1.1  
  
```  
 Cet exemple illustre l’utilisation de classes PowerShell sur Nano Server :  
   
```  
class TestNetConnectionResult    
{    
   # The compute name  
  [string] $ComputerName    
  
  #The Remote IP address used for connectivity    
  [System.Net.IPAddress] $RemoteAddress  
}  
# Create object and set properties  
$result = [TestNetConnectionResult]::new()  
$result.ComputerName = "Foo"  
$result.RemoteAddress = 1.1.1.1  
  
```  
  
### <a name="remotely-debugging-scripts"></a>Débogage de scripts à distance  
  
Pour déboguer un script à distance, connectez-vous à l’ordinateur distant à l’aide de `Enter-PSsession` à partir de PowerShell ISE. Une fois la session ouverte, vous pouvez exécuter `psedit <file_path>`. Une copie du fichier s’ouvre alors dans votre environnement PowerShell ISE local. Vous pouvez alors déboguer le script comme s’il s’exécutait localement en définissant des points d’arrêt. En outre, toutes les modifications que vous apporterez à ce fichier seront enregistrées dans la version distante.   
  
### <a name="migrating-from-wmi-net-to-mi-net"></a>Migration à partir de WMI .NET vers MI .NET  
  
[WMI .NET](https://msdn.microsoft.com/library/mt481551(v=vs.110).aspx) n'est pas pris en charge. Par conséquent, toutes les applets de commande utilisant l'ancienne API doivent migrer vers l'API WMI prise en charge : [MI. NET](https://msdn.microsoft.com/library/dn387184(v=vs.85).aspx). Vous pouvez accéder à MI .NET directement en C# ou par le biais des applets de commande du module CimCmdlets.   
  
### <a name="cimcmdlets-module"></a>Module CimCmdlets  
  
Les applets de commande WMI v1 (par exemple, Get-WmiObject) ne sont pas prises en charge par Nano Server. Toutefois, les applets de commande CIM (par exemple, Get-CimInstance) du module CimCmdlets sont prises en charge. Les applets de commande CIM correspondent assez précisément aux applets de commande WMI v1. Par exemple, Get-WmiObject correspond à Get-CimInstance et utilise des paramètres très similaires. La syntaxe d’appel de méthode est légèrement différente, mais elle est bien documentée avec Invoke-CimMethod. Soyez attentif au typage des paramètres. MI .NET présente des exigences plus strictes en ce qui concerne les types de paramètres de méthode.  
  
### <a name="c-api"></a>API C#  
  
WMI .NET encapsule l’interface WMIv1, tandis que MI .NET encapsule l’interface WMIv2 (CIM). Les classes exposées peuvent être différentes, mais les opérations sous-jacentes sont très similaires. Vous énumérer ou obtenez des instances d’objets et appelez des opérations sur ces objets pour accomplir des tâches.   
  
  


