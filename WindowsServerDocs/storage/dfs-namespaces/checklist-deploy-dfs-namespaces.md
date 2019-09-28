---
title: Liste de vérification Déployer des espaces de noms DFS
description: Cet article explique comment configurer et déployer des espaces de noms DFS.
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: ab38c41c32ec88285a69fb94e62abc1453ddc3d1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402274"
---
# <a name="checklist-deploy-dfs-namespaces"></a>Liste de vérification : Déployer des espaces de noms DFS

> S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008

Les espaces de noms et les réplication DFS système de fichiers DFS (DFS) peuvent être utilisés pour publier des documents, des logiciels et des données métier aux utilisateurs au sein d’une organisation. Même si réplication DFS seule est suffisante pour distribuer des données, vous pouvez utiliser des espaces de noms DFS pour configurer l’espace de noms afin qu’un dossier soit hébergé par plusieurs serveurs, chacun contenant une copie mise à jour du dossier. Cela augmente la disponibilité des données et répartit la charge du client entre les serveurs.

Quand ils parcourent le dossier dans l’espace de noms, les utilisateurs ignorent que le dossier est hébergé par plusieurs serveurs. Quand un utilisateur ouvre le dossier, l’ordinateur client est automatiquement renvoyé à un serveur sur son site. Si aucun serveur de site n’est disponible, vous pouvez configurer l’espace de noms pour qu’il fasse référence au client sur un serveur dont le coût de connexion est le plus faible, tel que défini dans Active Directory Directory Services (AD DS).

Pour déployer des espaces de noms DFS, effectuez les tâches suivantes :

-   Passer en revue les concepts et la configuration requise pour les espaces de noms DFS.
[Vue d’ensemble des espaces de noms DFS](dfs-overview.md)
-   [Choisir un type d’espace de noms](choose-a-namespace-type.md)
-   [Créer un espace de noms DFS](create-a-dfs-namespace.md) 
-   Migrer des espaces de noms basés sur un domaine existants vers des espaces de noms basés sur un domaine en mode Windows Server 2008. [Migrer un espace de noms basé sur un domaine vers le mode Windows Server 2008](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md) 
-   Augmenter la disponibilité en ajoutant des serveurs d’espaces de noms à un espace de noms basé sur un domaine. [Ajouter des serveurs d’espaces de noms à un espace de noms DFS basé sur un domaine](add-namespace-servers-to-a-domain-based-dfs-namespace.md)
-   Ajouter des dossiers à un espace de noms. [Créer un dossier dans un espace de noms DFS](create-a-folder-in-a-dfs-namespace.md)
-   Ajouter des cibles de dossier à des dossiers dans un espace de noms. [Ajouter des cibles de dossier](add-folder-targets.md)
-   Répliquer du contenu entre les cibles de dossier à l’aide de la réplication DFS (facultatif). [Répliquer les cibles de dossier à l’aide de réplication DFS](replicate-folder-targets-using-dfs-replication.md)


## <a name="see-also"></a>Voir aussi

-   [Espaces](https://technet.microsoft.com/library/cc771914(v=ws.11).aspx)
-   [Liste de vérification : Optimiser un espace de noms DFS](checklist-tune-a-dfs-namespace.md)
-   [La](https://technet.microsoft.com/library/cc770278(v=ws.11).aspx)


