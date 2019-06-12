---
title: Analyser la charge existante sur le serveur d'accès à distance
description: Cette rubrique fait partie du guide de la surveillance de l’accès à distance et la gestion des comptes dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 62fa2895-62ae-42cf-817c-53e06ac2a26c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6d7a5aa7b699f5a8f24c4a36ee8ae314768329b4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446858"
---
# <a name="monitor-the-existing-load-on-the-remote-access-server"></a>Analyser la charge existante sur le serveur d'accès à distance

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

**Remarque :** Windows Server 2012 associe DirectAccess et le service Routage et accès distant (RRAS) dans un rôle Accès à distance unique.  
  
Le terme **charge** fait référence aux statistiques relatives au nombre de connexions sur le serveur d’accès à distance. Voici les étapes requises pour effectuer le suivi de la charge sur le serveur d’accès à distance.  
  
Vous pouvez utiliser le tableau de bord de surveillance qui est disponible dans la console de gestion sur le serveur d’accès à distance pour afficher les statistiques de charge pour le serveur, ou vous pouvez utiliser des compteurs de l’Analyseur de performances pour suivre les statistiques.  
  
> [!NOTE]  
> Vous devez être connecté en tant que membre du groupe Admins du domaine ou un membre du groupe Administrateurs sur chaque ordinateur pour effectuer les tâches décrites dans cette rubrique. Si vous ne pouvez pas effectuer une tâche pendant que vous êtes connecté avec un compte qui est membre du groupe Administrateurs, essayez d’effectuer la tâche pendant que vous êtes connecté avec un compte qui est membre du groupe Admins du domaine.  
  
#### <a name="to-use-the-monitoring-dashboard-to-monitor-the-remote-access-server-load"></a>Pour utiliser le tableau de bord de surveillance pour surveiller la charge du serveur accès à distance  
  
1.  Dans **Gestionnaire de serveur**, cliquez sur **Outils**, puis sur **Gestion de l'accès à distance**.  
  
2.  Cliquez sur **TABLEAU DE BORD** pour accéder à **Tableau de bord des accès distants** dans la **Console de gestion de l'accès distant**.  
  
3.  Dans le tableau de bord de surveillance, notez le **statut du Client distant** vignette dans le **état du serveur** vignette. Cette vignette répertorie des statistiques, telles que le nombre total de clients distants qui sont connectés, le nombre total de clients DirectAccess qui sont connectés et le nombre maximal d’utilisateurs qui se sont connectés dans les dernières 24 heures.  
  
4.  Vous pouvez cliquer sur **Actualiser** sous **tâches** dans le volet droit pour recharger l’état d’intégrité. Pour modifier l’intervalle d’actualisation par défaut, cliquez sur **configurer l’intervalle d’actualisation** sous **tâches**.  
  
#### <a name="to-use-the-performance-monitor-tool-to-monitor-performance-counters-on-the-remote-access-server"></a>Pour utiliser l’outil Analyseur de performances pour surveiller les compteurs de performances sur le serveur d’accès à distance  
  
1.  Cliquez sur **Démarrer**, cliquez sur **outils d’administration**, puis double-cliquez sur **Analyseur de performances**.  
  
2.  Sous **performances**, cliquez sur **Analyseur de performances**.  
  
3.  Cliquez sur le **ajouter** bouton (indiquée par une icône de croix vert) dans le **Analyseur de performances** barre d’outils.  
  
4.  Dans la liste des **compteurs disponibles**, sélectionnez tous les compteurs dans la **RAS** et **RAmgmtsvc** catégories, puis cliquez sur **Ajouter >>** .  
  
5.  Là encore, dans la liste des **compteurs disponibles**, sélectionnez tous les compteurs dans la **des connexions IPsec** catégorie, puis cliquez sur **Ajouter >>.**  
  
6.  Cliquez sur **OK** pour ajouter les compteurs sélectionnés dans le **Analyseur de performances** console pour le suivi.  
  
**Analyseur de performances** affichent désormais sous forme graphique les statistiques de charge de serveur sélectionné.  
  
![Windows PowerShell](../../../media/Monitor-the-existing-load-on-the-Remote-Access-server/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```  
PS> Get-RemoteAccessConnectionStatisticsSummary  
```  
  


