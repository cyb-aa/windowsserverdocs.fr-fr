---
title: Ajouter des cibles de dossier
description: "Cette rubrique explique comment ajouter des cibles de dossier (chemins d’accès UNC)"
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: brianlic
ms.technology: storage
ms.topic: article
author: jasongerend
ms-date: 06/05/2017
ms.openlocfilehash: 71305089553d622f54cb4e5608034edfaf7abc5f
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="add-folder-targets"></a>Ajouter des cibles de dossier

> S’applique à: WindowsServer (canal semi-annuel), WindowsServer2016, WindowsServer2012R2, WindowsServer2012, WindowsServer2008R2, WindowsServer2008

Une cible de dossier est le chemin d’accès UNC (Universal Naming Convention) d’un dossier partagé ou d’un autre espace de noms associé à un dossier dans un espace de noms. L’ajout de plusieurs cibles de dossier augmente la disponibilité du dossier dans l’espace de noms.

## <a name="to-add-a-folder-target"></a>Pour ajouter une cible de dossier

Pour ajouter une cible de dossier à l’aide de la gestionDFS, effectuez les étapes suivantes:

1.  Cliquez sur **Démarrer**, pointez sur **Outils d’administration**, puis cliquez sur **GestionDFS**.

2.  Dans l’arborescence de la console, sous le nœud **Espaces de noms**, cliquez avec le bouton droit sur un dossier avec cibles, puis cliquez sur **Ajouter une cible de dossier**.

3.  Tapez le chemin de la cible de dossier, ou cliquez sur **Parcourir** pour la localiser.

4.  Si le dossier est répliqué à l’aide de la réplicationDFS, vous pouvez indiquer si vous souhaitez ajouter la nouvelle cible de dossier au groupe de réplication.

> [!TIP]
> Pour ajouter une cible de dossier à l’aide de Windows PowerShell, utilisez l’applet de commande [New-DfsnFolderTarget](https://docs.microsoft.com/powershell/module/dfsn/new-dfsnfoldertarget). Le module Windows PowerShell DFSN a été introduit dans WindowsServer2012.

> [!NOTE]
> Les dossiers peuvent contenir des cibles de dossier ou d’autres dossiersDFS, mais pas les deux, au même niveau dans l’arborescence des dossiers.

## <a name="see-also"></a>Articles associés

-   [Déploiement d’espaces de nomsDFS](deploying-dfs-namespaces.md)
-   [Déléguer les autorisations de gestion pour les espaces de nomsDFS](delegate-management-permissions-for-dfs-namespaces.md)
-   [Répliquer des cibles de dossier à l’aide de la réplicationDFS](replicate-folder-targets-using-dfs-replication.md)