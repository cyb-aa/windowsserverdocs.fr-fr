---
title: Analyser l'état de fonctionnement du serveur d'accès à distance et de ses composants
description: Cette rubrique fait partie du guide de la surveillance de l’accès à distance et la gestion des comptes dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 077a3a64-2fa3-4994-9711-ec1fbdc081ba
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9ccfd0cd65a3504349dcad3bd7a549ed18eb6279
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281145"
---
# <a name="monitor-the-operations-status-of-the-remote-access-server-and-its-components"></a>Analyser l'état de fonctionnement du serveur d'accès à distance et de ses composants

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

**Remarque :** Windows Server 2012 associe DirectAccess et le service Routage et accès distant (RRAS) dans un rôle Accès à distance unique.  
  
La console de gestion dans le serveur d’accès à distance peut être utilisée pour surveiller son état des opérations.  
  
> [!NOTE]  
> Vous devez être connecté en tant que membre du groupe Admins du domaine ou un membre du groupe Administrateurs sur chaque ordinateur pour terminer la tâche décrite dans cette rubrique. Si vous ne pouvez pas effectuer une tâche pendant que vous êtes connecté avec un compte qui est membre du groupe Administrateurs, essayez d’effectuer la tâche pendant que vous êtes connecté avec un compte qui est membre du groupe Admins du domaine.  
  
#### <a name="to-monitor-the-remote-access-server-operations-status"></a>Pour surveiller l’état de fonctionnement du serveur accès à distance  
  
1.  Dans **Gestionnaire de serveur**, cliquez sur **Outils**, puis sur **Gestion de l'accès à distance**.  
  
2.  Cliquez sur **tableau de bord** pour accéder à **rapports l’accès à distance** dans le **Console de gestion des accès à distance**.  
  
3.  Dans le tableau de bord de surveillance, notez le **état des opérations** vignette dans le **état du serveur** vignette. Cette vignette répertorie l’état des opérations serveur et l’état de tous les composants du serveur.  
  
4.  Cliquez sur **Actualiser** sous **tâches** dans le volet droit pour recharger l’état des opérations. L’état des opérations est automatiquement actualisé toutes les cinq minutes, qui est l’intervalle d’actualisation par défaut. Pour modifier l’intervalle d’actualisation par défaut, cliquez sur **configurer l’intervalle d’actualisation**.  
  
![Windows PowerShell](../../../media/Monitor-the-operations-status-of-the-Remote-Access-server-and-its-components/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
> [!NOTE]  
> La commande pour l’état des opérations d’un cluster est incluse pour référence.  
  
```  
PS> Get-RemoteAccessHealth  
PS> Get-RemoteAccessHealth -Cluster  
```  
  


