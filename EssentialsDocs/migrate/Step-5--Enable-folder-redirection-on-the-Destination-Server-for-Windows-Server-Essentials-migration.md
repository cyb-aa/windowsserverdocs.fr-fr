---
title: "Étape5: Activer la redirection de dossiers sur la migration du serveur de Destination pour WindowsServerEssentials"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d3925f80-552d-431f-b2a6-2af202e50ca4
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 613ff4c80a80ed4f3207cb0c1ead6db12c723e85
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="step-5-enable-folder-redirection-on-the-destination-server-for-windows-server-essentials-migration"></a>Étape5: Activer la redirection de dossiers sur la migration du serveur de Destination pour WindowsServerEssentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Si la redirection de dossiers est activée sur le serveur Source, vous pouvez activer la redirection de dossiers sur le serveur de Destination, puis supprimez l’ancien paramètre de stratégie de groupe de Redirection de dossiers.  
  
 Tout d’abord, utilisez le tableau de bord WindowsServerEssentials pour activer la redirection de dossiers sur le serveur de Destination. Ensuite, supprimez l’ancien paramètre de stratégie de groupe de Redirection de dossiers.  
  
### <a name="to-enable-folder-redirection-on-the-destination-server"></a>Pour activer la redirection de dossiers sur le serveur de Destination  
  
1.  Sur le serveur de Destination, ouvrez le tableau de bord WindowsServerEssentials.  
  
2.  Dans la barre de navigation, cliquez sur **périphériques**.  
  
3.  Dans le **tâches périphériques** volet, cliquez sur **implémenter la stratégie de groupe**.  
  
4.  Sur le **activer la stratégie de groupe de Redirection de dossier**, sélectionnez les dossiers à rediriger, puis cliquez sur **suivant**.  
  
5.  Sur le **activer les paramètres de stratégie de sécurité**, cliquez sur **Terminer**.  
  
### <a name="to-delete-the-old-folder-redirection-group-policy-setting"></a>Pour supprimer l’ancien paramètre de stratégie de groupe de Redirection de dossiers  
  
1.  Sur le serveur de Destination, ouvrez le **gestion des stratégies de groupe** outil d’administration.  
  
2.  Dans **gestion des stratégies de groupe**, développez **forêt:***Nomdomainedevotreréseau*, développez **domaines**, développez *Nomdomainedevotreréseau*, puis développez **objets de stratégie de groupe**.  
  
3.  Avec le bouton droit de la stratégie que vous souhaitez supprimer, puis cliquez sur **supprimer**.  
  
4.  Lisez l’avertissement, puis cliquez sur **Oui**.  
  
5.  Fermer **gestion des stratégies de groupe**.  
  
 Pour appliquer la modification de la redirection de dossiers, les utilisateurs du réseau doivent session leurs ordinateurs, puis rouvrez-en une sous. Cela garantit le transfert de tous les dossiers redirigés vers le serveur de Destination.  
  
## <a name="next-steps"></a>Étapes suivantes  
 Vous avez activé la redirection de dossiers sur le serveur de Destination. Passez maintenant à [étape6: rétrograder et supprimer le serveur Source du nouveau réseau WindowsServerEssentials](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md).  
  

Pour afficher toutes les étapes, voir [migrer vers WindowsServerEssentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

