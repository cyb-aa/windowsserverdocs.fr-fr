---
ms.assetid: 38bbc002-a8fa-4211-9328-4ef67fca0acf
title: Personnalisation de la localisation
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ac206d5aa8af970b65a014955ac66a8cf2835eb6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="customization-for-localization"></a>Personnalisation de la localisation 

>S’applique à: Windows Server2016, Windows Server2012R2

Il est possible de localisation de contenu web dans des langues autres que l’anglais. Tenez compte des considérations suivantes lorsque vous localisez.  
  
Une fois le contenu personnalisé, la personnalisation est prioritaire; par conséquent, vous devez personnaliser pour toutes les langues que vous souhaitez prendre en charge. Tout le contenu personnalisé prend un paramètre local. Lorsque vous configurez un contenu localisé, configurez-le avec une country\ dépourvu tout d’abord, par exemple, «en», avant de configurer un pays et les paramètres régionaux spécifiques region\ tel que «en\-us».  
  
Voici quelques exemples de code supplémentaire.  
  
    
    Set-AdfsWebTheme -TargetName default -Logo @{Locale="";Path="c:\contoso.png"}  
      
    Set-AdfsWebTheme -TargetName default -Illustration @{Locale="";Path="c:\illustration.png"}  

  
Voici quelques exemples de code supplémentaire.  
  
 
    Set-AdfsGlobalWebContent -ErrorPageDescriptionText "This is Contoso's error page description" –locale "en"  
  
  

    Set-AdfsGlobalWebContent -ErrorPageDescriptionText "Il s'agit de description de page erreur de Contoso" –locale "fr"  
 
  
Si vous souhaitez personnaliser le contenu web dans des langues autres que l’anglais nécessitant la saisie de caractères Unicode, nous vous recommandons d’utiliser WindowsPowerShellISE. Pour plus d’informations, voir [présentation de WindowsPowerShellISE](https://technet.microsoft.com/library/dd315244.aspx).  

## <a name="additional-references"></a>Références supplémentaires 
[ADFS Sign-in personnalisation de l’utilisateur](AD-FS-user-sign-in-customization.md) 
