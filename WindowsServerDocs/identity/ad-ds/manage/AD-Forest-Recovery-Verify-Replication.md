---
title: "Forêt ActiveDirectory récupération - vérifier la réplication"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 302e522a-fb40-43bc-bc63-83dcc87ebde5
ms.technology: identity-adfs
ms.openlocfilehash: d85336a10e808755677a99777af8ecf5c07b05ab
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="resources-to-verify-replication-is-working"></a>Ressources pour vérifier la réplication fonctionne 

>S’applique à: Windows Server2016, Windows Server2012 et 2012R2, Windows Server2008 et 2008R2
 
 Une fois que vous avez restauré ou réinstallé sur tous les contrôleurs de domaine, vous pouvez vérifier que de domaine ActiveDirectory et SYSVOL sont récupérés et réplication correctement à l’aide de **repadmin /replsum**, qui s’exécute sur n’importe quelle version de Windows Server.  
  
> [!TIP]
>  Vous pouvez également télécharger et exécuter le [outil de l’état de la réplication ActiveDirectory](https://www.microsoft.com/download/details.aspx?id=30005) (ADReplStatus), un outil gratuit qui surveille l’état de réplication de contrôleurs de domaine et signale les erreurs. ADReplStatus requiert .NETFramework 4, qui sera installée si elle n’est pas déjà présent.  
  
 Vérifiez le journal de la réplication DFS dans l’Observateur d’événements 4602 d’ID d’événement (ou l’ID d’événement 13516 File Replication Service), ce qui indique SYSVOL a été initialisé avec succès.  
  
 Si récupéré le premier contrôleur de domaine enregistre l’événement ID4614 («le contrôleur de domaine est en attente pour effectuer la réplication initiale. Le dossier répliqué demeurera dans l’état de la synchronisation initiale jusqu'à ce qu’il a été répliqué avec son partenaire») dans le journal de la réplication DFS, puis l’ID d’événement 4602 n’apparaît pas, et vous devez effectuer les étapes manuelles suivantes pour récupérer SYSVOL si elle est répliquée par DFSR:  
  
1.  DFSR événement 4612 s’affiche sur le premier contrôleur de domaine restauré effectuer une restauration forcée manuelle comme décrit dans [2218556: comment forcer une synchronisation faisant autorité et ne faisant pas autorité pour SYSVOL répliqué DFSR (comme "D4/D2" pour le service FRS)](https://support.microsoft.com/kb/2218556) (https://support.microsoft.com/kb/2218556).  
  
2.  Définissez **SysvolReady indicateur** 1 manuellement, comme décrit dans [947022 partage l’accès réseau n’est pas présent après avoir installé les Services de domaine ActiveDirectory sur un contrôleur de domaine basé sur Windows Server2008 complète ou en lecture seule](https://support.microsoft.com/kb/947022).  
  
 Vous pouvez également créer un rapport de Diagnostics de la réplication DFS. Pour plus d’informations, voir [créer un rapport de Diagnostic pour la réplication DFS](https://technet.microsoft.com/library/cc754227.aspx) et [DFS Step-by-Step Guide pour Windows Server2008](https://technet.microsoft.com/library/cc732863\(WS.10\).aspx). Si le serveur exécute Windows Server2008R2, vous pouvez utiliser [commutateur de ligne de commande dfsrdiag.exe ReplicationState](http://blogs.technet.com/b/filecab/archive/2009/05/28/dfsrdiag-exe-replicationstate-what-s-dfsr-up-to.aspx).  
  
 Vous pouvez également exécuter le test de réplications à l’aide de dcdiag.exe pour vérifier les erreurs de réplication. Pour plus d’informations, consultez la Base de connaissances [article 249256](https://support.microsoft.com/kb/249256).

## <a name="next-steps"></a>Étapes suivantes

- [Guide de récupération de forêt ActiveDirectory](AD-Forest-Recovery-Guide.md)
- [Récupération de la forêt ActiveDirectory - procédures](AD-Forest-Recovery-Procedures.md)
