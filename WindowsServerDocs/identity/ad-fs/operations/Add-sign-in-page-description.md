---
ms.assetid: 330c7b61-dde0-432f-9b74-d250ad9cc808
title: Ajouter une\-de connexion dans la description de la page
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 8d3cc69bde1c9126f97926802b53d049ed1ef501
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859992"
---
# <a name="add-sign-in-page-description"></a>Ajouter une\-de connexion dans la description de la page


## <a name="to-add-sign-in-page-description"></a>Pour ajouter une\-de signe dans la description de la page  
Pour ajouter une\-de connexion dans la description de la page à la page signer\-dans, utilisez l’applet de commande Windows PowerShell et la syntaxe suivantes.  

![Ajouter une description de connexion](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>" 
 
  
> [!IMPORTANT]  
> La chaîne du paramètre `SignInPageDescriptionText` prend en charge le code HTML pur, avec et sans balises. Par conséquent, vous pouvez également exécuter l’applet de commande suivante sans utiliser la balise &lt;p&gt;.  `Set-AdfsGlobalWebContent -SignInPageDescriptionText "Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information." ` 

une fois que le\-de signature de la page est personnalisé, la personnalisation est prioritaire. par conséquent, vous devez personnaliser pour toutes les langues que vous souhaitez prendre en charge. Tout le contenu personnalisé prend un paramètre local. Quand vous configurez un contenu localisé, il doit être configuré avec un pays\-moins de paramètres régionaux, par exemple, « en », avant de configurer le pays et la région\-paramètres régionaux spécifiques tels que « en\-US ».  

## <a name="additional-references"></a>Références supplémentaires 
[Personnalisation de la connexion de l’utilisateur AD FS](AD-FS-user-sign-in-customization.md)  
