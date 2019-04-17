---
ms.assetid: 28043fc4-a34d-4710-ac3b-5c9d4d6a895c
title: "Modifier le nom de la société sur la page de connexion ADFS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8e99f6fed8922ed15bf78a98b207b6f46767763a
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="change-the-company-name-on-the-ad-fs-sign-in-page"></a>Modifier le nom de la société sur la page de connexion ADFS

>S’applique à: Windows Server2016, Windows Server2012R2
 
Pour modifier le nom de la société qui s’affiche sur la page de connexion, utilisez l’applet de commande PowerShell de Windows PowerShell suivante et la syntaxe. Cette valeur est définie par défaut à l’aide de la valeur du nom complet de Service de fédération que vous avez entré pendant l’installation.  

![Modifier le nom](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1.png)
  
  
    Set-AdfsGlobalWebContent –CompanyName "Contoso Corp"  
 
  
> [!NOTE]  
> Vous pouvez également utiliser l’environnement d’écriture de scripts intégré Windows PowerShell \(ISE\) pour modifier le nom de l’entreprise. À l’aide de WindowsPowerShellISE, vous pouvez afficher du contenu dans un environnement de syntaxe et compatible Unicode. Pour plus d’informations, voir [présentation de WindowsPowerShellISE](https://technet.microsoft.com/library/dd315244.aspx).  

## <a name="additional-references"></a>Références supplémentaires 
[ADFS Sign-in personnalisation de l’utilisateur](AD-FS-user-sign-in-customization.md)  
  
