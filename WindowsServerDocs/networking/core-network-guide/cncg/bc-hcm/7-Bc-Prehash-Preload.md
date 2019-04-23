---
title: Préhacher et précharger le contenu sur le serveur de cache hébergé (facultatif)
description: Ce guide fournit des instructions sur le déploiement de BranchCache en mode de cache hébergé sur les ordinateurs exécutant Windows Server 2016 et Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 7e79c66a-8555-4d8e-8691-d6c37377aab4
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b60a1f24b8988d6e394df0faf678467021e0c882
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839390"
---
# <a name="prehash-and-preload-content-on-the-hosted-cache-server-optional"></a>Préhacher et précharger du contenu sur le serveur de Cache hébergé \(facultatif\)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous pouvez utiliser les procédures dans cette section à préhacher le contenu sur vos serveurs de contenu, ajoutez le contenu aux packages de données et puis précharger le contenu sur vos serveurs de cache hébergé. 

Ces procédures sont facultatives, car vous n’êtes pas obligé de prehash et préchargement du contenu sur vos serveurs de cache hébergé. 

Si vous ne pas précharger contenu, données sont automatiquement ajoutées au cache hébergé, comme les clients la téléchargent via la connexion WAN.

>[!IMPORTANT]
>Bien que ces procédures sont collectivement facultatifs, si vous décidez de prehash et préchargement du contenu sur vos serveurs de cache hébergé, les deux procédures est requis.

- [Créer des Packages de données de serveur de contenu pour le Web et le contenu du fichier &#40;facultatif&#41;](8-Bc-Data-Packages.md)
  
- [Importer des Packages de données sur le serveur de Cache hébergé &#40;facultatif&#41;](9-Bc-Import-Data.md)

Pour continuer avec ce guide, consultez [créer des Packages de données serveur de contenu pour le Web et le contenu du fichier &#40;facultatif&#41;](8-Bc-Data-Packages.md).