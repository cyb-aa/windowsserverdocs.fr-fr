---
title: Récupération de la forêt Active Directory-sauvegarde d’un serveur complet
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 398918dc-c8ab-41a6-a377-95681ec0b543
ms.technology: identity-adds
ms.openlocfilehash: e9222685e8f6369e560a841990bc13ab8b0e4d37
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390256"
---
# <a name="resetting-a-trust-password-on-one-side-of-the-trust"></a>Réinitialisation d’un mot de passe d’approbation d’un côté de l’approbation  

>S'applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

 Si la récupération de la forêt est liée à une violation de la sécurité, utilisez la procédure suivante pour réinitialiser un mot de passe d’approbation d’un côté de l’approbation. Cela comprend les approbations implicites entre les domaines enfants et parents, ainsi que les approbations explicites entre ce domaine (le domaine d’approbation) et un autre domaine (domaine approuvé). 
  
 Réinitialisez le mot de passe uniquement sur le côté approbation du domaine de l’approbation, également appelé approbation entrante (le côté auquel appartient ce domaine). Ensuite, utilisez le même mot de passe sur le domaine approuvé de l’approbation, également appelé approbation sortante. Réinitialisez le mot de passe de l’approbation sortante lorsque vous restaurez le premier contrôleur de domaine dans chacun des autres domaines (approuvés). 
  
 La réinitialisation du mot de passe d’approbation garantit que le contrôleur de domaine ne réplique pas avec des contrôleurs de domaine potentiellement incorrects en dehors de son domaine En définissant le même mot de passe d’approbation lors de la restauration du premier contrôleur de domaine dans chacun des domaines, vous vous assurez que ce contrôleur de domaine est répliqué avec chacun des contrôleurs de domaine récupérés. Les contrôleurs de domaine suivants dans le domaine qui sont récupérés par l’installation de AD DS répliquent automatiquement ces nouveaux mots de passe lors du processus d’installation. 
  
## <a name="to-reset-a-trust-password-on-one-side-of-the-trust"></a>Pour réinitialiser un mot de passe d’approbation d’un côté de l’approbation  
  
1. À l’invite de commandes, tapez la commande suivante, puis appuyez sur Entrée :  

   ```  
   netdom experthelp trust  
   ```  
  
2. Utilisez la syntaxe fournie par cette commande pour utiliser l’outil NetDom afin de réinitialiser le mot de passe d’approbation.
   Par exemple, s’il existe deux domaines dans la forêt (parent et enfant) et si vous exécutez cette commande sur le contrôleur de domaine restauré dans le domaine parent, utilisez la syntaxe de commande suivante :  

   ```  
   netdom trust parent domain name /domain:child domain name /resetOneSide /passwordT:password /userO:administrator /passwordO:*  
   ```  

   Lorsque vous exécutez cette commande dans le domaine enfant, utilisez la syntaxe de commande suivante :  

   ```  
   netdom trust child domain name /domain:parent domain name /resetOneSide /passwordT:password /userO:administrator /passwordO:*  
   ```  

   > [!NOTE]
   > **passwordt** doit avoir la même valeur des deux côtés de l’approbation. Exécutez cette commande une seule fois (contrairement à la commande **NETDOM resetpwd** ), car elle réinitialise automatiquement le mot de passe à deux reprises. 
  
## <a name="next-steps"></a>Étapes suivantes

- [Guide de récupération de la forêt Active Directory](AD-Forest-Recovery-Guide.md)
- [Récupération de la forêt Active Directory : procédures](AD-Forest-Recovery-Procedures.md)
