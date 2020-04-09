---
ms.assetid: 309d6358-777d-496a-856d-728246c7d9a1
title: Personnaliser les noms complets et les descriptions des méthodes d’authentification
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 218643bbd5ada63b6bee2b91a7bace3f9959c0bd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80816442"
---
# <a name="customize-the-display-names-and-descriptions-for-authentication-methods"></a>Personnaliser les noms complets et les descriptions des méthodes d’authentification 


Pour personnaliser le nom complet et la description des méthodes d'authentification, utilisez l’applet de commande PowerShell `Set-AdfsAuthenticationProviderWebContent` .  Pour utiliser cette applet de commande, vous devez d'abord obtenir le nom de la méthode d'authentification que vous voulez personnaliser.  Cette opération s’effectue avec `Get-AdfsGlobalAuthenticationPolicy`.  Dans l’exemple ci-dessous, nous voyons que, sur la page Sign\-in, les éléments suivants s’affichent : « Connectez-vous à l’aide d’un certificat X. 509 ».  Nous allons simplifier cet affichage pour nos utilisateurs.  
  
![personnaliser DisplayName](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update1.PNG)  
  
Tout d'abord, nous obtenons le nom de la méthode d'authentification, puis nous modifions le texte affiché.  
  
 
    Get-AdfsGlobalAuthenticationPolicy  
      
    AdditionalAuthenticationProvider  : {}  
    DeviceAuthenticationEnabled   : False  
    PrimaryIntranetAuthenticationProvider : {FormsAuthentication, CertificateAuthentication}  
    PrimaryExtranetAuthenticationProvider : {FormsAuthentication, CertificateAuthentication}  
    WindowsIntegratedFallbackEnabled  : True  
      
    Set-AdfsAuthenticationProviderWebContent -Name CertificateAuthentication -DisplayName "Sign in with a certificate"  
  
  
![personnaliser DisplayName](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update2.PNG)  
  
Maintenant, c’est le nouveau message qui s’affiche.  
  
![personnaliser DisplayName](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update3.PNG)  

## <a name="additional-references"></a>Références supplémentaires 
[Personnalisation de la connexion de l’utilisateur AD FS](AD-FS-user-sign-in-customization.md) 
