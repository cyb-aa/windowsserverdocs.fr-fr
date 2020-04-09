---
ms.assetid: f7f6bac2-1100-4b00-a248-4ca3eb3cdbe9
title: Modification du logo de la société sur la page de connexion AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/08/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: d12429b22265495cb8168ce3e5993a5cf3e74a0c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859972"
---
# <a name="changing-the-company-logo-on-the-ad-fs-sign-in-page"></a>Modification du logo de la société sur la page de connexion AD FS

#### <a name="change-company-logo"></a>Modifier le logo de l'entreprise  
Pour modifier le logo de la société affichée sur la page Sign\-in, utilisez l’applet de commande PowerShell Windows PowerShell et la syntaxe suivantes.  

![modifier le logo](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  
> [!IMPORTANT]  
> Pour le logo, nous recommandons les dimensions 260 x 35 pixels, avec une résolution de 96 ppp et une taille de fichier inférieure ou égale à 10 Ko.  
  
    
    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.png"}  

  
> [!NOTE]  
> Le paramètre `TargetName` est obligatoire. Le thème par défaut publié avec AD FS est nommé *default*.  

## <a name="additional-references"></a>Références supplémentaires 
[Personnalisation de la connexion de l’utilisateur AD FS](AD-FS-user-sign-in-customization.md)  
