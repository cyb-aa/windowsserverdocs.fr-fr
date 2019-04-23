---
ms.assetid: 2bac7744-9de3-491a-b0a2-4e843cec7344
title: Ajouter le lien du support technique
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1654add6a81169b3d4831d6ebba320402e0734c5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849860"
---
# <a name="add-help-desk-link"></a>Ajouter le lien du support technique 

>S'applique à : Windows Server 2016, Windows Server 2012 R2


## <a name="to-add-a-help-desk-link"></a>Pour ajouter un lien de support technique  
Pour ajouter le lien du support technique qui s’affiche sur le signe\-dans la page, utilisez l’applet de commande Windows PowerShell suivante et la syntaxe.  

![ajouter le support technique](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  

`Set-AdfsGlobalWebContent -HelpDeskLink https://fs1.contoso.com/help/ -HelpDeskLinkText Help`  
 
  
> [!IMPORTANT]  
> Le paramètre `linkText` de cette applet de commande n'est nécessaire que si vous utilisez une valeur autre que la valeur par défaut ( *Help*). L'avantage de l'utilisation de la valeur par défaut est que les pages sont localisées d'après tous les paramètres régionaux du client. Une fois la page personnalisée, vous devez étendre la personnalisation à toutes les langues à prendre en charge.  


## <a name="additional-references"></a>Références supplémentaires 
[AD FS Sign-in personnalisation de l’utilisateur](AD-FS-user-sign-in-customization.md)  
