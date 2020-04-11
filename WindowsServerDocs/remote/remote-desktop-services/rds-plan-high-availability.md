---
title: Services Bureau à distance - Haute disponibilité
description: Informations de planification concernant la configuration d’un déploiement des services Bureau à distance hautement disponible.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: ec630ea0-ab80-4dfe-a25f-f4f601651f72
author: lizap
ms.author: elizapo
ms.date: 09/07/2016
manager: dongill
ms.openlocfilehash: 86c5f59fcd403e838a316174840b93608d7f06ec
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858142"
---
# <a name="remote-desktop-services---high-availability"></a>Services Bureau à distance - Haute disponibilité

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016

Échecs et limitations sont inévitables dans les systèmes à grande échelle. Il est simple de configurer des rôles d’infrastructure de Bureau à distance pour prendre en charge la haute disponibilité et permettre aux utilisateurs finals de se connecter de manière fluide, à chaque fois.

Dans les services Bureau à distance, les éléments suivants représentent les rôles d’infrastructure de Bureau à distance, avec leurs instructions propres pour établir une haute disponibilité :
- [Service Broker pour les connexions Bureau à distance](Deploy-a-Remote-Desktop-Connection-Broker-cluster.md)
- [Passerelle des services Bureau à distance](Deploy-a-RD-Web-Access-and-Gateway-farm.md)
- Gestionnaire de licences des services Bureau à distance
- [Accès Bureau à distance par le web](Deploy-a-RD-Web-Access-and-Gateway-farm.md)

La haute disponibilité est établie en dupliquant chacun des services de rôles sur une seconde machine. Dans Azure, vous pouvez obtenir un temps d’activité garanti en plaçant le groupe des deux machines virtuelles (qui héberge le même rôle) dans des groupes à haute disponibilité.

Avec les groupes à haute disponibilité, vous pouvez tirer parti de la puissance d’Azure SQL Database et de son contrat SLA Azure, pour être sûr d’avoir toujours à votre disposition les informations de connexion et de pouvoir rediriger les utilisateurs vers leurs bureaux et leurs applications.

Pour des bonnes pratiques sur la création de votre environnement des services Bureau à distance, consultez [Architecture d’hébergement de bureaux](desktop-hosting-reference-architecture.md).