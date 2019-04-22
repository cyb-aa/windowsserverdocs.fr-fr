---
title: Activer la redirection de dossiers sur le serveur1 de destination Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
H1: Activer la redirection de dossiers sur le serveur de Destination Windows Server Essentials
ms.assetid: f67d195e-36f6-495a-8361-6d5faa889441
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f93d7b28177f96725f2e62c40f9c81cbf186ee6d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819380"
---
# <a name="enable-folder-redirection-on-the-windows-server-essentials-destination-server1"></a>Activer la redirection de dossiers sur le serveur1 de destination Windows Server Essentials

>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Vous pouvez effectuer cette tâche si la redirection de dossiers est activée sur le serveur source.  
  
 Tout d’abord, utilisez le tableau de bord Windows Server Essentials pour activer la redirection de dossiers sur le serveur de Destination. Ensuite, supprimez l'ancien paramètre de stratégie de groupe de redirection de dossiers.  
  
### <a name="to-enable-folder-redirection-on-the-destination-server"></a>Pour activer la redirection de dossiers sur le serveur de destination  
  
1.  Sur le serveur de Destination, ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans la barre de navigation, cliquez sur **PÉRIPHÉRIQUES**.  
  
3.  Dans le volet **Tâches des périphériques** , cliquez sur **Implémenter la stratégie de groupe**.  
  
4.  Dans la page **Activer la stratégie de groupe de redirection de dossiers** , sélectionnez les dossiers à rediriger, puis cliquez sur **Suivant**.  
  
5.  Dans la page **Activer les paramètres de stratégie de sécurité**, cliquez sur **Terminer**.  
  
### <a name="to-delete-the-old-folder-redirection-group-policy-setting"></a>Pour supprimer l'ancien paramètre de stratégie de groupe de redirection de dossiers  
  
1.  Sur le serveur de destination, ouvrez l'outil d'administration **Gestion des stratégies de groupe**.  
  
2.  Dans **Group Policy Management**, développez **forêt : *** Nomdomainedevotreréseau*, développez **domaines**, développez *Nomdomainedevotreréseau* , puis développez **les objets de stratégie de groupe**.  
  
3.  Cliquez avec le bouton droit sur **Redirection de dossiers W7PVP**, puis cliquez sur **Supprimer**.  
  
4.  Lisez l'avertissement, puis cliquez sur **Oui**.  
  
5.  Fermez **Gestion des stratégies de groupe**.  
  
 Pour appliquer la modification apportée à la redirection de dossiers, les utilisateurs réseau doivent fermer la session de leur ordinateur, puis en rouvrir une. Cela garantit le transfert de tous les dossiers redirigés vers le serveur de destination.
