---
title: Préhacher et précharger du contenu sur le serveur de Cache hébergé (facultatif)
description: Ce guide fournit des instructions sur le déploiement de BranchCache en mode de cache hébergé sur les ordinateurs exécutant Windows Server2016 et Windows10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 7e79c66a-8555-4d8e-8691-d6c37377aab4
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0e7ffaac4e427222d5539195ecef91768f61c4a3
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="prehash-and-preload-content-on-the-hosted-cache-server-optional"></a>Préhacher et précharger du contenu sur hébergé \(Optional\) serveur de Cache

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016, Windows Server2012R2, Windows Server2012

Vous pouvez utiliser les procédures de cette section pour préhacher le contenu sur vos serveurs de contenu, ajoutez le contenu des packages de données et puis précharger du contenu sur vos serveurs de cache hébergé. 

Ces procédures sont facultatives, car vous n’êtes pas obligé de prehash et préchargement du contenu sur vos serveurs de cache hébergé. 

Si vous ne préchargez pas de contenu, données sont automatiquement ajoutées au cache hébergé que les clients la téléchargent via la connexion de réseau étendue.

>[!IMPORTANT]
>Bien que ces procédures sont collectivement facultatives, si vous décidez de prehash et préchargement du contenu sur vos serveurs de cache hébergé, les deux procédures est requis.

- [Créer des Packages de données de serveur de contenu pour le Web et de contenu du fichier et #40; facultatif et #41;](8-Bc-Data-Packages.md)
  
- [Importer des Packages de données sur le serveur de Cache hébergé et #40; facultatif et #41;](9-Bc-Import-Data.md)

Pour continuer avec ce guide, consultez [créer des Packages de données de serveur de contenu Web et de contenu du fichier et #40; facultatif et #41;](8-Bc-Data-Packages.md).