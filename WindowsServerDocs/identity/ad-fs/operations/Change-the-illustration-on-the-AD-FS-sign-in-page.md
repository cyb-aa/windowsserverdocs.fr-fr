---
ms.assetid: a4526500-24b3-423d-805c-24b0d8061aba
title: "Modifier l’illustration sur la page de connexion ADFS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ac5e60aaad864248b58a3908e7aa9622165fbc14
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="change-the-illustration-on-the-ad-fs-sign-in-page"></a>Modifier l’illustration sur la page de connexion ADFS

>S’applique à: Windows Server2016, Windows Server2012R2

## <a name="change-the-illustration"></a>Modifier l’Illustration  


Pour modifier l’illustration, le graphique sur la gauche, qui s’affiche sur la page de connexion, utilisez l’applet de commande PowerShell de Windows PowerShell suivante et la syntaxe.  

![Modifier l’illustration](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  
> [!IMPORTANT]  
> Nous recommandons les dimensions 1420 x 1080pixels 96PPP avec une taille de fichier n’est supérieure à 200Ko l’illustration.  
  
 
    Set-AdfsWebTheme -TargetName default -Illustration @{path="c:\Contoso\illustration.png"}  

## <a name="additional-references"></a>Références supplémentaires 
[ADFS Sign-in personnalisation de l’utilisateur](AD-FS-user-sign-in-customization.md)  
  
  
