---
title: "Créer un espace de nomsDFS"
description: "Cet article explique comment créer un espace de nomsDFS."
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: b01a83f165e0ef01c6413dcf8785435c8f3aca5a
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="create-a-dfs-namespace"></a>Créer un espace de nomsDFS

> S’applique à: WindowsServer (canal semi-annuel), WindowsServer2016, WindowsServer2012R2, WindowsServer2012, WindowsServer2008R2, WindowsServer2008

Pour créer un nouvel espace de noms, vous pouvez utiliser le Gestionnaire de serveur pour créer l’espace de noms lorsque vous installez le service de rôle des espaces de nomsDFS. Vous pouvez également utiliser l’[applet de commande New-DfsnRoot](https://docs.microsoft.com/powershell/module/dfsn/new-dfsnroot) à partir d’une session WindowsPowerShell. 

Le module WindowsPowerShell DFSN a été introduit dans WindowsServer2012. 

Vous pouvez également créer un espace de noms à l’aide de la procédure suivante après avoir installé le service de rôle.

## <a name="to-create-a-namespace"></a>Pour créer un espace de noms:

1.  Cliquez sur **Démarrer**, pointez sur **Outils d’administration**, puis cliquez sur **GestionDFS**.

2.  Dans l’arborescence de la console, cliquez avec le bouton droit sur le nœud **Espaces de noms**, puis cliquez sur **Nouvel espace de noms**.

3.  Suivez les instructions affichées dans l’**Assistant Nouvel espace de noms**.

    Pour créer un espace de noms autonome sur un cluster de basculement, spécifiez le nom d’une instance de serveur de fichiers en cluster sur la page **Serveur d’espaces de noms** de l’**Assistant Nouvel espace de noms**.

> [!IMPORTANT]
> N’essayez pas de créer un espace de noms basé sur un domaine avec le mode WindowsServer2008, sauf si le niveau fonctionnel de la forêt est Windows Server2003 ou version ultérieure. Cela peut générer un espace de noms pour lequel vous ne pouvez pas supprimer les dossiersDFS, avec le message d’erreur suivant: «Le dossier ne peut pas être supprimé. Impossible d’accomplir cette fonction.»

## <a name="see-also"></a>Articles associés

-   [Déploiement d’espaces de nomsDFS](deploying-dfs-namespaces.md)
-   [Choisir un type d’espace de noms](choose-a-namespace-type.md)
-   [Ajouter des serveurs d’espaces de noms à un espace de nomsDFS basé sur un domaine](add-namespace-servers-to-a-domain-based-dfs-namespace.md)
-   [Déléguer les autorisations de gestion pour les espaces de nomsDFS](delegate-management-permissions-for-dfs-namespaces.md).


