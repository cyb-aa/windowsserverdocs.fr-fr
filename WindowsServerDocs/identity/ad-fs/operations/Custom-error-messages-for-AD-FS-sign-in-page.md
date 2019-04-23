---
ms.assetid: 1df78c2a-5054-4b54-8310-c48ea62e6e0b
title: Messages d’erreur personnalisés pour la page de connexion AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3a015c27f784d5b1f488f984450612820d4a06b1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828980"
---
# <a name="custom-error-messages-for-ad-fs-sign-in-page"></a>Messages d’erreur personnalisés pour la page de connexion AD FS  

>S'applique à : Windows Server 2016, Windows Server 2012 R2

Vous pouvez configurer des messages d'erreur personnalisés adaptés à votre organisation. L'illustration suivante montre la description d'une page d'erreur personnalisée et un message d'erreur générique. Utilisez les applets de commande Windows PowerShell suivante pour personnaliser vos messages d’erreur.  
  
![erreur personnalisée](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom3.png)  
  
## <a name="customize-the-error-page-description"></a>Personnaliser la description de la page d'erreur  
Pour personnaliser la description de page d’erreur, utilisez l’applet de commande Windows PowerShell suivante et la syntaxe.  
  

`Set-AdfsGlobalWebContent -ErrorPageDescriptionText "This is Contoso's error page description" ` 

  
## <a name="customize-a-generic-error-message"></a>Personnaliser un message d'erreur générique  
Pour personnaliser le message d’erreur générique, utilisez l’applet de commande Windows PowerShell suivante et la syntaxe.  
  
 
`Set-AdfsGlobalWebContent -ErrorPageGenericErrorMessage "This is a generic error message.  Contact Contoso IT for assistance." ` 

  
## <a name="customize-an-authorization-error-message"></a>Personnaliser un message d'erreur d'autorisation  
Pour personnaliser le message d’erreur d’autorisation, utilisez l’applet de commande Windows PowerShell suivante et la syntaxe.  
  

    Set-AdfsGlobalWebContent -ErrorPageAuthorizationErrorMessage "You have received an Authorization error.  Contact Contoso IT for assistance."  

  
## <a name="customize-a-device-authentication-error-message"></a>Personnaliser un message d'erreur d'authentification d'appareil  
Pour personnaliser le message d’erreur de dispositif d’authentification, utilisez l’applet de commande Windows PowerShell suivante et la syntaxe.  
  
 
`Set-AdfsGlobalWebContent -ErrorPageDeviceAuthenticationErrorMessage "Your device is not authorized.  Contact Contoso IT for assistance."`  
 
  
## <a name="customize-a-support-email-error-message"></a>Personnaliser un message d'erreur comportant l'adresse e-mail du support technique  
Vous pouvez configurer une adresse de messagerie prise en charge dans AD FS. Si configuré, AD FS affiche automatiquement un lien permettant aux utilisateurs finaux par e-mail les détails de l’erreur.  
  
Pour personnaliser le message d’erreur de messagerie prise en charge, utilisez l’applet de commande Windows PowerShell suivante et la syntaxe.  
  

    Set-AdfsGlobalWebContent -ErrorPageSupportEmail  "admin@contoso.com"  

  
## <a name="customize-a-relying-party-authorization-message"></a>Personnaliser un message d'autorisation de partie de confiance  
Vous pouvez configurer un message erreur d’autorisation partie de confiance dans AD FS.  
  
Pour personnaliser le message d’erreur de partie de confiance, utilisez l’applet de commande Windows PowerShell suivante et la syntaxe.  

    Set-AdfsRelyingPartyWebContent -Name fedpassive -ErrorPageAuthorizationErrorMessage "<p> You need to be a member of Security Auditors to access this site. Click <A href='http://accessrequest/'>here</A> for more information.</p>“  


## <a name="additional-references"></a>Références supplémentaires 
[AD FS Sign-in personnalisation de l’utilisateur](AD-FS-user-sign-in-customization.md)    
