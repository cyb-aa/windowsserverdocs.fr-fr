---
ms.assetid: 28043fc4-a34d-4710-ac3b-5c9d4d6a895c
title: Modifier le nom de la société sur la page de connexion AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1711e7d7de871c9ae9b1b7ea7b21f6e75ae15220
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868470"
---
# <a name="change-the-company-name-on-the-ad-fs-sign-in-page"></a>Modifier le nom de la société sur la page de connexion AD FS

>S'applique à : Windows Server 2016, Windows Server 2012 R2
 
Pour modifier le nom de la société qui s’affiche sur le signe\-dans la page, utilisez l’applet de commande Windows PowerShell suivante et la syntaxe. Par défaut, cette valeur est tirée du nom complet du service de fédération que vous avez entré pendant l'installation.  

![modifier le nom](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1.png)
  
  
    Set-AdfsGlobalWebContent –CompanyName "Contoso Corp"  
 
  
> [!NOTE]  
> Vous pouvez également utiliser l’environnement de script intégré Windows PowerShell \(ISE\) pour modifier le nom de l’entreprise. À l’aide de Windows PowerShell ISE, vous pouvez afficher le contenu dans Unicode\-environnement compatible. Pour plus d'informations, consultez [Présentation de Windows PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx).  

## <a name="additional-references"></a>Références supplémentaires 
[AD FS Sign-in personnalisation de l’utilisateur](AD-FS-user-sign-in-customization.md)  
  
