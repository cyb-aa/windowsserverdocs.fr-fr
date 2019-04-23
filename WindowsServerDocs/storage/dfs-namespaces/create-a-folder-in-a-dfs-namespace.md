---
title: Créer un dossier dans un espace de noms DFS
description: Cet article décrit comment créer un dossier dans un espace de noms DFS.
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 47bb13aa404cdf4fef86b7250425a92cc208ba9d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856850"
---
# <a name="create-a-folder-in-a-dfs-namespace"></a>Créer un dossier dans un espace de noms DFS

> S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Vous pouvez utiliser des dossiers pour créer des niveaux hiérarchiques supplémentaires dans un espace de noms. Vous pouvez également créer des dossiers avec cibles de dossier pour ajouter des dossiers partagés dans l’espace de noms. Les dossiers DFS avec cibles de dossier ne peuvent pas contenir d’autres dossiers DFS. Par conséquent, si vous voulez ajouter un niveau hiérarchique dans l’espace de noms, n’ajoutez pas de cibles de dossier dans le dossier.

Pour créer un dossier dans un espace de noms à l’aide de Gestion DFS :

## <a name="to-create-a-folder-in-a-dfs-namespace"></a>Pour créer un dossier dans un espace de noms DFS

1.  Cliquez sur **Démarrer**, pointez sur **Outils d'administration**, puis cliquez sur **Gestion du système de fichiers distribués DFS**.

2.  Dans l’arborescence de la console, sous le nœud **Espaces de noms**, cliquez avec le bouton droit sur un espace de noms ou un dossier situé dans un espace de noms, puis cliquez sur **Nouveau dossier**.

3.  Dans la zone de texte **Nom**, tapez le nom du nouveau dossier.

4.  Pour ajouter une ou plusieurs cibles de dossier au dossier, cliquez sur **Ajouter**, indiquez le chemin d’accès UNC (Universal Naming Convention) de la cible de dossier, puis cliquez sur **OK**.


> [!TIP]
> Pour créer un dossier dans un espace de noms à l’aide de Windows PowerShell, utilisez l’[applet de commande New-DfsnFolder](https://docs.microsoft.com/powershell/module/dfsn/new-dfsnfolder). Le module Windows PowerShell DFSN a été introduit dans Windows Server 2012.


## <a name="see-also"></a>Voir aussi

-   [Déploiement d’espaces de noms DFS](deploying-dfs-namespaces.md)
-   [Déléguer des autorisations de gestion pour les espaces de noms DFS](delegate-management-permissions-for-dfs-namespaces.md)


