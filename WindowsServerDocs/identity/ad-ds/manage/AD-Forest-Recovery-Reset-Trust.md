---
title: "Récupération de la forêt ActiveDirectory - sauvegarde complète du serveur."
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 398918dc-c8ab-41a6-a377-95681ec0b543
ms.technology: identity-adfs
ms.openlocfilehash: 51b53e7348ae00ed2fc63711b9c3297279ebe033
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/07/2017
---
# <a name="resetting-a-trust-password-on-one-side-of-the-trust"></a>Réinitialiser un mot de passe d’approbation d’un côté de l’approbation  

>S’applique à: Windows Server2016, Windows Server2012 et 2012R2, Windows Server2008 et 2008R2

 Si la récupération de forêt est liée à une violation de la sécurité, utilisez la procédure suivante pour réinitialiser un mot de passe d’approbation d’un côté de l’approbation. Cela inclut les approbations implicites entre les enfants et les domaines parents, ainsi que les approbations explicites entre ce domaine (le domaine d’approbation) et un autre domaine (le domaine approuvé).  
  
 Réinitialiser le mot de passe uniquement la partie domaine autorisé à approuver de l’approbation, également appelé l’approbation entrante (du côté où ce domaine appartient). Ensuite, utilisez le même mot de passe sur le côté du domaine approuvé de l’approbation, également connu sous le nom de l’approbation sortante. Réinitialiser le mot de passe de l’approbation sortante lorsque vous restaurez le premier contrôleur de domaine dans chacun des autres domaines (approuvés).  
  
 Réinitialiser le mot de passe d’approbation permet de s’assurer que le contrôleur de domaine n’est pas répliqué avec potentiellement des contrôleurs de domaine en dehors de son domaine. En définissant le même mot de passe d’approbation lors de la restauration du premier contrôleur de domaine dans chaque domaine, vous assurez que ce contrôleur de domaine réplique avec chacun des contrôleurs de domaine restaurés. Contrôleurs de domaine suivants dans le domaine qui sont récupérés en installant ADDS répliquera automatiquement ces nouveaux mots de passe pendant le processus d’installation.  
  
## <a name="to-reset-a-trust-password-on-one-side-of-the-trust"></a>Pour réinitialiser un mot de passe d’approbation d’un côté de l’approbation  
  
1.  À une invite de commandes, tapez la commande suivante et appuyez sur ENTRÉE:  
  
    ```  
    netdom experthelp trust  
    ```  
  
2.  Utilisez la syntaxe que cette commande fournit pour l’utilisation de l’outil NetDom pour réinitialiser le mot de passe d’approbation.  
  
     Par exemple, s’il existe deux domaines dans la forêt: parent et enfant et que vous exécutez cette commande sur le contrôleur de domaine restauré dans le domaine parent, utilisez la syntaxe de commande suivante:  
  
    ```  
    netdom trust parent domain name /domain:child domain name /resetOneSide /passwordT:password /userO:administrator /passwordO:*  
    ```  
  
     Lorsque vous exécutez cette commande dans le domaine enfant, utilisez la syntaxe de commande suivante:  
  
    ```  
    netdom trust child domain name /domain:parent domain name /resetOneSide /password:password /userO:administrator /passwordO:*  
    ```  
  
    > [!NOTE]
    >  **passwordT** doit être identique sur les deux côtés de l’approbation. Exécutez cette commande une seule fois (contrairement à la **netdom resetpwd** commande) dans la mesure où il réinitialise automatiquement le mot de passe deux fois.  
  
## <a name="next-steps"></a>Étapes suivantes

- [Guide de récupération de forêt ActiveDirectory](AD-Forest-Recovery-Guide.md)
- [Récupération de la forêt ActiveDirectory - procédures](AD-Forest-Recovery-Procedures.md)
