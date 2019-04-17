---
ms.assetid: da035189-e87f-4597-9933-49bf391a8d5d
title: "Ajouter la liaison d’origine"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fb903c62e717e36099934e64e1c939a502f691a3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="add-home-link"></a>Ajouter la liaison d’origine 

>S’applique à: Windows Server2016, Windows Server2012R2

Pour ajouter la liaison d’origine qui s’affiche sur la page de connexion, utilisez l’applet de commande Windows PowerShell suivante et la syntaxe. 


![Ajouter la liaison d’origine](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png) 
  

`Set-AdfsGlobalWebContent -HomeLink https://fs1.contoso.com/home/ -HomeLinkText Home ` 
 
  
> [!IMPORTANT]  
> Le `linkText`paramètre dans cette applet de commande n’est pas obligatoire, sauf si vous utilisez une valeur autre que la valeur par défaut, qui est *accueil*. L’avantage de l’utilisation de la valeur par défaut est que les pages sont localisées pour tous les paramètres régionaux du client. Une fois la page de connexion personnalisée, la personnalisation est prioritaire; par conséquent, vous devez personnaliser pour toutes les langues que vous souhaitez prendre en charge.

## <a name="additional-references"></a>Références supplémentaires 
[ADFS Sign-in personnalisation de l’utilisateur](AD-FS-user-sign-in-customization.md)  
