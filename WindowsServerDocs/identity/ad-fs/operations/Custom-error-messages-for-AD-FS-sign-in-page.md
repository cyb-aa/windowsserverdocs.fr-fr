---
ms.assetid: 1df78c2a-5054-4b54-8310-c48ea62e6e0b
title: Messages d’erreur personnalisés pour AD FS page de connexion
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 31da3e65e69910817a78ab1007e897fb5a9ad683
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80816432"
---
# <a name="custom-error-messages-for-ad-fs-sign-in-page"></a>Messages d’erreur personnalisés pour AD FS page de connexion  


Vous pouvez configurer des messages d'erreur personnalisés adaptés à votre organisation. L'illustration suivante montre la description d'une page d'erreur personnalisée et un message d'erreur générique. Utilisez les applets de commande Windows PowerShell suivantes pour personnaliser vos messages d’erreur.  
  
![erreur personnalisée](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom3.png)  
  
## <a name="customize-the-error-page-description"></a>Personnaliser la description de la page d'erreur  
Pour personnaliser la description de la page d’erreur, utilisez l’applet de commande Windows PowerShell et la syntaxe suivantes.  
  

`Set-AdfsGlobalWebContent -ErrorPageDescriptionText "This is Contoso's error page description" ` 

  
## <a name="customize-a-generic-error-message"></a>Personnaliser un message d'erreur générique  
Pour personnaliser le message d’erreur générique, utilisez l’applet de commande Windows PowerShell et la syntaxe suivantes.  
  
 
`Set-AdfsGlobalWebContent -ErrorPageGenericErrorMessage "This is a generic error message.  Contact Contoso IT for assistance." ` 

  
## <a name="customize-an-authorization-error-message"></a>Personnaliser un message d'erreur d'autorisation  
Pour personnaliser le message d’erreur d’autorisation, utilisez l’applet de commande Windows PowerShell et la syntaxe suivantes.  
  

    Set-AdfsGlobalWebContent -ErrorPageAuthorizationErrorMessage "You have received an Authorization error.  Contact Contoso IT for assistance."  

  
## <a name="customize-a-device-authentication-error-message"></a>Personnaliser un message d'erreur d'authentification d'appareil  
Pour personnaliser le message d’erreur d’authentification de l’appareil, utilisez l’applet de commande Windows PowerShell et la syntaxe suivantes.  
  
 
`Set-AdfsGlobalWebContent -ErrorPageDeviceAuthenticationErrorMessage "Your device is not authorized.  Contact Contoso IT for assistance."`  
 
  
## <a name="customize-a-support-email-error-message"></a>Personnaliser un message d'erreur comportant l'adresse e-mail du support technique  
Vous pouvez configurer une adresse de messagerie de support technique dans AD FS. S’il est configuré, AD FS affiche automatiquement un lien permettant aux utilisateurs finaux d’envoyer par courrier électronique les détails de l’erreur.  
  
Pour personnaliser le message d’erreur de support électronique, utilisez l’applet de commande Windows PowerShell et la syntaxe suivantes.  
  

    Set-AdfsGlobalWebContent -ErrorPageSupportEmail  "admin@contoso.com"  

  
## <a name="customize-a-relying-party-authorization-message"></a>Personnaliser un message d'autorisation de partie de confiance  
Vous pouvez configurer un message d’erreur d’autorisation de partie de confiance dans AD FS.  
  
Pour personnaliser le message d’erreur de la partie de confiance, utilisez l’applet de commande Windows PowerShell et la syntaxe suivantes.  

    Set-AdfsRelyingPartyWebContent -Name fedpassive -ErrorPageAuthorizationErrorMessage "<p> You need to be a member of Security Auditors to access this site. Click <A href='http://accessrequest/'>here</A> for more information.</p>"  


## <a name="additional-references"></a>Références supplémentaires 
[Personnalisation de la connexion de l’utilisateur AD FS](AD-FS-user-sign-in-customization.md)    
