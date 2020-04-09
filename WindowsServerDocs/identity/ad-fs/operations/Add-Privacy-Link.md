---
ms.assetid: 1ca6f87f-7272-4767-b609-3e295ac7d32f
title: Ajouter le lien de la déclaration de confidentialité
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: cf111fbfc14665d489201f7d88419bdeffb737f9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859392"
---
# <a name="add-privacy-link"></a>Ajouter le lien de la déclaration de confidentialité 


Pour ajouter le lien de confidentialité affiché dans la page Sign\-in, utilisez l’applet de commande Windows PowerShell et la syntaxe suivantes.  

![Ajouter un lien de confidentialité](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png) 
  
 
`Set-AdfsGlobalWebContent -PrivacyLink https://fs1.contoso.com/privacy/ -PrivacyLinkText Privacy`  
 
  
> [!IMPORTANT]  
> Le paramètre `linkText` de cette applet de commande n'est nécessaire que si vous utilisez une valeur autre que la valeur par défaut (*Privacy*). L'avantage de l'utilisation de la valeur par défaut est que les pages sont localisées d'après tous les paramètres régionaux du client. une fois que le\-de signature de la page est personnalisé, la personnalisation est prioritaire. par conséquent, vous devez personnaliser pour toutes les langues que vous souhaitez prendre en charge. Tout le contenu personnalisé prend un paramètre local. Quand vous configurez un contenu localisé, vous devez d’abord le configurer avec un pays\-moins de paramètres régionaux, par exemple « en », avant de configurer le pays et la région\-paramètres régionaux spécifiques, par exemple « en\-US ».  

## <a name="additional-references"></a>Références supplémentaires 
[Personnalisation de la connexion de l’utilisateur AD FS](AD-FS-user-sign-in-customization.md)  
