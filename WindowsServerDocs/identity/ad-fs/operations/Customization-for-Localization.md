---
ms.assetid: 38bbc002-a8fa-4211-9328-4ef67fca0acf
title: Personnalisation pour la localisation
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ac206d5aa8af970b65a014955ac66a8cf2835eb6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882030"
---
# <a name="customization-for-localization"></a>Personnalisation pour la localisation 

>S'applique à : Windows Server 2016, Windows Server 2012 R2

La localisation de contenu web dans des langues autres que l'anglais est possible. Gardez à l'esprit les considérations suivantes quand vous localisez du contenu.  
  
Une fois le contenu personnalisé, vous devez étendre la personnalisation à toutes les langues à prendre en charge. Tout le contenu personnalisé prend un paramètre local. Lorsque vous configurez le contenu localisé, configurez-le avec un pays\-inférieure aux paramètres régionaux tout d’abord, par exemple, « en », avant de configurer un pays et une région\-des paramètres régionaux spécifiques tels que « fr\-nous ».  
  
Voici quelques exemples de code supplémentaires.  
  
    
    Set-AdfsWebTheme -TargetName default -Logo @{Locale="";Path="c:\contoso.png"}  
      
    Set-AdfsWebTheme -TargetName default -Illustration @{Locale="";Path="c:\illustration.png"}  

  
Voici quelques exemples de code supplémentaires.  
  
 
    Set-AdfsGlobalWebContent -ErrorPageDescriptionText "This is Contoso's error page description" –locale "en"  
  
  

    Set-AdfsGlobalWebContent -ErrorPageDescriptionText "Il s'agit de description de page erreur de Contoso" –locale "fr"  
 
  
Si vous souhaitez personnaliser le contenu web dans des langues autres que l’anglais nécessitant la saisie de caractères Unicode, nous vous recommandons d’utiliser Windows PowerShell ISE. Pour plus d'informations, consultez [Présentation de Windows PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx).  

## <a name="additional-references"></a>Références supplémentaires 
[AD FS Sign-in personnalisation de l’utilisateur](AD-FS-user-sign-in-customization.md) 
