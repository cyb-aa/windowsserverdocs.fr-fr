---
title: "Répliquer des cibles de dossier à l’aide de la réplicationDFS"
description: "Cet article décrit comment répliquer des cibles de dossier à l’aide de la réplicationDFS"
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: ef13c338d8b13c24a02efb0468f06d5fef803cea
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="replicate-folder-targets-using-dfs-replication"></a>Répliquer des cibles de dossier à l’aide de la réplicationDFS

> S’applique à: WindowsServer (canal semi-annuel), WindowsServer2016, WindowsServer2012R2, WindowsServer2012, WindowsServer2008R2 et WindowsServer2008

Vous pouvez utiliser la réplicationDFS pour assurer la synchronisation du contenu des cibles de dossier afin que les utilisateurs voient les mêmes fichiers, quelle que soit la cible de dossier référencée pour l’ordinateur client.

## <a name="to-replicate-folder-targets-using-dfs-replication"></a>Pour répliquer des cibles de dossier à l’aide de la réplicationDFS

1.  Cliquez sur **Démarrer**, pointez sur **Outils d’administration**, puis cliquez sur **GestionDFS**.

2.  Dans l’arborescence de la console, sous le nœud **Espaces de noms**, cliquez avec le bouton droit sur un dossier doté de deux cibles de dossier ou plus, puis cliquez sur **Répliquer un dossier**.

3.  Suivez les instructions affichées dans l’Assistant Réplication de dossier.

> [!NOTE]
> Les modifications de configuration ne sont pas appliquées immédiatement à tous les membres, sauf lorsque vous utilisez les appels de commande [Suspend-DfsReplicationGroup](https://technet.microsoft.com/itpro/powershell/windows/dfsr/suspend-dfsreplicationgroup) et [Sync-DfsReplicationGroup](https://technet.microsoft.com/itpro/powershell/windows/dfsr/sync-dfsreplicationgroup). La nouvelle configuration doit être répliquée sur tous les contrôleurs de domaine, et chaque membre du groupe de réplication doit interroger son contrôleur de domaine le plus proche pour obtenir les modifications. Le temps qui y est consacré dépend de la latence de réplication Active Directory Directory Services (ADDS) et du long intervalle d’interrogation (60minutes) sur chaque membre. Pour demander immédiatement les modifications de configuration, ouvrez une fenêtre d’invite de commandes et tapez la commande suivante une fois pour chaque membre du groupe de réplication: <br /> dfsrdiag.exe PollAD /Member:DOMAIN\Server1
<br />
Pour effectuer cette opération à partir d’une session WindowsPowerShell, utilisez l’applet de commande [Update-DfsrConfigurationFromAD](https://technet.microsoft.com/itpro/powershell/windows/dfsr/update-dfsrconfigurationfromad), qui a été introduite dans WindowsServer2012R2.

## <a name="see-also"></a>Articles associés

-   [Déploiement d’espaces de nomsDFS](deploying-dfs-namespaces.md)
-   [Déléguer les autorisations de gestion pour les espaces de nomsDFS](delegate-management-permissions-for-dfs-namespaces.md)
-   [Réplication](https://technet.microsoft.com/library/cc770278(v=ws.11).aspx)