---
title: "Migrer un espace de noms basé sur un domaine vers le mode WindowsServer2008"
description: "Cet article décrit comment migrer un espace de noms basé sur un domaine vers le mode WindowsServer2008."
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 1ef356e4f2abccb375732b7ec60a374c64dc0e7f
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="migrate-a-domain-based-namespace-to-windows-server-2008-mode"></a>Migrer un espace de noms basé sur un domaine vers le mode Windows Server2008

> S’applique à: WindowsServer (canal semi-annuel), WindowsServer2016, WindowsServer2012R2, WindowsServer2012, WindowsServer2008R2, WindowsServer2008

Le mode WindowsServer2008 pour les espaces de noms de domaine prend en charge l’énumération basée sur l’accès et une extensibilité accrue.

## <a name="to-migrate-a-domain-based-namespace-to-windows-server-2008-mode"></a>Pour migrer un espace de noms basé sur un domaine vers le mode WindowsServer2008

Pour migrer un espace de noms de domaine du mode Windows2000Server vers le mode WindowsServer2008, vous devez exporter l’espace de noms dans un fichier, supprimer l’espace de noms, le recréer dans le mode WindowsServer2008, puis importer les paramètres de l’espace de noms. Pour ce faire, procédez comme suit:

1.  Ouvrez une fenêtre d’invite de commandes et tapez la commande suivante pour exporter l’espace de noms vers un fichier, où \\\\*domain*\\*namespace* correspond au domaine et à l’espace de noms appropriés, et *path\\filename* correspond au chemin d’accès et au nom du fichier d’exportation:
     ```
     Dfsutil root export \\domain\namespace path\filename.xml 
     ```
2.  Notez le chemin d’accès ( \\\*server* \\*share* ) pour chaque serveur d’espaces de noms. Vous devez ajouter manuellement les serveurs d’espaces de noms à l’espace de noms recréé, car Dfsutil ne peut pas importer les serveurs d’espaces de noms.
3.  Dans la console de gestion du système de fichiers distribués DFS, cliquez avec le bouton droit sur l’espace de noms, puis cliquez sur **Supprimer**, ou tapez la commande suivante à une invite de commandes, <br /> où \\\\*domain*\\*namespace* correspond au domaine et à l’espace de noms appropriés:
     ```
     Dfsutil root remove \\domain\namespace
     ```
4.  Dans la console de gestion du système de fichiers distribués DFS, recréez l’espace de noms avec le même nom, mais utilisez le mode WindowsServer2008, ou tapez la commande suivante à une invite de commandes, où <br /> \\\\*server*\\*namespace* correspond au nom du serveur approprié et share correspond à la racine de l’espace de noms:
     ```
     Dfsutil root adddom \\server\namespace v2
     ```
5.  Pour importer l’espace de noms à partir du fichier d’exportation, tapez la commande suivante à une invite de commandes, où <br /> \\\\*domain*\\*namespace* correspond au domaine et à l’espace de noms appropriés et *path\\filename* correspond au chemin d’accès et au nom du fichier d’importation:
     ```
     Dfsutil root import merge path\filename.xml \\domain\namespace
     ```

    > [!NOTE]
    > Pour réduire le temps nécessaire pour importer un espace de noms de grande taille, exécutez la commande **Dfsutil** root import localement sur un serveur d’espaces de noms.
6.  Ajoutez les autres serveurs d’espaces de noms à l’espace de noms recréé en cliquant avec le bouton droit sur l’espace de noms dans la console de gestion du système de fichiers distribués DFS, puis en cliquant sur **Ajouter un serveur d’espaces de noms**, ou en tapant la commande suivante à une invite de commandes, où <br /> \\\\*server*\\*share* correspond au nom du serveur approprié et share correspond à la racine de l’espace de noms:
     ```
     Dfsutil target add \\server\share 
     ```

    > [!NOTE]
    > Vous pouvez ajouter des serveurs d’espaces de noms avant d’importer l’espace de noms mais, avec cette opération, les serveurs d’espaces de noms téléchargent de manière incrémentielle les métadonnées pour l’espace de noms, au lieu de télécharger immédiatement l’espace de noms tout entier après ajout en tant que serveur d’espaces de noms.

## <a name="see-also"></a>Articles associés
-   [Déploiement d’espaces de nomsDFS](deploying-dfs-namespaces.md)
-   [Choisir un type d’espace de noms](choose-a-namespace-type.md)