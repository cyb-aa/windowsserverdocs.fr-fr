---
title: 'Étape 5 : Activer la redirection de dossiers sur le serveur de destination pour la migration de Windows Server Essentials'
description: Décrit comment utiliser Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: d3925f80-552d-431f-b2a6-2af202e50ca4
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 710522f52791f3ee6c1c453c883f4265d08023be
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852342"
---
# <a name="step-5-enable-folder-redirection-on-the-destination-server-for-windows-server-essentials-migration"></a>Étape 5 : Activer la redirection de dossiers sur le serveur de destination pour la migration de Windows Server Essentials

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Si la redirection de dossiers est activée sur le serveur source, vous pouvez l’activer sur le serveur de destination, puis supprimer l’ancien paramètre Stratégie de groupe de redirection de dossier.  
  
 Tout d’abord, utilisez le tableau de bord Windows Server Essentials pour activer la redirection de dossiers sur le serveur de destination. Ensuite, supprimez l'ancien paramètre de stratégie de groupe de redirection de dossiers.  
  
### <a name="to-enable-folder-redirection-on-the-destination-server"></a>Pour activer la redirection de dossiers sur le serveur de destination  
  
1.  Sur le serveur de destination, ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans la barre de navigation, cliquez sur **PÉRIPHÉRIQUES**.  
  
3.  Dans le volet **Tâches des périphériques**, cliquez sur **Implémenter la stratégie de groupe**.  
  
4.  Dans la page **Activer la stratégie de groupe de redirection de dossiers**, sélectionnez les dossiers à rediriger, puis cliquez sur **Suivant**.  
  
5.  Dans la page **Activer les paramètres de stratégie de sécurité**, cliquez sur **Terminer**.  
  
### <a name="to-delete-the-old-folder-redirection-group-policy-setting"></a>Pour supprimer l'ancien paramètre de stratégie de groupe de redirection de dossiers  
  
1. Sur le serveur de destination, ouvrez l'outil d'administration **Gestion des stratégies de groupe**.  
  
2. Dans **Gestion des stratégies de groupe**, développez **Forêt :** <em>NomDomaineDeVotreRéseau</em>, développez **Domaines**, développez *NomDomaineDeVotreRéseau*, puis **Objets de stratégie de groupe**.  
  
3. Cliquez avec le bouton droit sur la stratégie que vous souhaitez supprimer, puis cliquez sur **Supprimer**.  
  
4. Lisez l'avertissement, puis cliquez sur **Oui**.  
  
5. Fermez **Gestion des stratégies de groupe**.  
  
   Pour appliquer la modification apportée à la redirection de dossiers, les utilisateurs réseau doivent fermer la session de leur ordinateur, puis en rouvrir une. Cela garantit le transfert de tous les dossiers redirigés vers le serveur de destination.  
  
## <a name="next-steps"></a>Étapes suivantes :  
 Vous avez activé la redirection de dossiers sur le serveur de destination. Passez maintenant à [l’étape 6 : rétrograder et supprimer le serveur source du nouveau réseau Windows Server Essentials](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md).  
  

Pour afficher toutes les étapes, consultez [migrer vers Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

