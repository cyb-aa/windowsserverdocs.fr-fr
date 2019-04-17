---
title: "Personnalisation des dossiers partagés"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 47bc4986-14eb-4a29-9930-83a25704a3a0
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: bcdd43183512bb225dd4afa916f2782c6eb79d7e
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="customize-shared-folders"></a>Personnalisation des dossiers partagés

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Par défaut, les dossiers du serveur sont créés sur la plus grande partition de données sur le disque 0. Les partenaires peuvent personnaliser l’emplacement et spécifier les dossiers de serveur supplémentaire à l’aide de la procédure suivante:  
  
1.  À l’aide d’une configuration de partition personnalisée, créer l’image d’usine, puis créer une nouvelle clé de Registre de stockage avant d’utiliser sysprep. Lors de la Configuration initiale (IC), la tâche de stockage IC vérifie cette clé de Registre. Si elle existe, les dossiers du serveur par défaut sont créés dans le répertoire C:\ServerFolders.  
  
    #### <a name="to-create-a-new-storage-registry-key"></a>Pour créer une nouvelle clé de Registre de stockage  
  
    1.  Sur le serveur, déplacez votre souris vers le coin supérieur droit de l’écran, puis cliquez sur **recherche**.  
  
    2.  Dans la zone de recherche, tapez **regedit**, puis cliquez sur le **Regedit** application.  
  
    3.  Dans le volet de navigation, développez **HKEY_LOCAL_MACHINE**, développez **logiciel**, puis développez **Microsoft**.  
  
    4.  Avec le bouton droit **Windows Server**, cliquez sur **New**, puis cliquez sur **clé**.  
  
    5.  Nommez la clé **stockage**.  
  
    6.  Dans le volet de navigation, cliquez sur le nouveau stockage du Registre, clic sur une touche **New**, puis cliquez sur **valeur DWORD (32bits)**.  
  
    7.  La chaîne de nom **CreateFoldersOnSystem**.  
  
    8.  Avec le bouton droit **CreateFoldersOnSystem**, puis cliquez sur **modifier**. Le **modification de la chaîne** boîte de dialogue s’affiche.  
  
    9. Définissez la valeur de cette nouvelle clé **1**, puis cliquez sur **OK**.  
  
2.  Utilisez le script PostIC.cmd pour déplacer les dossiers vers un emplacement différent ou pour créer des dossiers supplémentaires. Consultez l’exemple suivant: [exemple 1: créer un dossier personnalisé et déplacer les dossiers par défaut vers un nouvel emplacement de PostIC.cmd à l’aide de Windows PowerShell](Customize-Shared-Folders.md#BKMK_Example1).  
  
3.  Utilisez le SDK WindowsServerSolutions pour déplacer les dossiers vers un emplacement différent ou pour créer des dossiers supplémentaires. Consultez l’exemple suivant: [exemple 2: créer un dossier personnalisé et déplacer un dossier existant à l’aide du SDK WindowsServerSolutions](Customize-Shared-Folders.md#BKMK_Example2).  
  
 Si vous le souhaitez, des partenaires laissent les dossiers de données sur le lecteur C. Cela permet à l’utilisateur final ou au revendeur de déterminer la structure des dossiers de données sur les lecteurs de données.  
  
###  <a name="BKMK_Example1"></a>Exemple 1: Créer un dossier personnalisé et déplacer les dossiers par défaut vers un nouvel emplacement de PostIC.cmd à l’aide de Windows PowerShell  
  
1.  Créer un fichier PostIC.cmd pour exécuter des tâches de Configuration initiales suivant comme expliqué dans les [créer le fichier PostIC.cmd pour exécuter Post Initial Configuration Tasks](Create-the-PostIC.cmd-File-for-Running-Post-Initial-Configuration-Tasks.md) section.  
  
2.  À l’aide du bloc-notes, créez un fichier nommé **customizefolders.ps1** dans le dossier C:\Windows\Setup\Scripts, puis collez le Windows PowerShell® suivantes des commandes dans le fichier (décocher les lignes appropriées selon le comportement désiré).  
  
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
  
3.  Ajoutez la ligne suivante dans le fichier PostIC.cmd pour exécuter ce script.  
  
    ```  
    REM Lower the execution policy  
    "%programfiles%\Windows Server\bin\WssPowershell.exe" "Set-ExecutionPolicy RemoteSigned"  
  
    REM Execute the folder customization script  
    "%programfiles%\Windows Server\bin\WssPowershell.exe" -NoProfile -Noninteractive -command ". %windir%\setup\scripts\customizefolders.ps1;exit $LASTEXITCODE"  
    Set error_level=%ERRORLEVEL%  
  
    REM Restore the execution policy to deafult  
    "%programfiles%\Windows Server\bin\WssPowershell.exe" "Set-ExecutionPolicy Restricted"  
    Set ERRORLEVEL=%error_level%  
    ```  
  
###  <a name="BKMK_Example2"></a>Exemple 2: Créer un dossier personnalisé et déplacer un dossier existant à l’aide du SDK WindowsServerSolutions  
 Le code que vous créez peut être lancé comme un fichier exécutable, puis appelé à partir du fichier PostIC.cmd ou appelé directement à partir d’un complément installé.  
  
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
 [Préparation de l’Image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience client](Testing-the-Customer-Experience.md)