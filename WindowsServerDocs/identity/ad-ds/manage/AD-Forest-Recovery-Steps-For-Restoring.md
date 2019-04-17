---
title: "Récupération de la forêt ActiveDirectory - étapes pour la restauration de la forêt"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: 29988c262897649173e039cc052bb64f38a1a0ca
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/07/2017
---
#<a name="ad-forest-recovery---steps-for-restoring-the-forest"></a>Récupération de la forêt ActiveDirectory - étapes pour la restauration de la forêt 

>S’applique à: Windows Server2016, Windows Server2012 et 2012R2, Windows Server2008 et 2008R2

Cette section fournit une vue d’ensemble de la procédure recommandée pour la récupération d’une forêt. La procédure de récupération de forêt est décrites en détail ultérieurement.  
  
 La liste suivante récapitule les étapes de récupération à un niveau élevé:  
  
1.  [Identifier le problème](AD-Forest-Recovery-Identify-the-Problem.md)  
  
     Travailler avec informatique et le Support Microsoft pour déterminer l’étendue du problème et causes possibles et évaluer les solutions possibles avec toutes les parties prenantes métier. Dans de nombreux cas récupération de forêt total doit être le dernier recours.  
  
2.  [Décider comment récupérer la forêt](AD-Forest-Recovery-Determine-how-to-Recover.md)  
  
     Après avoir déterminé que la récupération de la forêt est nécessaire, version préliminaire terminé les étapes pour préparer: déterminer la structure de la forêt actuelle, d’identifier les fonctions que chaque contrôleur de domaine effectue, décider quel contrôleur de domaine pour restaurer pour chaque domaine et vous assurer que tous les contrôleurs de domaine accessible en écriture sont mis hors connexion.  
  
3.  [Effectuer une récupération initiale](AD-Forest-Recovery-Perform-initial-recovery.md)  
  
     Dans l’isolation, récupérer un contrôleur de domaine pour chaque domaine, les nettoyer et reconnecter les domaines. Réinitialiser les comptes privilégiés, rechercher et corriger les problèmes causés par des violations de sécurité de cette phase.  
  
4.  [Redéployez le reste des contrôleurs de domaine](AD-Forest-Recovery-Restore-Additional-DCs.md)  
  
     Redéployez la forêt pour retourner à son état avant la défaillance. Cette étape devra être adaptées à votre conception spécifique et la configuration requise. Clonage de contrôleur de domaine virtualisé peut aider à accélérer ce processus.  
  
5.  [Nettoyage](AD-Forest-Recovery-Cleanup.md)  
  
     Une fois que la fonctionnalité a été restaurée, reconfigurez la résolution de noms en fonction des besoins et obtenir des applications cœur de métier fonctionne.  

  
 Les étapes décrites dans ce guide sont conçus pour réduire la possibilité de réintroduire des données dangereuses dans la forêt récupérée. Vous devrez peut-être modifier ces étapes pour prendre en compte des facteurs tels que:  
  
-   Extensibilité  
-   Facilité de gestion à distance  
-   Vitesse de récupération  

