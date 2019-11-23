---
title: Analyser l'état de fonctionnement du serveur d'accès à distance et de ses composants
description: Cette rubrique fait partie du Guide d’analyse et de gestion de l’accès à distance dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 077a3a64-2fa3-4994-9711-ec1fbdc081ba
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d0ad63ec88a428239a174a0217db94c44ab799bc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404550"
---
# <a name="monitor-the-operations-status-of-the-remote-access-server-and-its-components"></a>Analyser l'état de fonctionnement du serveur d'accès à distance et de ses composants

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

**Remarque :** Windows Server 2012 associe DirectAccess et le service Routage et accès distant (RRAS) dans un rôle Accès à distance unique.  
  
La console de gestion du serveur d’accès à distance peut être utilisée pour surveiller l’état des opérations.  
  
> [!NOTE]  
> Vous devez être connecté en tant que membre du groupe Admins du domaine ou membre du groupe Administrateurs sur chaque ordinateur pour effectuer la tâche décrite dans cette rubrique. Si vous ne pouvez pas effectuer une tâche lorsque vous êtes connecté avec un compte membre du groupe administrateurs, essayez d’exécuter la tâche lorsque vous êtes connecté avec un compte membre du groupe Admins du domaine.  
  
#### <a name="to-monitor-the-remote-access-server-operations-status"></a>Pour surveiller l’état des opérations du serveur d’accès à distance  
  
1.  Dans **Gestionnaire de serveur**, cliquez sur **Outils**, puis sur **Gestion de l'accès à distance**.  
  
2.  Cliquez sur **tableau de bord** pour accéder à rapports d' **accès à distance** dans la console de gestion de l' **accès à distance**.  
  
3.  Dans le tableau de bord de surveillance, notez la vignette **État des opérations** dans la vignette **État du serveur** . Cette vignette indique l’état des opérations du serveur et l’état de tous les composants du serveur.  
  
4.  Cliquez sur **Actualiser** sous **tâches** dans le volet droit pour recharger l’état des opérations. L’état des opérations est actualisé automatiquement toutes les cinq minutes, ce qui correspond à l’intervalle d’actualisation par défaut. Pour modifier l’intervalle d’actualisation par défaut, cliquez sur **configurer l’intervalle d’actualisation**.  
  
![les commandes Windows PowerShell](../../../media/Monitor-the-operations-status-of-the-Remote-Access-server-and-its-components/PowerShellLogoSmall.gif)***<em>équivalentes</em> Windows PowerShell***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
> [!NOTE]  
> La commande d’état des opérations d’un cluster est incluse à des fins de référence.  
  
```  
PS> Get-RemoteAccessHealth  
PS> Get-RemoteAccessHealth -Cluster  
```  
  


