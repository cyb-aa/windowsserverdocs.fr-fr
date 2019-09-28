---
title: 'Récupération de la forêt Active Directory : étapes de restauration de la forêt'
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: 07a043c4361f8eaae30b1dea665c604c0df42333
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390291"
---
# <a name="ad-forest-recovery---steps-for-restoring-the-forest"></a>Récupération de la forêt Active Directory : étapes de restauration de la forêt

>S'applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

Cette section fournit une vue d’ensemble du chemin d’accès recommandé pour la récupération d’une forêt. Les étapes de récupération de la forêt sont décrites en détail plus loin.  
  
La liste suivante résume les étapes de récupération à un niveau élevé :  
  
1. [Identifier le problème](AD-Forest-Recovery-Identify-the-Problem.md)  

   Collaborez avec le service informatique et Support Microsoft pour déterminer l’étendue du problème et des causes potentielles, et évaluez les solutions possibles avec toutes les parties prenantes de l’entreprise. Dans de nombreux cas, la récupération de forêt totale doit être la dernière option.  
  
2. [Décider comment récupérer la forêt](AD-Forest-Recovery-Determine-how-to-Recover.md)  

   Une fois que vous avez déterminé que la récupération de la forêt est nécessaire, effectuez les étapes préliminaires pour la préparer : déterminer la structure actuelle de la forêt, identifier les fonctions effectuées par chaque contrôleur de domaine, décider du contrôleur de domaine à restaurer pour chaque domaine et vérifier que tous les contrôleurs de domaine accessibles en écriture sont mis hors connexion.  

3. [Effectuer la récupération initiale](AD-Forest-Recovery-Perform-initial-recovery.md)  

   En isolation, récupérez un contrôleur de domaine pour chaque domaine, nettoyez-les, puis reconnectez les domaines. Réinitialiser les comptes privilégiés et résoudre les problèmes causés par des violations de sécurité dans cette phase.  
  
4. [Redéployer les contrôleurs de contrôle restants](AD-Forest-Recovery-Restore-Additional-DCs.md)  

   Redéployez la forêt pour la ramener à son état avant la défaillance. Cette étape doit être adaptée à votre conception et à vos exigences spécifiques. Le clonage des contrôleurs de domaine virtualisés peut vous aider à accélérer ce processus.  

5. [Nettoyage](AD-Forest-Recovery-Cleanup.md)  

   Une fois que la fonctionnalité a été restaurée, reconfigurez la résolution de noms en fonction des besoins et récupérez les applications LOB opérationnelles.  

Les étapes de ce guide sont conçues pour réduire la possibilité de réintroduire des données dangereuses dans la forêt récupérée. Vous devrez peut-être modifier ces étapes pour prendre en compte les facteurs suivants :  
  
- Extensibilité  
- Facilité de gestion à distance  
- Vitesse de récupération  
