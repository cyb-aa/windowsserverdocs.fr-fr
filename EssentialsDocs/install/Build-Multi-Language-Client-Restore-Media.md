---
title: "Création d’un support de restauration du Client multilingue"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2fdbc016-d464-43cb-bd75-8a63e61588a2
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 1ad934d297c3092050bd6adbb6bb0f50d1ec6f36
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="build-multi-language-client-restore-media"></a>Création d’un support de restauration du Client multilingue

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

> [!NOTE]
>  Vous devez d’abord créer une image Windows multilingue comme décrit dans le [procédure pas à pas: création d’images système Windows](https://technet.microsoft.com/library/jj126995) avant d’ajouter le pack de langue de WindowsServerEssentials dans install.wim.  
  
 Lors de la création du DVD d’installation serveur multilingues, les modules linguistiques seront installés pour le serveur install.wim. Les ressources localisées pour l’Assistant Restauration seront installés dans le cadre du pack de langue.  
  
### <a name="to-build-a-multi-language-client-restore-media"></a>Pour créer un client multilingue support de restauration  
  
1.  Montez install.wim sur c:\mount, nous appelons le dossier c:\mount\Program Files\Windows Server\bin\ClientRestore en tant que racine du support de restauration du client: [RestoreMediaRoot] ci-dessous:  
  
    ```  
    dism /mount-wim /wimfile:install.wim /index:1 /mountdir:c:\mount  
    ```  
  
2.  Montez le fichier WIM de restauration de client à [RestoreMediaRoot]\Sources\Boot.wim (mêmes étapes doivent être exécutées également pour boot_x86.wim). La ligne de commande est:  
  
    ```  
    dism /mount-wim /wimfile:boot.wim /index:1 /mountdir:c:\mountRestore  
    ```  
  
3.  Ajoutez le package WinPE-Setup.cab au support de restauration, en exécutant:  
  
    ```  
    dism /image:c:\mountRestore /add-package /packagepath:WinPE-Setup.cab  
    ```  
  
4.  Utilisez le bloc-notes pour modifier c:\mountRestore\windows\system32\winpeshl.ini, remplir avec le contenu suivant:  
  
    ```  
    [LaunchApp]  
    AppPath = %SYSTEMDRIVE%\sources\SelectLanguage.exe  
    ```  
  
5.  Ajouter des modules linguistiques au support de restauration. Ajout de modules linguistiques peut être effectuée en exécutant la commande suivante:  
  
    ```  
    dism /image:c:\mountRestore /add-package /packagepath:[language pack path]  
    ```  
  
     Les modules linguistiques suivants doivent être ajoutés:  
  
    1.  WinPE language pack (lp.cab)  
  
    2.  Module linguistique WinPE-Setup (WinPE-Setup_ [langue] .cab, par exemple, WinPE-Setup_en-us.cab)  
  
    3.  Pour les polices asiatiques, notamment zh-cn, zh-tw, zh-hk, ko-kr, ja-jp, un pack de police supplémentaire doit être installé (winpe - fontsupport-[langue] .cab, par exemple, winpe-fontsupport-zh-cn.cab)  
  
6.  Générer un nouveau fichier Lang.ini en exécutant:  
  
    ```  
    dism /image:c:\mountRestore /Gen-LangINI /distribution:mount  
    ```  
  
7.  Les commandes Commit et unmount pour l’image en exécutant:  
  
    ```  
    dism /unmount-wim /mountdir:c:\mountRestore /commit  
    ```  
  
8.  Répétez l’étape2 à l’étape7 pour [RestoreMediaroot]\Sources\Boot_x86.wim.  
  
9. Les commandes Commit et unmount pour l’image en exécutant:  
  
    ```  
    dism /unmount-wim /mountdir:c:\mount /commit  
    ```  
  
## <a name="see-also"></a>Voir aussi  

 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’Image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience client](Testing-the-Customer-Experience.md)

 [Création et personnalisation de l’Image](../install/Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](../install/Additional-Customizations.md)   
 [Préparation de l’Image pour le déploiement](../install/Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience client](../install/Testing-the-Customer-Experience.md)

