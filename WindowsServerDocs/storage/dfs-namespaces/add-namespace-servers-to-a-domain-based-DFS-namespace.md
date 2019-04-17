---
title: "Ajouter des serveurs d’espaces de noms à un espace de nomsDFS basé sur un domaine"
description: "Cet article explique comment spécifier des serveurs d’espaces de noms supplémentaires pour héberger un espace de noms à l’aide de la gestionDFS."
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 70ab3cac71f5766bc572015c6b23c0937e5252f0
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="add-namespace-servers-to-a-domain-based-dfs-namespace"></a>Ajouter des serveurs d’espaces de noms à un espace de nomsDFS basé sur un domaine

> S’applique à: WindowsServer (canal semi-annuel), WindowsServer2016, WindowsServer2012R2, WindowsServer2012, WindowsServer2008R2, WindowsServer2008

Vous pouvez augmenter la disponibilité d’un espace de noms basé sur un domaine en spécifiant des serveurs d’espaces de noms supplémentaires pour héberger l’espace de noms.

## <a name="to-add-a-namespace-server-to-a-domain-based-namespace"></a>Pour ajouter un serveur d’espace de noms à un espace de noms basé sur un domaine

Pour ajouter un serveur d’espace de noms à un espace de noms basé sur un domaine à l’aide de la gestionDFS, effectuez les étapes suivantes:

1.  Cliquez sur **Démarrer**, pointez sur **Outils d’administration**, puis cliquez sur **GestionDFS**.

2.  Dans l’arborescence de la console, sous le nœud **Espaces de noms**, cliquez avec le bouton droit sur un espace de noms basé sur un domaine, puis cliquez sur **Ajouter un serveur d’espaces de noms**.

3.  Entrez le chemin d’un autre serveur, ou cliquez sur **Parcourir** pour localiser un serveur.

> [!NOTE]
> Cette procédure n’est pas applicable pour les espaces de noms autonomes, car ils ne prennent en charge qu’un seul serveur d’espace de noms. Pour augmenter la disponibilité d’un espace de noms autonome, spécifiez un cluster de basculement comme serveur d’espace de noms dans l’Assistant Nouvel espace de noms.


> [!TIP]
> Pour ajouter un serveur d’espace de noms à l’aide de Windows PowerShell, utilisez l’[applet de commande New-DfsnRootTarget](https://docs.microsoft.com/powershell/module/dfsn/set-dfsnroottarget). Le module Windows PowerShell DFSN a été introduit dans WindowsServer2012.

## <a name="see-also"></a>Articles associés

-   [Déploiement d’espaces de nomsDFS](deploying-dfs-namespaces.md)
-   [Analyser la configuration requise pour un serveur d’espaces de nomsDFS](https://technet.microsoft.com/library/cc753448(v=ws.11).aspx)
-   [Créer un espace de nomsDFS](create-a-dfs-namespace.md)
-   [Déléguer les autorisations de gestion pour les espaces de nomsDFS](delegate-management-permissions-for-dfs-namespaces.md)

