---
ms.assetid: 1ca6f87f-7272-4767-b609-3e295ac7d32f
title: "Ajouter le lien de confidentialité"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 81a453b45693b8222bdfc0231885b506fdfcd2fc
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="add-privacy-link"></a>Ajouter le lien de confidentialité 

>S’applique à: Windows Server2016, Windows Server2012R2

Pour ajouter le lien de confidentialité qui apparaît dans la page de connexion, utilisez l’applet de commande Windows PowerShell suivante et la syntaxe.  

![Ajouter le lien de confidentialité](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png) 
  
 
`Set-AdfsGlobalWebContent -PrivacyLink https://fs1.contoso.com/privacy/ -PrivacyLinkText Privacy`  
 
  
> [!IMPORTANT]  
> Le `linkText`paramètre dans cette applet de commande n’est pas obligatoire, sauf si vous utilisez une valeur autre que la valeur par défaut, qui est *confidentialité*. L’avantage d’utiliser la valeur par défaut est que les pages sont localisées pour tous les paramètres régionaux du client. Une fois la page de connexion personnalisée, la personnalisation est prioritaire; par conséquent, vous devez personnaliser pour toutes les langues que vous souhaitez prendre en charge. Tout le contenu personnalisé prend un paramètre local. Lorsque vous configurez un contenu localisé, vous devez configurer il avec les paramètres régionaux country\ moins tout d’abord, par exemple, «en», avant de configurer les pays et les paramètres régionaux region\ spécifiques, tels que «en\-us».  

## <a name="additional-references"></a>Références supplémentaires 
[ADFS Sign-in personnalisation de l’utilisateur](AD-FS-user-sign-in-customization.md)  
