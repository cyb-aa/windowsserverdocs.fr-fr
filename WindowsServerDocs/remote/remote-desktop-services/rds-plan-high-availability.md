---
title: Services Bureau à distance - haute disponibilité
description: Planification d’informations sur la configuration d’un déploiement de RDS hautement disponible.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: b5a2bd38c8831063d6fd2ba525b71a10403b8fc2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839260"
---
# <a name="remote-desktop-services---high-availability"></a>Services Bureau à distance - haute disponibilité

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Échecs et limitation sont inévitables dans les systèmes à grande échelle. Il est simple à configurer les rôles d’infrastructure de bureau à distance pour prendre en charge la haute disponibilité et permettre aux utilisateurs finaux pour se connecter en toute transparence, chaque fois.

Dans les Services Bureau à distance, les éléments suivants représentent les rôles d’infrastructure de bureau à distance, avec leurs instructions respectives pour établir une haute disponibilité :
- [Service Broker pour les connexions Bureau à distance](Deploy-a-Remote-Desktop-Connection-Broker-cluster.md)
- [Passerelle des services Bureau à distance](Deploy-a-RD-Web-Access-and-Gateway-farm.md)
- Gestionnaire de licences bureau à distance
- [Accès Web Bureau à distance](Deploy-a-RD-Web-Access-and-Gateway-farm.md)

Haute disponibilité est établie en dupliquant chacun des services de rôles sur un deuxième groupe de machines. Dans Azure, vous pouvez recevoir un temps d’activité garanti en plaçant l’ensemble des deux machines virtuelles (qui héberge le même rôle) dans un groupe de disponibilité définit.

En même temps que les groupes à haute disponibilité, vous pouvez désormais exploiter la puissance de la base de données SQL Azure et son contrat SLA reposant sur Azure pour vous assurer que vous avez toujours avez des informations de connexion et que vous pouvez rediriger les utilisateurs à leurs ordinateurs de bureau et les applications.

Pour obtenir des recommandations sur la création de votre environnement de services Bureau à distance, consultez le [architecture d’hébergement de bureau](desktop-hosting-reference-architecture.md).