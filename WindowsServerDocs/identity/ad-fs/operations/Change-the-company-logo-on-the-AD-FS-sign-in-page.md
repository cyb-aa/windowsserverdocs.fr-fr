---
ms.assetid: f7f6bac2-1100-4b00-a248-4ca3eb3cdbe9
title: Modification du logo de société dans la page de connexion AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/08/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fe5c138466ea288b5dfb8c7c284603150ab9d874
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190031"
---
# <a name="changing-the-company-logo-on-the-ad-fs-sign-in-page"></a>Modification du logo de société dans la page de connexion AD FS

#### <a name="change-company-logo"></a>Modifier le logo de l'entreprise  
Pour changer le logo de la société qui s’affiche sur le signe\-dans la page, utilisez l’applet de commande PowerShell de Windows PowerShell suivante et la syntaxe.  

![modifier le logo](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  
> [!IMPORTANT]  
> Pour le logo, nous recommandons les dimensions 260 x 35 pixels, avec une résolution de 96 ppp et une taille de fichier inférieure ou égale à 10 Ko.  
  
    
    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.png"}  

  
> [!NOTE]  
> Le paramètre `TargetName` est obligatoire. Le thème par défaut qui est publié avec AD FS est nommé *par défaut*.  

## <a name="additional-references"></a>Références supplémentaires 
[AD FS Sign-in personnalisation de l’utilisateur](AD-FS-user-sign-in-customization.md)  
