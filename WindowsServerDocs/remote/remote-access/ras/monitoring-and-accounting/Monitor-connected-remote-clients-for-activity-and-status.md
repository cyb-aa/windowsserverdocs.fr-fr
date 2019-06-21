---
title: Analyser l'activité et l'état des clients distants connectés
description: Cette rubrique fait partie du guide de la surveillance de l’accès à distance et la gestion des comptes dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: beb94475-b21f-46a9-ac51-bf2bb28ca94e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bf7aed239eb56eae599078a6088cbb2971c45821
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282740"
---
# <a name="monitor-connected-remote-clients-for-activity-and-status"></a>Analyser l'activité et l'état des clients distants connectés

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

**Remarque :** Windows Server 2012 associe DirectAccess et le Service d’accès à distance (RAS) en un seul rôle accès à distance.  
  
Vous pouvez utiliser la console de gestion sur le serveur d’accès à distance pour contrôler l’état et l’activité des clients distants.  
  
> [!NOTE]  
> Vous devez être connecté en tant que membre du groupe Admins du domaine ou un membre du groupe Administrateurs sur chaque ordinateur pour effectuer les tâches décrites dans cette rubrique. Si vous ne pouvez pas effectuer une tâche pendant que vous êtes connecté avec un compte qui est membre du groupe Administrateurs, essayez d’effectuer la tâche pendant que vous êtes connecté avec un compte qui est membre du groupe Admins du domaine.  
  
#### <a name="to-monitor-remote-client-activity-and-status"></a>Pour surveiller l’état et l’activité des clients distants  
  
1.  Dans **Gestionnaire de serveur**, cliquez sur **Outils**, puis sur **Gestion de l'accès à distance**.  
  
2.  Cliquez sur **REPORTING** pour accéder à **rapports l’accès à distance** dans le **Console de gestion des accès à distance**.  
  
3.  Cliquez sur **statut du Client distant** pour accéder à l’interface client à distance activité et l’état utilisateur dans le **Console de gestion des accès à distance**.  
  
4.  Vous verrez la liste des utilisateurs qui sont connectés au serveur d’accès à distance et détaillée des statistiques les concernant. Cliquez sur la première ligne dans la liste qui correspond à un client. Lorsque vous sélectionnez une ligne, l’activité de l’utilisateur distant est indiquée dans le volet de visualisation.  
  
![Windows PowerShell](../../../media/Monitor-connected-remote-clients-for-activity-and-status/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```  
PS> Get-RemoteAccessConnectionStatistics  
```  
  
Statistiques de l’utilisateur peuvent être filtrés selon les critères sélectionnés, en utilisant les champs dans le tableau suivant.  
  
|Nom du champ|Value|  
|-------|-----|  
|Nom d'utilisateur|Nom de l’utilisateur ou alias de l’utilisateur distant. Caractères génériques peuvent être utilisés pour sélectionner un groupe d’utilisateurs, tel que contoso\\* ou \*\administrator.|  
|Hostname|Nom du compte d’ordinateur de l’utilisateur distant. Une adresse IPv4 ou IPv6 peut également être spécifiée.|  
|type|DirectAccess ou VPN. Si DirectAccess est sélectionné, tous les utilisateurs distants qui sont connectés à l’aide de DirectAccess sont répertoriés. Si VPN est sélectionné, tous les utilisateurs distants qui sont connectés à l’aide de VPN sont répertoriés.|  
|Adresse FAI|Adresse IPv4 ou IPv6 de l’utilisateur distant.|  
|Adresse IPv4|L’adresse IPv4 interne du tunnel qui se connectent de l’utilisateur distant au réseau d’entreprise.|  
|Adresse IPv6|Adresse IPv6 interne du tunnel connectant l’utilisateur distant au réseau d’entreprise.|  
|Protocole/Tunnel|Technologie de transition qui est utilisée par le client distant. Il s’agit Teredo, 6to4 ou IP-HTTPS pour les utilisateurs de DirectAccess, et il est PPTP, L2TP, SSTP ou IKEv2 pour les utilisateurs VPN.|  
|Ressource ayant fait l’objet d’un accès|Tous les utilisateurs accédant à un point de terminaison particulier ou à une ressource d’entreprise particulière. La valeur qui correspond à ce champ est le nom d’hôte/adresse IP du serveur.|  
|Server|Serveur d’accès à distance auquel les clients sont connectés. Convient uniquement pour les déploiements de cluster et multisites.|  
  
  
  


