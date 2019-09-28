---
ms.assetid: 38bbc002-a8fa-4211-9328-4ef67fca0acf
title: Personnalisation pour la localisation
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: ba3441d362960e054791ca3d6872dba6c7bd9a12
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357981"
---
# <a name="customization-for-localization"></a>Personnalisation pour la localisation 


La localisation de contenu web dans des langues autres que l'anglais est possible. Gardez à l'esprit les considérations suivantes quand vous localisez du contenu.  
  
Une fois le contenu personnalisé, vous devez étendre la personnalisation à toutes les langues à prendre en charge. Tout le contenu personnalisé prend un paramètre local. Quand vous configurez un contenu localisé, configurez-le avec les paramètres régionaux Country @ no__t-0less First, par exemple, « en », avant de configurer un pays et une région @ no__t-1specific locale, par exemple « en @ no__t-2us ».  
  
Voici quelques exemples de code supplémentaires.  
  
    
    Set-AdfsWebTheme -TargetName default -Logo @{Locale="";Path="c:\contoso.png"}  
      
    Set-AdfsWebTheme -TargetName default -Illustration @{Locale="";Path="c:\illustration.png"}  

  
Voici quelques exemples de code supplémentaires.  
  
 
    Set-AdfsGlobalWebContent -ErrorPageDescriptionText "This is Contoso's error page description" –locale "en"  
  
  

    Set-AdfsGlobalWebContent -ErrorPageDescriptionText "Il s'agit de description de page erreur de Contoso" –locale "fr"  
 
  
Si vous souhaitez personnaliser le contenu Web dans des langues autres que l’anglais qui requièrent l’entrée Unicode, nous vous recommandons d’utiliser la Windows PowerShell ISE. Pour plus d'informations, consultez [Présentation de Windows PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx).  

## <a name="additional-references"></a>Références supplémentaires 
[Personnalisation de la connexion de l’utilisateur AD FS](AD-FS-user-sign-in-customization.md) 
