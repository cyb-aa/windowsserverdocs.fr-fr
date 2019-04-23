---
title: Hachage préalable et préchargement du contenu sur les serveurs de cache hébergé (facultatifs)
description: Cette rubrique fait partie de BranchCache déploiement Guide pour Windows Server 2016, qui montre comment déployer BranchCache en mode cache distribué et hébergé pour optimiser l’utilisation de la bande passante WAN dans les succursales.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 5a09d9f1-1049-447f-a9bf-74adf779af27
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b421132a44240520e3e3ba294623584c36b18ab4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867000"
---
# <a name="prehashing-and-preloading-content-on-hosted-cache-servers-optional"></a>Hachage préalable et préchargement du contenu sur les serveurs de cache hébergé (facultatifs)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette procédure pour forcer la création d’informations de contenu - également appelées hachages - sur les serveurs de fichiers et de Web prenant en charge BranchCache. Vous pouvez également collecter les données sur les serveurs de fichiers et web dans des packages qui peuvent être transférés vers les serveurs de cache hébergé à distance.  Cela vous offre la possibilité de précharger le contenu sur les serveurs de cache hébergé à distance afin que les données sont disponibles pour le premier accès au client.  
  
Vous devez être membre du **administrateurs**, ou équivalent pour effectuer cette procédure.  
  
### <a name="to-prehash-content-and-preload-the-content-on-hosted-cache-servers"></a>Préhacher le contenu et de précharger du contenu sur les serveurs de cache hébergé  
  
1.  Ouvrez une session sur le fichier ou le serveur Web qui contient les données que vous souhaitez précharger et identifiez les dossiers et fichiers que vous souhaitez charger sur un ou plusieurs serveurs de cache hébergé à distance.  
  
2.  Exécutez Windows PowerShell en tant qu’administrateur. Pour chaque dossier et le fichier, exécutez une le `Publish-BCFileContent` commande ou le `Publish-BCWebContent` commande, selon le type de serveur de contenu, pour déclencher la génération du hachage et d’ajouter des données à un package de données.  
  
3.  Une fois que toutes les données a été ajouté au package de données, l’exporter à l’aide de la `Export-BCCachePackage` commande pour générer un fichier de package de données.  
  
4.  Déplacer le fichier de package de données vers les serveurs de cache hébergé à distance à l’aide de votre choix de la technologie de transfert de fichier.  FTP, SMB, HTTP, DVD et disques durs portables sont viables tous les transports.  
  
5.  Importer le fichier de package de données sur les serveurs de cache hébergé à distance à l’aide de la `Import-BCCachePackage` commande.  
  

