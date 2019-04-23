---
title: Créer un espace de noms DFS
description: Cet article explique comment créer un espace de noms DFS.
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 4256e124e75be72f94cbd35c182edfe38e92bc90
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847500"
---
# <a name="create-a-dfs-namespace"></a>Créer un espace de noms DFS

> S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Pour créer un nouvel espace de noms, vous pouvez utiliser le Gestionnaire de serveur pour créer l’espace de noms lorsque vous installez le service de rôle des espaces de noms DFS. Vous pouvez également utiliser l’[applet de commande New-DfsnRoot](https://docs.microsoft.com/powershell/module/dfsn/new-dfsnroot) à partir d’une session Windows PowerShell. 

Le module Windows PowerShell DFSN a été introduit dans Windows Server 2012. 

Vous pouvez également créer un espace de noms à l’aide de la procédure suivante après avoir installé le service de rôle.

## <a name="to-create-a-namespace"></a>Pour créer un espace de noms :

1.  Cliquez sur **Démarrer**, pointez sur **Outils d'administration**, puis cliquez sur **Gestion du système de fichiers distribués DFS**.

2.  Dans l’arborescence de la console, cliquez avec le bouton droit sur le nœud **Espaces de noms**, puis cliquez sur **Nouvel espace de noms**.

3.  Suivez les instructions affichées dans l’**Assistant Nouvel espace de noms**.

    Pour créer un espace de noms autonome sur un cluster de basculement, spécifiez le nom d’une instance de serveur de fichiers en cluster sur la page **Serveur d’espaces de noms** de l’**Assistant Nouvel espace de noms**.

> [!IMPORTANT]
> N’essayez pas de créer un espace de noms basés sur un domaine à l’aide du mode Windows Server 2008, sauf si le niveau fonctionnel de forêt est Windows Server 2003 ou version ultérieure. Cela peut entraîner un espace de noms pour lequel vous ne pouvez pas supprimer les dossiers DFS, générant le message d’erreur suivant : « Impossible de supprimer le dossier. Impossible d’accomplir cette fonction. »

## <a name="see-also"></a>Voir aussi

-   [Déploiement d’espaces de noms DFS](deploying-dfs-namespaces.md)
-   [Choisissez un Type Namespace](choose-a-namespace-type.md)
-   [Ajouter des serveurs de Namespace à un Namespace DFS de domaine](add-namespace-servers-to-a-domain-based-dfs-namespace.md)
-   [Déléguer les autorisations de gestion pour les espaces de noms DFS](delegate-management-permissions-for-dfs-namespaces.md).


