---
title: Ajouter des cibles de dossier
description: Cette rubrique explique comment ajouter des cibles de dossier (chemins d’accès UNC)
ms.prod: windows-server
ms.author: jgerend
manager: brianlic
ms.technology: storage
ms.topic: article
author: jasongerend
ms-date: 06/05/2017
ms.openlocfilehash: d2f3845a612556a51692aaf51d256bbedd518e7a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854102"
---
# <a name="add-folder-targets"></a>Ajouter des cibles de dossier

> S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008

Une cible de dossier est le chemin d’accès UNC (Universal Naming Convention) d’un dossier partagé ou d’un autre espace de noms associé à un dossier dans un espace de noms. L’ajout de plusieurs cibles de dossier augmente la disponibilité du dossier dans l’espace de noms.

## <a name="to-add-a-folder-target"></a>Pour ajouter une cible de dossier

Pour ajouter une cible de dossier à l’aide de la gestion DFS, effectuez les étapes suivantes :

1.  Cliquez sur **Démarrer**, pointez sur **Outils d'administration**, puis cliquez sur **Gestion du système de fichiers distribués DFS**.

2.  Dans l’arborescence de la console, sous le nœud **Espaces de noms**, cliquez avec le bouton droit sur un dossier avec cibles, puis cliquez sur **Ajouter une cible de dossier**.

3.  Tapez le chemin de la cible de dossier, ou cliquez sur **Parcourir** pour la localiser.

4.  Si le dossier est répliqué à l’aide de la réplication DFS, vous pouvez indiquer si vous souhaitez ajouter la nouvelle cible de dossier au groupe de réplication.

> [!TIP]
> Pour ajouter une cible de dossier à l’aide de Windows PowerShell, utilisez l’applet de commande [New-DfsnFolderTarget](https://docs.microsoft.com/powershell/module/dfsn/new-dfsnfoldertarget). Le module Windows PowerShell DFSN a été introduit dans Windows Server 2012.

> [!NOTE]
> Les dossiers peuvent contenir des cibles de dossier ou d’autres dossiers DFS, mais pas les deux, au même niveau dans l’arborescence des dossiers.

## <a name="see-also"></a>Voir aussi

-   [Déploiement d’espaces de noms DFS](deploying-dfs-namespaces.md)
-   [Déléguer les autorisations de gestion pour les espaces de noms DFS](delegate-management-permissions-for-dfs-namespaces.md)
-   [Répliquer les cibles de dossier à l’aide de réplication DFS](replicate-folder-targets-using-dfs-replication.md)