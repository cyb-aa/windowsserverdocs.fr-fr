---
ms.assetid: 330c7b61-dde0-432f-9b74-d250ad9cc808
title: Ajouter une inscription\-dans la description de la page
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 553cdcf2ac98b23d06cc43a64220cf8433117fc8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814010"
---
# <a name="add-sign-in-page-description"></a>Ajouter une inscription\-dans la description de la page

>S'applique à : Windows Server 2016, Windows Server 2012 R2

## <a name="to-add-sign-in-page-description"></a>Pour ajouter l’authentification\-dans la description de la page  
Pour ajouter un panneau\-dans la description de la page vers la connexion\-dans la page, utilisez l’applet de commande Windows PowerShell suivante et la syntaxe.  

![Ajouter une inscription dans la description](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>" 
 
  
> [!IMPORTANT]  
> La chaîne du paramètre `SignInPageDescriptionText` prend en charge le code HTML pur, avec et sans balises. Par conséquent, vous pouvez également exécuter l’applet de commande suivante sans utiliser le &lt;p&gt; balise.  `Set-AdfsGlobalWebContent -SignInPageDescriptionText "Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information." ` 

Après le signe\-dans la page est personnalisé, la personnalisation est prioritaire ; par conséquent, vous devez personnaliser pour toutes les langues que vous souhaitez prendre en charge. Tout le contenu personnalisé prend un paramètre local. Lorsque vous configurez le contenu localisé, il doit être configuré avec un pays\-inférieure aux paramètres régionaux tout d’abord, par exemple, « en », avant de configurer les colonnes country et region\-des paramètres régionaux spécifiques tels que « fr\-nous ».  

## <a name="additional-references"></a>Références supplémentaires 
[AD FS Sign-in personnalisation de l’utilisateur](AD-FS-user-sign-in-customization.md)  
