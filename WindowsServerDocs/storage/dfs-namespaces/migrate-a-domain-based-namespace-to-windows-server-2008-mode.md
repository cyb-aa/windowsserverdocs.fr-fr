---
title: Migrer un espace de noms basé sur un domaine vers le mode Windows Server 2008
description: Cet article décrit comment migrer un espace de noms basé sur un domaine vers le mode Windows Server 2008.
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 75e366d6b72e9f08ece77558cff253239474777c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386211"
---
# <a name="migrate-a-domain-based-namespace-to-windows-server-2008-mode"></a>Migrer un espace de noms basé sur un domaine vers le mode Windows Server 2008

> S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008

Le mode Windows Server 2008 pour les espaces de noms de domaine prend en charge l’énumération basée sur l’accès et une extensibilité accrue.

## <a name="to-migrate-a-domain-based-namespace-to-windows-server-2008-mode"></a>Pour migrer un espace de noms basé sur un domaine vers le mode Windows Server 2008

Pour migrer un espace de noms de domaine du mode Windows 2000 Server vers le mode Windows Server 2008, vous devez exporter l’espace de noms dans un fichier, supprimer l’espace de noms, le recréer dans le mode Windows Server 2008, puis importer les paramètres de l’espace de noms. Pour ce faire, procédez comme suit :

1.  Ouvrez une fenêtre d’invite de commandes et tapez la commande suivante pour exporter l’espace de noms dans un fichier, où \\ @ no__t-1*domain*\\*namespace* est le nom du domaine approprié, et Namespace et *path @ no__t-6filename* est le chemin d’accès et le nom de fichier du fichier à exporter :
     ```
     Dfsutil root export \\domain\namespace path\filename.xml 
     ```
2.  Notez le chemin d’accès (\\ @ no__t-1*server* \\*share* ) pour chaque serveur d’espaces de noms. Vous devez ajouter manuellement les serveurs d’espaces de noms à l’espace de noms recréé, car Dfsutil ne peut pas importer les serveurs d’espaces de noms.
3.  Dans la console de gestion du système de fichiers distribués DFS, cliquez avec le bouton droit sur l’espace de noms, puis cliquez sur **Supprimer**, ou tapez la commande suivante à une invite de commandes, <br /> où \\ @ no__t-1*domain*\\*namespace* est le nom du domaine et de l’espace de noms appropriés :
     ```
     Dfsutil root remove \\domain\namespace
     ```
4.  Dans la console de gestion du système de fichiers distribués DFS, recréez l’espace de noms avec le même nom, mais utilisez le mode Windows Server 2008, ou tapez la commande suivante à une invite de commandes, où <br /> \\ @ no__t-1*server*\\*namespace* est le nom du serveur et du partage appropriés pour la racine de l’espace de noms :
     ```
     Dfsutil root adddom \\server\namespace v2
     ```
5.  Pour importer l’espace de noms à partir du fichier d’exportation, tapez la commande suivante à une invite de commandes, où <br /> \\ @ no__t-1 @no__t de l'*espace de noms* de*domaine*-3 est le nom du domaine et de l’espace de noms appropriés et le *chemin d’accès @ no__t-6filename* est le chemin d’accès et le nom du fichier à importer :
     ```
     Dfsutil root import merge path\filename.xml \\domain\namespace
     ```

    > [!NOTE]
    > Pour réduire le temps nécessaire pour importer un espace de noms de grande taille, exécutez la commande **Dfsutil** root import localement sur un serveur d’espaces de noms.
6.  Ajoutez les autres serveurs d’espaces de noms à l’espace de noms recréé en cliquant avec le bouton droit sur l’espace de noms dans la console de gestion du système de fichiers distribués DFS, puis en cliquant sur **Ajouter un serveur d’espaces de noms**, ou en tapant la commande suivante à une invite de commandes, où <br /> \\ @ no__t-1*server*\\*share* est le nom du serveur et du partage appropriés pour la racine de l’espace de noms :
     ```
     Dfsutil target add \\server\share 
     ```

    > [!NOTE]
    > Vous pouvez ajouter des serveurs d’espaces de noms avant d’importer l’espace de noms mais, avec cette opération, les serveurs d’espaces de noms téléchargent de manière incrémentielle les métadonnées pour l’espace de noms, au lieu de télécharger immédiatement l’espace de noms tout entier après ajout en tant que serveur d’espaces de noms.

## <a name="see-also"></a>Voir aussi
-   [Déploiement d’espaces de noms DFS](deploying-dfs-namespaces.md)
-   [Choisir un type d’espace de noms](choose-a-namespace-type.md)