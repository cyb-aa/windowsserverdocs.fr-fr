---
title: Récupération de la forêt Active Directory-réinitialisation du compte d’ordinateur sur le contrôleur de domaine
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 4e1a6070-df0a-4dfe-8773-899a010bfabd
ms.technology: identity-adds
ms.openlocfilehash: 320d02f789b8dda771b648b0aa9a58f545f3358b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71368870"
---
# <a name="ad-forest-recovery---resetting-the-computer-account-on-the-dc"></a>Récupération de la forêt Active Directory-réinitialisation du compte d’ordinateur sur le contrôleur de domaine

>S'applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

 Utilisez la procédure suivante pour réinitialiser le mot de passe du compte d’ordinateur du contrôleur de périphérique. 
  
## <a name="to-reset-the-computer-account-password-of-the-domain-controller"></a>Pour réinitialiser le mot de passe du compte d’ordinateur du contrôleur de domaine  

1. À l’invite de commandes, tapez la commande suivante, puis appuyez sur Entrée :  

   ```
   netdom help resetpwd  
   ```
  
2. Utilisez la syntaxe fournie par cette commande pour utiliser l’outil en ligne de commande Netdom afin de réinitialiser le mot de passe du compte d’ordinateur, par exemple :  

   ```
   netdom resetpwd /server:domain controller name /userD:administrator /passwordd:*  
   ```  
  
    Où *nom du contrôleur de domaine* est le contrôleur de domaine local que vous récupérez. 
  
   > [!NOTE]
   > Vous devez exécuter cette commande deux fois.
  
## <a name="next-steps"></a>Étapes suivantes

- [Guide de récupération de la forêt Active Directory](AD-Forest-Recovery-Guide.md)
- [Récupération de la forêt Active Directory : procédures](AD-Forest-Recovery-Procedures.md)
