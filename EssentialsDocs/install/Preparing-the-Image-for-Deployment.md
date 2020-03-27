---
title: Préparation de l’image en vue du déploiement
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 681c6cad-7fde-494f-86a5-f4c7c15d23f9
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: aac776253c094c4a77269720bcc5762d6c41d720
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80311549"
---
# <a name="preparing-the-image-for-deployment"></a>Préparation de l’image en vue du déploiement

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Un outil standard pour la préparation de l’image est sysprep.exe. L’exécution de cet outil permet de généraliser l’image et d’éteindre le serveur de sorte que la configuration initiale est exécutée lorsque le serveur contenant l’image est redémarré. Toutes les modifications apportées à l'image doivent être terminées avant l'exécution de sysprep.exe.  
  
> [!NOTE]
>  Vous pouvez réinitialiser l'activation de Windows trois fois au maximum à l'aide du fichier sysprep.exe.  
  
#### <a name="to-prepare-the-image"></a>Pour préparer l'image  
  
1.  Supprimer le fichier SkipIC.txt que vous avez ajouté.  
  
2.  Ouvrez une fenêtre d’invite de commandes avec élévation de privilèges. Cliquez sur **Démarrer**, cliquez avec le bouton droit sur **Invite de commandes**, puis sélectionnez **Exécuter en tant qu’administrateur**.  
  
3.  Exécutez la commande suivante pour réinitialiser la clé de registre afin que l'utilisateur puisse bénéficier de la période de grâce complète avant que le serveur ne soit plus conforme.  
  
    ```  
    %systemroot%\system32\reg.exe add HKLM\Software\Microsoft\ServerInfrastructureLicensing /v Rearm /t REG_DWORD /d 1 /f  
    ```  
  
4.  Exécutez la commande suivante pour ajouter la clé de registre pour afficher la clé, la page de langue, la page locale et la page CLUF. Par défaut, ces pages ne s’affichent pas pendant la configuration initiale. Par conséquent, si vous lancez une boîte de dialogue pré-installée, vous devez ajouter cette clé de registre. Cependant, si vous lancez un DVD, vous devez ajouter cette clé car ces pages seront affichées pendant WinPE et la configuration initiale.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v ShowPreinstallPages /t REG_SZ /d true /f  
    ```  
  
5.  Désactivez la page de la clé de la configuration initiale si votre boîte de dialogue a une clé préconfigurée. La page de la clé ne sera affichée que si ShowPreinstallPages = true et KeyPreInstalled ! = true.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v KeyPreInstalled /t REG_SZ /d true /f  
    ```  
  
6.  Exécutez la commande suivante pour ajouter la clé de registre si vous souhaitez désactiver la vérification de la configuration matérielle requise. Cela vaut uniquement pour votre boîte de dialogue préinstallée qui ne répond pas à la configuration matérielle requise. Si vous lancez un DVD, ou si votre boîte de dialogue répond à la configuration matérielle requise, il est recommandé de ne pas ajouter cette clé.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v HWRequirementChecks /t REG_DWORD /d 0 /f  
    ```  
  
7.  (Facultatif) Supprimer les journaux sous **%programdata%\Microsoft\Windows Server\Logs**.  
  
8.  Préparez le fichier xml en mode sans assistance comme décrit dans le modèle suivant.  
  
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
    >  Vous pouvez également ajouter le fichier unattend.xml sous %systemdrive% au lieu de l'ajouter en tant que paramètre de sysprep. Si le fichier se trouve sous c:\ il sera couvert par les paramètres de l’utilisateur, mais s’il est utilisé en tant que paramètre de Sysprep, il ne sera pas couvert par les paramètres de l’utilisateur. Le fichier unattend.xml sous %systemdrive% sera supprimé à chaque redémarrage du serveur. En conséquence, assurez-vous que le serveur ne redémarre pas une fois le fichier unattend.xml créé sous %systemdrive%.  
  
10. Exécutez la commande suivante pour ajouter la clé de registre afin d'ignorer la page de la clé OOBE Windows  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\Windows\CurrentVersion\Setup\OOBE" /v SetupDisplayedProductKey /t REG_DWORD /d 1 /f  
    ```  
  
11. Exécutez la commande suivante pour ajouter la clé de registre afin d'ignorer la page de sélection de la langue Windows.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\Windows\CurrentVersion\Setup\OOBE" /v SetupDisplayedLanguageSelection /t REG_DWORD /d 1 /f  
    ```  
  
    > [!IMPORTANT]
    >  Vous devez exécuter les 2 dernières étapes, ou la page OOBE Windows OOBE s’affichera avec la page de configuration initiale et rompra le scénario du serveur administré à distance.  
  
12. Fermez la boîte de dialogue après l’exécution de sysprep, vous pouvez capturer une image ou redémarrer le serveur pour continuer la configuration initiale à partir d’un ordinateur client.  
  
> [!IMPORTANT]
>  Les partenaires envisageant de créer un support de récupération du serveur doivent capturer l'image et créer le support de récupération avant de passer à l'étape suivante.