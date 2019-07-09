---
title: Services Bureau à distance - Accès en tout lieu
description: Informations de planification pour une passerelle Bureau à distance
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5c38fab1-3586-4b7a-8bf0-7d85a8d5361d
author: lizap
ms.author: elizapo
ms.date: 11/03/2016
manager: dongill
ms.openlocfilehash: 0d3d8ed036b3befd81da6d5bbe8702ee866c6aa8
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/17/2019
ms.locfileid: "63748822"
---
# <a name="remote-desktop-services---access-from-anywhere"></a>Services Bureau à distance - Accès en tout lieu

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016

Les utilisateurs finals peuvent se connecter aux ressources réseau internes en toute sécurité à partir de l’extérieur du pare-feu d’entreprise via la passerelle Bureau à distance.

Quelle que soit la configuration des bureaux pour vos utilisateurs finals, vous pouvez facilement incorporer la passerelle Bureau à distance dans le flux de connexion pour une connexion rapide et sécurisée. Pour les utilisateurs finals qui se connectent via des flux publiés, vous pouvez configurer la propriété de passerelle Bureau à distance lorsque vous configurez les propriétés de déploiement global. Pour les utilisateurs finals qui se connectent via leurs bureaux sans flux, ils peuvent facilement ajouter le nom de passerelle Bureau à distance de l’organisation comme propriété de connexion, quelle que soit l’application de client Bureau à distance qu’ils utilisent.

Les trois principaux objectifs de la passerelle Bureau à distance, dans l’ordre de la séquence de connexion, sont :
1. **Établir un tunnel SSL chiffré entre l’appareil de l’utilisateur final et le serveur de passerelle Bureau à distance** : pour pouvoir vous connecter via n’importe quel serveur de passerelle Bureau à distance, le serveur de passerelle Bureau à distance doit avoir un certificat installé que l’appareil de l’utilisateur final reconnaît. Dans les tests et les preuves de concepts, des certificats auto-signés peuvent être utilisés, mais seuls les certificats approuvés publiquement d’une autorité de certification doivent être utilisés dans n’importe quel environnement de production.
2. **Authentifier l’utilisateur dans l’environnement** : la passerelle Bureau à distance utilise le service IIS intégré pour effectuer l’authentification, et peut même utiliser le protocole RADIUS pour tirer parti des solutions [d’authentification multifacteur](rds-plan-mfa.md) telles qu’Azure MFA. En plus des stratégies par défaut générées, vous pouvez créer des stratégies d’autorisation des ressources Bureau à distance et des stratégies d’autorisation de connexion Bureau à distance supplémentaires pour définir plus spécifiquement quels utilisateurs doivent avoir accès à quelles ressources au sein de l’environnement sécurisé.
3. **Assurer le trafic dans les deux sens entre l’appareil de l’utilisateur final et la ressource spécifiée** : la passerelle Bureau à distance continue à effectuer cette tâche tant que la connexion est établie. Vous pouvez spécifier différentes propriétés de délai d’attente sur les serveurs de passerelle Bureau à distance pour maintenir la sécurité de l’environnement si l’utilisateur s’éloigne de l’appareil.

Vous trouverez plus d’informations sur l’architecture générale d’un déploiement Services Bureau à distance [dans l’architecture de référence d’hébergement de bureaux](desktop-hosting-reference-architecture.md).