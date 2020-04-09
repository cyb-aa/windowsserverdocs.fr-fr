---
title: Analyser la charge existante sur le serveur d'accès à distance
description: Cette rubrique fait partie du Guide d’analyse et de gestion de l’accès à distance dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 62fa2895-62ae-42cf-817c-53e06ac2a26c
ms.author: lizross
author: eross-msft
ms.openlocfilehash: ba9aa4ea0e8031601ce04fd25814785e4b739f51
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860532"
---
# <a name="monitor-the-existing-load-on-the-remote-access-server"></a>Analyser la charge existante sur le serveur d'accès à distance

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

**Remarque :** Windows Server 2012 associe DirectAccess et le service Routage et accès distant (RRAS) dans un rôle Accès à distance unique.  
  
Le terme « **charge** » fait référence aux statistiques relatives au nombre de connexions sur le serveur d’accès à distance. Voici les étapes nécessaires pour suivre la charge sur le serveur d’accès à distance.  
  
Vous pouvez utiliser le tableau de bord de surveillance qui est disponible dans la console de gestion sur le serveur d’accès à distance pour afficher les statistiques de charge du serveur, ou vous pouvez utiliser les compteurs de l’analyseur de performances pour effectuer le suivi des statistiques.  
  
> [!NOTE]  
> Vous devez être connecté en tant que membre du groupe Admins du domaine ou membre du groupe Administrateurs sur chaque ordinateur pour effectuer les tâches décrites dans cette rubrique. Si vous ne pouvez pas effectuer une tâche lorsque vous êtes connecté avec un compte membre du groupe administrateurs, essayez d’exécuter la tâche lorsque vous êtes connecté avec un compte membre du groupe Admins du domaine.  
  
#### <a name="to-use-the-monitoring-dashboard-to-monitor-the-remote-access-server-load"></a>Pour utiliser le tableau de bord de surveillance pour surveiller la charge du serveur d’accès à distance  
  
1.  Dans **Gestionnaire de serveur**, cliquez sur **Outils**, puis sur **Gestion de l'accès à distance**.  
  
2.  Cliquez sur **TABLEAU DE BORD** pour accéder à **Tableau de bord des accès distants** dans la **Console de gestion de l'accès distant**.  
  
3.  Dans le tableau de bord de surveillance, notez la vignette **État du client distant** dans la vignette **État du serveur** . Cette vignette répertorie des statistiques telles que le nombre total de clients distants qui sont connectés, le nombre total de clients DirectAccess connectés et le nombre maximal d’utilisateurs qui se sont connectés au cours des dernières 24 heures.  
  
4.  Vous pouvez cliquer sur **Actualiser** sous **tâches** dans le volet droit pour recharger l’état d’intégrité. Pour modifier l’intervalle d’actualisation par défaut, cliquez sur **configurer l’intervalle d’actualisation** sous **tâches**.  
  
#### <a name="to-use-the-performance-monitor-tool-to-monitor-performance-counters-on-the-remote-access-server"></a>Pour utiliser l’outil Analyseur de performances pour surveiller les compteurs de performances sur le serveur d’accès à distance  
  
1.  Cliquez sur **Démarrer**, sur **Outils d’administration**, puis double-cliquez sur **Analyseur de performances**.  
  
2.  Sous **performances**, cliquez sur **Analyseur de performances**.  
  
3.  Cliquez sur le bouton **Ajouter** (indiqué par une croix verte) dans la barre d’outils de l' **Analyseur de performances** .  
  
4.  Dans la liste des **compteurs disponibles**, sélectionnez tous les compteurs dans les catégories **RAS** et **RAmgmtsvc** , puis cliquez sur **Ajouter > >** .  
  
5.  Là encore, dans la liste des **compteurs disponibles**, sélectionnez tous les compteurs de la catégorie **connexions IPsec** , puis cliquez sur **Ajouter > >.**  
  
6.  Cliquez sur **OK** pour ajouter les compteurs sélectionnés dans la console de l' **Analyseur de performances** pour le suivi.  
  
L' **Analyseur de performances** affiche désormais sous forme graphique les statistiques de chargement de serveur sélectionnées.  
  
![les commandes Windows PowerShell](../../../media/Monitor-the-existing-load-on-the-Remote-Access-server/PowerShellLogoSmall.gif)***<em>équivalentes</em> Windows PowerShell***  
  
La ou les applets de commande Windows PowerShell suivantes ont la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles apparaissent ici sur plusieurs lignes en raison de contraintes de mise en forme.  
  
```  
PS> Get-RemoteAccessConnectionStatisticsSummary  
```  
  


