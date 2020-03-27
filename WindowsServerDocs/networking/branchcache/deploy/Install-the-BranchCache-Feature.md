---
title: Installer la fonctionnalité BranchCache
description: Cette rubrique fait partie du Guide de déploiement BranchCache pour Windows Server 2016, qui montre comment déployer BranchCache en mode de cache distribué et hébergé pour optimiser l’utilisation de la bande passante WAN dans les filiales.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 4f31dc61-2dbe-4c7e-b3f9-85ae49a45049
ms.author: lizross
author: eross-msft
ms.openlocfilehash: a895e65686a6ccfb1453bc7cc7ddfcab5720a206
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319212"
---
# <a name="install-the-branchcache-feature"></a>Installer la fonctionnalité BranchCache

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette procédure pour installer la fonctionnalité BranchCache et démarrer le service BranchCache sur un ordinateur exécutant Windows Server&reg; 2016, Windows Server 2012 R2 ou Windows Server 2012.  
  
Pour effectuer cette procédure, il est nécessaire d'appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent.  
  
Avant d’effectuer cette procédure, il est recommandé d’installer et de configurer votre application ou serveur Web basé sur BITS.  
  
> [!NOTE]  
> Pour effectuer cette procédure à l’aide de Windows PowerShell, exécutez Windows PowerShell en tant qu’administrateur, tapez les commandes suivantes à l’invite Windows PowerShell, puis appuyez sur entrée.  
>   
> `Install-WindowsFeature BranchCache`  
>   
> `Restart-Computer`  
  
### <a name="to-install-and-enable-the-branchcache-feature"></a>Pour installer et activer la fonctionnalité BranchCache  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **Gérer**, puis sur **Ajouter des rôles et des fonctionnalités**. L’Assistant Ajout de rôles et de fonctionnalités s’ouvre. Cliquez sur **Suivant**.  
  
2.  Dans **Sélectionner le type d’installation**, vérifiez que installation basée sur **un rôle ou une fonctionnalité** est sélectionné, puis cliquez sur **suivant**.  
  
3.  Dans **Sélectionner le serveur de destination**, vérifiez que le serveur correct est sélectionné, puis cliquez sur **suivant**.  
  
4.  Dans **Sélectionner des rôles de serveurs**, cliquez sur **Suivant**.  
  
5.  Dans **Sélectionner des fonctionnalités**, cliquez sur **BranchCache**, puis sur **suivant**.  
  
6.  Dans **Confirmer les sélections pour l’installation**, cliquez sur **Installer**. Dans la progression de l' **installation**, l’installation de la fonctionnalité BranchCache se poursuit. Une fois l’installation terminée, cliquez sur **Fermer**.  
  
Après l’installation de la fonctionnalité BranchCache, le service BranchCache, également appelé PeerDistSvc-est activé, et le type de démarrage est automatique.  
  


