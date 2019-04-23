---
title: Installer la fonctionnalité BranchCache
description: Cette rubrique fait partie de BranchCache déploiement Guide pour Windows Server 2016, qui montre comment déployer BranchCache en mode cache distribué et hébergé pour optimiser l’utilisation de la bande passante WAN dans les succursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 4f31dc61-2dbe-4c7e-b3f9-85ae49a45049
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8b4aecd9e9355a6c2d5ac485ac77c76428fe295f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872190"
---
# <a name="install-the-branchcache-feature"></a>Installer la fonctionnalité BranchCache

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette procédure pour installer la fonctionnalité BranchCache et démarrer le service BranchCache sur un ordinateur exécutant Windows Server&reg; 2016, Windows Server 2012 R2 ou Windows Server 2012.  
  
Pour effectuer cette procédure, il est nécessaire d'appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent.  
  
Avant d’effectuer cette procédure, il est recommandé d’installer et de configurer votre application basée sur les BITS ou un serveur Web.  
  
> [!NOTE]  
> Pour effectuer cette procédure à l’aide de Windows PowerShell, exécutez Windows PowerShell en tant qu’administrateur, tapez les commandes suivantes à l’invite Windows PowerShell, puis appuyez sur ENTRÉE.  
>   
> `Install-WindowsFeature BranchCache`  
>   
> `Restart-Computer`  
  
### <a name="to-install-and-enable-the-branchcache-feature"></a>Pour installer et activer la fonctionnalité BranchCache  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **Gérer**, puis sur **Ajouter des rôles et des fonctionnalités**. L’Assistant Ajout de rôles et fonctionnalités s’ouvre. Cliquez sur **Suivant**.  
  
2.  Dans **sélectionner type d’installation**, vérifiez que **installation en fonction du rôle ou une fonctionnalité** est sélectionnée, puis cliquez sur **suivant**.  
  
3.  Dans **server de sélectionner la destination**, assurez-vous que le serveur correct est sélectionné, puis cliquez sur **suivant**.  
  
4.  Dans **Sélectionner des rôles de serveurs**, cliquez sur **Suivant**.  
  
5.  Dans **sélectionner des fonctionnalités**, cliquez sur **BranchCache**, puis cliquez sur **suivant**.  
  
6.  Dans **Confirmer les sélections pour l’installation**, cliquez sur **Installer**. Dans **progression de l’Installation**, l’installation de la fonctionnalité BranchCache se poursuit. Lors de l’installation est terminée, cliquez sur **fermer**.  
  
Après avoir installé la fonctionnalité BranchCache, le service BranchCache - également appelé le PeerDistSvc - est activé, et le type de démarrage est automatique.  
  


