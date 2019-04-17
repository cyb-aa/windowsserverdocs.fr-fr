---
title: Configurer la redirection de dossiers sur le serveur de Destination WindowsServerEssentials
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fe77ba67-128c-4fc3-9361-30fa6af42516
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 7cc1207f36d3a921b49cc3ecd02acf3fe4fa243c
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="configure-folder-redirection-on-the-windows-server-essentials-destination-server"></a>Configurer la redirection de dossiers sur le serveur de Destination WindowsServerEssentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Effectuez cette tâche si la redirection de dossiers est activée sur le serveur Source.  
  
 Tout d’abord, supprimez l’ancien paramètre de stratégie de groupe de Redirection de dossiers. Puis utilisez le tableau de bord WindowsServerEssentials pour activer la redirection de dossiers sur le serveur de Destination.  
  
### <a name="to-delete-the-old-folder-redirection-group-policy-setting"></a>Pour supprimer l’ancien paramètre de stratégie de groupe de Redirection de dossiers  
  
1.  Sur le serveur de Destination, ouvrez le **gestion des stratégies de groupe** outil d’administration.  
  
2.  Dans **gestion des stratégies de groupe**, développez **forêt:***Nomdomainedevotreréseau*, développez **domaines**, développez *Nomdomainedevotreréseau*, puis développez **objets de stratégie de groupe**.  
  
3.  Avec le bouton droit **Redirection de dossiers de stratégie de groupe SBS**, puis cliquez sur **supprimer**.  
  
4.  Avec le bouton droit **modèles de sécurité de stratégie de groupe SBS**, puis cliquez sur **supprimer**.  
  
5.  Lisez l’avertissement, puis cliquez sur **Oui**.  
  
6.  Fermer **gestion des stratégies de groupe**.  
  
### <a name="to-enable-folder-redirection-on-the-destination-server"></a>Pour activer la redirection de dossiers sur le serveur de Destination  
  
1.  Sur le serveur de Destination, ouvrez le tableau de bord WindowsServerEssentials.  
  
2.  Dans la barre de navigation, cliquez sur **périphériques**.  
  
3.  Dans le **tâches périphériques** volet, cliquez sur **implémenter la stratégie de groupe**.  
  
4.  Sur le **activer la stratégie de groupe de Redirection de dossier**, sélectionnez les dossiers à rediriger, puis cliquez sur **suivant**.  
  
5.  Sur le **activer les paramètres de stratégie de sécurité**, cliquez sur **Terminer**.  
  
 Pour appliquer la modification à la redirection de dossiers, les utilisateurs du réseau doivent session sur leur ordinateur, puis rouvrez-en une sous. Cela garantit le transfert de tous les dossiers redirigés vers le serveur de Destination.
