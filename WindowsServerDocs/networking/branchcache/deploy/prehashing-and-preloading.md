---
title: Hachage préalable et préchargement du contenu sur les serveurs de cache hébergé (facultatifs)
description: Cette rubrique fait partie du Guide de déploiement BranchCache pour Windows Server 2016, qui montre comment déployer BranchCache en mode de cache distribué et hébergé pour optimiser l’utilisation de la bande passante du réseau étendu dans les filiales.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 5a09d9f1-1049-447f-a9bf-74adf779af27
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 7fe43b3a7c8dc7906e678a219b67ed096aa951d4
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319128"
---
# <a name="prehashing-and-preloading-content-on-hosted-cache-servers-optional"></a>Hachage préalable et préchargement du contenu sur les serveurs de cache hébergé (facultatifs)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette procédure pour forcer la création d’informations de contenu, également appelées hachages, sur des serveurs Web et de fichiers prenant en charge BranchCache. Vous pouvez également collecter les données sur des serveurs de fichiers et Web dans des packages qui peuvent être transférés vers des serveurs de cache hébergé à distance.  Cela vous permet de précharger du contenu sur des serveurs de cache hébergé à distance afin que les données soient disponibles pour le premier accès client.  
  
Vous devez être membre des **administrateurs**ou équivalent pour effectuer cette procédure.  
  
### <a name="to-prehash-content-and-preload-the-content-on-hosted-cache-servers"></a>Pour préhacher le contenu et précharger le contenu sur les serveurs de cache hébergé  
  
1.  Connectez-vous au fichier ou au serveur Web qui contient les données que vous souhaitez précharger, puis identifiez les dossiers et les fichiers que vous souhaitez charger sur un ou plusieurs serveurs de cache hébergé à distance.  
  
2.  Exécutez Windows PowerShell en tant qu’administrateur. Pour chaque dossier et fichier, exécutez la commande `Publish-BCFileContent` ou la commande `Publish-BCWebContent`, en fonction du type de serveur de contenu, pour déclencher la génération de hachage et ajouter des données à un package de données.  
  
3.  Une fois que toutes les données ont été ajoutées au package de données, exportez-les à l’aide de la commande `Export-BCCachePackage` pour produire un fichier de package de données.  
  
4.  Déplacez le fichier du package de données vers les serveurs de cache hébergé à distance à l’aide de votre choix de technologie de transfert de fichiers.  Les disques durs FTP, SMB, HTTP, DVD et portables sont tous des transports viables.  
  
5.  Importez le fichier de package de données sur les serveurs de cache hébergé à distance à l’aide de la commande `Import-BCCachePackage`.  
  

