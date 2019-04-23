---
title: Migrer un espace de noms basé sur un domaine vers le mode Windows Server 2008
description: Cet article décrit comment migrer un espace de noms basé sur un domaine vers le mode Windows Server 2008.
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: e2624e53d306dd198525b0abe8adf96fe6060bc1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847860"
---
# <a name="migrate-a-domain-based-namespace-to-windows-server-2008-mode"></a>Migrer un espace de noms basé sur un domaine vers le mode Windows Server 2008

> S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Le mode Windows Server 2008 pour les espaces de noms de domaine prend en charge l’énumération basée sur l’accès et une extensibilité accrue.

## <a name="to-migrate-a-domain-based-namespace-to-windows-server-2008-mode"></a>Pour migrer un espace de noms basé sur un domaine vers le mode Windows Server 2008

Pour migrer un espace de noms de domaine du mode Windows 2000 Server vers le mode Windows Server 2008, vous devez exporter l’espace de noms dans un fichier, supprimer l’espace de noms, le recréer dans le mode Windows Server 2008, puis importer les paramètres de l’espace de noms. Pour ce faire, procédez comme suit :

1.  Ouvrez une fenêtre d’invite de commandes et tapez la commande suivante pour exporter l’espace de noms dans un fichier, où \\ \\ *domaine*\\*espace de noms* est le nom de la domaine approprié et espace de noms et *chemin d’accès\\filename* est le chemin d’accès et le nom du fichier d’exportation :
     ```
     Dfsutil root export \\domain\namespace path\filename.xml 
     ```
2.  Notez le chemin d’accès ( \\ \\ *server* \\ *partager* ) pour chaque serveur d’espace de noms. Vous devez ajouter manuellement les serveurs d’espaces de noms à l’espace de noms recréé, car Dfsutil ne peut pas importer les serveurs d’espaces de noms.
3.  Dans la console de gestion du système de fichiers distribués DFS, cliquez avec le bouton droit sur l’espace de noms, puis cliquez sur **Supprimer**, ou tapez la commande suivante à une invite de commandes, <br /> où \\ \\ *domaine*\\*espace de noms* est le nom de domaine approprié et de l’espace de noms :
     ```
     Dfsutil root remove \\domain\namespace
     ```
4.  Dans la console de gestion du système de fichiers distribués DFS, recréez l’espace de noms avec le même nom, mais utilisez le mode Windows Server 2008, ou tapez la commande suivante à une invite de commandes, où <br /> \\\\*serveur*\\*espace de noms* est le nom du serveur approprié et de partage pour l’espace de noms racine :
     ```
     Dfsutil root adddom \\server\namespace v2
     ```
5.  Pour importer l’espace de noms à partir du fichier d’exportation, tapez la commande suivante à une invite de commandes, où <br /> \\\\*domaine*\\*espace de noms* est le nom de domaine approprié et de l’espace de noms et *chemin d’accès\\filename* est le chemin d’accès et le nom du fichier à importer :
     ```
     Dfsutil root import merge path\filename.xml \\domain\namespace
     ```

    > [!NOTE]
    > Pour réduire le temps nécessaire pour importer un espace de noms de grande taille, exécutez la commande **Dfsutil** root import localement sur un serveur d’espaces de noms.
6.  Ajoutez les autres serveurs d’espaces de noms à l’espace de noms recréé en cliquant avec le bouton droit sur l’espace de noms dans la console de gestion du système de fichiers distribués DFS, puis en cliquant sur **Ajouter un serveur d’espaces de noms**, ou en tapant la commande suivante à une invite de commandes, où <br /> \\\\*serveur*\\*partager* est le nom du serveur approprié et de partage pour l’espace de noms racine :
     ```
     Dfsutil target add \\server\share 
     ```

    > [!NOTE]
    > Vous pouvez ajouter des serveurs d’espaces de noms avant d’importer l’espace de noms mais, avec cette opération, les serveurs d’espaces de noms téléchargent de manière incrémentielle les métadonnées pour l’espace de noms, au lieu de télécharger immédiatement l’espace de noms tout entier après ajout en tant que serveur d’espaces de noms.

## <a name="see-also"></a>Voir aussi
-   [Déploiement d’espaces de noms DFS](deploying-dfs-namespaces.md)
-   [Choisissez un Type Namespace](choose-a-namespace-type.md)