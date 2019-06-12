---
title: Fonctionnalité à la demande (FOD) de compatibilité des applications Server Core
description: Comment installer des fonctionnalités de Windows Server à la demande
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
ms.assetid: 99f7daa4-30ce-4d13-be65-0a4d55cca754
author: jasongerend
ms.author: jgerend
manager: jasgroce
ms.localizationpriority: medium
ms.date: 06/07/2019
ms.openlocfilehash: 747258601aa05885d209aacde6947eb7b05e8121
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66810796"
---
# <a name="server-core-app-compatibility-feature-on-demand-fod"></a>Fonctionnalité à la demande (FOD) de compatibilité des applications Server Core

> S’applique à : Windows Server 2019, canal semi-annuel de serveur Windows

Le **fonctionnalité de compatibilité de Server Core application à la demande** est un fonctionnalité facultative du package qui peut être ajouté à des installations de Windows Server 2019 Server Core, ou canal semi-annuel de serveur Windows, à tout moment.

Pour plus d’informations sur les fonctionnalités à la demande (DOM), consultez [fonctionnalités On Demand](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities).

## <a name="why-install-the-app-compatibility-fod"></a>Pourquoi installer DOM de compatibilité d’application ?

Compatibilité des applications, une fonctionnalité à la demande pour Server Core, améliore considérablement la compatibilité d’application de l’option d’installation de Windows Server Core en incluant un sous-ensemble des fichiers binaires et des packages à partir de Windows Server avec expérience utilisateur, sans ajouter le Environnement graphique d’expérience de bureau de Windows Server. Ce package facultatif est disponible sur un fichier ISO distinct, ou à partir de Windows Update, mais ne peut être ajouté aux images et les installations de Windows Server Core.

Les deux valeurs de principales que fournit le DOM de compatibilité d’application sont :

- Augmente la compatibilité de Server Core pour les applications serveur qui sont déjà dans le marché ou qui ont déjà été développés par les organisations et déployés.
- Aide en fournissant des composants de système d’exploitation et compatibilité des applications accrue des outils logiciels utilisés dans la résolution des problèmes et scénarios de débogage.

Composants de système d’exploitation qui sont disponibles comme partie du DOM de compatibilité de Server Core application incluent :

-   Microsoft Management Console (mmc.exe)

-   Observateur d’événements (Eventvwr.msc)

-   Analyseur de performances (PerfMon.exe)

-   Moniteur de ressource (Resmon.exe)

-   Gestionnaire de périphériques (Devmgmt.msc)

-   Explorateur de fichiers (Explorer.exe)

-   Windows PowerShell (Powershell_ISE.exe)

-   Gestion des disques (Diskmgmt.msc)

-   Gestionnaire du Cluster de basculement (CluAdmin.msc)

    -   Nécessite l’ajout de la fonctionnalité de serveur de Windows de Clustering de basculement tout d’abord.

        -   À partir d’une session PowerShell avec élévation de privilèges : 

            ```PowerShell
            Install-WindowsFeature -NameFailover-Clustering -IncludeManagementTools
            ```

        -   Pour exécuter le Gestionnaire du Cluster de basculement, entrez **cluadmin** à l’invite de commandes.

Les serveurs exécutant Windows Server, version 1903 et versions ultérieure prennent également en charge les composants suivants (lorsque vous utilisez la même version du DOM de compatibilité d’application) :

- Gestionnaire Hyper-V (virtmgmt.msc)
- Planificateur de tâches (taskschd.msc)

## <a name="installing-the-app-compatibility-fod"></a>L’installation de la compatibilité des applications DOM

Le DOM de compatibilité d’application peut uniquement être installé sur Server Core. N’essayez pas d’ajouter le DOM de compatibilité de Server Core application vers une installation de Windows Server de Windows Server avec la fonctionnalité expérience utilisateur. Les packages facultatifs DOM mêmes ISO utilisable pour les installations Server Core de Windows Server 2019 ou les installations de canal semi-annuel de serveur Windows.

1. Si le serveur peut se connecter à Windows Update, il vous suffit est exécutez la commande suivante à partir d’une session PowerShell avec élévation de privilèges et puis redémarrez Windows Server après la fin de la commande :

    ```PowerShell
    Add-WindowsCapability -Online -Name ServerCore.AppCompatibility~~~~0.0.1.0
    ```

2. Si le serveur ne peut pas se connecter à Windows Update, au lieu de cela télécharger les packages facultatifs du serveur DOM ISO et copier l’image ISO sur un dossier partagé sur votre réseau local :

   - Si vous avez une licence en volume, vous pouvez télécharger le fichier image ISO de DOM de serveur à partir du même portail où le fichier image ISO du système d’exploitation est obtenu : [Volume Licensing Service Center](https://www.microsoft.com/Licensing/servicecenter/default.aspx).
   - Le fichier image ISO de DOM de serveur est également disponible sur le [Microsoft Evaluation Center](https://www.microsoft.com/evalcenter/evaluate-windows-server) ou sur le [portail Visual Studio](https://visualstudio.microsoft.com) pour les abonnés.

3. Connectez-vous avec un compte d’administrateur sur l’ordinateur Server Core, qui est connecté à votre réseau local et que vous souhaitez ajouter le DOM à.

4. Utilisez **net utilisation**, ou toute autre méthode pour se connecter à l’emplacement de l’ISO FOD.

5. Copiez l’ISO FOD dans un dossier local de votre choix.

6. Monter l’ISO FOD à l’aide de la commande suivante dans une session PowerShell avec élévation de privilèges :

    ```PowerShell
    Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso
    ```

7. Exécutez la commande suivante :

    ```PowerShell
    Add-WindowsCapability -Online -Name ServerCore.AppCompatibility~~~~0.0.1.0 -Source <Mounted_Server_FOD_Drive> -LimitAccess
     ```

8. Une fois la barre de progression terminée, redémarrez le système d’exploitation.

   Pour plus d’informations sur les commandes DISM, consultez [utiliser DISM dans Windows PowerShell](https://docs.microsoft.com/windows-hardware/manufacture/desktop/use-dism-in-windows-powershell-s14)

## <a name="to-optionally-add-internet-explorer-11-to-server-core-after-adding-the-server-core-app-compatibility-fod"></a>Pour ajouter éventuellement Internet Explorer 11 pour Server Core (après avoir ajouté le DOM de compatibilité Application Server Core)

 >[!NOTE]  
   > Server Core application compatibilité DOM est requis pour l’ajout d’Internet Explorer 11, mais Internet Explorer 11 n’est pas nécessaire d’ajouter le DOM de compatibilité Application Server Core.

1. Connectez-vous en tant qu’administrateur sur l’ordinateur Server Core ayant le DOM de compatibilité d’application déjà ajouté et le package facultatif Server DOM QU'ISO copié localement.

2. Démarrez PowerShell en entrant **powershell.exe** à une invite de commandes.

3. Montez l’ISO FOD à l’aide de la commande suivante :

    ```PowerShell
    Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso
    ```

4. Exécutez la commande suivante à l’aide de la commande le `$package_path` variable à entrer le chemin d’accès au fichier cab Internet Explorer :

    ```PowerShell
    $package_path = "D:\Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~~.cab"

    Add-WindowsPackage -Online -PackagePath $package_path
    ```

5. Une fois la barre de progression terminée, redémarrez le système d’exploitation.

## <a name="release-notes-and-suggestions-for-the-server-core-app-compatibility-fod-and-internet-explorer-11-optional-package"></a>Notes de publication et des suggestions pour le package facultatif de Server Core application compatibilité DOM et Internet Explorer 11

> [!IMPORTANT]
> FODs installés sur Windows Server, version 1809 ne reste en place après une mise à niveau sur place vers Windows Server, version 1903, afin que vous seriez obligé de les installer à nouveau après la mise à niveau. Vous pouvez également ajouter FODs vers la nouvelle source d’installation de Windows Server avant la mise à niveau. Cela garantit que la nouvelle version de n’importe quel FODs figurent une fois la mise à niveau terminée. Pour plus d’informations, consultez le [Ajout de fonctionnalités et des packages facultatifs à une image WIM Server Core hors connexion](install-fod-19.md#add-capabilities).

- **Important :** Lire les notes de publication de Windows Server 2019 pour tout problème, les considérations relatives à l’ou les instructions avant de procéder à l’installation et l’utilisation du package facultatif Server Core application compatibilité DOM et Internet Explorer 11.

- Il est possible de rencontrer lors de l’ajout du DOM de compatibilité d’application après l’utilisation de la mise à jour de Windows pour installer les mises à jour cumulatives le scintillement avec l’expérience de console Server Core.  Ce problème est résolu avec décembre 2018 met à jour.  Pour plus d’informations et la résolution, consultez [article de la Base de connaissances 4481610 : Écran clignote après l’installation de Server Core application compatibilité DOM dans Windows Server 2019 Server Core](https://support.microsoft.com/help/4481610/screen-flickers-after-fod-installation-windows2019-server-core).

- Après l’installation du DOM de compatibilité d’application et de redémarrage du serveur, la couleur de frame de fenêtre de console commande devient un dégradé Bleu différents.

- Si vous choisissez d’installer également le package facultatif Internet Explorer 11, notez que double-clic pour ouvrir les fichiers .htm enregistré localement n’est pas pris en charge. Toutefois, vous pouvez **avec le bouton droit** et choisissez **ouvrir avec IE**, ou vous pouvez l’ouvrir directement à partir d’Internet Explorer **fichier** -> **ouvrir**.

- Pour améliorer la compatibilité des applications de Server Core avec le DOM de compatibilité d’application, la Console de gestion IIS a été ajoutée pour Server Core en tant que composant facultatif.  Toutefois, il est absolument nécessaire d’abord ajouter le DOM de compatibilité d’application pour utiliser la Console de gestion IIS. Console de gestion IIS s’appuie sur Microsoft Management Console (mmc.exe), qui est uniquement disponible sur Server Core avec l’ajout des DOM de compatibilité d’application.  Utiliser Powershell [ **Install-WindowsFeature** ](https://docs.microsoft.com/powershell/module/microsoft.windows.servermanager.migration/install-windowsfeature?view=win10-ps) pour ajouter la Console de gestion IIS.

- Comme un point général de conseils, lors de l’installation d’applications sur le serveur de base (avec ou sans ces packages facultatifs) il est parfois nécessaire d’utiliser des instructions et des options d’installation sans assistance. 
    
  - Par exemple, SQL Server Management Studio pour SQL Server 2016 et SQL Server 2017 peut être installé sur Server Core et est entièrement fonctionnelle lorsque le DOM de compatibilité d’application est présent.  Consultez, [installer SQL Server à partir de l’invite de commandes](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-from-the-command-prompt?view=sql-server-2017).
  - Si SQL Server Management Studio n’est pas souhaité, il est inutile d’installer Server Core application compatibilité DOM.  Consultez, [installer SQL Server sur Server Core](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-on-server-core?view=sql-server-2017).

## <a name="a-idadd-capabilities-adding-capabilities-and-optional-packages-to-an-offline-wim-server-core-image"></a><a id="add-capabilities"> Ajout de fonctionnalités et des packages facultatifs à une image WIM Server Core hors connexion

1. Téléchargez les fichiers image Windows Server et serveur DOM ISO dans un dossier local sur un ordinateur Windows.

   - Si vous avez une licence en volume, vous pouvez télécharger les fichiers image Windows Server et serveur DOM ISO à partir de la [Volume Licensing Service Center](https://www.microsoft.com/Licensing/servicecenter/default.aspx).
   - Le fichier image ISO de DOM de serveur est également disponible pour les versions de canal de maintenance à long terme sur le [Microsoft Evaluation Center](https://www.microsoft.com/evalcenter/evaluate-windows-server) ou sur le [portail Visual Studio](https://visualstudio.microsoft.com) pour les abonnés.

2. Ouvrez une session PowerShell en tant qu’administrateur et utilisez ensuite les commandes suivantes pour monter les fichiers image en tant que lecteurs :

   ```PowerShell
   Mount-DiskImage -ImagePath Path_To_ServerFOD_ISO
   Mount-DiskImage -ImagePath Path_To_Windows_Server_ISO
   ```

3. Copie le le contenu du fichier ISO Windows Server dans un dossier local (par exemple, *C:\SetupFiles\WindowsServer*).

4. Obtenir le nom d’image que vous souhaitez modifier dans le fichier Install.wim à l’aide de la commande suivante.<br>
Utilisez le `$install_wim_path` variable à entrer le chemin d’accès au fichier Install.wim, situé dans le dossier \Sources du fichier ISO.

   ```PowerShell
   $install_wim_path = "C:\SetupFiles\WindowsServer\sources\install.wim"

   Get-WindowsImage -ImagePath $install_wim_path
   ```

5. Monter le fichier Install.wim dans un nouveau dossier à l’aide de la commande en remplaçant les valeurs des variables exemple par les vôtres et en réutilisant le `$install_wim_path` variable à partir de la commande précédente.<br>

   - `$image_name` -Entrez le nom de l’image que vous voulez monter.
   - `$mount_folder variable` -Spécifiez le dossier à utiliser lors de l’accès au contenu du fichier Install.wim.

   ```PowerShell
   $image_name = "Windows Server Datacenter"
   $mount_folder = "c:\test\offline"

   Mount-WindowsImage -ImagePath $install_wim_path -Name $image_name -path $mount_folder
   ```

6. Ajouter des capacités et les packages souhaités à l’image Install.wim monté en utilisant les commandes suivantes, en remplaçant les valeurs des variables exemple par les vôtres.<br>

   - `$capability_name` -Spécifiez le nom de la capacité à installer (dans ce cas, la fonctionnalité AppCompatibility).
   - `$package_path` -Spécifiez le chemin d’accès au package à installer (dans ce cas, Internet Explorer).
   - `$fod_drive` -Spécifiez la lettre de lecteur de l’image serveur DOM monté.

   ```PowerShell
   $capability_name = "ServerCore.AppCompatibility~~~~0.0.1.0"
   $package_path = "D:\Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~~.cab"
   $fod_drive = "d:\"

   Add-WindowsCapability -Path $mount_folder -Name $capability_name -Source $fod_drive -LimitAccess
   Add-WindowsPackage -Path $mount_folder -PackagePath $package_path
   ```

7. Démontez et validez des modifications dans le fichier Install.wim à l’aide de la commande suivante, qui utilise le `$mount_folder` variable à partir des commandes précédentes :

   ```PowerShell
   Dismount-WindowsImage -Path $mount_folder -Save
   ```

Vous pouvez maintenant passer votre serveur en exécutant setup.exe à partir du dossier que vous avez créé pour les fichiers d’installation de Windows Server (dans cet exemple : *C:\SetupFiles\WindowsServer*). Ce dossier contient désormais les fichiers d’installation de Windows Server avec les fonctionnalités supplémentaires et les packages facultatifs inclus.