---
ms.assetid: 309d6358-777d-496a-856d-728246c7d9a1
title: "Personnaliser les noms complets et les descriptions des méthodes d’authentification"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 699622a8a075dd6c78ab1b536dce2abfee642e9e
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="customize-the-display-names-and-descriptions-for-authentication-methods"></a>Personnaliser les noms complets et les descriptions des méthodes d’authentification 

>S’applique à: Windows Server2016, Windows Server2012R2

Pour personnaliser les noms complets et les descriptions des méthodes d’authentification que vous pouvez utiliser le `Set-AdfsAuthenticationProviderWebContent`applet de commande PowerShell.  Pour pouvoir utiliser cette applet de commande, vous devez d’abord obtenir le nom de la méthode d’authentification que vous souhaitez personnaliser.  Cela est possible en utilisant `Get-AdfsGlobalAuthenticationPolicy`.  Dans l’exemple ci-dessous, nous voyons que, dans notre page de connexion, les éléments suivants sont affiche: «se connecter à l’aide d’un certificat X.509».  Nous allons simplifier cet affichage pour nos utilisateurs.  
  
![Personnaliser displayname](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update1.PNG)  
  
Tout d’abord nous obtenons le nom de la méthode d’authentification, puis nous modifions le texte affiché.  
  
 
    Get-AdfsGlobalAuthenticationPolicy  
      
    AdditionalAuthenticationProvider  : {}  
    DeviceAuthenticationEnabled   : False  
    PrimaryIntranetAuthenticationProvider : {FormsAuthentication, CertificateAuthentication}  
    PrimaryExtranetAuthenticationProvider : {FormsAuthentication, CertificateAuthentication}  
    WindowsIntegratedFallbackEnabled  : True  
      
    Set-AdfsAuthenticationProviderWebContent -Name CertificateAuthentication -DisplayName "Sign in with a certificate"  
  
  
![Personnaliser displayname](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update2.PNG)  
  
Maintenant, nous voyons que notre afficher un message a été modifiée.  
  
![Personnaliser displayname](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update3.PNG)  

## <a name="additional-references"></a>Références supplémentaires 
[ADFS Sign-in personnalisation de l’utilisateur](AD-FS-user-sign-in-customization.md) 
