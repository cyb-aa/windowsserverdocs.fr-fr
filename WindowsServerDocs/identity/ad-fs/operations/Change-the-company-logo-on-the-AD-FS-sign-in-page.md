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
ms.openlocfilehash: 6e0271ae3d7ac120e510a2fb81fb55c8d10b3c87
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813580"
---
# <a name="changing-the-company-logo-on-the-ad-fs-sign-in-page"></a>Modification du logo de société dans la page de connexion AD FS

>S'applique à : Windows Server 2016, Windows Server 2012 R2

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
