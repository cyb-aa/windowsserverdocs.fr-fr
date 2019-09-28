---
title: Gérer Nano Server
description: mises à jour, packages de mise en service, suivi de mise en réseau, analyse des performances
ms.prod: windows-server
ms.service: na
manager: DonGill
ms.technology: server-nano
ms.date: 09/06/2017
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 599d6438-a506-4d57-a0ea-1eb7ec19f46e
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: dc3a2386573c5beb4ec156fdfca3b77f025b1738
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71391758"
---
# <a name="manage-nano-server"></a>Gérer Nano Server

>S'applique à : Windows Server 2016

> [!IMPORTANT]
> À compter de Windows Server, version 1709, Nano Server sera uniquement disponible sous forme d’[image de système d’exploitation de base du conteneur](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image). Pour en savoir plus, consultez [Changements apportés à Nano Server](nano-in-semi-annual-channel.md).   

La gestion de Nano Server s’effectue à distance. Il n’existe aucune fonctionnalité d’ouverture de session locale et les services Terminal Server ne sont pas pris en charge. Toutefois, vous disposez d’un large éventail d’options pour gérer Nano Server à distance : Windows PowerShell, Windows Management Instrumentation (WMI), Gestion à distance de Windows, Services de gestion d’urgence (EMS), etc.  

Pour utiliser un outil de gestion à distance, il vous faudra probablement connaître l’adresse IP du serveur Nano Server. Pour trouver cette adresse IP, vous pouvez entre autres recourir aux méthodes suivantes :  
  
-   Utiliser la console de récupération Nano Server (pour plus d’informations, voir la rubrique Utilisation de la console de récupération Nano Server de cette rubrique).  
  
-   Connecter un câble série à l’ordinateur et utiliser EMS.  
  
-   À l’aide du nom d’ordinateur attribué au serveur Nano Server lors de la configuration de ce dernier, vous pouvez obtenir l’adresse IP avec la commande ping. Par exemple, `ping NanoServer-PC /4`.  
  
## <a name="using-windows-powershell-remoting"></a>Utilisation de la communication à distance Windows PowerShell  
Pour gérer Nano Server avec la communication à distance Windows PowerShell, vous devez ajouter l’adresse IP de l’instance Nano Server à la liste des hôtes approuvés par votre ordinateur de gestion, ajouter le compte que vous utilisez à la liste des administrateurs de Nano Server, et activer CredSSP si vous prévoyez d’utiliser cette fonctionnalité.  

> [!NOTE]
> Si l'instance Nano Server cible et votre ordinateur de gestion se trouvent dans la même forêt AD DS (ou dans des forêts ayant une relation d'approbation), vous n'avez pas besoin d'ajouter l'instance Nano Server à la liste des hôtes approuvés. Il vous suffit de vous connecter à l'instance Nano Server à l'aide de son nom de domaine complet, par exemple : PS C:\> Enter-PSSession -ComputerName nanoserver.contoso.com -Credential (Get-Credential)
  
  
Pour ajouter le serveur Nano Server à la liste des hôtes approuvés, exécutez cette commande à partir d’une invite Windows PowerShell avec élévation de privilèges :  
  
`Set-Item WSMan:\localhost\Client\TrustedHosts "<IP address of Nano Server>"`  
  
Pour démarrer la session Windows PowerShell à distance, démarrez une session Windows PowerShell locale avec élévation de privilèges, puis exécutez les commandes suivantes :  
  
  
```  
$ip = "<IP address of Nano Server>"  
$user = "$ip\Administrator"  
Enter-PSSession -ComputerName $ip -Credential $user  
```  
  
  
Vous pouvez maintenant exécuter normalement des commandes Windows PowerShell sur le serveur Nano Server.  
  
> [!NOTE]  
> Certaines commandes Windows PowerShell ne sont pas disponibles dans cette version de Nano Server. Pour voir les commandes disponibles, exécutez `Get-Command -CommandType Cmdlet`  
  
Pour arrêter la session à distance, utilisez la commande `Exit-PSSession`  
  
## <a name="using-windows-powershell-cim-sessions-over-winrm"></a>Utilisation de sessions CIM Windows PowerShell sur WinRM  
Vous pouvez utiliser des sessions et instances CIM dans Windows PowerShell pour exécuter des commandes WMI sur le service Gestion à distance de Windows (WinRM).  
  
Pour démarrer la session CIM, exécutez les commandes suivantes dans une invite de commandes Windows PowerShell :  
  
  
```  
$ip = "<IP address of the Nano Server\>"  
$user = $ip\Administrator  
$cim = New-CimSession -Credential $user -ComputerName $ip  
```  
  
  
Une fois la session établie, vous pouvez exécuter diverses commandes WMI, par exemple :  
  
  
```  
Get-CimInstance -CimSession $cim -ClassName Win32_ComputerSystem | Format-List *  
Get-CimInstance -CimSession $Cim -Query "SELECT * from Win32_Process WHERE name LIKE 'p%'"  
```  
  
  
## <a name="windows-remote-management"></a>Gestion à distance de Windows  
Vous pouvez exécuter des programmes à distance sur le serveur Nano Server à l’aide du service Gestion à distance de Windows (WinRM). Pour utiliser WinRM, commencez par configurer le service et définir la page de code avec les commandes suivantes dans une invite de commandes avec élévation de privilèges:  
  
```
winrm quickconfig
winrm set winrm/config/client @{TrustedHosts="<ip address of Nano Server>"}
chcp 65001
```
  
Vous pouvez maintenant exécuter des commandes à distance sur le serveur Nano Server. Par exemple :  

```
winrs -r:<IP address of Nano Server> -u:Administrator -p:<Nano Server administrator password> ipconfig
```
  
Pour plus d’informations sur le service Gestion à distance de Windows, voir [Vue d’ensemble de la Gestion à distance de Windows (WinRM)](https://technet.microsoft.com/library/dn265971.aspx).  
   
   
  
## <a name="running-a-network-trace-on-nano-server"></a>Exécution d’un suivi réseau sur Nano Server  
 Les outils netsh trace, Tracelog.exe et Logman.exe ne sont pas disponibles dans Nano Server. Pour capturer des paquets réseau, vous pouvez utiliser les applets de commande Windows PowerShell suivantes :  
   
   
```  
New-NetEventSession [-Name]  
Add-NetEventPacketCaptureProvider -SessionName  
Start-NetEventSession [-Name]  
Stop-NetEventSession [-Name]  
```  
Ces applets de commande sont décrites de façon détaillée dans l’article [réseau événement paquet capturer les applets de commande Windows PowerShell](https://technet.microsoft.com/library/dn268520(v=wps.630).aspx) (Applets de commande pour la capture de paquets d’événements réseau dans Windows PowerShell).  

## <a name="installing-servicing-packages"></a>Installation de packages de mise en service  
Si vous voulez installer un package de mise en service, utilisez le paramètre -ServicingPackagePath (vous pouvez transmettre un tableau de chemins à des fichiers .cab) :  
  
`New-NanoServerImage -DeploymentType Guest -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -ServicingPackagePath \\path\to\kb123456.cab`  
  
Un package de mise en service ou un correctif logiciel est souvent téléchargé en tant qu’élément de la base de connaissances qui contient un fichier .CAB. Procédez comme suit pour extraire le fichier .cab, que vous pouvez ensuite installer à l’aide du paramètre -ServicingPackagePath :  
  
1.  Téléchargez le package de mise en service (depuis l’article correspondant de la Base de connaissances ou depuis le [catalogue Microsoft Update](https://catalog.update.microsoft.com/v7/site/home.aspx). Enregistrez-le dans un répertoire ou un partage réseau local, par exemple : C:\ServicingPackages  
2.  Créez un dossier dans lequel enregistrer le package de mise en service extrait.  Exemple : c:\KB3157663_développé  
3.  Ouvrez une console Windows PowerShell et utilisez la commande `Expand` en spécifiant le chemin d’accès au fichier .msu du package de mise en service, y compris le paramètre `-f:*` et l’emplacement où vous souhaitez que le package de mise en service soit extrait.  Par exemple : `Expand "C:\ServicingPackages\Windows10.0-KB3157663-x64.msu" -f:* "C:\KB3157663_expanded"`  
  
    Les fichiers développés se présenteront comme suit:  
C:>dir C:\KB3157663_développé   
Le volume dans le lecteur C s’appelle OS  
Le numéro de série du volume est B05B-CC3D  
   
      Répertoire de C:\KB3157663_développé  
   
      19/04/2016 13:17    \<REP>.  
      19/04/2016 13:17    \<REP&gt;.  
        17/04/2016 00:31               517 Windows10.0-KB3157663-x64-pkgProperties.txt  
17/04/2016  00:30        93 886 347 Windows10.0-KB3157663-x64.cab  
17/04/2016  00:31        454 Windows10.0-KB3157663-x64.xml  
17/04/2016  00:36           185 818 WSUSSCAN.cab  
               4 fichier(s) 94 073 136 octets  
               2Rép(s) 328559427584 octets libres  
4.  Exécutez `New-NanoServerImage` avec le paramètre -ServicingPackagePath pointant vers le fichier .cab dans ce répertoire, par exemple : `New-NanoServerImage -DeploymentType Guest -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -ServicingPackagePath C:\KB3157663_expanded\Windows10.0-KB3157663-x64.cab`  

## <a name="managing-updates-in-nano-server"></a>Gestion des mises à jour dans Nano Server

À l’heure actuelle, vous pouvez utiliser le fournisseur Windows Update pour Windows Management Instrumentation (WMI) afin de rechercher la liste des mises à jour applicables, puis installer l’ensemble ou un sous-ensemble de celles-ci. Si vous utilisez Windows Server Update Services (WSUS), vous pouvez également configurer le serveur Nano Server de sorte qu’il contacte le serveur WSUS pour obtenir les mises à jour.  

Dans tous les cas, commencez par établir une session Windows PowerShell à distance avec l’ordinateur Nano Server. Ces exemples utilisent *$sess* pour la session ; si vous utilisez autre chose, remplacez cet élément là où il y a lieu.  


### <a name="view-all-available-updates"></a>Afficher toutes les mises à jour disponibles  
---  
Les commandes suivantes permettent d’obtenir la liste complète des mises à jour applicables:  
```  
$sess = New-CimInstance -Namespace root/Microsoft/Windows/WindowsUpdate -ClassName MSFT_WUOperationsSession  

$scanResults = Invoke-CimMethod -InputObject $sess -MethodName ScanForUpdates -Arguments @{SearchCriteria="IsInstalled=0";OnlineScan=$true}  
```  
**Remarque :**  
Si aucune mise à jour n’est disponible, cette commande renvoie l’erreur suivante :  
```  
Invoke-CimMethod : A general error occurred that is not covered by a more specific error code.  

At line:1 char:16  

+ ... anResults = Invoke-CimMethod -InputObject $sess -MethodName ScanForUp ...  

+                 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~  

    + CategoryInfo          : NotSpecified: (MSFT_WUOperatio...-5b842a3dd45d")  

   :CimInstance) [Invoke-CimMethod], CimException  

    + FullyQualifiedErrorId : MI RESULT 1,Microsoft.Management.Infrastructure.  

   CimCmdlets.InvokeCimMethodCommand  
```  

### <a name="install-all-available-updates"></a>Installer toutes les mises à jour disponibles  
---  
Vous pouvez rechercher, télécharger et installer **toutes** les mises à jour simultanément à l’aide des commandes suivantes :  

```  
$sess = New-CimInstance -Namespace root/Microsoft/Windows/WindowsUpdate -ClassName MSFT_WUOperationsSession  

$scanResults = Invoke-CimMethod -InputObject $sess -MethodName ApplyApplicableUpdates  

Restart-Computer  
```  
**Remarque :**  
Windows Defender empêche l’installation des mises à jour. Pour contourner ce problème, désinstallez Windows Defender, installez les mises à jour, puis réinstallez Windows Defender. Vous pouvez également télécharger les mises à jour sur un autre ordinateur, les copier sur le serveur Nano Server, puis les appliquer avec DISM.exe.  


### <a name="verify-installation-of-updates"></a>Vérifier l’installation des mises à jour  
---  
Pour obtenir la liste des mises à jour actuellement installées, utilisez les commandes suivantes :  
```  
$sess = New-CimInstance -Namespace root/Microsoft/Windows/WindowsUpdate -ClassName MSFT_WUOperationsSession  

$scanResults = Invoke-CimMethod -InputObject $sess -MethodName ScanForUpdates -Arguments @{SearchCriteria="IsInstalled=1";OnlineScan=$true}  
```  

**Remarque :**  
Ces commandes répertorient les éléments installés, mais n’indiquent pas spécifiquement «installé» dans la sortie. Si vous avez besoin d’une sortie qui inclut cette information, par exemple pour un rapport, vous pouvez exécuter  
```  
Get-WindowsPackage--Online  
```  

### <a name="using-wsus"></a>Utilisation de WSUS  
---  
Les commandes répertoriées ci-dessus interrogent les services Windows Update et Microsoft Update sur Internet pour rechercher et télécharger les mises à jour disponibles. Si vous utilisez WSUS, vous pouvez définir des clés de Registre sur le serveur Nano Server de manière à utiliser votre serveur WSUS à la place.  
  
Consultez le tableau « Clés de Registre des options d’environnement de l’Agent Windows Update » dans l’article [Configurer les mises à jour automatiques dans un environnement non Active Directory](https://technet.microsoft.com/library/cc708449(v=ws.10).aspx).  
  
Vous devez définir au moins les clés de Registre **WUServer** et **WUStatusServer**, mais selon le mode d’implémentation de WSUS, d’autres valeurs peuvent être nécessaires. Vous pouvez toujours vérifier ces paramètres en examinant un autre serveur Windows dans le même environnement.  

Une fois ces valeurs définies pour votre serveur WSUS, les commandes de la section ci-dessus interrogent ce serveur pour rechercher les mises à jour et l’utilisent comme source de téléchargement.  

### <a name="automatic-updates"></a>Mises à jour automatiques  
---  
Actuellement, la solution pour automatiser l’installation des mises à jour consiste à convertir les étapes ci-dessus dans un script Windows PowerShell local, puis à créer une tâche planifiée pour l’exécuter et redémarrer le système à la fréquence souhaitée.


## <a name="performance-and-event-monitoring-on-nano-server"></a>Analyse des performances et des événements sur Nano Server
[comment]: # (à partir de Venkat Yalla.)
Nano Server prend entièrement en charge l’infrastructure de [suivi d’événements pour Windows](https://aka.ms/u2pa0i) (ETW), mais certains outils familiers utilisés pour gérer le suivi et les compteurs de performances ne sont pas disponibles pour le moment sur Nano Server. Toutefois, Nano Server offre des outils et des applets de commande permettant d’accomplir les scénarios d’analyse des performances les plus courants.

Le flux de travail global reste le même que sur n’importe quelle installation de Windows Server : le suivi à faible charge est réalisé sur l’ordinateur cible (Nano Server) et les fichiers ou journaux de suivi obtenus sont post-traités hors connexion sur un autre ordinateur à l’aide d’outils tels que [Windows Performance Analyzer](https://msdn.microsoft.com/library/windows/hardware/hh448170.aspx) ou l’[Analyseur de messages](https://www.microsoft.com/download/details.aspx?id=44226).

> [!NOTE]
> Pour revoir comment transférer des fichiers à l’aide de la communication à distance PowerShell, consultez [How to copy files to and from Nano Server](https://aka.ms/nri9c8) (Copie de fichiers vers et depuis Nano Server).

Les sections suivantes répertorient les activités de collecte de données les plus courantes, ainsi qu’un moyen pour les accomplir sur Nano Server.

### <a name="query-available-event-providers"></a>Interroger les fournisseurs d’événements disponibles
L’[Enregistreur de performance Windows](https://msdn.microsoft.com/library/hh448229.aspx) permet d’interroger les fournisseurs d’événements disponibles à l’aide de la commande suivante :
```
wpr.exe -providers
```

Vous pouvez filtrer la sortie sur le type d’événements qui vous intéresse. Par exemple :
```
PS C:\> wpr.exe -providers | select-string "Storage"

       595f33ea-d4af-4f4d-b4dd-9dacdd17fc6e                              : Microsoft-Windows-StorageManagement-WSP-Host
       595f7f52-c90a-4026-a125-8eb5e083f15e                              : Microsoft-Windows-StorageSpaces-Driver
       69c8ca7e-1adf-472b-ba4c-a0485986b9f6                              : Microsoft-Windows-StorageSpaces-SpaceManager
       7e58e69a-e361-4f06-b880-ad2f4b64c944                              : Microsoft-Windows-StorageManagement
       88c09888-118d-48fc-8863-e1c6d39ca4df                              : Microsoft-Windows-StorageManagement-WSP-Spaces
```

### <a name="record-traces-from-a-single-etw-provider"></a>Enregistrer les suivis d’un fournisseur ETW unique
Pour ce faire, vous pouvez utiliser les nouvelles [applets de commande de gestion du suivi des événements](https://technet.microsoft.com/library/dn919247.aspx). Voici un exemple de flux de travail :

Créez et démarrez le suivi, en spécifiant un nom de fichier pour stocker les événements.
```
PS C:\> New-EtwTraceSession -Name "ExampleTrace" -LocalFilePath c:\etrace.etl
```

Ajoutez un GUID de fournisseur au suivi. Utilisez ```wpr.exe -providers``` pour la conversion du nom du fournisseur en GUID. 
```
PS C:\> wpr.exe -providers | select-string "Kernel-Memory"

       d1d93ef7-e1f2-4f45-9943-03d245fe6c00                              : Microsoft-Windows-Kernel-Memory

PS C:\> Add-EtwTraceProvider -Guid "{d1d93ef7-e1f2-4f45-9943-03d245fe6c00}" -SessionName "ExampleTrace"
```

Supprimez le suivi ; la session de suivi est alors arrêtée et les événements vidés dans le fichier journal associé.
```
PS C:\> Remove-EtwTraceSession -Name "ExampleTrace"

PS C:\> dir .\etrace.etl

    Directory: C:\

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        9/14/2016  11:17 AM       16515072 etrace.etl
```
> [!NOTE]
> Cet exemple illustre l’ajout d’un fournisseur de suivi unique à la session, mais vous pouvez également utiliser l’applet de commande ```Add-EtwTraceProvider``` plusieurs fois sur une session de suivi avec des GUID de fournisseur différents pour activer le suivi par plusieurs sources. Une autre solution consiste à utiliser les profils ```wpr.exe``` décrits ci-dessous.

### <a name="record-traces-from-multiple-etw-providers"></a>Enregistrer les suivis de plusieurs fournisseurs ETW
L’option ```-profiles``` de l’[Enregistreur de performance Windows](https://msdn.microsoft.com/library/hh448229.aspx) permet le suivi par plusieurs fournisseurs en même temps. Vous pouvez choisir parmi un certain nombre de profils intégrés (CPU, Network, DiskIO, etc.) :
```
PS C:\Users\Administrator\Documents> wpr.exe -profiles 

Microsoft Windows Performance Recorder Version 10.0.14393 (CoreSystem)
Copyright (c) 2015 Microsoft Corporation. All rights reserved.

        GeneralProfile              First level triage
        CPU                         CPU usage
        DiskIO                      Disk I/O activity
        FileIO                      File I/O activity
        Registry                    Registry I/O activity
        Network                     Networking I/O activity
        Heap                        Heap usage
        Pool                        Pool usage
        VirtualAllocation           VirtualAlloc usage
        Audio                       Audio glitches
        Video                       Video glitches
        Power                       Power usage
        InternetExplorer            Internet Explorer
        EdgeBrowser                 Edge Browser
        Minifilter                  Minifilter I/O activity
        GPU                         GPU activity
        Handle                      Handle usage
        XAMLActivity                XAML activity
        HTMLActivity                HTML activity
        DesktopComposition          Desktop composition activity
        XAMLAppResponsiveness       XAML App Responsiveness analysis
        HTMLResponsiveness          HTML Responsiveness analysis
        ReferenceSet                Reference Set analysis
        ResidentSet                 Resident Set analysis
        XAMLHTMLAppMemoryAnalysis   XAML/HTML application memory analysis
        UTC                         UTC Scenarios
        DotNET                      .NET Activity
        WdfTraceLoggingProvider     WDF Driver Activity
```

Pour obtenir des instructions détaillées sur la création de profils personnalisés, consultez la [documentation sur WPR.exe](https://msdn.microsoft.com/library/windows/hardware/hh448223.aspx).

### <a name="record-etw-traces-during-operating-system-boot-time"></a>Enregistrer des suivis ETW pendant le démarrage du système d’exploitation
Pour collecter les événements pendant le démarrage du système, utilisez l’applet de commande ```New-AutologgerConfig```. Son utilisation est très similaire à celle de l’applet de commande ```New-EtwTraceSession```, mais les fournisseurs ajoutés à la configuration du journal automatique seront seulement activés au début lors du prochain démarrage. Le flux de travail global se présente comme suit :

Commencez par créer une nouvelle configuration de journal automatique.
```
PS C:\> New-AutologgerConfig -Name "BootPnpLog" -LocalFilePath c:\bootpnp.etl 
```

Ajoutez un fournisseur ETW à cette configuration. Dans cet exemple, le fournisseur Plug-and-Play de noyau est utilisé. Appelez ```Add-EtwTraceProvider``` à nouveau en indiquant le même nom de journal automatique mais un GUID différent pour activer la collecte de suivis de démarrage de plusieurs sources.
```
Add-EtwTraceProvider -Guid "{9c205a39-1250-487d-abd7-e831c6290539}" -AutologgerName BootPnpLog
```

La session ETW ne démarre pas immédiatement, mais au prochain démarrage. Après le redémarrage, une nouvelle session ETW présentant le nom de la configuration de journal automatique est automatiquement démarrée avec les fournisseurs de suivi ajoutés activés. Une fois Nano Server démarré, la commande suivante permet d’arrêter la session de suivi après le vidage des événements consignés dans le fichier de suivi associé :
```
PS C:\> Remove-EtwTraceSession -Name BootPnpLog
```

Pour empêcher la création automatique d’une autre session de suivi au prochain démarrage, supprimez la configuration de journal automatique en procédant comme suit :
```
PS C:\> Remove-AutologgerConfig -Name BootPnpLog
```

Pour collecter les suivis de démarrage et d’installation sur un certain nombre de systèmes ou sur un système sans disque, vous pouvez utiliser la [collecte d’événements de configuration et de démarrage](../administration/get-started-with-setup-and-boot-event-collection.md).

### <a name="capture-performance-counter-data"></a>Capturer les données de compteur de performance
En règle générale, l’analyse des données de compteur de performance se fait à l’aide de l’interface utilisateur graphique Perfmon.exe. Sur Nano Server, utilisez l’équivalent de ligne de commande ```Typeperf.exe```. Par exemple :

Interrogez les compteurs disponibles (vous pouvez filtrer la sortie pour trouver facilement ceux qui vous intéressent).
```
PS C:\> typeperf.exe -q | Select-String "UDPv6"

\UDPv6\Datagrams/sec
\UDPv6\Datagrams Received/sec
\UDPv6\Datagrams No Port/sec
\UDPv6\Datagrams Received Errors
\UDPv6\Datagrams Sent/sec
```

Les options permettent de spécifier le nombre de fois que les valeurs de compteur sont collectées et l’intervalle entre deux collectes. Dans l’exemple ci-dessous, la durée d’inactivité de processeur est collectée 5 fois avec un intervalle de 3 secondes.
```
PS C:\> typeperf.exe "\Processor Information(0,0)\% Idle Time" -si 3 -sc 5

"(PDH-CSV 4.0)","\\ns-g2\Processor Information(0,0)\% Idle Time"
"09/15/2016 09:20:56.002","99.982990"
"09/15/2016 09:20:59.002","99.469634"
"09/15/2016 09:21:02.003","99.990081"
"09/15/2016 09:21:05.003","99.990454"
"09/15/2016 09:21:08.003","99.998577"
Exiting, please wait...
The command completed successfully.
```

Les autres options de ligne de commande vous permettent entre autres de définir les noms des compteurs de performance qui vous intéressent dans un fichier de configuration, en redirigeant la sortie vers un fichier journal. Pour plus d’informations, consultez la [documentation sur typeperf.exe](https://technet.microsoft.com/library/bb490960.aspx).

Vous pouvez également utiliser l’interface graphique de Perfmon.exe à distance avec des cibles Nano Server. Lorsque vous ajoutez des compteurs de performance à l’affichage, spécifiez la cible Nano Server dans le nom de l’ordinateur au lieu de la valeur par défaut *<Local computer>* .

### <a name="interact-with-the-windows-event-log"></a>Interagir avec le journal des événements Windows

Nano Server prend en charge l’applet de commande ```Get-WinEvent```, qui fournit des fonctionnalités de filtrage et d’interrogation du journal des événements Windows, aussi bien en local que sur un ordinateur distant. Vous trouverez les options détaillées ainsi que des exemples dans la [page de documentation sur Get-WinEvent](https://technet.microsoft.com/library/hh849682.aspx). Cet exemple simple récupère les erreurs (*Errors*) consignées dans le journal *System* au cours des deux derniers jours.
```
PS C:\> $StartTime = (Get-Date) - (New-TimeSpan -Day 2)
PS C:\> Get-WinEvent -FilterHashTable @{LogName='System'; Level=2; StartTime=$StartTime} | select TimeCreated, Message

TimeCreated           Message
-----------           -------
9/15/2016 11:31:19 AM Task Scheduler service failed to start Task Compatibility module. Tasks may not be able to reg...
9/15/2016 11:31:16 AM The Virtualization Based Security enablement policy check at phase 6 failed with status: {File...
9/15/2016 11:31:16 AM The Virtualization Based Security enablement policy check at phase 0 failed with status: {File...
```

Nano Server prend également en charge ```wevtutil.exe```, qui permet de récupérer des informations sur les journaux et les éditeurs d’événements. Pour plus d’informations, consultez la [documentation sur wevtutil.exe](https://aka.ms/qvod7p). 

### <a name="graphical-interface-tools"></a>Outils d’interface graphique
Des [outils web de gestion de serveur](https://blogs.technet.microsoft.com/servermanagement/2016/08/17/deploy-setup-server-management-tools/) peuvent être utilisés pour gérer à distance des cibles Nano Server et présenter un journal des événements Nano Server à l’aide d’un navigateur web. Enfin, l’observateur d’événements du composant logiciel enfichable MMC (eventvwr.msc) peut également être utilisé pour consulter les journaux. Il suffit pour cela de l’ouvrir sur un ordinateur doté d’un bureau et de le faire pointer vers un serveur Nano Server distant.




## <a name="using-windows-powershell-desired-state-configuration-with-nano-server"></a>Utilisation de la configuration d’état souhaité Windows PowerShell avec Nano Server  
  
Vous pouvez gérer Nano Server en tant que nœuds cibles avec la configuration d’état souhaité Windows PowerShell (DSC). Actuellement, vous pouvez gérer les nœuds exécutant Nano Server avec DSC en mode par envoi uniquement. Certaines fonctionnalités DSC ne sont pas compatibles avec Nano Server.  
  
Pour plus d’informations, consultez [Utilisation de DSC sur Nano Server](https://msdn.microsoft.com/powershell/dsc/nanoDsc).  
