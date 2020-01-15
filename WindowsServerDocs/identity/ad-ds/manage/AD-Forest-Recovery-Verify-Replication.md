---
title: Récupération de la forêt Active Directory-vérifier la réplication
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 302e522a-fb40-43bc-bc63-83dcc87ebde5
ms.technology: identity-adds
ms.openlocfilehash: f6bee5164849d6643c1744ce121b9ce91b5e7f7f
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949042"
---
# <a name="resources-to-verify-replication-is-working"></a>Ressources pour vérifier le bon fonctionnement de la réplication 

>S’applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

Une fois que vous avez restauré ou réinstallé tous les contrôleurs de contrôle, vous pouvez vérifier que AD DS et SYSVOL sont récupérés et répliqués correctement à l’aide de **repadmin/replsum.** , qui s’exécute sur n’importe quelle version de Windows Server.  
  
> [!TIP]
> Vous pouvez également télécharger et exécuter l' [outil Active Directory Replication Status](https://www.microsoft.com/download/details.aspx?id=30005) (ADReplStatus), un outil gratuit qui surveille l’état de la réplication des contrôleurs de compte et signale les erreurs. ADReplStatus requiert .NET Framework 4, qui sera installé s’il n’est pas déjà présent.  

Vérifiez le réplication DFS Connectez-vous observateur d’événements pour l’ID d’événement 4602 (ou l’ID d’événement du service de réplication de fichiers 13516), qui indique que SYSVOL a été initialisé.  

Si le premier contrôleur de domaine récupéré est l’ID d’événement 4614 («le contrôleur de domaine attend pour effectuer la réplication initiale. Le dossier répliqué restera dans l’état de synchronisation initial jusqu’à ce qu’il soit répliqué avec son partenaire» dans le journal réplication DFS, alors l’ID d’événement 4602 n’apparaît pas et vous devez effectuer les étapes manuelles suivantes pour récupérer SYSVOL s’il est répliqué par Member  

1. Lorsque l’événement DFSR 4612 apparaît sur le premier contrôleur de périphérique restauré, effectuez une restauration faisant autorité manuelle comme décrit dans [2218556 : comment forcer une synchronisation faisant autorité et ne faisant pas autorité pour le SYSVOL répliqué par DFSR (comme « D4/D2 » pour le service FRS)](https://support.microsoft.com/kb/2218556) (https://support.microsoft.com/kb/2218556).  
2. Définissez l' **indicateur SysvolReady** sur 1 manuellement, comme décrit dans [947022 le partage Netlogon n’est pas présent après l’installation de Active Directory Domain Services sur un nouveau contrôleur de domaine Windows Server 2008 complet ou en lecture seule](https://support.microsoft.com/kb/947022).  

Vous pouvez également créer un réplication DFS de rapport de diagnostic. Pour plus d’informations, consultez le guide pas à pas de [création d’un rapport de diagnostic pour réplication DFS](https://technet.microsoft.com/library/cc754227.aspx) et [DFS pour Windows Server 2008](https://technet.microsoft.com/library/cc732863\(WS.10\).aspx). Si le serveur exécute Windows Server 2008 R2, vous pouvez utiliser le [commutateur de ligne de commande Dfsrdiag. exe ReplicationState](https://blogs.technet.com/b/filecab/archive/2009/05/28/dfsrdiag-exe-replicationstate-what-s-dfsr-up-to.aspx).  

Vous pouvez également exécuter le test de réplication à l’aide de Dcdiag. exe pour rechercher les erreurs de réplication. Pour plus d’informations, consultez [l’article 249256](https://support.microsoft.com/kb/249256)de la base de connaissances.

## <a name="next-steps"></a>Étapes suivantes

- [Guide de récupération de la forêt Active Directory](AD-Forest-Recovery-Guide.md)
- [Récupération de la forêt Active Directory : procédures](AD-Forest-Recovery-Procedures.md)
