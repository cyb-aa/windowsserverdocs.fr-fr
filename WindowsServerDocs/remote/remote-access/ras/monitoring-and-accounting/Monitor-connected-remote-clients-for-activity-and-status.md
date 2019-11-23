---
title: Analyser l'activité et l'état des clients distants connectés
description: Cette rubrique fait partie du Guide d’analyse et de gestion de l’accès à distance dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: beb94475-b21f-46a9-ac51-bf2bb28ca94e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 03d87fb086a9f2797af8399be3d833b11bed79a5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367259"
---
# <a name="monitor-connected-remote-clients-for-activity-and-status"></a>Analyser l'activité et l'état des clients distants connectés

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

**Remarque :** Windows Server 2012 combine DirectAccess et le service d’accès à distance (RAS) dans un seul rôle d’accès à distance.  
  
Vous pouvez utiliser la console de gestion sur le serveur d’accès à distance pour surveiller l’activité et l’état des clients distants.  
  
> [!NOTE]  
> Vous devez être connecté en tant que membre du groupe Admins du domaine ou membre du groupe Administrateurs sur chaque ordinateur pour effectuer les tâches décrites dans cette rubrique. Si vous ne pouvez pas effectuer une tâche lorsque vous êtes connecté avec un compte membre du groupe administrateurs, essayez d’exécuter la tâche lorsque vous êtes connecté avec un compte membre du groupe Admins du domaine.  
  
#### <a name="to-monitor-remote-client-activity-and-status"></a>Pour surveiller l’activité et l’état des clients distants  
  
1.  Dans **Gestionnaire de serveur**, cliquez sur **Outils**, puis sur **Gestion de l'accès à distance**.  
  
2.  Cliquez sur **rapports** pour accéder à **rapports d’accès à distance** dans la console de gestion de l' **accès à distance**.  
  
3.  Cliquez sur **État du client distant** pour accéder à l’interface utilisateur de l’activité et de l’état du client distant dans la console de gestion de l' **accès à distance**.  
  
4.  Vous verrez la liste des utilisateurs qui sont connectés au serveur d’accès à distance et des statistiques détaillées à leur sujet. Cliquez sur la première ligne de la liste qui correspond à un client. Lorsque vous sélectionnez une ligne, l’activité de l’utilisateur distant s’affiche dans le volet de visualisation.  
  
![les commandes Windows PowerShell](../../../media/Monitor-connected-remote-clients-for-activity-and-status/PowerShellLogoSmall.gif)***<em>équivalentes</em> Windows PowerShell***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```  
PS> Get-RemoteAccessConnectionStatistics  
```  
  
Les statistiques utilisateur peuvent être filtrées en fonction des sélections de critères, à l’aide des champs du tableau suivant.  
  
|Nom du champ|Valeur|  
|-------|-----|  
|Nom d'utilisateur|Nom de l’utilisateur ou alias de l’utilisateur distant. Les caractères génériques peuvent être utilisés pour sélectionner un groupe d’utilisateurs, par exemple Contoso\\* ou \*\administrator.|  
|Hostname|Nom du compte d’ordinateur de l’utilisateur distant. Une adresse IPv4 ou IPv6 peut également être spécifiée.|  
|Type|DirectAccess ou VPN. Si DirectAccess est sélectionné, tous les utilisateurs distants qui sont connectés à l’aide de DirectAccess sont répertoriés. Si VPN est sélectionné, tous les utilisateurs distants qui sont connectés à l’aide d’un VPN sont répertoriés.|  
|Adresse FAI|Adresse IPv4 ou IPv6 de l’utilisateur distant.|  
|Adresse IPv4|Adresse IPv4 interne du tunnel qui connecte l’utilisateur distant au réseau d’entreprise.|  
|Adresse IPv6|Adresse IPv6 interne du tunnel connectant l’utilisateur distant au réseau d’entreprise.|  
|Protocole/Tunnel|Technologie de transition utilisée par le client distant. Il s’agit de Teredo, 6to4 ou IP-HTTPs pour les utilisateurs DirectAccess, et il s’agit de PPTP, L2TP, SSTP ou IKEv2 pour les utilisateurs VPN.|  
|Ressource ayant fait l’objet d’un accès|Tous les utilisateurs accédant à un point de terminaison particulier ou à une ressource d’entreprise particulière. La valeur qui correspond à ce champ est le nom d’hôte/l’adresse IP du serveur.|  
|Server|Serveur d’accès à distance auquel les clients sont connectés. Convient uniquement pour les déploiements de cluster et multisites.|  
  
  
  


