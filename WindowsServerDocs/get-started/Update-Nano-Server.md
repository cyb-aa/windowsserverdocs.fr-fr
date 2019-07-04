---
title: Mise à jour de Nano Server
description: " "
ms.prod: windows-server-threshold
ms.service: na
manager: DonGill
ms.technology: server-nano
ms.date: 09/06/2017
ms.tgt_pltfrm: na
ms.topic: get-started-article
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: 0fd21f5c38968a303169b99186effd3d66f1e69b
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280299"
---
# <a name="updating-nano-server"></a>Mise à jour de Nano Server

> [!IMPORTANT]
> À compter de Windows Server, version 1709, Nano Server sera uniquement disponible sous forme d’[image de système d’exploitation de base du conteneur](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image). Pour en savoir plus, consultez [Changements apportés à Nano Server](nano-in-semi-annual-channel.md). 

Nano Server propose plusieurs méthodes d’actualisation. Par rapport aux autres options d’installation de Windows Server, Nano Server suit un modèle de maintenance plus actif, similaire à celui de Windows 10. Ces versions périodiques sont qualifiées de **CBB (Current Branch for Business, ou branche actuelle pour entreprise)** . Cette approche prend en charge les clients qui veulent innover plus rapidement et profiter des cycles de développement rapides autorisés par le cloud. Pour plus d’informations sur les versions CBB, consultez le [Blog de Windows Server](https://blogs.technet.microsoft.com/windowsserver/2016/07/12/windows-server-2016-new-current-branch-for-business-servicing-option/).

**Entre ces versions CBB**, Nano Server est actualisé par une série de *mises à jour cumulatives*. Par exemple, la première mise à jour cumulative de Nano Server a été publiée le 26 septembre 2016 sous le numéro [KB4093120](https://support.microsoft.com/help/4093120/windows-10-update-kb4093120). Dans cette mise à jour et les suivantes, nous proposons différentes options d’installation sur Nano Server. Dans cet article, nous allons utiliser la mise à jour KB3192366 comme exemple pour montre comment obtenir et appliquer des mises à jour cumulatives sur Nano Server. Pour plus d’informations sur le modèle de mise à jour cumulative, consultez le [blog de Microsoft Update](https://blogs.technet.microsoft.com/mu/2016/10/25/patching-with-windows-server-2016/).

> [!NOTE]
> Si vous installez un package Nano Serveur facultatif à partir d’un support ou d’un référentiel en ligne, aucun correctif de sécurité récent n’est inclus. Pour éviter une incompatibilité de versions entre les packages facultatifs et le système d’exploitation, vous devez installer la dernière mise à jour cumulative immédiatement après avoir installé les packages facultatifs et **avant** de redémarrer le serveur.

Dans le cas de la mise à jour cumulative de Windows Server 2016 du 26 septembre 2016 ([KB3192366](https://support.microsoft.com/en-us/kb/3192366)), vous devez tout d’abord installer la dernière mise à jour de la pile de maintenance pour Windows 10 version 1607 du 23 août 2016 comme prérequis ([KB3176936](https://support.microsoft.com/en-us/kb/3176936)). Pour la plupart des options ci-dessous, vous devez disposer des fichiers .msu contenant les packages de mise à jour .cab. Visitez le catalogue Microsoft Update pour télécharger chacun de ces packages de mise à jour :
- [https://catalog.update.microsoft.com/v7/site/Search.aspx?q=KB3192366](https://catalog.update.microsoft.com/v7/site/Search.aspx?q=KB3192366)
- [https://catalog.update.microsoft.com/v7/site/Search.aspx?q=KB3176936](https://catalog.update.microsoft.com/v7/site/Search.aspx?q=KB3176936)

Après avoir téléchargé les fichiers .msu à partir du catalogue Microsoft Update, enregistrez-les dans un partage réseau ou dans un répertoire local tel que C:\ServicingPackages. Vous pouvez renommer les fichiers .msu en fonction de leur numéro KB, comme nous l’avons fait ci-dessous, pour faciliter leur identification. Puis utilisez l’utilitaire EXPAND pour extraire les fichiers .cab des fichiers .msu dans des répertoires distincts et copier les fichiers .cab dans un dossier.

```code
    mkdir C:\ServicingPackages_expanded
    mkdir C:\ServicingPackages_expanded\KB3176936
    mkdir C:\ServicingPackages_expanded\KB3192366
    Expand C:\ServicingPackages\KB3176936.msu -F:* C:\ServicingPackages_expanded\KB3176936
    Expand C:\ServicingPackages\KB3192366.msu -F:* C:\ServicingPackages_expanded\KB3192366
    mkdir C:\ServicingPackages_cabs
    copy C:\ServicingPackages_expanded\KB3176936\Windows10.0-KB3176936-x64.cab C:\ServicingPackages_cabs
    copy C:\ServicingPackages_expanded\KB3192366\Windows10.0-KB3192366-x64.cab C:\ServicingPackages_cabs
```

Maintenant, vous pouvez utiliser les fichiers .cab extraits pour appliquer les mises à jour à une image Nano Server de différentes manières, selon vos besoins. Les options suivantes ne sont présentées dans aucun ordre particulier de préférence. Utilisez celle qui vous semble la meilleure pour votre environnement.

> [!NOTE]
> Lorsque vous utilisez les outils DISM pour actualiser Nano Server, vous devez utiliser une version de DISM identique ou plus récente que celle de la version de Nano Server que vous employez. Pour ce faire, exécutez l’outil DISM à partir d’une version correspondante de Windows, installez une version correspondante du [Kit de déploiement et d’évaluation Windows (ADK)](https://developer.microsoft.com/en-us/windows/hardware/windows-assessment-deployment-kit) ou exécutez DISM sur Nano Server.

## <a name="option-1-integrate-a-cumulative-update-into-a-new-image"></a>Option 1 : Intégrer une mise à jour cumulative à une nouvelle image
Si vous créez une image Nano Server, vous pouvez y intégrer directement la dernière mise à jour cumulative, afin que tous les correctifs lui soient appliqués au premier démarrage.

```powershell
New-NanoServerImage -ServicingPackagePath 'C:\ServicingPackages_cabs\Windows10.0-KB3176936-x64.cab', 'C:\ServicingPackages_cabs\Windows10.0-KB3192366-x64.cab' -<other parameters>
```

## <a name="option-2-integrate-a-cumulative-update-into-an-existing-image"></a>Option 2 : Intégrer une mise à jour cumulative à une image existante
Si vous utilisez une image Nano Server comme base pour créer des instances spécifiques de Nano Server, vous pouvez y intégrer directement la dernière mise à jour cumulative, afin que tous les correctifs soient appliqués aux ordinateurs créés à l’aide de cette image lors du premier démarrage.

```powershell
Edit-NanoServerImage -ServicingPackagePath 'C:\ServicingPackages_cabs\Windows10.0-KB3176936-x64.cab', 'C:\ServicingPackages_cabs\Windows10.0-KB3192366-x64.cab' -TargetPath .\NanoServer.wim
```

## <a name="option-3-apply-the-cumulative-update-to-an-existing-offline-vhd-or-vhdx"></a>Option 3 : Appliquer la mise à jour cumulative à un disque VHD ou VHDX hors connexion
Si vous avez un disque dur virtuel (VHD ou VHDX), vous pouvez utiliser les outils DISM pour lui appliquer la mise à jour. Vous devez vous assurer que le disque n’est pas en cours d’utilisation, soit en arrêtant les machines virtuelles qui l’utilisent, soit en démontant son fichier.

- À l'aide de PowerShell

   ```powershell
   Mount-WindowsImage -ImagePath .\NanoServer.vhdx -Path .\MountDir -Index 1
   Add-WindowsPackage -Path .\MountDir -PackagePath  C:\ServicingPackages_cabs
   Dismount-WindowsImage -Path .\MountDir -Save
   ```

- Avec dism.exe

   ```code
   dism.exe /Mount-Image /ImageFile:C:\NanoServer.vhdx /Index:1 /MountDir:C:\MountDir
   dism.exe /Image:C:\MountDir /Add-Package /PackagePath:C:\ServicingPackages_cabs
   dism.exe /Unmount-Image /MountDir:C:\MountDir /Commit
   ```

## <a name="option-4-apply-the-cumulative-update-to-a-running-nano-server"></a>Option 4 : Appliquer la mise à jour cumulative à une instance Nano Server en cours d’exécution
Si vous avez une machine virtuelle ou un hôte physique Nano Server en cours d’exécution et que vous avez téléchargé le fichier .cab de la mise à jour, vous pouvez utiliser les outils DISM pour appliquer la mise à jour pendant que le système d’exploitation est en ligne. Vous devez copier le fichier .cab localement sur le Nano Server ou sur un emplacement réseau accessible. Si vous appliquez une mise à jour de la pile de traitements, veillez à redémarrer le serveur après l’opération, avant d’appliquer d’autres mises à jour.

> [!NOTE]
> Si vous avez créé l’image VHD ou VHDX de Nano Server avec l’applet de commande New-NanoServerImage, sans indiquer la taille maximale (MaxSize) du fichier du disque dur virtuel, la taille par défaut (4 Go) est trop petite pour appliquer la mise à jour cumulative. Avant d’installer la mise à jour, utilisez Hyper-V Manager, Disk Management, PowerShell ou un autre outil pour porter la taille du disque dur virtuel et du volume système à au moins 10 Go, ou utilisez le paramètre ScratchDir sur les outils DISM pour localiser le répertoire de travail sur un volume disposant d’au minimum 10 Go d’espace libre.

```powershell
$s = New-PSSession -ComputerName (Read-Host "Enter Nano Server IP address") -Credential (Get-Credential)
Copy-Item -ToSession $s -Path C:\ServicingPackages_cabs -Destination C:\ServicingPackages_cabs -Recurse
Enter-PSSession $s
```

- À l'aide de PowerShell

   ```powershell
   # Apply the servicing stack update first and then restart
   Add-WindowsPackage -Online -PackagePath C:\ServicingPackages_cabs\Windows10.0-KB3176936-x64.cab
   Restart-Computer; exit

   # After restarting, apply the cumulative update and then restart
   Enter-PSSession -ComputerName (Read-Host "Enter Nano Server IP address") -Credential (Get-Credential)
   Add-WindowsPackage -Online -PackagePath C:\ServicingPackages_cabs\Windows10.0-KB3192366-x64.cab
   Restart-Computer; exit
   ```

- Avec dism.exe
   ```powershell
   # Apply the servicing stack update first and then restart
   dism.exe /Online /Add-Package /PackagePath:C:\ServicingPackages_cabs\Windows10.0-KB3176936-x64.cab
   
   # After the operation completes successfully and you are prompted to restart, it's safe to
   # press Ctrl+C to cancel the pipeline and return to the prompt
   Restart-Computer; exit

   # After restarting, apply the cumulative update and then restart
   Enter-PSSession -ComputerName (Read-Host "Enter Nano Server IP address") -Credential (Get-Credential)
   dism.exe /Online /Add-Package /PackagePath:C:\ServicingPackages_cabs\Windows10.0-KB3192366-x64.cab
   Restart-Computer; exit
   ```

## <a name="option-5-download-and-install-the-cumulative-update-to-a-running-nano-server"></a>Option 5 : Télécharger et installer la mise à jour cumulative sur une instance Nano Server en cours d’exécution

Si vous avez une machine virtuelle ou un hôte physique Nano Server en cours d’exécution, vous pouvez utiliser le fournisseur WMI de Windows Update pour télécharger et installer la mise à jour pendant que le système d’exploitation est en ligne. Cela vous évite de télécharger le fichier .msu séparément du catalogue Microsoft Update. Le fournisseur WMI détecte, télécharge et installe toutes les mises à jour disponibles en une seule fois.

```powershell
Enter-PSSession -ComputerName (Read-Host "Enter Nano Server IP address") -Credential (Get-Credential)
```

- Rechercher les mises à jour disponibles
   ```powershell
   $ci = New-CimInstance -Namespace root/Microsoft/Windows/WindowsUpdate -ClassName MSFT_WUOperationsSession  
   $result = $ci | Invoke-CimMethod -MethodName ScanForUpdates -Arguments @{SearchCriteria="IsInstalled=0";OnlineScan=$true}
   $result.Updates
   ```

- Installer toutes les mises à jour disponibles
   ```powershell
   $ci = New-CimInstance -Namespace root/Microsoft/Windows/WindowsUpdate -ClassName MSFT_WUOperationsSession
   Invoke-CimMethod -InputObject $ci -MethodName ApplyApplicableUpdates
   Restart-Computer; exit
   ```

- Obtenir la liste des mises à jour installées
   ```powershell
   $ci = New-CimInstance -Namespace root/Microsoft/Windows/WindowsUpdate -ClassName MSFT_WUOperationsSession
   $result = $ci | Invoke-CimMethod -MethodName ScanForUpdates -Arguments @{SearchCriteria="IsInstalled=1";OnlineScan=$true}
   $result.Updates
   ```
   
## <a name="additional-options"></a>Autres options
D’autres méthodes de mise à jour de Nano Server peuvent remplacer ou compléter celles ci-dessus. Ces options incluent l’utilisation de Windows Server Update Services (WSUS), de System Center Virtual Machine Manager (VMM), du Planificateur de tâches ou d’une solution tierce.
- [Configuration de Windows Update pour WSUS](https://msdn.microsoft.com/library/dd939844(v=ws.10).aspx) en définissant les clés de Registre suivantes :
  - WUServer
  - WUStatusServer (utilise généralement la même valeur que WUServer)
  - UseWUServer
  - AUOptions
- [Gestion des mises à jour de structure dans VMM](https://technet.microsoft.com/library/gg675084(v=sc.12).aspx)
- [Inscription d’une tâche planifiée](https://technet.microsoft.com/library/jj649811.aspx)