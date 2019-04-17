---
ms.assetid: 2bac7744-9de3-491a-b0a2-4e843cec7344
title: Ajouter le lien du support technique
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6d16cc0a75bfe636c29b44687b669e87f31b69ce
ms.sourcegitcommit: 76e57a5453d6ee9a04dcff6a8cca087132cb1d5f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/20/2018
---
# <a name="add-help-desk-link"></a>Ajouter le lien du support technique 

>S’applique à: Windows Server2016, Windows Server2012R2


## <a name="to-add-a-help-desk-link"></a>Pour ajouter un lien de support technique  
Pour ajouter le lien du support technique qui apparaît dans la page de connexion, utilisez l’applet de commande PowerShell de Windows PowerShell suivante et la syntaxe.  

![Ajouter le support technique](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  

`Set-AdfsGlobalWebContent -HelpDeskLink https://fs1.contoso.com/help/ -HelpDeskLinkText Help`  
 
  
> [!IMPORTANT]  
> Le `linkText`paramètre dans cette applet de commande n’est pas obligatoire, sauf si vous utilisez une valeur autre que la valeur par défaut, qui est *aide*. L’avantage de l’utilisation de la valeur par défaut est que les pages sont localisées pour tous les paramètres régionaux du client. Une fois la page personnalisée, la personnalisation est prioritaire; par conséquent, vous devez personnaliser pour toutes les langues que vous souhaitez prendre en charge.  


## <a name="additional-references"></a>Références supplémentaires 
[ADFS Sign-in personnalisation de l’utilisateur](AD-FS-user-sign-in-customization.md)  
