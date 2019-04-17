---
title: Installer la fonctionnalité BranchCache
description: Cette rubrique fait partie de la BranchCache déploiement Guide pour Windows Server2016, qui montre comment déployer BranchCache en mode de cache distribué et hébergé d’optimiser l’utilisation de la bande passante réseau étendu dans les filiales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 4f31dc61-2dbe-4c7e-b3f9-85ae49a45049
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a69848536b56521da9b5ef07689aba7f8690e888
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="install-the-branchcache-feature"></a>Installer la fonctionnalité BranchCache

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette procédure pour installer la fonctionnalité BranchCache et démarrer le service BranchCache sur un ordinateur exécutant Windows Server&reg; 2016, Windows Server2012R2 ou Windows Server2012.  
  
L’appartenance au groupe **administrateurs** ou équivalent est le minimum requis pour effectuer cette procédure.  
  
Avant d’effectuer cette procédure, il est recommandé d’installer et configurer votre application BITS ou un serveur Web.  
  
> [!NOTE]  
> Pour effectuer cette procédure à l’aide de Windows PowerShell, exécutez Windows PowerShell en tant qu’administrateur, tapez les commandes suivantes à l’invite Windows PowerShell et appuyez sur ENTRÉE.  
>   
> `Install-WindowsFeature BranchCache`  
>   
> `Restart-Computer`  
  
### <a name="to-install-and-enable-the-branchcache-feature"></a>Pour installer et activer la fonctionnalité BranchCache  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **gérer**, puis cliquez sur **Ajout de rôles et fonctionnalités**. L’Assistant Ajout de rôles et fonctionnalités s’ouvre. Cliquez sur **suivant**.  
  
2.  Dans **sélectionner le type d’installation**, assurez-vous que **installation basée sur un rôle ou une fonctionnalité** est sélectionné, puis cliquez sur **suivant**.  
  
3.  Dans **serveur de destination sélectionnez**, assurez-vous que le serveur correct est sélectionné, puis cliquez sur **suivant**.  
  
4.  Dans **sélectionner des rôles de serveur**, cliquez sur **suivant**.  
  
5.  Dans **sélectionner des fonctionnalités**, cliquez sur **BranchCache**, puis cliquez sur **suivant**.  
  
6.  Dans **confirmer les sélections d’installation**, cliquez sur **installer**. Dans **progression de l’Installation**, l’installation de la fonctionnalité BranchCache se poursuit. Lorsque l’installation est terminée, cliquez sur **fermer**.  
  
Après avoir installé la fonctionnalité BranchCache, le service BranchCache - également appelé le PeerDistSvc - est activé, et le type de démarrage est défini sur automatique.  
  


