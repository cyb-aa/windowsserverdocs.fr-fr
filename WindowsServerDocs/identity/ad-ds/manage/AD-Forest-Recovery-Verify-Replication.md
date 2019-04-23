---
title: La forêt Active Directory récupération - vérification de la réplication
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 302e522a-fb40-43bc-bc63-83dcc87ebde5
ms.technology: identity-adds
ms.openlocfilehash: fb05586f281460dc2c7a1afea4c0423493e3fc46
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878310"
---
# <a name="resources-to-verify-replication-is-working"></a>Ressources pour vérifier l’utilisation de la réplication 

>S'applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

Une fois que vous avez restauré ou réinstallé sur tous les contrôleurs de domaine, vous pouvez vérifier que les services AD DS et SYSVOL sont récupérés et réplication correctement à l’aide de **repadmin /replsum**, qui s’exécute sur n’importe quelle version de Windows Server.  
  
> [!TIP]
> Vous pouvez également télécharger et exécuter le [outil État de réplication Active Directory](https://www.microsoft.com/download/details.aspx?id=30005) (ADReplStatus), un outil gratuit qui surveille l’état de réplication de contrôleurs de domaine et signale les erreurs. ADReplStatus nécessite .NET Framework 4, qui sera installé si elle n’est pas déjà présent.  

Consultez le journal de la réplication DFS dans l’Observateur d’événements pour 4602 d’ID d’événement (ou l’ID d’événement 13516 File Replication Service), ce qui indique le que SYSVOL a été initialisé.  

Si récupéré le premier contrôleur de domaine consigne 4614 d’ID d’événement (« le contrôleur de domaine est en attente pour effectuer la réplication initiale. Le dossier répliqué restera dans l’état de synchronisation initial jusqu'à ce qu’il a été répliqué avec son partenaire ») dans le journal de la réplication DFS, puis l’événement ID 4602 n’apparaît pas et vous devez effectuer manuellement les étapes suivantes pour récupérer le SYSVOL si elle est répliquée par DFSR :  

1. Lorsque 4612 d’événement DFSR apparaît sur le premier contrôleur de domaine restauré effectuer une restauration forcée manuelle comme décrit dans [2218556 : Comment forcer une synchronisation faisant autoritée et ne faisant pas autorité pour SYSVOL de réplication DFSR (comme « D4/D2 » pour le service FRS)](https://support.microsoft.com/kb/2218556) (https://support.microsoft.com/kb/2218556).  
2. Définissez **SysvolReady indicateur** 1 manuellement, comme décrit dans [947022 partage NETLOGON le n’est pas présent après avoir installé les Services de domaine Active Directory sur un contrôleur de domaine complet ou en lecture seule basé sur Windows Server 2008 ](https://support.microsoft.com/kb/947022).  

Vous pouvez également créer un rapport de Diagnostics de la réplication DFS. Pour plus d’informations, consultez [créer un rapport de Diagnostic pour la réplication DFS](https://technet.microsoft.com/library/cc754227.aspx) et [Guide de pas à pas de DFS pour Windows Server 2008](https://technet.microsoft.com/library/cc732863\(WS.10\).aspx). Si le serveur exécute Windows Server 2008 R2, vous pouvez utiliser [commutateur de ligne de commande dfsrdiag.exe ReplicationState](http://blogs.technet.com/b/filecab/archive/2009/05/28/dfsrdiag-exe-replicationstate-what-s-dfsr-up-to.aspx).  

Vous pouvez également exécuter le test de réplications à l’aide de dcdiag.exe pour rechercher les erreurs de réplication. Pour plus d’informations, consultez la Base de connaissances [article 249256](https://support.microsoft.com/kb/249256).

## <a name="next-steps"></a>Étapes suivantes

- [Guide de la forêt Active Directory](AD-Forest-Recovery-Guide.md)
- [Récupération de forêt AD - procédures](AD-Forest-Recovery-Procedures.md)
