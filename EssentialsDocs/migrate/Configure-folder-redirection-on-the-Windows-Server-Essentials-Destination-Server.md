---
title: Configurer la redirection de dossiers sur le serveur de destination Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: fe77ba67-128c-4fc3-9361-30fa6af42516
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 15a2dee75b5a57f843e8e63b486efe55645e5de2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852612"
---
# <a name="configure-folder-redirection-on-the-windows-server-essentials-destination-server"></a>Configurer la redirection de dossiers sur le serveur de destination Windows Server Essentials

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Effectuez cette tâche si la redirection de dossiers est activée sur le serveur source.  
  
 Pour commencer, supprimez l'ancien paramètre de stratégie de groupe de redirection de dossiers. Utilisez ensuite le tableau de bord Windows Server Essentials pour activer la redirection de dossiers sur le serveur de destination.  
  
### <a name="to-delete-the-old-folder-redirection-group-policy-setting"></a>Pour supprimer l'ancien paramètre de stratégie de groupe de redirection de dossiers  
  
1. Sur le serveur de destination, ouvrez l'outil d'administration **Gestion des stratégies de groupe**.  
  
2. Dans **Gestion des stratégies de groupe**, développez **Forêt :** <em>NomDomaineDeVotreRéseau</em>, développez **Domaines**, développez *NomDomaineDeVotreRéseau*, puis **Objets de stratégie de groupe**.  
  
3. Cliquez avec le bouton droit sur **Redirection de dossiers de la stratégie de groupe SBS**, puis cliquez sur **Supprimer**.  
  
4. Cliquez avec le bouton droit sur **Modèles de sécurité de la stratégie de groupe SBS**, puis cliquez sur **Supprimer**.  
  
5. Lisez l'avertissement, puis cliquez sur **Oui**.  
  
6. Fermez **Gestion des stratégies de groupe**.  
  
### <a name="to-enable-folder-redirection-on-the-destination-server"></a>Pour activer la redirection de dossiers sur le serveur de destination  
  
1. Sur le serveur de destination, ouvrez le tableau de bord Windows Server Essentials.  
  
2. Dans la barre de navigation, cliquez sur **Périphériques**.  
  
3. Dans le volet **Tâches des périphériques**, cliquez sur **Implémenter la stratégie de groupe**.  
  
4. Dans la page **Activer la stratégie de groupe de redirection de dossiers**, sélectionnez les dossiers à rediriger, puis cliquez sur **Suivant**.  
  
5. Dans la page **Activer les paramètres de stratégie de sécurité**, cliquez sur **Terminer**.  
  
   Pour appliquer la modification apportée à la redirection de dossiers, les utilisateurs réseau doivent fermer la session de leur ordinateur, puis en rouvrir une. Cela garantit le transfert de tous les dossiers redirigés vers le serveur de destination.
