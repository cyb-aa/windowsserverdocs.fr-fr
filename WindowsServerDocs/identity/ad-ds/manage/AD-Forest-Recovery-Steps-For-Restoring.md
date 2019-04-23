---
title: Récupération de forêt AD - étapes de la restauration de la forêt
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: 1712d3a636160f82495539afdd42ab2ee85ffae2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861470"
---
# <a name="ad-forest-recovery---steps-for-restoring-the-forest"></a>Récupération de forêt AD - étapes de la restauration de la forêt

>S'applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

Cette section fournit une vue d’ensemble de la procédure recommandée pour la récupération d’une forêt. La procédure de récupération de forêt est décrites en détail ultérieurement.  
  
La liste suivante résume les étapes de récupération à un niveau élevé :  
  
1. [Identifier le problème](AD-Forest-Recovery-Identify-the-Problem.md)  

   Travailler avec informatique et de Support Microsoft pour déterminer l’étendue du problème et les causes potentielles et évaluer les solutions possibles avec toutes les parties prenantes. Dans de nombreux cas, récupération de forêt total doit être la dernière option.  
  
2. [Décider comment récupérer la forêt](AD-Forest-Recovery-Determine-how-to-Recover.md)  

   Après avoir déterminé que la récupération de forêt est nécessaire, version préliminaire terminé les étapes pour vous y préparer : déterminer la structure actuelle de la forêt, identifier les fonctions de chaque contrôleur de domaine effectue, décider quel contrôleur de domaine pour la restauration pour chaque domaine et assurez-vous que tous les contrôleurs de domaine accessible en écriture sont mis hors connexion.  

3. [Effectuer une récupération initiale](AD-Forest-Recovery-Perform-initial-recovery.md)  

   De manière isolée, récupérer un contrôleur de domaine pour chaque domaine, les nettoyer et se reconnecter les domaines. Réinitialiser les comptes privilégiés, rechercher et corriger les problèmes causés par des failles de sécurité dans cette phase.  
  
4. [Redéployer restant des contrôleurs de domaine](AD-Forest-Recovery-Restore-Additional-DCs.md)  

   Redéployez la forêt pour lui rendre son état avant la défaillance. Cette étape devra être adaptée à vos exigences et de conception spécifiques. Clonage du contrôleur de domaine virtualisé peut aider à accélérer ce processus.  

5. [Cleanup](AD-Forest-Recovery-Cleanup.md)  

   Une fois que la fonctionnalité a été restaurée, reconfigurer la résolution de noms en fonction des besoins et utilisation des applications métier.  

Les étapes décrites dans ce guide sont conçus pour minimiser le risque de réintroduction de données dangereuses dans la forêt récupérée. Il se peut que vous deviez modifier ces étapes pour tenir compte des facteurs tels que :  
  
- Extensibilité  
- Facilité de gestion à distance  
- Vitesse de récupération  
