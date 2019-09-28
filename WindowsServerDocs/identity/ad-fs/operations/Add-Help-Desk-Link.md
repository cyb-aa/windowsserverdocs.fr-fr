---
ms.assetid: 2bac7744-9de3-491a-b0a2-4e843cec7344
title: Ajouter le lien du support technique
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 1673e6ee6357a9d59e8ac5891625d453bb434088
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358462"
---
# <a name="add-help-desk-link"></a>Ajouter le lien du support technique 


## <a name="to-add-a-help-desk-link"></a>Pour ajouter un lien au support technique  
Pour ajouter le lien du support technique affiché sur la page Sign @ no__t-0in, utilisez l’applet de commande Windows PowerShell et la syntaxe suivantes.  

![Ajouter le support technique](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  

`Set-AdfsGlobalWebContent -HelpDeskLink https://fs1.contoso.com/help/ -HelpDeskLinkText Help`  
 
  
> [!IMPORTANT]  
> Le paramètre `linkText` de cette applet de commande n'est nécessaire que si vous utilisez une valeur autre que la valeur par défaut ( *Help*). L'avantage de l'utilisation de la valeur par défaut est que les pages sont localisées d'après tous les paramètres régionaux du client. Une fois la page personnalisée, vous devez étendre la personnalisation à toutes les langues à prendre en charge.  


## <a name="additional-references"></a>Références supplémentaires 
[Personnalisation de la connexion de l’utilisateur AD FS](AD-FS-user-sign-in-customization.md)  
