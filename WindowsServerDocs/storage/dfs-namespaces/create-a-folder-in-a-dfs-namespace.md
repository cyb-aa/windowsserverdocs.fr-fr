---
title: "Créer un dossier dans un espace de nomsDFS"
description: "Cet article décrit comment créer un dossier dans un espace de nomsDFS."
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 1ca9fa0b87c6e995f3f0c38abec80fef9068df90
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="create-a-folder-in-a-dfs-namespace"></a>Créer un dossier dans un espace de nomsDFS

> S’applique à: WindowsServer (canal semi-annuel), WindowsServer2016, WindowsServer2012R2, WindowsServer2012, WindowsServer2008R2, WindowsServer2008

Vous pouvez utiliser des dossiers pour créer des niveaux hiérarchiques supplémentaires dans un espace de noms. Vous pouvez également créer des dossiers avec cibles de dossier pour ajouter des dossiers partagés dans l’espace de noms. Les dossiersDFS avec cibles de dossier ne peuvent pas contenir d’autres dossiersDFS. Par conséquent, si vous voulez ajouter un niveau hiérarchique dans l’espace de noms, n’ajoutez pas de cibles de dossier dans le dossier.

Pour créer un dossier dans un espace de noms à l’aide de Gestion DFS:

## <a name="to-create-a-folder-in-a-dfs-namespace"></a>Pour créer un dossier dans un espace de nomsDFS

1.  Cliquez sur **Démarrer**, pointez sur **Outils d’administration**, puis cliquez sur **Gestion DFS**.

2.  Dans l’arborescence de la console, sous le nœud **Espaces de noms**, cliquez avec le bouton droit sur un espace de noms ou un dossier situé dans un espace de noms, puis cliquez sur **Nouveau dossier**.

3.  Dans la zone de texte **Nom**, tapez le nom du nouveau dossier.

4.  Pour ajouter une ou plusieurs cibles de dossier au dossier, cliquez sur **Ajouter**, indiquez le chemin d’accès UNC (Universal Naming Convention) de la cible de dossier, puis cliquez sur **OK**.


> [!TIP]
> Pour créer un dossier dans un espace de noms à l’aide de WindowsPowerShell, utilisez l’[applet de commande New-DfsnFolder](https://docs.microsoft.com/powershell/module/dfsn/new-dfsnfolder). Le module WindowsPowerShell DFSN a été introduit dans WindowsServer2012.


## <a name="see-also"></a>Articles associés

-   [Déploiement d’espaces de nomsDFS](deploying-dfs-namespaces.md)
-   [Déléguer les autorisations de gestion pour les espaces de nomsDFS](delegate-management-permissions-for-dfs-namespaces.md)


