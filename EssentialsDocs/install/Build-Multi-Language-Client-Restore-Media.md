---
title: Créer un support de restauration du client multilingue
description: Décrit comment utiliser Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879870"
---
# <a name="build-multi-language-client-restore-media"></a>Créer un support de restauration du client multilingue

>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

> [!NOTE]
>  Vous devez d’abord créer une image Windows multilingue comme décrit dans la [procédure pas à pas : Création de l’Image Windows multilingue](https://technet.microsoft.com/library/jj126995) avant d’ajouter le module linguistique de Windows Server Essentials dans install.wim.  
  
 Lors de la création du DVD multilingue d’installation du serveur, les modules linguistiques seront installés pour le fichier install.wim du serveur. Les ressources localisées pour l’assistant de restauration seront installées en tant que partie du module linguistique.  
  
### <a name="to-build-a-multi-language-client-restore-media"></a>Pour créer un support de restauration du client multilingue  
  
1.  Monter install.wim sur c:\mount. Nous appelons le dossier c:\mount\Program Files\Windows Server\bin\ClientRestore la racine du support de restauration du client : [Racine du support de restauration] ci-dessous :  
  
    ```  
    dism /mount-wim /wimfile:install.wim /index:1 /mountdir:c:\mount  
    ```  
  
2.  Montez le fichier WIM de restauration du client [Racine du support de restauration]\Sources\Boot.wim (les mêmes étapes doivent être exécutées également pour boot_x86.wim). La ligne de commande est la suivante :  
  
    ```  
    dism /mount-wim /wimfile:boot.wim /index:1 /mountdir:c:\mountRestore  
    ```  
  
3.  Ajoutez le pack WinPE-Setup.cab au support de restauration en exécutant :  
  
    ```  
    dism /image:c:\mountRestore /add-package /packagepath:WinPE-Setup.cab  
    ```  
  
4.  Utilisez le Bloc-notes pour éditer le fichier c:\mountRestore\windows\system32\winpeshl.ini en le remplissant avec le contenu suivant :  
  
    ```  
    [LaunchApp]  
    AppPath = %SYSTEMDRIVE%\sources\SelectLanguage.exe  
    ```  
  
5.  Ajoutez des modules linguistiques au support de restauration. L’ajout de modules linguistiques peut être effectué en exécutant la commande suivante :  
  
    ```  
    dism /image:c:\mountRestore /add-package /packagepath:[language pack path]  
    ```  
  
     Les modules linguistiques suivants sont à ajouter :  
  
    1.  Module linguistique WinPE (lp.cab)  
  
    2.  Le fichier d’installation du module linguistique WinPE- (WinPE-Setup_[langue].cab, par exemple, WinPE-Setup_fr-fr.cab)  
  
    3.  Pour les polices asiatiques, notamment zh-cn, zh-tw, zh-hk, ko-kr, ja-jp, un pack de police supplémentaire doit être installé (winpe-fontsupport-[langue].cab, par exemple winpe-fontsupport-zh-cn.cab)  
  
6.  Générez un nouveau fichier Lang.ini en exécutant :  
  
    ```  
    dism /image:c:\mountRestore /Gen-LangINI /distribution:mount  
    ```  
  
7.  Les commandes Commit et Unmount pour l’image en exécutant :  
  
    ```  
    dism /unmount-wim /mountdir:c:\mountRestore /commit  
    ```  
  
8.  Répétez les étapes 2 à 7 pour [Racine du support de restauration]\Sources\Boot_x86.wim.  
  
9. Les commandes Commit et Unmount pour l’image en exécutant :  
  
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

