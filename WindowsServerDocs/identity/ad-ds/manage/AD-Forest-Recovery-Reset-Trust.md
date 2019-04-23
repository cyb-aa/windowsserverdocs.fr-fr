---
title: Récupération de forêt AD - sauvegarde d’un serveur complet
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 398918dc-c8ab-41a6-a377-95681ec0b543
ms.technology: identity-adds
ms.openlocfilehash: 455c930a90cd443cf1fe62f436abca88384f79c1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865560"
---
# <a name="resetting-a-trust-password-on-one-side-of-the-trust"></a>Réinitialiser un mot de passe d’approbation d’un côté de l’approbation  

>S'applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

 Si la récupération de forêt est liée à une violation de sécurité, procédez comme suit pour réinitialiser un mot de passe d’approbation d’un côté de l’approbation. Cela inclut des approbations implicites entre les enfants et les domaines parents, ainsi que des approbations explicites entre ce domaine (le domaine d’approbation) et un autre domaine (le domaine approuvé). 
  
 Réinitialiser le mot de passe uniquement du côté de domaine approuvé de l’approbation, également appelé l’approbation entrante (côté où ce domaine appartient). Ensuite, utilisez le même mot de passe sur le côté du domaine approuvé de l’approbation, également appelé l’approbation sortante. Réinitialiser le mot de passe de l’approbation sortante lorsque vous restaurez le premier contrôleur de domaine dans chacun des autres domaines (approuvés). 
  
 Réinitialiser le mot de passe d’approbation permet de s’assurer que le contrôleur de domaine n’est pas répliqué avec des contrôleurs de domaine potentiellement incorrectes à l’extérieur de son domaine. En définissant le même mot de passe d’approbation lors de la restauration du premier contrôleur de domaine dans chaque domaine, vous vous assurer que ce contrôleur de domaine réplique avec chacun des contrôleurs de domaine restaurés. Contrôleurs de domaine suivants dans le domaine qui sont récupérées en installant AD DS vont être répliqués automatiquement ces nouveaux mots de passe pendant le processus d’installation. 
  
## <a name="to-reset-a-trust-password-on-one-side-of-the-trust"></a>Pour réinitialiser un mot de passe d’approbation d’un côté de l’approbation  
  
1. À l’invite de commandes, tapez la commande suivante, puis appuyez sur Entrée :  

   ```  
   netdom experthelp trust  
   ```  
  
2. Utilisez la syntaxe que cette commande fournit pour l’utilisation de l’outil NetDom pour réinitialiser le mot de passe d’approbation.
   Par exemple, s’il existe deux domaines dans la forêt, parents et enfants et que vous exécutez cette commande sur le DC restauré dans le domaine parent, utilisez la syntaxe de commande suivante :  

   ```  
   netdom trust parent domain name /domain:child domain name /resetOneSide /passwordT:password /userO:administrator /passwordO:*  
   ```  

   Lorsque vous exécutez cette commande dans le domaine enfant, utilisez la syntaxe de commande suivante :  

   ```  
   netdom trust child domain name /domain:parent domain name /resetOneSide /passwordT:password /userO:administrator /passwordO:*  
   ```  

   > [!NOTE]
   > **passwordT** doit avoir la même valeur des deux côtés de l’approbation. Exécutez cette commande une seule fois (contrairement à la **netdom resetpwd** commande), car il réinitialise automatiquement le mot de passe à deux reprises. 
  
## <a name="next-steps"></a>Étapes suivantes

- [Guide de la forêt Active Directory](AD-Forest-Recovery-Guide.md)
- [Récupération de forêt AD - procédures](AD-Forest-Recovery-Procedures.md)
