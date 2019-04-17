---
title: Activer la redirection de dossiers sur le serveur1 de Destination WindowsServerEssentials
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
H1: Activer la redirection de dossiers sur le serveur de Destination WindowsServerEssentials
ms.assetid: f67d195e-36f6-495a-8361-6d5faa889441
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f93d7b28177f96725f2e62c40f9c81cbf186ee6d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="enable-folder-redirection-on-the-windows-server-essentials-destination-server1"></a>Activer la redirection de dossiers sur le serveur1 de Destination WindowsServerEssentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Vous pouvez effectuer cette tâche si la redirection de dossiers est activée sur le serveur Source.  
  
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
  
3.  Avec le bouton droit **la Redirection de dossiers W7PVP**, puis cliquez sur **supprimer**.  
  
4.  Lisez l’avertissement, puis cliquez sur **Oui**.  
  
5.  Fermer **gestion des stratégies de groupe**.  
  
 Pour appliquer la modification à la redirection de dossiers, les utilisateurs du réseau doivent session sur leur ordinateur, puis rouvrez-en une sous. Cela garantit le transfert de tous les dossiers redirigés vers le serveur de Destination.
