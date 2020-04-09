---
ms.assetid: 28043fc4-a34d-4710-ac3b-5c9d4d6a895c
title: Modifier le nom de la société dans la page de connexion AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 378979825c0e8e505c3996cf8cbcdb471a0c7d79
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859962"
---
# <a name="change-the-company-name-on-the-ad-fs-sign-in-page"></a>Modifier le nom de la société dans la page de connexion AD FS
 
Pour modifier le nom de la société affichée sur la page Sign\-in, utilisez l’applet de commande Windows PowerShell et la syntaxe suivantes. Par défaut, cette valeur est tirée du nom complet du service de fédération que vous avez entré pendant l'installation.  

![modifier le nom](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1.png)
  
  
    Set-AdfsGlobalWebContent –CompanyName "Contoso Corp"  
 
  
> [!NOTE]  
> Vous pouvez également utiliser la\) Environnement d’écriture de scripts intégré de Windows PowerShell \(ISE pour modifier le nom de la société. À l’aide de l’Windows PowerShell ISE, vous pouvez afficher le contenu dans un environnement compatible Unicode\-. Pour plus d'informations, consultez [Présentation de Windows PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx).  

## <a name="additional-references"></a>Références supplémentaires 
[Personnalisation de la connexion de l’utilisateur AD FS](AD-FS-user-sign-in-customization.md)  
  
