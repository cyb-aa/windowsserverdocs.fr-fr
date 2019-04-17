---
ms.assetid: 1df78c2a-5054-4b54-8310-c48ea62e6e0b
title: "Messages d’erreur personnalisés pour la page de connexion ADFS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/09/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b73caad520abae1fcfdbd5d04b88c0608df12cca
ms.sourcegitcommit: 877a50cd8d6e727048cdfac9b614a98ac3220876
ms.translationtype: HT
ms.contentlocale: fr-FR
---
# <a name="custom-error-messages-for-ad-fs-sign-in-page"></a>Messages d’erreur personnalisés pour la page de connexion ADFS  

>S’applique à: Windows Server2016, Windows Server2012R2

Vous pouvez configurer des messages d’erreur personnalisés adaptés à votre organisation. L’illustration suivante montre une description de page d’erreur personnalisée et un message d’erreur générique. Utilisez les applets de commande Windows PowerShell suivantes pour personnaliser vos messages d’erreur.  
  
![Erreur personnalisés](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom3.png)  
  
## <a name="customize-the-error-page-description"></a>Personnaliser la description de page d’erreur  
Pour personnaliser la description de page d’erreur, utilisez l’applet de commande Windows PowerShell suivante et la syntaxe.  
  

`Set-AdfsGlobalWebContent -ErrorPageDescriptionText "This is Contoso's error page description" ` 

  
## <a name="customize-a-generic-error-message"></a>Personnaliser un message d’erreur générique  
Pour personnaliser le message d’erreur générique, utilisez l’applet de commande Windows PowerShell suivante et la syntaxe.  
  
 
`Set-AdfsGlobalWebContent -ErrorPageGenericErrorMessage "This is a generic error message.  Contact Contoso IT for assistance." ` 

  
## <a name="customize-an-authorization-error-message"></a>Personnaliser un message d’erreur d’autorisation  
Pour personnaliser le message d’erreur d’autorisation, utilisez l’applet de commande Windows PowerShell suivante et la syntaxe.  
  

    Set-AdfsGlobalWebContent -ErrorPageAuthorizationErrorMessage "You have received an Authorization error.  Contact Contoso IT for assistance."  

  
## <a name="customize-a-device-authentication-error-message"></a>Personnaliser un message d’erreur d’authentification périphérique  
Pour personnaliser le message d’erreur d’authentification appareil, utilisez l’applet de commande Windows PowerShell suivante et la syntaxe.  
  
 
`Set-AdfsGlobalWebContent -ErrorPageDeviceAuthenticationErrorMessage "Your device is not authorized.  Contact Contoso IT for assistance."`  
 
  
## <a name="customize-a-support-email-error-message"></a>Personnaliser un message d’erreur de prise en charge par courrier électronique  
Vous pouvez configurer une adresse de messagerie prise en charge dans ADFS. Si configuré, ADFS affiche automatiquement un lien pour les utilisateurs finaux envoyer par courrier électronique les détails de l’erreur.  
  
Pour personnaliser le message d’erreur de prise en charge par courrier électronique, utilisez l’applet de commande Windows PowerShell suivante et la syntaxe.  
  

    Set-AdfsGlobalWebContent -ErrorPageSupportEmail  "admin@contoso.com"  

  
## <a name="customize-a-relying-party-authorization-message"></a>Personnaliser un message d’autorisation de partie de confiance  
Vous pouvez configurer un message erreur d’autorisation partie de confiance dans ADFS.  
  
Pour personnaliser le message d’erreur partie de confiance, utilisez l’applet de commande Windows PowerShell suivante et la syntaxe.  

    Set-AdfsRelyingPartyWebContent -Name fedpassive -ErrorPageAuthorizationErrorMessage "<p> You need to be a member of Security Auditors to access this site. Click <A href='http://accessrequest/'>here</A> for more information.</p>“  


## <a name="additional-references"></a>Références supplémentaires 
[ADFS Sign-in personnalisation de l’utilisateur](AD-FS-user-sign-in-customization.md)    
