---
title: "Liste de vérification Déployer des espaces de nomsDFS"
description: "Cet article explique comment configurer et déployer des espaces de nomsDFS."
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 8f1dd7a8a44f5427d6464a6be2057a529638eca7
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="checklist-deploy-dfs-namespaces"></a>Liste de vérification: Déployer des espaces de nomsDFS

> S’applique à: WindowsServer (canal semi-annuel), WindowsServer2016, WindowsServer2012R2, WindowsServer2012, WindowsServer2008R2, WindowsServer2008

Les espaces de noms DFS et la réplication DFS peuvent être utilisés pour publier des documents, des logiciels et des données métier destinés aux utilisateurs d’une organisation. Même si la réplicationDFS à elle seule est suffisante pour distribuer des données, vous pouvez utiliser les espaces de nomsDFS pour configurer l’espace de noms de façon à ce qu’un dossier soit hébergé par plusieurs serveurs détenant chacun une copie à jour du dossier. Cela augmente la disponibilité des données et répartit la charge du client entre les serveurs.

Quand ils parcourent le dossier dans l’espace de noms, les utilisateurs ignorent que le dossier est hébergé par plusieurs serveurs. Quand un utilisateur ouvre le dossier, l’ordinateur client est automatiquement renvoyé à un serveur sur son site. Si aucun serveur du même site n’est disponible, vous pouvez configurer l’espace de noms afin de faire référencer le client par un serveur ayant le coût de connexion le moins élevé, tel que défini dans Active Directory Directory Services (ADDS).

Pour déployer des espaces de noms DFS, effectuez les tâches suivantes:

-   Passer en revue les concepts et la configuration requise pour les espaces de nomsDFS.
[Vued’ensemble des espaces de nomsDFS](dfs-overview.md)
-   [Choisir un type d’espace de noms](choose-a-namespace-type.md)
-   [Créer un espace de nomsDFS](create-a-dfs-namespace.md) 
-   Migrer des espaces de noms basés sur un domaine existants vers des espaces de noms basés sur un domaine en mode WindowsServer2008. [Migrer un espace de noms basé sur un domaine vers le mode WindowsServer2008](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md) 
-   Augmenter la disponibilité en ajoutant des serveurs d’espaces de noms à un espace de noms basé sur un domaine. [Ajouter des serveurs d’espaces de noms à un espace de nomsDFS basé sur un domaine](add-namespace-servers-to-a-domain-based-dfs-namespace.md)
-   Ajouter des dossiers à un espace de noms. [Créer un dossier dans un espace de nomsDFS](create-a-folder-in-a-dfs-namespace.md)
-   Ajouter des cibles de dossier à des dossiers dans un espace de noms. [Ajouter des cibles de dossier](add-folder-targets.md)
-   Répliquer du contenu entre les cibles de dossier à l’aide de la réplicationDFS (facultatif). [Répliquer des cibles de dossier à l’aide de la réplicationDFS](replicate-folder-targets-using-dfs-replication.md)


## <a name="see-also"></a>Articles associés

-   [Espaces de noms](https://technet.microsoft.com/library/cc771914(v=ws.11).aspx)
-   [Liste de vérification: Régler un espace de nomDFS](checklist-tune-a-dfs-namespace.md)
-   [Réplication](https://technet.microsoft.com/library/cc770278(v=ws.11).aspx)


