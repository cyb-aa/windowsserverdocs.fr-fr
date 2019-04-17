---
title: "Récupération de la forêt ActiveDirectory - réinitialiser le compte d’ordinateur sur le contrôleur de domaine"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 4e1a6070-df0a-4dfe-8773-899a010bfabd
ms.technology: identity-adfs
ms.openlocfilehash: 588510a27f56abb4497b2f80fa0281a858a7e8eb
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/07/2017
---
# <a name="ad-forest-recovery---resetting-the-computer-account-on-the-dc"></a>Récupération de la forêt ActiveDirectory - réinitialiser le compte d’ordinateur sur le contrôleur de domaine 

>S’applique à: Windows Server2016, Windows Server2012 et 2012R2, Windows Server2008 et 2008R2

 Utilisez la procédure suivante pour réinitialiser le mot de passe du compte ordinateur du contrôleur de domaine.  
  
## <a name="to-reset-the-computer-account-password-of-the-domain-controller"></a>Pour réinitialiser le mot de passe du compte ordinateur du contrôleur de domaine  
  
1.  À une invite de commandes, tapez la commande suivante et appuyez sur ENTRÉE:  
  
    ```  
    netdom help resetpwd  
    ```  
  
2.  Utilisez la syntaxe que cette commande fournit de l’utilisation de l’outil de ligne de commande Netdom pour réinitialiser le mot de passe de compte ordinateur, par exemple:  
  
    ```  
    netdom resetpwd /server:domain controller name /userD:administrator /passwordd:*  
    ```  
  
     Où *nom du contrôleur de domaine* est le contrôleur de domaine local que vous récupérez.  
  
    > [!NOTE]
    >  Vous devez exécuter cette commande deux fois.  
  
## <a name="next-steps"></a>Étapes suivantes

- [Guide de récupération de forêt ActiveDirectory](AD-Forest-Recovery-Guide.md)
- [Récupération de la forêt ActiveDirectory - procédures](AD-Forest-Recovery-Procedures.md)
