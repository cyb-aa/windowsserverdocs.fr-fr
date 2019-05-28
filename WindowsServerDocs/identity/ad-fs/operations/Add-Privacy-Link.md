---
ms.assetid: 1ca6f87f-7272-4767-b609-3e295ac7d32f
title: Ajouter le lien de la déclaration de confidentialité
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3617335a179ab419982ab57343999ad4fcaf522a
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190164"
---
# <a name="add-privacy-link"></a>Ajouter le lien de la déclaration de confidentialité 


Pour ajouter le lien de confidentialité s’affiche sur le signe\-dans la page, utilisez l’applet de commande Windows PowerShell suivante et la syntaxe.  

![Ajouter un lien de confidentialité](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png) 
  
 
`Set-AdfsGlobalWebContent -PrivacyLink https://fs1.contoso.com/privacy/ -PrivacyLinkText Privacy`  
 
  
> [!IMPORTANT]  
> Le paramètre `linkText` de cette applet de commande n'est nécessaire que si vous utilisez une valeur autre que la valeur par défaut (*Privacy*). L'avantage de l'utilisation de la valeur par défaut est que les pages sont localisées d'après tous les paramètres régionaux du client. Après le signe\-dans la page est personnalisé, la personnalisation est prioritaire ; par conséquent, vous devez personnaliser pour toutes les langues que vous souhaitez prendre en charge. Tout le contenu personnalisé prend un paramètre local. Lorsque vous configurez le contenu localisé, vous devez les configurer avec un pays\-inférieure aux paramètres régionaux tout d’abord, par exemple, « en », avant de configurer les colonnes country et region\-des paramètres régionaux spécifiques, tels que « fr\-nous ».  

## <a name="additional-references"></a>Références supplémentaires 
[AD FS Sign-in personnalisation de l’utilisateur](AD-FS-user-sign-in-customization.md)  
