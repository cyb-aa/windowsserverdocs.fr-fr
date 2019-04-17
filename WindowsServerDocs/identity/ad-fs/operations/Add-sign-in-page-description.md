---
ms.assetid: 330c7b61-dde0-432f-9b74-d250ad9cc808
title: Ajouter la description de la page de connexion
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 128a4ffd8d4b9dfcfe5f0e8e2df8a0e1505cbb24
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="add-sign-in-page-description"></a>Ajouter la description de la page de connexion

>S’applique à: Windows Server2016, Windows Server2012R2

## <a name="to-add-sign-in-page-description"></a>Pour ajouter la description de la page de connexion  
Pour ajouter une description de la page de connexion à la page de connexion, utilisez l’applet de commande PowerShell de Windows PowerShell suivante et la syntaxe.  

![Ajouter le journal dans la description](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>" 
 
  
> [!IMPORTANT]  
> La chaîne pour le `SignInPageDescriptionText`paramètre prend en charge le code HTML pur avec les balises et sans. Par conséquent, vous pouvez également exécuter l’applet de commande suivante sans utiliser le &lt;p&gt; balise.  `Set-AdfsGlobalWebContent -SignInPageDescriptionText "Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information." ` 

Une fois la page de connexion personnalisée, la personnalisation est prioritaire; par conséquent, vous devez personnaliser pour toutes les langues que vous souhaitez prendre en charge. Tout le contenu personnalisé prend un paramètre local. Lorsque vous configurez un contenu localisé, il doit être configuré avec les paramètres régionaux country\ moins tout d’abord, par exemple, «en», avant de configurer les pays et paramètres régionaux spécifiques region\ tel que «en\-us».  

## <a name="additional-references"></a>Références supplémentaires 
[ADFS Sign-in personnalisation de l’utilisateur](AD-FS-user-sign-in-customization.md)  
