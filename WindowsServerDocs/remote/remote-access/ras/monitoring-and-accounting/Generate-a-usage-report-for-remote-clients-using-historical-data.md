---
title: Générer un rapport d'utilisation pour les clients distants à l'aide de données d'historique
description: Cette rubrique fait partie du guide de la surveillance de l’accès à distance et la gestion des comptes dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0305467b-ce39-4532-a05a-2cc5ff946f55
ms.author: pashort
author: shortpatti
ms.openlocfilehash: cfbac18f64123f97c54b29c1aeef7364af55e49a
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281129"
---
# <a name="generate-a-usage-report-for-remote-clients-using-historical-data"></a>Générer un rapport d'utilisation pour les clients distants à l'aide de données d'historique

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

**Remarque :** Windows Server 2012 associe DirectAccess et le service Routage et accès distant (RRAS) dans un rôle Accès à distance unique.  
  
La console de gestion sur le serveur d’accès à distance peut être utilisée pour générer un rapport d’utilisation pour les clients distants qui accèdent au serveur. Pour générer un rapport d’utilisation pour les clients distants, vous tout d’abord activez la gestion des comptes sur le serveur d’accès à distance. Après avoir généré le rapport, vous pouvez utiliser le tableau de bord de surveillance qui est disponible dans la console de gestion sur le serveur d’accès à distance pour afficher les statistiques de charge sur le serveur.  
  
> [!NOTE]  
> Vous devez être connecté en tant que membre du groupe Admins du domaine ou un membre du groupe Administrateurs sur chaque ordinateur pour effectuer les tâches décrites dans cette rubrique. Si vous ne pouvez pas effectuer une tâche pendant que vous êtes connecté avec un compte qui est membre du groupe Administrateurs, essayez d’effectuer la tâche pendant que vous êtes connecté avec un compte qui est membre du groupe Admins du domaine.  
  
#### <a name="to-enable-accounting-on-the-remote-access-server"></a>Pour activer la gestion des comptes sur le serveur d’accès à distance  
  
1.  Dans **Gestionnaire de serveur**, cliquez sur **Outils**, puis sur **Gestion de l'accès à distance**.  
  
2.  Cliquez sur **REPORTING** pour accéder à **rapports l’accès à distance** dans le **Console de gestion des accès à distance**.  
  
3.  Cliquez sur **configurer Comptabilité** dans le **rapports l’accès à distance** volet de tâches.  
  
4.  Sélectionnez le **utiliser la boîte de réception comptabilité** case à cocher pour activer la gestion des comptes sur le serveur d’accès à distance.  
  
5.  Cliquez sur **appliquer** pour activer la configuration de gestion sur le serveur, puis cliquez sur **fermer** une fois que le serveur a appliqué la configuration avec succès.  
  
#### <a name="to-generate-the-usage-report"></a>Pour générer le rapport d’utilisation  
  
1.  Dans **Gestionnaire de serveur**, cliquez sur **Outils**, puis sur **Gestion de l'accès à distance**.  
  
2.  Cliquez sur **REPORTING** pour accéder à **rapports l’accès à distance** dans le **Console de gestion des accès à distance**.  
  
3.  Dans le volet central, cliquez sur les dates du calendrier pour sélectionner la durée de rapport **date de début :** et **date de fin :** , puis cliquez sur **générer le rapport**.  
  
4.  Vous verrez la liste des utilisateurs qui se sont connectés au serveur d’accès à distance au sein de l’heure et sélectionnées des statistiques détaillées à leur sujet. Cliquez sur la première ligne dans la liste. Lorsque vous sélectionnez une ligne, l’activité de l’utilisateur distant est indiquée dans le volet de visualisation. Sélectionnez maintenant le **Server charge statistiques** onglet dans le volet de visualisation pour afficher l’historique de la charge sur le serveur.  
  
    Cliquez sur le **Server charge statistiques** onglet dans le volet de visualisation pour afficher l’historique de la charge sur le serveur.  
  
> [!NOTE]  
> **Description des sessions**  
>   
> Gestion d’accès à distance est basée sur le concept de **sessions**. Contrairement à un **connexion**, un **session** est identifiée par une combinaison de nom d’utilisateur et l’adresse IP client distant. Par exemple, si un tunnel d’ordinateur est formé à partir du client à distance, nommé CLIENT1 et, une session sera créée et stockée dans la base de données de comptabilité. Lors de la passe d’un utilisateur nommé User1 se connecte à partir de ce client après un certain temps (mais le tunnel d’ordinateur est toujours actif), la session est enregistrée en tant qu’une session distincte. La distinction des sessions consiste à conserver la distinction entre le tunnel d’ordinateur et le tunnel de l’utilisateur.  
  
![Windows PowerShell](../../../media/Generate-a-usage-report-for-remote-clients-using-historical-data/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
Dans le script suivant, modifiez la plage de dates pour laquelle vous souhaitez qu’un rapport dans le **- StartDateTime** et **- EndDateTime** paramètres.  
  
```  
PS> Get-RemoteAccessConnectionStatisticsSummary -StartDateTime "1 October 2010 00:00:00" -EndDateTime "14 October 2010 00:00:00"  
Shows server load statistics.  
PS> Get-RemoteAccessUserActivity -HostIPAddress 10.0.0.1 -StartDateTime "1 October 2010 00:00:00" -EndDateTime "14 October 2010 00:00:00"  
```  
  


