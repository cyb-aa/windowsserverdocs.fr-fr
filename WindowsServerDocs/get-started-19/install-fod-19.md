---
title: Fonctionnalité à la demande de compatibilité des applications Server Core
description: Guide pratique pour installer des fonctionnalités Windows Server à la demande
ms.prod: windows-server
ms.technology: server-general
ms.topic: article
ms.assetid: 99f7daa4-30ce-4d13-be65-0a4d55cca754
author: jasongerend
ms.author: jgerend
manager: jasgroce
ms.localizationpriority: medium
ms.date: 06/07/2019
ms.openlocfilehash: 9ba0303d1eebc2138db7c5f1428ce920b0cb8af0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71360823"
---
# <a name="server-core-app-compatibility-feature-on-demand-fod"></a>Fonctionnalité à la demande de compatibilité des applications Server Core

> S’applique à : Windows Server 2019, Windows Server (canal semi-annuel)

La **fonctionnalité à la demande de compatibilité des applications Server Core** est un package de fonctionnalités facultatif qui peut être ajouté à des installations Server Core de Windows Server 2019, ou au canal semi-annuel Windows Server, à tout moment.

Pour plus d’informations sur les fonctionnalités à la demande, consultez [Fonctionnalités à la demande](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities).

## <a name="why-install-the-app-compatibility-fod"></a>Pourquoi installer la fonctionnalité à la demande de compatibilité des applications ?

La Compatibilité des applications, fonctionnalité à la demande de Server Core, améliore considérablement la compatibilité des applications de l’option d’installation Windows Server Core en incluant une partie des fichiers binaires et des packages de Windows Server avec Expérience utilisateur, sans ajouter l’environnement graphique de l’Expérience utilisateur de Windows Server. Ce package facultatif est disponible sur un fichier ISO distinct, ou à partir de Windows Update, mais il peut être ajouté à des installations et images Windows Server Core uniquement.

Les deux valeurs principales fournies par la fonctionnalité à la demande de compatibilité des applications sont les suivantes :

- Augmente la compatibilité de Server Core pour les applications serveur qui sont déjà sur le marché ou qui ont déjà été développées par des organisations et déployées.
- Contribue à fournir des composants de système d’exploitation et à augmenter la compatibilité des applications des outils logiciels utilisés dans les scénarios de résolution des problèmes et de débogage perspicaces.

Les composants de système d’exploitation qui sont disponibles dans le cadre de la fonctionnalité à la demande de compatibilité des applications Server Core sont notamment les suivants :

-   Microsoft Management Console (mmc.exe)

-   Observateur d’événements (Eventvwr.msc)

-   Analyseur de performances (PerfMon.exe)

-   Moniteur de ressource (Resmon.exe)

-   Gestionnaire de périphériques (Devmgmt.msc)

-   Explorateur de fichiers (Explorer.exe)

-   Windows PowerShell (Powershell_ISE.exe)

-   Gestion des disques (Diskmgmt.msc)

-   Gestionnaire du cluster de basculement (CluAdmin.msc)

    -   Nécessite l’ajout préalable de la fonctionnalité Clustering de basculement de Windows Server.

        -   À partir d’une session PowerShell avec élévation de privilèges : 

            ```PowerShell
            Install-WindowsFeature -NameFailover-Clustering -IncludeManagementTools
            ```

        -   Pour exécuter le Gestionnaire du cluster de basculement, entrez **cluadmin** à l’invite de commandes.

Les serveurs exécutant Windows Server, version 1903 et ultérieure, prennent également en charge les composants suivants (quand vous utilisez la même version de la fonctionnalité à la demande de compatibilité des applications) :

- Gestionnaire Hyper-V (virtmgmt.msc)
- Planificateur de tâches (taskschd.msc)

## <a name="installing-the-app-compatibility-fod"></a>Installation de la fonctionnalité à la demande de compatibilité des applications

La fonctionnalité à la demande de compatibilité des applications peut uniquement être installée sur Server Core. N’essayez pas d’ajouter la fonctionnalité à la demande de compatibilité des applications Server Core à une installation de Windows Server avec Expérience utilisateur. Il est possible d’utiliser les mêmes fichiers ISO de packages facultatifs de fonctionnalités à la demande pour les installations Server Core de Windows Server 2019 que pour les installations du canal semi-annuel Windows Server.

1. Si le serveur peut se connecter à Windows Update, il vous suffit d’exécuter la commande suivante à partir d’une session PowerShell avec élévation de privilèges, puis de redémarrer Windows Server une fois l’exécution de la commande terminée :

    ```PowerShell
    Add-WindowsCapability -Online -Name ServerCore.AppCompatibility~~~~0.0.1.0
    ```

2. Si le serveur ne peut pas se connecter à Windows Update, téléchargez à la place les fichiers ISO de packages facultatifs de fonctionnalités à la demande de serveur, puis copiez le fichier ISO dans un dossier partagé sur votre réseau local :

   - Si vous disposez d’une licence en volume, vous pouvez télécharger le fichier image ISO de fonctionnalités à la demande de serveur à partir du même portail que celui où le fichier image ISO du système d’exploitation est obtenu : [Centre de gestion des licences en volume](https://www.microsoft.com/Licensing/servicecenter/default.aspx).
   - Le fichier image ISO de fonctionnalités à la demande de serveur est également à la disposition des abonnés dans le [Centre d’évaluation de Microsoft](https://www.microsoft.com/evalcenter/evaluate-windows-server) ou sur le [portail Visual Studio](https://visualstudio.microsoft.com).

3. Connectez-vous avec un compte d’administrateur sur l’ordinateur Server Core qui est connecté à votre réseau local auquel vous voulez ajouter les fonctionnalités à la demande.

4. Utilisez **net use**, ou toute autre méthode, pour vous connecter à l’emplacement du fichier ISO des fonctionnalités à la demande.

5. Copiez le fichier ISO des fonctionnalités à la demande dans le dossier local de votre choix.

6. Montez le fichier ISO des fonctionnalités à la demande en utilisant la commande suivante dans une session PowerShell avec élévation de privilèges :

    ```PowerShell
    Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso
    ```

7. Exécutez la commande suivante :

    ```PowerShell
    Add-WindowsCapability -Online -Name ServerCore.AppCompatibility~~~~0.0.1.0 -Source <Mounted_Server_FOD_Drive> -LimitAccess
     ```

8. Une fois la barre de progression terminée, redémarrez le système d’exploitation.

   Pour plus d’informations sur les commandes DISM, consultez [Utiliser DISM dans Windows PowerShell](https://docs.microsoft.com/windows-hardware/manufacture/desktop/use-dism-in-windows-powershell-s14).

## <a name="to-optionally-add-internet-explorer-11-to-server-core-after-adding-the-server-core-app-compatibility-fod"></a>Pour ajouter éventuellement Internet Explorer 11 pour Server Core (après avoir ajouté la fonctionnalité à la demande de compatibilité des applications Server Core)

 >[!NOTE]  
   > La fonctionnalité à la demande de compatibilité des applications Server Core est nécessaire pour ajouter Internet Explorer 11, mais Internet Explorer 11 n’est pas nécessaire pour ajouter la fonctionnalité à la demande de compatibilité des applications Server Core.

1. Connectez-vous en tant qu’administrateur sur l’ordinateur Server Core auquel la fonctionnalité à la demande de compatibilité des applications a déjà été ajoutée et sur lequel le fichier ISO de packages facultatifs de fonctionnalités à la demande a été copié localement.

2. Démarrez PowerShell en entrant **powershell.exe** à une invite de commandes.

3. Montez le fichier ISO des fonctionnalités à la demande à l’aide de la commande suivante :

    ```PowerShell
    Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso
    ```

4. Exécutez la commande suivante, en utilisant la variable `$package_path` pour entrer le chemin du fichier cab Internet Explorer :

    ```PowerShell
    $package_path = "D:\Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~~.cab"

    Add-WindowsPackage -Online -PackagePath $package_path
    ```

5. Une fois la barre de progression terminée, redémarrez le système d’exploitation.

## <a name="release-notes-and-suggestions-for-the-server-core-app-compatibility-fod-and-internet-explorer-11-optional-package"></a>Notes de publication et suggestions relatives à la fonctionnalité à la demande de compatibilité des applications Server Core et au package facultatif Internet Explorer 11

> [!IMPORTANT]
> Les fonctionnalités à la demande installées sur Windows Server version 1809 ne restent pas en place après une mise à niveau sur place vers Windows Server version 1903. Vous devez donc les réinstaller après la mise à niveau. Vous pouvez également ajouter des fonctionnalités à la demande à la nouvelle source d’installation de Windows Server avant d’effectuer la mise à niveau. Cela garantit que la nouvelle version de toutes les fonctionnalités à la demande est présente une fois la mise à niveau terminée. Pour plus d’informations, consultez [Ajout de fonctionnalités et de packages facultatifs à une image Server Core WIM hors connexion](install-fod-19.md#add-capabilities).

- **Important :** Lisez les notes de publication de Windows Server 2019 si vous rencontrez des problèmes, pour connaître les éléments à prendre en considération ou pour obtenir des instructions avant d’installer et d’utiliser la fonctionnalité à la demande de compatibilité des applications Server Core et au package facultatif Internet Explorer 11.

- Il est possible de rencontrer un scintillement de l’écran avec l’expérience de console Server Core lors de l’ajout de la fonctionnalité à la demande de compatibilité des applications après avoir utilisé Windows Update pour installer des mises à jour cumulatives.  Ce problème est résolu avec les mises à jour de décembre 2018.  Pour obtenir plus d’informations et les étapes de résolution, consultez l’[article 4481610 de la Base de connaissances : L’écran scintille après l’installation de la fonctionnalité à la demande de compatibilité des applications Server Core dans Windows Server 2019 Server Core](https://support.microsoft.com/help/4481610/screen-flickers-after-fod-installation-windows2019-server-core).

- Après l’installation de la fonctionnalité à la demande de compatibilité des applications et le redémarrage du serveur, le cadre de fenêtre de la console de commande adopte une nuance de bleu différente.

- Si vous choisissez d’installer également le package facultatif Internet Explorer 11, notez qu’un double-clic pour ouvrir des fichiers .htm enregistrés localement n’est pas pris en charge. Toutefois, vous pouvez **cliquer avec le bouton droit** et choisir **Ouvrir avec Internet Explorer**, ou vous pouvez l’ouvrir directement à partir de **Fichier** -> **Ouvrir** dans Internet Explorer.

- Pour renforcer la compatibilité des applications de Server Core avec la fonctionnalité à la demande de compatibilité des applications, la Console de gestion IIS a été ajoutée à Server Core en tant que composant facultatif.  Toutefois, il est absolument nécessaire d’ajouter d’abord la fonctionnalité à la demande de compatibilité des applications pour utiliser la Console de gestion IIS. La Console de gestion IIS s’appuie sur Microsoft Management Console (mmc.exe), qui est disponible uniquement sur Server Core avec l’ajout de la fonctionnalité à la demande de compatibilité des applications.  Utilisez [**Install-WindowsFeature**](https://docs.microsoft.com/powershell/module/microsoft.windows.servermanager.migration/install-windowsfeature?view=win10-ps) de PowerShell pour ajouter la Console de gestion IIS.

- D’une manière générale, lors de l’installation d’applications sur Server Core (avec ou sans ces packages facultatifs), il est parfois nécessaire d’utiliser des instructions et des options d’installation sans assistance. 
    
  - À titre d’exemple, SQL Server Management Studio pour SQL Server 2016 et SQL Server 2017 peut être installé sur Server Core. De plus, il est entièrement opérationnel quand la fonctionnalité à la demande de compatibilité des applications est présente.  Consultez [Installer SQL Server à partir de l’invite de commandes](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-from-the-command-prompt?view=sql-server-2017).
  - Si SQL Server Management Studio n’est pas souhaité, il est inutile d’installer la fonctionnalité à la demande de compatibilité des applications Server Core.  Consultez [Installer SQL Server sur Server Core](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-on-server-core?view=sql-server-2017).

## <a name="a-idadd-capabilities-adding-capabilities-and-optional-packages-to-an-offline-wim-server-core-image"></a><a id="add-capabilities"> Ajout de fonctionnalités et de packages facultatifs à une image Server Core WIM hors connexion

1. Téléchargez les fichiers image ISO Windows Server et de fonctionnalités à la demande de serveur dans un dossier local sur un ordinateur Windows.

   - Si vous disposez d’une licence en volume, vous pouvez télécharger les fichiers image ISO Windows Server et de fonctionnalités à la demande de serveur à partir du [Centre de gestion des licences en volume](https://www.microsoft.com/Licensing/servicecenter/default.aspx).
   - Le fichier image ISO de fonctionnalités à la demande de serveur est également disponible aux abonnés pour les versions du canal de maintenance à long terme dans le [Centre d’évaluation de Microsoft](https://www.microsoft.com/evalcenter/evaluate-windows-server) ou sur le [portail Visual Studio](https://visualstudio.microsoft.com).

2. Ouvrez une session PowerShell en tant qu’administrateur, puis utilisez les commandes suivantes pour monter les fichiers image en tant que lecteurs :

   ```PowerShell
   Mount-DiskImage -ImagePath Path_To_ServerFOD_ISO
   Mount-DiskImage -ImagePath Path_To_Windows_Server_ISO
   ```

3. Copiez le contenu du fichier ISO Windows Server dans un dossier local (par exemple, *C:\SetupFiles\WindowsServer*).

4. Obtenez le nom de l’image que vous voulez modifier dans le fichier Install.wim à l’aide de la commande suivante.<br>
Utilisez la variable `$install_wim_path` pour entrer le chemin du fichier Install.wim, situé dans le dossier \Sources du fichier ISO.

   ```PowerShell
   $install_wim_path = "C:\SetupFiles\WindowsServer\sources\install.wim"

   Get-WindowsImage -ImagePath $install_wim_path
   ```

5. Montez le fichier Install.wim dans un nouveau dossier à l’aide de la commande suivante, en remplaçant les exemples de valeurs des variables par vos propres valeurs et en réutilisant la variable `$install_wim_path` de la commande précédente.<br>

   - `$image_name` -Entrez le nom de l’image que vous voulez monter.
   - `$mount_folder variable` - Spécifiez le dossier à utiliser lors de l’accès au contenu du fichier Install.wim.

   ```PowerShell
   $image_name = "Windows Server Datacenter"
   $mount_folder = "c:\test\offline"

   Mount-WindowsImage -ImagePath $install_wim_path -Name $image_name -path $mount_folder
   ```

6. Ajoutez les fonctionnalités et les packages souhaités à l’image Install.wim montée à l’aide des commandes suivantes, en remplaçant les exemples de valeurs des variables par vos propres valeurs.<br>

   - `$capability_name` - Spécifiez le nom de la fonctionnalité à installer (dans le cas présent, la fonctionnalité AppCompatibility).
   - `$package_path` - Spécifiez le chemin du package à installer (dans le cas présent, Internet Explorer).
   - `$fod_drive` - Spécifiez la lettre de lecteur de l’image des fonctionnalités à la demande de serveur montée.

   ```PowerShell
   $capability_name = "ServerCore.AppCompatibility~~~~0.0.1.0"
   $package_path = "D:\Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~~.cab"
   $fod_drive = "d:\"

   Add-WindowsCapability -Path $mount_folder -Name $capability_name -Source $fod_drive -LimitAccess
   Add-WindowsPackage -Path $mount_folder -PackagePath $package_path
   ```

7. Démontez et validez les changements apportés au fichier Install.wim à l’aide de la commande suivante, qui utilise la variable `$mount_folder` des commandes précédentes :

   ```PowerShell
   Dismount-WindowsImage -Path $mount_folder -Save
   ```

Vous pouvez maintenant mettre à niveau votre serveur en exécutant setup.exe à partir du dossier que vous avez créé pour les fichiers d’installation de Windows Server (dans cet exemple : *C:\SetupFiles\WindowsServer*). Ce dossier contient maintenant les fichiers d’installation de Windows Server avec les fonctionnalités supplémentaires et les packages facultatifs inclus.