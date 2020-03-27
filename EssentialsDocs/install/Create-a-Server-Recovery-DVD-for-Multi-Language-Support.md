---
title: Création d’un DVD de récupération de serveur prenant en charge plusieurs langues
description: Décrit comment utiliser Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: c7da0f6c-9732-4784-9c28-7dad72c4071d
author: daveba
ms.author: daveba
ms.openlocfilehash: b71fc748f7cc8d82420b7a62fe502135036db727
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80312113"
---
# <a name="create-a-server-recovery-dvd-for-multi-language-support"></a>Création d’un DVD de récupération de serveur prenant en charge plusieurs langues

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="create-a-server-setup-and-server-recovery-dvd-for-multiple-language-support-on-locally-administered-servers"></a><a name="BKMK_MLHeadedRecovery"></a>Créer une configuration de serveur et un DVD de récupération de serveur pour la prise en charge de plusieurs langues sur des serveurs administrés localement  
  
> [!NOTE]
>  Vous devez d’abord créer une image Windows multilingue, comme décrit dans la [procédure pas à pas : création d’une image Windows multilingue](https://technet.microsoft.com/library/jj126995) avant d’ajouter Windows Server Essentials linguistique au fichier Pack dans Install. wim.  
  
 La configuration s’effectue en deux phases : l’environnement de préinstallation Windows (Windows PE) et la configuration initiale. Par défaut, la page de sélection de la langue de la configuration initiale ne sera pas affichée.  
  
- Pour une installation OEM administrée à distance ou un scénario de préinstallation OEM, vous devez ajouter une clé de registre en utilisant la commande suivante pour afficher la page de sélection de la langue de la configuration initiale.  
  
  ```  
  %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v ShowPreinstallPages /t REG_SZ /d true /f  
  ```  
  
  > [!IMPORTANT]
  >  Lorqu’un OEM crée une image en laboratoire, il doit obligatoirement choisir l’ **anglais** pendant la phase Windows PE de l’installation.  
  
- Dans le cas d’un kit d’option revendeur (ROK), les clients reçoivent un DVD et éventuellement du matériel. Le client doit être en mesure de sélectionner la langue pendant l'installation de Windows PE, et la page de sélection de la langue n’est plus affichée pendant la configuration initiale.  
  
  Vous pouvez choisir de livrer un seul DVD double couche contenant plusieurs langues.  
  
  Cette section explique comment ajouter un soutien linguistique à l'installation de Windows. Le principal outil de personnalisation de Windows PE 3.0 est la gestion et maintenance des images de déploiement (DISM), un outil de ligne de commande. Cette solution permet les scénarios suivants :  
  
1.  Création d'installations multilingue  
  
2.  Création d'un support distribuable  
  
### <a name="prerequisites"></a>Composants requis  
 Pour ajouter la prise en charge multilingue à l'installation de Windows, vous avez besoin des éléments suivants :  
  

-   Ordinateur de référence fournissant tous les outils et fichiers source nécessaires à la création d’une image Windows PE personnalisée. Pour plus d'informations, voir [Préparation de l'ordinateur de référence](Prepare-the-Technician-Computer.md).  

-   Ordinateur de référence fournissant tous les outils et fichiers source nécessaires à la création d’une image Windows PE personnalisée. Pour plus d'informations, voir [Préparation de l'ordinateur de référence](../install/Prepare-the-Technician-Computer.md).  

  
-   Un DVD Windows Server Essentials.  
  
-   Un DVD de module linguistique Windows Server Essentials.  
  
###  <a name="adding-multiple-language-support"></a><a name="BKMK_Steps"></a>Ajout de la prise en charge de plusieurs langues  
 Pour ajouter la prise en charge de plusieurs langues à installation de Windows vous mettez à jour le fichier Install. wim en y ajoutant les modules linguistiques Windows Server 2012 et Windows Server Essentials.  
  
#### <a name="update-installwim"></a>Mettre à jour Install.wim  
 Au cours de cette étape, vous allez ajouter des modules linguistiques Windows Server 2012 et Windows Server Essentials dans Install. wim.  
  
> [!NOTE]
>  Vérifiez que vous installez les modules linguistiques pour Windows Server 2012. Cela garantit l'obtention de l'image de marque appropriée. Les modules linguistiques de l’interface utilisateur multilingue de Windows Server 2012 sont disponibles sur [Microsoft.com](https://www.microsoft.com/OEM/en/installation/downloads/Pages/technical-downloads.aspx). Suivez les instructions décrites dans la [procédure pas à pas : création d’images système Windows multilingues sur](https://technet.microsoft.com/library/jj126995.aspx) la création d’une image Windows multilingue avant d’ajouter le module linguistique Windows Server Essentials dans Install. wim.  
>   
>  Les modules linguistiques Windows Server Essentials sont disponibles dans le support du module linguistique de sous \Language packs\\< CultureName\>.  
  
> [!NOTE]
>  Tous les modules linguistiques ne sont pas disponibles avant la sortie de Windows Server 2012.  
  
###### <a name="to-add-language-packs-to-installwim"></a>Pour ajouter des modules linguistiques à Install.wim  
  
1.  Procédez comme suit pour ajouter les modules linguistiques du système d’exploitation et du produit dans Install.wim (la langue proposée dans cet exemple est le français) :  
  
    ```  
    Dism /Mount-Wim /WimFile:C:\my_distribution\sources\install.wim /index:1 /MountDir:C:\InstallMount  
    Mkdir c:\temp_Scratch  
    Dism /Image:C:\InstallMount /ScratchDir:C:\temp_Scratch /Add-Package /PackagePath:<path_to_OS_language_packs>\fr-fr\lp.cab  
    Dism /Image:C:\InstallMount /ScratchDir:C:\temp_Scratch /Add-Package /PackagePath:<OCD\Language Packs\FR-FR\lp.cab  
  
    ```  
  

2.  Ajoutez des fichiers spécifiques à la langue pour prendre en charge la création du lecteur flash USB de restauration de sauvegarde du client, à l’aide de la procédure décrite dans [créer un support de restauration de client multilingue](Build-Multi-Language-Client-Restore-Media.md).  

2.  Ajoutez des fichiers spécifiques à la langue pour prendre en charge la création du lecteur flash USB de restauration de sauvegarde du client, à l’aide de la procédure décrite dans [créer un support de restauration de client multilingue](../install/Build-Multi-Language-Client-Restore-Media.md).  

  
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
 [Préparation de l’image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience utilisateur](Testing-the-Customer-Experience.md)

 [Création et personnalisation de l’Image](../install/Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](../install/Additional-Customizations.md)   
 [Préparation de l’image pour le déploiement](../install/Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience utilisateur](../install/Testing-the-Customer-Experience.md)

