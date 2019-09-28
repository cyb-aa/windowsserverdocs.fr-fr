---
title: Ajouter des serveurs d’espaces de noms à un espace de noms DFS basé sur un domaine
description: Cet article explique comment spécifier des serveurs d’espaces de noms supplémentaires pour héberger un espace de noms à l’aide de la gestion DFS.
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: fa28ec06448b67724ce8e3f29894fc70f6a6dae9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386247"
---
# <a name="add-namespace-servers-to-a-domain-based-dfs-namespace"></a>Ajouter des serveurs d’espaces de noms à un espace de noms DFS basé sur un domaine

> S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008

Vous pouvez augmenter la disponibilité d’un espace de noms basé sur un domaine en spécifiant des serveurs d’espaces de noms supplémentaires pour héberger l’espace de noms.

## <a name="to-add-a-namespace-server-to-a-domain-based-namespace"></a>Pour ajouter un serveur d’espace de noms à un espace de noms basé sur un domaine

Pour ajouter un serveur d’espace de noms à un espace de noms basé sur un domaine à l’aide de la gestion DFS, effectuez les étapes suivantes :

1.  Cliquez sur **Démarrer**, pointez sur **Outils d'administration**, puis cliquez sur **Gestion du système de fichiers distribués DFS**.

2.  Dans l’arborescence de la console, sous le nœud **Espaces de noms**, cliquez avec le bouton droit sur un espace de noms basé sur un domaine, puis cliquez sur **Ajouter un serveur d’espaces de noms**.

3.  Entrez le chemin d’un autre serveur, ou cliquez sur **Parcourir** pour localiser un serveur.

> [!NOTE]
> Cette procédure n’est pas applicable pour les espaces de noms autonomes, car ils ne prennent en charge qu’un seul serveur d’espace de noms. Pour augmenter la disponibilité d’un espace de noms autonome, spécifiez un cluster de basculement comme serveur d’espace de noms dans l’Assistant Nouvel espace de noms.


> [!TIP]
> Pour ajouter un serveur d’espace de noms à l’aide de Windows PowerShell, utilisez l’[applet de commande New-DfsnRootTarget](https://docs.microsoft.com/powershell/module/dfsn/new-dfsnroottarget). Le module Windows PowerShell DFSN a été introduit dans Windows Server 2012.

## <a name="see-also"></a>Voir aussi

-   [Déploiement d’espaces de noms DFS](deploying-dfs-namespaces.md)
-   [Examiner la configuration requise du serveur d’espaces de noms DFS](https://technet.microsoft.com/library/cc753448(v=ws.11).aspx)
-   [Créer un espace de noms DFS](create-a-dfs-namespace.md)
-   [Déléguer les autorisations de gestion pour les espaces de noms DFS](delegate-management-permissions-for-dfs-namespaces.md)

