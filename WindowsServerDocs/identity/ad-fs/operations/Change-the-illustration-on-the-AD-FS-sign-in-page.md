---
ms.assetid: a4526500-24b3-423d-805c-24b0d8061aba
title: Modifier l’illustration sur la page de connexion AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a182b6243777d119394615008cee63b5e5a71ab8
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189952"
---
# <a name="change-the-illustration-on-the-ad-fs-sign-in-page"></a>Modifier l’illustration sur la page de connexion AD FS

## <a name="change-the-illustration"></a>Modifier l’Illustration  


Pour modifier l’illustration, le graphique sur la gauche, ce qui est affiché sur le signe\-dans la page, utilisez l’applet de commande Windows PowerShell suivante et la syntaxe.  

![modifier l’illustration](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  
> [!IMPORTANT]  
> Pour l'illustration, nous recommandons les dimensions 1420 x 1080 pixels, avec une résolution de 96 ppp et une taille de fichier inférieure ou égale à 200 Ko.  
  
 
    Set-AdfsWebTheme -TargetName default -Illustration @{path="c:\Contoso\illustration.png"}  

## <a name="additional-references"></a>Références supplémentaires 
[AD FS Sign-in personnalisation de l’utilisateur](AD-FS-user-sign-in-customization.md)  
  
  
