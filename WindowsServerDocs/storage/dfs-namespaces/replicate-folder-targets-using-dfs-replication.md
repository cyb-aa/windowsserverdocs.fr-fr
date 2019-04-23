---
title: Répliquer des cibles de dossier à l’aide de la réplication DFS
description: Cet article décrit comment répliquer des cibles de dossier à l’aide de la réplication DFS
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 098ffd1a47891d2355742682a002f80dcfc59650
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878080"
---
# <a name="replicate-folder-targets-using-dfs-replication"></a>Répliquer des cibles de dossier à l’aide de la réplication DFS

> S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008

Vous pouvez utiliser la réplication DFS pour assurer la synchronisation du contenu des cibles de dossier afin que les utilisateurs voient les mêmes fichiers, quelle que soit la cible de dossier référencée pour l’ordinateur client.

## <a name="to-replicate-folder-targets-using-dfs-replication"></a>Pour répliquer des cibles de dossier à l’aide de la réplication DFS

1.  Cliquez sur **Démarrer**, pointez sur **Outils d'administration**, puis cliquez sur **Gestion du système de fichiers distribués DFS**.

2.  Dans l’arborescence de la console, sous le nœud **Espaces de noms**, cliquez avec le bouton droit sur un dossier doté de deux cibles de dossier ou plus, puis cliquez sur **Répliquer un dossier**.

3.  Suivez les instructions affichées dans l’Assistant Réplication de dossier.

> [!NOTE]
> Les modifications de configuration ne sont pas appliquées immédiatement à tous les membres, sauf lorsque vous utilisez les appels de commande [Suspend-DfsReplicationGroup](https://technet.microsoft.com/itpro/powershell/windows/dfsr/suspend-dfsreplicationgroup) et [Sync-DfsReplicationGroup](https://technet.microsoft.com/itpro/powershell/windows/dfsr/sync-dfsreplicationgroup). La nouvelle configuration doit être répliquée sur tous les contrôleurs de domaine, et chaque membre du groupe de réplication doit interroger son contrôleur de domaine le plus proche pour obtenir les modifications. La quantité de cette opération dépend de la latence de réplication Active Directory Directory Services (AD DS) et du long intervalle d’interrogation (60 minutes) sur chaque membre. Pour demander immédiatement les modifications de configuration, ouvrez une fenêtre d’invite de commandes et tapez la commande suivante une fois pour chaque membre du groupe de réplication : <br /> dfsrdiag.exe PollAD /Member:DOMAIN\Server1
<br />
Pour ce faire, à partir d’une session Windows PowerShell, utilisez le [DfsrConfigurationFromAD de mise à jour](https://technet.microsoft.com/itpro/powershell/windows/dfsr/update-dfsrconfigurationfromad) applet de commande, qui a été introduite dans Windows Server 2012 R2.

## <a name="see-also"></a>Voir aussi

-   [Déploiement d’espaces de noms DFS](deploying-dfs-namespaces.md)
-   [Déléguer des autorisations de gestion pour les espaces de noms DFS](delegate-management-permissions-for-dfs-namespaces.md)
-   [Réplication DFS](../dfs-replication/dfsr-overview.md)