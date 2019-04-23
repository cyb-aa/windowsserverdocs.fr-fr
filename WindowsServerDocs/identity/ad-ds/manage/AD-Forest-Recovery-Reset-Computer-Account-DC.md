---
title: Récupération de forêt AD - réinitialisation du compte d’ordinateur sur le contrôleur de domaine
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 4e1a6070-df0a-4dfe-8773-899a010bfabd
ms.technology: identity-adds
ms.openlocfilehash: 388b460196888d4ca0cd12218972197afb6d49c5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852760"
---
# <a name="ad-forest-recovery---resetting-the-computer-account-on-the-dc"></a>Récupération de forêt AD - réinitialisation du compte d’ordinateur sur le contrôleur de domaine

>S'applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

 Utilisez la procédure suivante pour réinitialiser le mot de passe du compte ordinateur du contrôleur de domaine. 
  
## <a name="to-reset-the-computer-account-password-of-the-domain-controller"></a>Pour réinitialiser le mot de passe du compte ordinateur du contrôleur de domaine  

1. À l’invite de commandes, tapez la commande suivante, puis appuyez sur Entrée :  

   ```
   netdom help resetpwd  
   ```
  
2. Utilisez la syntaxe que cette commande fournit pour l’utilisation de l’outil de ligne de commande Netdom pour réinitialiser le mot de passe de compte ordinateur, par exemple :  

   ```
   netdom resetpwd /server:domain controller name /userD:administrator /passwordd:*  
   ```  
  
    Où *nom du contrôleur de domaine* est le contrôleur de domaine local que vous récupérez. 
  
   > [!NOTE]
   > Vous devez exécuter cette commande à deux reprises.
  
## <a name="next-steps"></a>Étapes suivantes

- [Guide de la forêt Active Directory](AD-Forest-Recovery-Guide.md)
- [Récupération de forêt AD - procédures](AD-Forest-Recovery-Procedures.md)
