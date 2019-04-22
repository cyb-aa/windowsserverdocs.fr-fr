---
ms.assetid: c89a977c-b09f-44ec-be42-41e76a6cf3ad
title: Supprimer les informations de copyright Microsoft
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: aa27a014b0803712a20fdd23f075486dc35f33c2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820980"
---
# <a name="remove-the-microsoft-copyright"></a>Supprimer les informations de copyright Microsoft 

>S'applique à : Windows Server 2016, Windows Server 2012 R2
 
Par défaut, les pages d’AD FS contiennent les informations de copyright de Microsoft. Pour supprimer ces informations de vos pages personnalisées, vous pouvez utiliser la procédure suivante. 

![supprimer les droits d’auteur](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1.png) 
  
## <a name="to-remove-the-microsoft-copyright"></a>Pour supprimer les informations de copyright Microsoft  
  
1. Créez un thème personnalisé basé sur le thème par défaut.

   ```powershell
   New-AdfsWebTheme –Name custom –SourceName default
   ```

2. Exportez le thème en spécifiant le dossier de sortie.  

   ```powershell
   Export-AdfsWebTheme -Name custom -DirectoryPath C:\CustomWebTheme
   ```

3. Recherchez le `Style.css` fichier qui se trouve dans le dossier de sortie. À l’aide de l’exemple précédent, le chemin d’accès serait `C:\CustomWebTheme\Css\Style.css.`
  
4. Ouvrez le `Style.css` fichier avec un éditeur, tel que le bloc-notes.  
  
5. Recherchez la partie `#copyright` , puis remplacez-la par le code suivant :  

   ```css
   #copyright {color:#696969; display:none;}
   ```

6. Créer un thème personnalisé basé sur le nouveau `Style.css` fichier.  

   ```powershell
   Set-AdfsWebTheme -TargetName custom -StyleSheet @{locale="";path="C:\customWebTheme\css\style.css"}
   ```

7. Activez le nouveau thème.  

   ```powershell
   Set-AdfsWebConfig -ActiveThemeName custom
   ```

Maintenant, vous devez voir n’est plus les informations de copyright au bas de la page de connexion.

![supprimer les droits d’auteur](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1a.png) 

## <a name="additional-references"></a>Références supplémentaires 
[AD FS Sign-in personnalisation de l’utilisateur](AD-FS-user-sign-in-customization.md) 
