---
title: Création d’un DVD de récupération de serveur prenant en charge plusieurs langues
description: Décrit comment utiliser Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855000"
---
# <a name="create-a-server-recovery-dvd-for-multi-language-support"></a>Création d’un DVD de récupération de serveur prenant en charge plusieurs langues

>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_MLHeadedRecovery"></a> Créer une installation du serveur et le DVD de récupération pour un support multilingue sur serveurs gérés localement  
  
> [!NOTE]
>  Vous devez d’abord créer une image Windows multilingue comme décrit dans la [procédure pas à pas : Création de l’Image Windows multilingue](https://technet.microsoft.com/library/jj126995) avant d’ajouter le module linguistique de Windows Server Essentials dans install.wim.  
  
 La configuration s’effectue en deux phases : l’environnement de préinstallation Windows (Windows PE) et la configuration initiale. Par défaut, la page de sélection de la langue de la configuration initiale ne sera pas affichée.  
  
-   Pour une installation OEM administrée à distance ou un scénario de préinstallation OEM, vous devez ajouter une clé de registre en utilisant la commande suivante pour afficher la page de sélection de la langue de la configuration initiale.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v ShowPreinstallPages /t REG_SZ /d true /f  
    ```  
  
    > [!IMPORTANT]
    >  Lorqu’un OEM crée une image en laboratoire, il doit obligatoirement choisir l’ **anglais** pendant la phase Windows PE de l’installation.  
  
-   Dans le cas d’un kit d’option revendeur (ROK), les clients reçoivent un DVD et éventuellement du matériel. Le client doit être en mesure de sélectionner la langue pendant l'installation de Windows PE, et la page de sélection de la langue n’est plus affichée pendant la configuration initiale.  
  
 Vous pouvez choisir de livrer un seul DVD double couche contenant plusieurs langues.  
  
 Cette section explique comment ajouter un soutien linguistique à l'installation de Windows. Le principal outil de personnalisation de Windows PE 3.0 est la gestion et maintenance des images de déploiement (DISM), un outil de ligne de commande. Cette solution permet les scénarios suivants :  
  
1.  Création d'installations multilingue  
  
2.  Création d'un support distribuable  
  
### <a name="prerequisites"></a>Prérequis  
 Pour ajouter la prise en charge multilingue à l'installation de Windows, vous avez besoin des éléments suivants :  
  

-   Ordinateur de référence fournissant tous les outils et fichiers source nécessaires à la création d’une image Windows PE personnalisée. Pour plus d'informations, voir [Prepare the Technician Computer](Prepare-the-Technician-Computer.md).  

-   Ordinateur de référence fournissant tous les outils et fichiers source nécessaires à la création d’une image Windows PE personnalisée. Pour plus d'informations, voir [Prepare the Technician Computer](../install/Prepare-the-Technician-Computer.md).  

  
-   Un DVD de Windows Server Essentials.  
  
-   Un Pack Windows Server Essentials langue DVD.  
  
###  <a name="BKMK_Steps"></a> Ajout de support multilingue  
 Pour ajouter un support multilingue à l’installation de Windows vous mettre à jour le fichier Install.wim en ajoutant Windows Server 2012 et le Windows Server Essentials linguistiques.  
  
#### <a name="update-installwim"></a>Mettre à jour Install.wim  
 Dans cette étape, vous ajoutez Windows Server 2012 et les modules linguistiques de Windows Server Essentials dans Install.wim.  
  
> [!NOTE]
>  Vérifiez que vous installez les modules linguistiques pour Windows Server 2012. Cela garantit l'obtention de l'image de marque appropriée. Le Windows Server 2012 multilingue Packs d’Interface utilisateur sont disponibles sur [Microsoft.com](https://www.microsoft.com/OEM/en/installation/downloads/Pages/technical-downloads.aspx). Veuillez suivre les instructions décrites dans le [procédure pas à pas : Création d’images système Windows multilingues](https://technet.microsoft.com/library/jj126995.aspx) sur la création d’une image Windows multilingue avant d’ajouter le module linguistique Windows Server Essentials dans install.wim.  
>   
>  Modules linguistiques de Windows Server Essentials sont disponibles dans le support de modules linguistiques à \Language Packs\\< CultureName\>.  
  
> [!NOTE]
>  Pas tous les modules linguistiques ne sont pas disponibles avant la version de Windows Server 2012.  
  
###### <a name="to-add-language-packs-to-installwim"></a>Pour ajouter des modules linguistiques à Install.wim  
  
1.  Procédez comme suit pour ajouter les modules linguistiques du système d’exploitation et du produit dans Install.wim (la langue proposée dans cet exemple est le français) :  
  
    ```  
    Dism /Mount-Wim /WimFile:C:\my_distribution\sources\install.wim /index:1 /MountDir:C:\InstallMount  
    Mkdir c:\temp_Scratch  
    Dism /Image:C:\InstallMount /ScratchDir:C:\temp_Scratch /Add-Package /PackagePath:<path_to_OS_language_packs>\fr-fr\lp.cab  
    Dism /Image:C:\InstallMount /ScratchDir:C:\temp_Scratch /Add-Package /PackagePath:<OCD\Language Packs\FR-FR\lp.cab  
  
    ```  
  

2.  Ajouter des fichiers de langue spécifiques pour prendre en charge la création de Client restaurer USB de sauvegarde du lecteur flash en suivant la procédure décrite dans [générer plusieurs langues Client un support de restauration](Build-Multi-Language-Client-Restore-Media.md).  

2.  Ajouter des fichiers de langue spécifiques pour prendre en charge la création de Client restaurer USB de sauvegarde du lecteur flash en suivant la procédure décrite dans [générer plusieurs langues Client un support de restauration](../install/Build-Multi-Language-Client-Restore-Media.md).  

  
3.  Regénérez le fichier Lang.ini dans le support amovible pour tenir compte de la prise en charge linguistique supplémentaire, à l’aide de la commande `DISM /Gen-LangINI` . Exemple :  
  
    ```  
    Dism /image:C:\InstallMount /Gen-LangINI /distribution:C:\my_distribution  
  
    ```  
  
4.  Enregistrez vos modifications dans l’image à l’aide de la commande `DISM /unmount /commit` . Exemple :  
  
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

