---
title: "Créer un DVD de récupération pour la prise en charge plusieurs langue"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c7da0f6c-9732-4784-9c28-7dad72c4071d
4author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: ac547f97b48e4cd0ebf87e0935cadc2c539b4d0b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-server-recovery-dvd-for-multi-language-support"></a>Créer un DVD de récupération pour la prise en charge plusieurs langue

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

##  <a name="BKMK_MLHeadedRecovery"></a>Créer un programme d’installation du serveur et d’un DVD de récupération pour la prise en charge de plusieurs sur les serveurs gérés localement  
  
> [!NOTE]
>  Vous devez d’abord créer une image Windows multilingue comme décrit dans le [procédure pas à pas: création d’images système Windows](https://technet.microsoft.com/library/jj126995) avant d’ajouter le pack de langue de WindowsServerEssentials dans install.wim.  
  
 Le programme d’installation s’effectue en deux phases: l’environnement de préinstallation Windows (WindowsPE) et la configuration initiale. Par défaut, la page de sélection de langue dans la configuration initiale ne sera pas être affichée.  
  
-   Pour une installation OEM administrée à distance ou un scénario de préinstallation OEM, vous devez ajouter un Registre clé utilisant la commande suivante pour afficher la page de sélection de langue dans la configuration initiale.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v ShowPreinstallPages /t REG_SZ /d true /f  
    ```  
  
    > [!IMPORTANT]
    >  Les fabricants OEM crée une image en laboratoire, il doit obligatoirement choisir **anglais** comme langue pendant la phase WindowsPE de l’installation.  
  
-   Pour un scénario de revendeur Option Kit (qui ont eu de lieu), les clients reçoivent un DVD et éventuellement du matériel. Le client doit être en mesure de sélectionner la langue pendant l’installation de WindowsPE, et la page de sélection de langue n’est plus affichée lors de la configuration initiale.  
  
 Vous pouvez choisir de livrer un seul DVD double couche contenant plusieurs langues.  
  
 Cette section décrit comment ajouter un soutien linguistique à l’installation de Windows. Le principal outil de personnalisation de WindowsPE3.0 est la maintenance des images de déploiement et de gestion (DISM), un outil de ligne de commande. Cette solution permet les scénarios suivants:  
  
1.  Création d’installations multilingue  
  
2.  Création d’un support distribuable  
  
### <a name="prerequisites"></a>Conditions préalables  
 Pour ajouter la prise en charge multilingue à l’installation de Windows, vous devez les éléments suivants:  
  

-   Un ordinateur de référence qui fournit tous les outils et les fichiers sources nécessaires pour créer une image WindowsPE personnalisée. Pour plus d’informations, voir [préparer l’ordinateur de référence](Prepare-the-Technician-Computer.md).  

-   Un ordinateur de référence qui fournit tous les outils et les fichiers sources nécessaires pour créer une image WindowsPE personnalisée. Pour plus d’informations, voir [préparer l’ordinateur de référence](../install/Prepare-the-Technician-Computer.md).  

  
-   Un DVD de WindowsServerEssentials.  
  
-   Un WindowsServerEssentialsDVD module linguistique.  
  
###  <a name="BKMK_Steps"></a>Ajout de prise en charge de plusieurs langues  
 Pour ajouter la prise en charge de plusieurs au programme d’installation de Windows vous mettez à jour Install.wim en ajoutant le serveur Windows Server2012 et WindowsServerEssentials linguistiques lui.  
  
#### <a name="update-installwim"></a>Mise à jour Install.wim  
 Dans cette étape, vous ajoutez Windows Server2012 et les modules linguistiques WindowsServerEssentials dans Install.wim.  
  
> [!NOTE]
>  Vérifiez que vous installez des modules linguistiques pour Windows Server2012. Cela garantit que vous obtenez la marque appropriée. Le Windows Server2012 multilingue Packs d’Interface utilisateur sont disponibles sur [Microsoft.com](https://www.microsoft.com/OEM/en/installation/downloads/Pages/technical-downloads.aspx). Suivez les instructions, comme décrit dans le [procédure pas à pas: création d’images système Windows multilingues](https://technet.microsoft.com/library/jj126995.aspx) sur la création d’une image Windows multilingue avant d’ajouter le module linguistique WindowsServerEssentials dans Install.wim.  
>   
>  Modules linguistiques WindowsServerEssentials sont disponibles dans le support de modules linguistiques à \Language Packs\\ < cultureName\ >.  
  
> [!NOTE]
>  Pas de tous les packs de langue ne sont pas disponible avant la version de Windows Server2012.  
  
###### <a name="to-add-language-packs-to-installwim"></a>Pour ajouter des modules linguistiques à Install.wim  
  
1.  Ajouter des packs de langue système d’exploitation et de produit dans Install.wim en procédant comme suit (cet exemple utilise le Français):  
  
    ```  
    Dism /Mount-Wim /WimFile:C:\my_distribution\sources\install.wim /index:1 /MountDir:C:\InstallMount  
    Mkdir c:\temp_Scratch  
    Dism /Image:C:\InstallMount /ScratchDir:C:\temp_Scratch /Add-Package /PackagePath:<path_to_OS_language_packs>\fr-fr\lp.cab  
    Dism /Image:C:\InstallMount /ScratchDir:C:\temp_Scratch /Add-Package /PackagePath:<OCD\Language Packs\FR-FR\lp.cab  
  
    ```  
  

2.  Ajouter des fichiers spécifiques de langue pour prendre en charge la création de USB de restauration de sauvegarde de Client d’un lecteur à l’aide de la procédure décrite dans flash [générer plusieurs langues Client un support de restauration ](Build-Multi-Language-Client-Restore-Media.md).  

2.  Ajouter des fichiers spécifiques de langue pour prendre en charge la création de USB de restauration de sauvegarde de Client d’un lecteur à l’aide de la procédure décrite dans flash [générer plusieurs langues Client un support de restauration ](../install/Build-Multi-Language-Client-Restore-Media.md).  

  
3.  Recréez le fichier Lang.ini dans le support amovible afin de refléter la prise en charge linguistique supplémentaire à l’aide de la `DISM /Gen-LangINI`de commande, par exemple:  
  
    ```  
    Dism /image:C:\InstallMount /Gen-LangINI /distribution:C:\my_distribution  
  
    ```  
  
4.  Enregistrez vos modifications dans l’image à l’aide de la `DISM /unmount /commit`de commande, par exemple:  
  
    ```  
    Dism /Unmount-Wim /MountDir:C:\InstallMount /Commit  
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

