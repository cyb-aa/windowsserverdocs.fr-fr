---
title: Personnalisation des dossiers partagés
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 47bc4986-14eb-4a29-9930-83a25704a3a0
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 387f9570e87bd2bd65266489b0f3eac6c945e3be
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80311915"
---
# <a name="customize-shared-folders"></a>Personnalisation des dossiers partagés

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Les dossiers du serveur sont créés par défaut sur la plus grande partition de données sur le disque 0. Les partenaires peuvent personnaliser l'emplacement et spécifier des dossiers de serveurs supplémentaires en suivant les étapes suivantes :  
  
1. À l'aide d'une configuration de partition personnalisée, créer l'image d'usine, puis créer une nouvelle clé de registre de stockage avant d'utiliser sysprep. Au cours de la configuration initiale (IC), la tâche de stockage IC vérifie cette clé de registre. Si elle existe, les dossiers du serveur sont créés par défaut dans le répertoire C:\ServerFolders.  
  
   #### <a name="to-create-a-new-storage-registry-key"></a>Pour créer une nouvelle clé de registre de stockage  
  
   1.  Sur le serveur, déplacez votre souris sur le coin supérieur droit de l’écran, puis cliquez sur **Rechercher**.  
  
   2.  Dans la zone de recherche, tapez **regedit**, puis cliquez sur l’application **Regedit** .  
  
   3.  Dans le volet de navigation, développez **HKEY_LOCAL_MACHINE**, développez **SOFTWARE**, puis développez **Microsoft**.  
  
   4.  Faites un clic droit sur **Windows Server**, cliquez sur **Nouveau**, puis cliquez sur **Clé**.  
  
   5.  Nommez la Clé **Stockage**.  
  
   6.  Dans le volet de navigation, faites un clic droit sur la nouvelle clé de registre de stockage, cliquez sur **Nouveau**, puis cliquez sur **Valeur DWORD (32-bit)** .  
  
   7.  Nommez la chaîne **CreateFoldersOnSystem**.  
  
   8.  Faites un clic droit **CreateFoldersOnSystem**, puis cliquez sur **Modifier**. La boîte de dialogue **Modification de la chaîne** s’affiche.  
  
   9. Attribuez à cette nouvelle clé la valeur **1**, puis cliquez sur **OK**.  
  
2. Utilisez le script PostIC.cmd pour déplacer les dossiers à un emplacement différent ou pour créer des dossiers supplémentaires. Voir l’exemple suivant : [Exemple 1 : créer un dossier personnalisé et déplacer les dossiers par défaut vers un nouvel emplacement de PostIC.cmd à l’aide de Windows PowerShell](Customize-Shared-Folders.md#BKMK_Example1).  
  
3. Utilisez le SDK Windows Server Solutions pour déplacer les dossiers à un emplacement différent ou pour créer des dossiers supplémentaires. Voir l’exemple suivant : [Exemple 2 : créer un dossier personnalisé et déplacer un dossier existant à l’aide des solutions SDK Windows Server](Customize-Shared-Folders.md#BKMK_Example2).  
  
   Il est possible que des partenaires laissent les dossiers de données sur le disque C. Ceci permet à l'utilisateur final ou au revendeur de déterminer la structure des dossiers de données sur les lecteurs de données.  
  
###  <a name="example-1-create-a-custom-folder-and-move-the-default-folders-to-a-new-location-from-posticcmd-by-using-windows-powershell"></a><a name="BKMK_Example1"></a>Exemple 1 : créer un dossier personnalisé et déplacer les dossiers par défaut vers un nouvel emplacement à partir de poster. cmd à l’aide de Windows PowerShell  
  
1.  Créer un fichier PostIC.cmd afin d'exécuter les tâches post configuration initiale comme indiqué dans la section [Créer le fichier PostIC.cmd pour l'exécution des tâches post configuration initiale post](Create-the-PostIC.cmd-File-for-Running-Post-Initial-Configuration-Tasks.md) .  
  
2.  À l’aide du Bloc-notes, créez un fichier nommé **customizefolders.ps1** dans le dossier C:\Windows\Setup\Scripts, puis collez les commandes Windows PowerShell® suivantes dans le fichier (décocher les lignes appropriées selon le comportement désiré).  
  
    ```  
    # Move the Documents folder to D:\ServerFolders  
    #Get-WssFolder -name Documents| Move-WssFolder - NewDrive D:\ -Force  
  
    # Check for last error. If last error is not null, exit with return code 1  
    #If ($error[0] -ne $null) { exit 1}   
  
    # Move all folders to D:\ServerFolders  
    #foreach( $folder in Get-WssFolder )  
    #{  
    #    Move-WssFolder $folder -NewDrive D:\ -Force  
    #}  
  
    # Check for last error. If last error is not null, exit with return code 1  
    #If ($error[0] -ne $null) { exit 1}   
  
    # Create a custom folder named Custom Folder  
    #Add-WssFolder -Name CustomFolder -Path D:\ServerFolders\CustomFolder -Description "Custom Folder"  
  
    # Check for last error. If last error is not null, exit with return code 1  
    #If ($error[0] -ne $null) { exit 1}   
  
    exit 0  
    ```  
  
3.  Ajoutez la ligne suivante dans le fichier PostIC.cmd file pour exécuter ce script.  
  
    ```  
    REM Lower the execution policy  
    "%programfiles%\Windows Server\bin\WssPowershell.exe" "Set-ExecutionPolicy RemoteSigned"  
  
    REM Execute the folder customization script  
    "%programfiles%\Windows Server\bin\WssPowershell.exe" -NoProfile -Noninteractive -command ". %windir%\setup\scripts\customizefolders.ps1;exit $LASTEXITCODE"  
    Set error_level=%ERRORLEVEL%  
  
    REM Restore the execution policy to default  
    "%programfiles%\Windows Server\bin\WssPowershell.exe" "Set-ExecutionPolicy Restricted"  
    Set ERRORLEVEL=%error_level%  
    ```  
  
###  <a name="example-2-create-a-custom-folder-and-move-an-existing-folder-by-using-the-windows-server-solutions-sdk"></a><a name="BKMK_Example2"></a>Exemple 2 : créer un dossier personnalisé et déplacer un dossier existant à l’aide du kit de développement logiciel (SDK) des solutions Windows Server  
 Le code que vous créez peut être lancé comme un fichier exécutable, puis appelé depuis le fichier PostIC.cmd ou appelé directement depuis un complément installé.  
  
```  
static void Main(string[] args)  
{  
 StorageManager storageManager = new StorageManager();  
 //Connect to the StorageManager  
 storageManager.Connect();  
  
 //Move the Documents folder to D:\ServerFolders  
 Folder targetFolder = storageManager.Folders.First(folder => folder.Name == "Documents");  
  
 MoveFolderRequest moveRequest = targetFolder.GetMoveRequest(@"D:\");  
 moveRequest.MoveFolder();  
  
 //Verify operation was successful, if so print result  
 if (moveRequest.Status == OperationStatus.Succeeded)  
 {  
  Console.WriteLine("Folder {0} now located at {1}", targetFolder.Name, targetFolder.Path);  
 }  
  
 string newFolderName = "New Custom Folder";  
 string newFolderLocation = @"C:\ServerFolders\New Custom Folder";  
  
 //Create add request based with specific name and location  
 CreateFolderRequest request = storageManager.GetCreateFolderRequest(newFolderName, newFolderLocation);  
  
 //Give Guest user read only permission to folder  
 request.PermissionsByName.Add(new NamePermission("Guest", Permission.ReadOnly));  
  
 //Create the new folders  
 request.CreateFolder();  
  
 //Verify operation was successful, if so print result  
 if( request.Status == OperationStatus.Succeeded)  
 {  
  Folder newFolder = storageManager.Folders.First(folder => folder.Path == newFolderLocation);  
  
  Console.WriteLine("Folder {0} created at {1}", newFolder.Name, newFolder.Path);  
 }  
}  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience utilisateur](Testing-the-Customer-Experience.md)
