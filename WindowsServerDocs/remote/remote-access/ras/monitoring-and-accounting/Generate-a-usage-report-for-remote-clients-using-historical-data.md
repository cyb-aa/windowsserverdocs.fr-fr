---
title: Générer un rapport d'utilisation pour les clients distants à l'aide de données d'historique
description: Cette rubrique fait partie du Guide d’analyse et de gestion de l’accès à distance dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0305467b-ce39-4532-a05a-2cc5ff946f55
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bae50345e8a6fd4018857e2a754d0274ce02855d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367255"
---
# <a name="generate-a-usage-report-for-remote-clients-using-historical-data"></a>Générer un rapport d'utilisation pour les clients distants à l'aide de données d'historique

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

**Remarque :** Windows Server 2012 associe DirectAccess et le service Routage et accès distant (RRAS) dans un rôle Accès à distance unique.  
  
La console de gestion sur le serveur d’accès à distance peut être utilisée pour générer un rapport d’utilisation pour les clients distants qui accèdent au serveur. Pour générer un rapport d’utilisation pour les clients distants, vous devez d’abord activer la gestion des comptes sur le serveur d’accès à distance. Une fois le rapport généré, vous pouvez utiliser le tableau de bord de surveillance qui est disponible dans la console de gestion sur le serveur d’accès à distance pour afficher les statistiques de charge sur le serveur.  
  
> [!NOTE]  
> Vous devez être connecté en tant que membre du groupe Admins du domaine ou membre du groupe Administrateurs sur chaque ordinateur pour effectuer les tâches décrites dans cette rubrique. Si vous ne pouvez pas effectuer une tâche lorsque vous êtes connecté avec un compte membre du groupe administrateurs, essayez d’exécuter la tâche lorsque vous êtes connecté avec un compte membre du groupe Admins du domaine.  
  
#### <a name="to-enable-accounting-on-the-remote-access-server"></a>Pour activer la gestion des comptes sur le serveur d’accès à distance  
  
1.  Dans **Gestionnaire de serveur**, cliquez sur **Outils**, puis sur **Gestion de l'accès à distance**.  
  
2.  Cliquez sur **rapports** pour accéder à **rapports d’accès à distance** dans la console de gestion de l' **accès à distance**.  
  
3.  Cliquez sur **configurer la gestion des comptes** dans le volet de tâches rapports d' **accès à distance** .  
  
4.  Activez la case à cocher **utiliser la gestion des comptes** de boîte de réception pour activer la gestion des comptes sur le serveur d’accès à distance.  
  
5.  Cliquez sur **appliquer** pour activer la configuration de la gestion des comptes sur le serveur, puis cliquez sur **Fermer** une fois que le serveur a correctement appliqué la configuration.  
  
#### <a name="to-generate-the-usage-report"></a>Pour générer le rapport d’utilisation  
  
1.  Dans **Gestionnaire de serveur**, cliquez sur **Outils**, puis sur **Gestion de l'accès à distance**.  
  
2.  Cliquez sur **rapports** pour accéder à **rapports d’accès à distance** dans la console de gestion de l' **accès à distance**.  
  
3.  Dans le volet central, cliquez sur dates dans le calendrier pour sélectionner le rapport **Date de début** de la durée : et **Date de fin :** , puis cliquez sur **générer le rapport**.  
  
4.  Vous verrez la liste des utilisateurs qui se sont connectés au serveur d’accès à distance dans l’intervalle de temps sélectionné et les statistiques détaillées les concernant. Cliquez sur la première ligne de la liste. Lorsque vous sélectionnez une ligne, l’activité de l’utilisateur distant s’affiche dans le volet de visualisation. Sélectionnez maintenant l’onglet **statistiques de chargement du serveur** dans le volet de visualisation pour afficher la charge historique sur le serveur.  
  
    Cliquez sur l’onglet **statistiques de chargement du serveur** dans le volet de visualisation pour afficher la charge historique sur le serveur.  
  
> [!NOTE]  
> **Fonctionnement des sessions**  
>   
> La gestion de comptes d’accès à distance est basée sur le concept de **sessions**. Contrairement à une **connexion**, une **session** est identifiée de manière unique par une combinaison de l’adresse IP et du nom d’utilisateur du client distant. Par exemple, si un tunnel d’ordinateur est formé à partir du client distant, appelé CLIENT1, une session est créée et stockée dans la base de données de comptabilité. Quand un utilisateur nommé User1 se connecte à partir de ce client après un certain temps (mais que le tunnel d’ordinateur est toujours actif), la session est enregistrée en tant que session distincte. La distinction entre les sessions consiste à conserver la distinction entre le tunnel de l’ordinateur et le tunnel de l’utilisateur.  
  
![les commandes Windows PowerShell](../../../media/Generate-a-usage-report-for-remote-clients-using-historical-data/PowerShellLogoSmall.gif)***<em>équivalentes</em> Windows PowerShell***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
Dans le script suivant, modifiez la plage de dates pour laquelle vous souhaitez un rapport dans les paramètres **-StartDateTime** et **-EndDateTime** .  
  
```  
PS> Get-RemoteAccessConnectionStatisticsSummary -StartDateTime "1 October 2010 00:00:00" -EndDateTime "14 October 2010 00:00:00"  
Shows server load statistics.  
PS> Get-RemoteAccessUserActivity -HostIPAddress 10.0.0.1 -StartDateTime "1 October 2010 00:00:00" -EndDateTime "14 October 2010 00:00:00"  
```  
  


