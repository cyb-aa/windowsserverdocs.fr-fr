---
title: Préhacher et précharger le contenu sur le serveur de cache hébergé (facultatif)
description: Ce guide fournit des instructions sur le déploiement de BranchCache en mode de cache hébergé sur les ordinateurs exécutant Windows Server 2016 et Windows 10
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: 7e79c66a-8555-4d8e-8691-d6c37377aab4
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fe206576278b09e4a360c7bb27f5ff076af97be7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356254"
---
# <a name="prehash-and-preload-content-on-the-hosted-cache-server-optional"></a>Contenu de préhachage et de préchargement sur le serveur de cache hébergé \(Optional @ no__t-1

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Vous pouvez utiliser les procédures de cette section pour préhacher le contenu sur vos serveurs de contenu, ajouter le contenu aux packages de données, puis précharger le contenu sur vos serveurs de cache hébergé. 

Ces procédures sont facultatives, car vous n’êtes pas obligé d’effectuer un préhachage et de précharger du contenu sur vos serveurs de cache hébergé. 

Si vous ne Préchargez pas le contenu, les données sont automatiquement ajoutées au cache hébergé au fur et à mesure que les clients les téléchargent via la connexion WAN.

>[!IMPORTANT]
>Bien que ces procédures soient collectivement facultatives, si vous décidez de préhacher et de précharger du contenu sur vos serveurs de cache hébergé, vous devez effectuer ces deux procédures.

- [Créer des packages de données de serveur de contenu pour &#40;le Web et le contenu de fichier facultatif&#41;](8-Bc-Data-Packages.md)
  
- [Importer des packages de données sur le serveur &#40;de cache hébergé (facultatif)&#41;](9-Bc-Import-Data.md)

Pour continuer ce guide, consultez [créer des packages de données de serveur de contenu pour le &#40;Web&#41;et le contenu de fichier facultatif](8-Bc-Data-Packages.md).