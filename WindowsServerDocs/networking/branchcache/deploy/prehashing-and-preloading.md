---
title: Hachage préalable et préchargement du contenu sur les serveurs de Cache hébergé (facultatifs)
description: Cette rubrique fait partie de BranchCache déploiement Guide pour Windows Server2016, qui montre comment déployer BranchCache en mode de cache distribué et hébergé d’optimiser l’utilisation de la bande passante réseau étendu dans les filiales.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 5a09d9f1-1049-447f-a9bf-74adf779af27
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c3d1ed62c6dca5b1de0ff560fde0a2e43ed0d080
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="prehashing-and-preloading-content-on-hosted-cache-servers-optional"></a>Hachage préalable et préchargement du contenu sur les serveurs de Cache hébergé (facultatifs)

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette procédure pour forcer la création des informations de contenu - également appelées «hachages -» sur les serveurs Web et des fichiers prenant en charge BranchCache. Vous pouvez également collecter les données sur les serveurs de fichiers et web dans des packages qui peuvent être transférés vers les serveurs de cache hébergé à distance.  Cela vous offre la possibilité de précharger le contenu sur des serveurs de cache hébergé à distance afin que les données sont disponibles pour le premier accès client.  
  
Vous devez être membre du **administrateurs**, ou équivalent pour effectuer cette procédure.  
  
### <a name="to-prehash-content-and-preload-the-content-on-hosted-cache-servers"></a>Préhacher le contenu et précharger du contenu sur les serveurs de cache hébergé  
  
1.  Ouvrez une session sur le fichier ou le serveur Web qui contient les données que vous souhaitez précharger et identifiez les dossiers et fichiers que vous souhaitez charger sur un ou plusieurs serveurs de cache hébergé à distance.  
  
2.  Exécutez Windows PowerShell en tant qu’administrateur. Pour chaque dossier et le fichier, exécutez le `Publish-BCFileContent`commande ou `Publish-BCWebContent`commande, selon le type de serveur de contenu, pour déclencher la génération de hachage et d’ajouter des données à un package de données.  
  
3.  Une fois que toutes les données a été ajouté pour le package de données, l’exporter à l’aide de la `Export-BCCachePackage`commande pour produire un fichier de données de package.  
  
4.  Déplacer le fichier de données de package vers les serveurs de cache hébergé à distance à l’aide de votre choix de la technologie de transfert de fichier.  FTP, SMB, HTTP, DVD et disques durs portables sont viables tous les transports.  
  
5.  Importer le fichier de package de données sur les serveurs de cache hébergé à distance à l’aide de la `Import-BCCachePackage`commande.  
  

