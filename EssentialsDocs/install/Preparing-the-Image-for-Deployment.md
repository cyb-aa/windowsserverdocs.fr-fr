---
title: "Préparation de l’Image pour le déploiement"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 681c6cad-7fde-494f-86a5-f4c7c15d23f9
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 16411ab073e9417c52592aa9a6b13707dd461537
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="preparing-the-image-for-deployment"></a>Préparation de l’Image pour le déploiement

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Un outil standard pour la préparation d’une image est sysprep.exe. Exécution de cet outil permet de généraliser l’image et arrête le serveur afin que la Configuration initiale s’exécute lorsque le serveur qui contient l’image est redémarré. Toutes les modifications apportées à l’image doivent être terminées avant d’exécuter sysprep.exe.  
  
> [!NOTE]
>  Vous pouvez réinitialiser l’Activation de produit Windows un maximum de trois fois à l’aide de sysprep.exe.  
  
#### <a name="to-prepare-the-image"></a>Pour préparer l’image  
  
1.  Supprimer SkipIC.txt que vous avez ajouté?  
  
2.  Ouvrez une fenêtre d’invite de commandes avec élévation de privilèges. Cliquez sur **Démarrer**, cliquez avec le bouton droit **invite de commandes**, puis sélectionnez **exécuter en tant qu’administrateur**.  
  
3.  Exécutez la commande suivante pour réinitialiser la clé de Registre afin que l’utilisateur aura la période de grâce complète avant que le serveur devient non conforme.  
  
    ```  
    %systemroot%\system32\reg.exe add HKLM\Software\Microsoft\ServerInfrastructureLicensing /v Rearm /t REG_DWORD /d 1 /f  
    ```  
  
4.  Exécutez la commande suivante pour ajouter la clé de Registre pour afficher la clé, page de la langue, page de paramètres régionaux et page CLUF. Par défaut ces pages ne s’afficheront pas lors de la configuration initiale. Par conséquent, si vous lancez une boîte de dialogue pré-installée, vous devez ajouter cette clé de Registre. Toutefois, si vous lancez un DVD, vous devez ajouter pas cette clé, car ces pages seront affichées pendant WinPE et la configuration initiale.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v ShowPreinstallPages /t REG_SZ /d true /f  
    ```  
  
5.  Désactiver la page configuration initiale de la clé si votre boîte de dialogue est une clé préconfigurée. Quand il n’affichera que la page de la clé ShowPreinstallPages = true et KeyPreInstalled! = true.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v KeyPreInstalled /t REG_SZ /d true /f  
    ```  
  
6.  Exécutez la commande suivante pour ajouter la clé de Registre si vous souhaitez désactiver les vérifications de configuration requise du matériel. Il s’agit uniquement pour votre boîte de dialogue préinstallée qui ne satisfait pas la configuration matérielle requise. Si vous lancez un DVD, ou, si votre boîte de dialogue répond à la configuration matérielle requise, il est recommandé de ne pas ajouter cette clé.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v HWRequirementChecks /t REG_DWORD /d 0 /f  
    ```  
  
7.  (Facultatif) Supprimer les journaux sous **%programdata%\Microsoft\Windows Server\Logs**.  
  
8.  Préparer le fichier xml sans assistance comme décrit dans le modèle suivant.  
  
    ```  
    <unattend xmlns="urn:schemas-microsoft-com:unattend" xmlns:ms="urn:schemas-microsoft-com:asm.v3">  
  
      <settings pass="oobeSystem">  
        <component name="Microsoft-Windows-Shell-Setup" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS" xmlns:wcm="https://schemas.microsoft.com/WMIConfig/2002/State" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
  
          <!-- Must have -->  
          <OOBE>  
             <HideEULAPage>true</HideEULAPage>  
          </OOBE>  
          <!-- Must have -->  
          <AutoLogon>   
            <Enabled>true</Enabled>   
            <Username>Administrator</Username>   
            <Domain>.</Domain>   
            <Password>   
              <!--You can set any password you like, but keep it consistent with password settings -->       
              <Value>Admin@123</Value>   
              <PlainText>true</PlainText>   
            </Password>   
          </AutoLogon>   
          <UserAccounts>   
           <AdministratorPassword>   
             <!--You can set any password you like, but keep it consistent with auto logon settings -->       
             <Value>Admin@123</Value>   
             <PlainText>true</PlainText>   
           </AdministratorPassword>   
          </UserAccounts>  
  
          <!-- Optional -->  
          <OEMInformation>  
             <HelpCustomized>true</HelpCustomized>  
             <Manufacturer>OEM name</Manufacturer>  
             <Model>model name</Model>  
             <SupportHours>hours</SupportHours>  
             <SupportPhone>123-456-7890</SupportPhone>  
             <SupportURL>http://www.contoso.com</SupportURL>  
          </OEMInformation>  
  
        </component>  
  
      </settings>  
  
      <settings pass="specialize">  
        <component name="Microsoft-Windows-Shell-Setup" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS" processorArchitecture="amd64">  
          <!-- Must have -->  
          <ComputerName>Server</ComputerName>          
          <!-- Optional -->  
          <ProductKey>XXXXX-XXXXX-XXXXX-XXXXX-XXXXX</ProductKey>  
        </component>  
      </settings>  
    </unattend>  
    ```  
  
9. Exécutez la commande suivante pour sysprep.  
  
    ```  
    %systemroot%\system32\sysprep\sysprep.exe /generalize /OOBE /unattend:xxx.xml /Quit  
    ```  
  
    > [!IMPORTANT]
    >  Vous pouvez également ajouter unattend.xml sous %systemdrive% au lieu d’en tant que paramètre de sysprep. Si le fichier se trouve sous c:\, qu'il sera couvert par les paramètres utilisateur, mais si utilisé en tant que paramètre de sysprep, il ne sera pas couvert par les paramètres utilisateur. Chaque fois que le serveur redémarre, unattend.xml sous %systemdrive% sera supprimé. Par conséquent, assurez-vous que le serveur n’est pas redémarré après avoir créé unattend.xml sous %SystemDrive%.  
  
10. Exécutez la commande suivante pour ajouter la clé de Registre afin d’ignorer la page de la clé OOBE Windows.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\Windows\CurrentVersion\Setup\OOBE" /v SetupDisplayedProductKey /t REG_DWORD /d 1 /f  
    ```  
  
11. Exécutez la commande suivante pour ajouter la clé de Registre afin d’ignorer la page Sélection de la langue Windows.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\Windows\CurrentVersion\Setup\OOBE" /v SetupDisplayedLanguageSelection /t REG_DWORD /d 1 /f  
    ```  
  
    > [!IMPORTANT]
    >  Vous devez effectuer les 2dernières étapes, faute de quoi la page OOBE Windows viendront s’affichera avec le scénario de serveur administré à distance page et d’arrêt de la configuration initiale.  
  
12. Arrêter la zone après sysprep, vous pouvez capturer une image ou redémarrer le serveur pour continuer la Configuration initiale à partir d’un ordinateur client.  
  
> [!IMPORTANT]
>  Les partenaires envisageant de créer le support de récupération du serveur doivent capturer l’image et créer le support de récupération avant de passer à l’étape suivante.