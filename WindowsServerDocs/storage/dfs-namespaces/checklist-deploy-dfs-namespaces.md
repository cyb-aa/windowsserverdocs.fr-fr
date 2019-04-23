---
title: Liste de vérification Déployer des espaces de noms DFS
description: Cet article explique comment configurer et déployer des espaces de noms DFS.
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 7f7cca6b67ff6fa8d81e88323381866315f07f33
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842120"
---
# <a name="checklist-deploy-dfs-namespaces"></a>Liste de vérification : Déployer des espaces de noms DFS

> S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Distribuée espaces de noms DFS (File System) et la réplication DFS peuvent être utilisé pour publier des documents, des logiciels et des données de line-of-business aux utilisateurs d’une organisation. Bien que seule la réplication DFS est suffisante pour distribuer des données, vous pouvez utiliser des espaces de noms DFS pour configurer l’espace de noms afin qu’un dossier est hébergé par plusieurs serveurs, chacun d’eux conserve une copie mise à jour du dossier. Cela augmente la disponibilité des données et répartit la charge du client entre les serveurs.

Quand ils parcourent le dossier dans l’espace de noms, les utilisateurs ignorent que le dossier est hébergé par plusieurs serveurs. Quand un utilisateur ouvre le dossier, l’ordinateur client est automatiquement renvoyé à un serveur sur son site. Si aucun serveur du même site n’est disponibles, vous pouvez configurer l’espace de noms pour rediriger le client vers un serveur doté de la connexion la plus faible coût tel que défini dans les Services de répertoire Active Directory (AD DS).

Pour déployer des espaces de noms DFS, effectuez les tâches suivantes :

-   Passer en revue les concepts et la configuration requise pour les espaces de noms DFS.
[Vue d’ensemble des espaces de noms DFS](dfs-overview.md)
-   [Choisissez un type d’espace de noms](choose-a-namespace-type.md)
-   [Créer un espace de noms DFS](create-a-dfs-namespace.md) 
-   Migrer des espaces de noms basés sur un domaine existants vers des espaces de noms basés sur un domaine en mode Windows Server 2008. [Migrer un Namespace basés sur un domaine en mode Windows Server 2008](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md) 
-   Augmenter la disponibilité en ajoutant des serveurs d’espaces de noms à un espace de noms basé sur un domaine. [Ajouter des serveurs de Namespace à un Namespace DFS de domaine](add-namespace-servers-to-a-domain-based-dfs-namespace.md)
-   Ajouter des dossiers à un espace de noms. [Créez un dossier dans un Namespace DFS](create-a-folder-in-a-dfs-namespace.md)
-   Ajouter des cibles de dossier à des dossiers dans un espace de noms. [Ajouter des cibles de dossier](add-folder-targets.md)
-   Répliquer du contenu entre les cibles de dossier à l’aide de la réplication DFS (facultatif). [Répliquer des cibles de dossier à l’aide de la réplication DFS](replicate-folder-targets-using-dfs-replication.md)


## <a name="see-also"></a>Voir aussi

-   [Espaces de noms](https://technet.microsoft.com/library/cc771914(v=ws.11).aspx)
-   [Liste de vérification : Paramétrer un Namespace DFS](checklist-tune-a-dfs-namespace.md)
-   [Réplication](https://technet.microsoft.com/library/cc770278(v=ws.11).aspx)


