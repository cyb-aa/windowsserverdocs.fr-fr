---
ms.assetid: da035189-e87f-4597-9933-49bf391a8d5d
title: Ajouter le lien de la page d'accueil
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a9a043390f5bfb412e549779ed4a9048d1c8a0b5
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190182"
---
# <a name="add-home-link"></a>Ajouter le lien de la page d'accueil 

Pour ajouter la liaison d’origine qui s’affiche sur le signe\-dans la page, utilisez l’applet de commande Windows PowerShell suivante et la syntaxe. 


![ajouter la liaison d’origine](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png) 
  

`Set-AdfsGlobalWebContent -HomeLink https://fs1.contoso.com/home/ -HomeLinkText Home ` 
 
  
> [!IMPORTANT]  
> Le paramètre `linkText` de cette applet de commande n'est nécessaire que si vous utilisez une valeur autre que la valeur par défaut (*Home*). L'avantage de l'utilisation de la valeur par défaut est que les pages sont localisées d'après tous les paramètres régionaux du client. Après le signe\-dans la page est personnalisé, la personnalisation est prioritaire ; par conséquent, vous devez personnaliser pour toutes les langues que vous souhaitez prendre en charge.

## <a name="additional-references"></a>Références supplémentaires 
[AD FS Sign-in personnalisation de l’utilisateur](AD-FS-user-sign-in-customization.md)  
