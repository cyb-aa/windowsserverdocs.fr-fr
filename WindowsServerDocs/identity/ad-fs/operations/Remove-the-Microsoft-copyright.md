---
ms.assetid: c89a977c-b09f-44ec-be42-41e76a6cf3ad
title: Supprimer les informations de copyright Microsoft
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c2e6f9445e53a5b5867a763d58ad4a6ca3600cbe
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="remove-the-microsoft-copyright"></a>Supprimer les informations de copyright Microsoft 

>S’applique à: Windows Server2016, Windows Server2012R2
 
Par défaut, les pages d’ADFS contiennent des informations de copyright Microsoft. Pour supprimer ces informations à partir de vos pages personnalisées, vous pouvez utiliser la procédure suivante. 

![Supprimer les droits d’auteur](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1.png) 
  
## <a name="to-remove-the-microsoft-copyright"></a>Pour supprimer les informations de copyright Microsoft  
  
1.  Créer un thème personnalisé basé sur la valeur par défaut.  
  

    `New-AdfsWebTheme –Name custom –SourceName default ` 
 
  
2.  Exporter le thème en spécifiant le dossier de sortie.  

    `Export-AdfsWebTheme -Name custom -DirectoryPath C:\customWebTheme ` 

  
3.  Recherchez le fichier Style.css qui se trouve dans le dossier de sortie. À l’aide de l’exemple précédent, le chemin d’accès serait C:\\CustomWebTheme\\Css\\Style.css.  
  
4.  Ouvrez le fichier Style.css avec un éditeur, tel que le bloc-notes.  
  
5.  Recherchez le `#copyright` partie, puis remplacez-la par les éléments suivants:  
  `#copyright {color:#696969; display:none;} ` 
 
6.  Créer un thème personnalisé basé sur le nouveau fichier Style.css.  
  
    `Set-AdfsWebTheme -TargetName custom -StyleSheet @{locale="";path="C:\customWebTheme\css\style.css"}  `

7.  Activez le nouveau thème.  
  

    `Set-AdfsWebConfig -ActiveThemeName custom ` 


Maintenant, vous devez voir n’est plus les informations de copyright au bas de la page de connexion.

![Supprimer les droits d’auteur](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1a.png) 

## <a name="additional-references"></a>Références supplémentaires 
[ADFS Sign-in personnalisation de l’utilisateur](AD-FS-user-sign-in-customization.md) 
