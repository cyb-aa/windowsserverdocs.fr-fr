---
ms.assetid: 330c7b61-dde0-432f-9b74-d250ad9cc808
title: Ajouter une\-de connexion dans la description de la page
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 3b34a4e54aebd5b9dc3655eecd770a25f7ea97cf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407746"
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
