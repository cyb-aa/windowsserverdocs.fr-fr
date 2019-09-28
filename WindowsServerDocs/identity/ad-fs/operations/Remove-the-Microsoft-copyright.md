---
ms.assetid: c89a977c-b09f-44ec-be42-41e76a6cf3ad
title: Supprimer le Copyright Microsoft
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 0c24173dd03e03f9e8a19ef5981a6dc1259d62d7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407515"
---
# <a name="remove-the-microsoft-copyright"></a>Supprimer le Copyright Microsoft 


 
Par défaut, les pages de AD FS contiennent le Copyright Microsoft. Pour supprimer ces informations de vos pages personnalisées, vous pouvez utiliser la procédure suivante. 

![supprimer le Copyright](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1.png) 
  
## <a name="to-remove-the-microsoft-copyright"></a>Pour supprimer les informations de copyright Microsoft  
  
1. Créez un thème personnalisé basé sur le thème par défaut.

   ```powershell
   New-AdfsWebTheme –Name custom –SourceName default
   ```

2. Exportez le thème en spécifiant le dossier de sortie.  

   ```powershell
   Export-AdfsWebTheme -Name custom -DirectoryPath C:\CustomWebTheme
   ```

3. Localisez le fichier `Style.css` qui se trouve dans le dossier de sortie. En utilisant l’exemple précédent, le chemin d’accès serait `C:\CustomWebTheme\Css\Style.css.`
  
4. Ouvrez le fichier `Style.css` à l’aide d’un éditeur, tel que le bloc-notes.  
  
5. Recherchez la partie `#copyright` , puis remplacez-la par le code suivant :  

   ```css
   #copyright {color:#696969; display:none;}
   ```

6. Créez un thème personnalisé basé sur le nouveau fichier `Style.css`.  

   ```powershell
   Set-AdfsWebTheme -TargetName custom -StyleSheet @{locale="";path="C:\customWebTheme\css\style.css"}
   ```

7. Activez le nouveau thème.  

   ```powershell
   Set-AdfsWebConfig -ActiveThemeName custom
   ```

À présent, vous ne devriez plus voir le Copyright au bas de la page de connexion.

![supprimer le Copyright](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1a.png) 

## <a name="additional-references"></a>Références supplémentaires 
[Personnalisation de la connexion de l’utilisateur AD FS](AD-FS-user-sign-in-customization.md) 
