---
ms.assetid: f7f6bac2-1100-4b00-a248-4ca3eb3cdbe9
title: "Modification du logo de société sur la page de connexion ADFS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/08/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ca54abe7fe852b22f2f4d9a717e38d219fa50694
ms.sourcegitcommit: a00fc4426dc4ad0098257f01f0124d6c733d1aef
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/09/2018
---
# <a name="changing-the-company-logo-on-the-ad-fs-sign-in-page"></a>Modification du logo de société sur la page de connexion ADFS

>S’applique à: Windows Server2016, Windows Server2012R2

#### <a name="change-company-logo"></a>Modifier le logo de société  
Pour modifier le logo de la société qui s’affiche sur la page de connexion, utilisez l’applet de commande PowerShell de Windows PowerShell suivante et la syntaxe.  

![Modifier le logo](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  
> [!IMPORTANT]  
> Nous recommandons les dimensions du logo à 260 x 350 résolution de 96PPP avec une taille de fichier n’est supérieure à 10Ko.  
  
    
    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.png"}  

  
> [!NOTE]  
> Le `TargetName`paramètre est obligatoire. Le thème par défaut fourni avec ADFS est nommé *par défaut*.  

## <a name="additional-references"></a>Références supplémentaires 
[ADFS Sign-in personnalisation de l’utilisateur](AD-FS-user-sign-in-customization.md)  
