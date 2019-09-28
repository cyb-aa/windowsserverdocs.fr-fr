---
title: Services Bureau à distance - Haute disponibilité
description: Informations de planification concernant la configuration d’un déploiement des services Bureau à distance hautement disponible.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ec630ea0-ab80-4dfe-a25f-f4f601651f72
author: lizap
ms.author: elizapo
ms.date: 09/07/2016
manager: dongill
ms.openlocfilehash: 3d3d216c528b0b83d9dfd5265fe153a388c7382a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387276"
---
# <a name="remote-desktop-services---high-availability"></a>Services Bureau à distance - Haute disponibilité

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016

Échecs et limitations sont inévitables dans les systèmes à grande échelle. Il est simple de configurer des rôles d’infrastructure de Bureau à distance pour prendre en charge la haute disponibilité et permettre aux utilisateurs finals de se connecter de manière fluide, à chaque fois.

Dans les services Bureau à distance, les éléments suivants représentent les rôles d’infrastructure de Bureau à distance, avec leurs instructions propres pour établir une haute disponibilité :
- [Service Broker pour les connexions Bureau à distance](Deploy-a-Remote-Desktop-Connection-Broker-cluster.md)
- [Passerelle des services Bureau à distance](Deploy-a-RD-Web-Access-and-Gateway-farm.md)
- Gestionnaire de licences des services Bureau à distance
- [Accès Bureau à distance par le web](Deploy-a-RD-Web-Access-and-Gateway-farm.md)

La haute disponibilité est établie en dupliquant chacun des services de rôles sur une seconde machine. Dans Azure, vous pouvez obtenir un temps d’activité garanti en plaçant le groupe des deux machines virtuelles (qui héberge le même rôle) dans des groupes à haute disponibilité.

Avec les groupes à haute disponibilité, vous pouvez tirer parti de la puissance d’Azure SQL Database et de son contrat SLA Azure, pour être sûr d’avoir toujours à votre disposition les informations de connexion et de pouvoir rediriger les utilisateurs vers leurs bureaux et leurs applications.

Pour des bonnes pratiques sur la création de votre environnement des services Bureau à distance, consultez [Architecture d’hébergement de bureaux](desktop-hosting-reference-architecture.md).