---
title: Services Bureau à distance - Accès en tout lieu
description: Informations de planification pour une passerelle Bureau à distance
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: 5c38fab1-3586-4b7a-8bf0-7d85a8d5361d
author: lizap
ms.author: elizapo
ms.date: 11/03/2016
manager: dongill
ms.openlocfilehash: fb0fddbe86c4c06280fdbe55f2f6a7f490a1340b
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80857392"
---
# <a name="remote-desktop-services---access-from-anywhere"></a>Services Bureau à distance - Accès en tout lieu

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016

Les utilisateurs finals peuvent se connecter aux ressources réseau internes en toute sécurité à partir de l’extérieur du pare-feu d’entreprise via la passerelle Bureau à distance.

Quelle que soit la configuration des bureaux pour vos utilisateurs finals, vous pouvez facilement incorporer la passerelle Bureau à distance dans le flux de connexion pour une connexion rapide et sécurisée. Pour les utilisateurs finals qui se connectent via des flux publiés, vous pouvez configurer la propriété de passerelle Bureau à distance lorsque vous configurez les propriétés de déploiement global. Pour les utilisateurs finals qui se connectent via leurs bureaux sans flux, ils peuvent facilement ajouter le nom de passerelle Bureau à distance de l’organisation comme propriété de connexion, quelle que soit l’application de client Bureau à distance qu’ils utilisent.

Les trois principaux objectifs de la passerelle Bureau à distance, dans l’ordre de la séquence de connexion, sont :
1. **Établir un tunnel SSL chiffré entre l’appareil de l’utilisateur final et le serveur de passerelle Bureau à distance** : pour pouvoir vous connecter via n’importe quel serveur de passerelle Bureau à distance, le serveur de passerelle Bureau à distance doit avoir un certificat installé que l’appareil de l’utilisateur final reconnaît. Dans les tests et les preuves de concepts, des certificats auto-signés peuvent être utilisés, mais seuls les certificats approuvés publiquement d’une autorité de certification doivent être utilisés dans n’importe quel environnement de production.
2. **Authentifier l’utilisateur dans l’environnement** : la passerelle Bureau à distance utilise le service IIS intégré pour effectuer l’authentification, et peut même utiliser le protocole RADIUS pour tirer parti des solutions [d’authentification multifacteur](rds-plan-mfa.md) telles qu’Azure MFA. En plus des stratégies par défaut générées, vous pouvez créer des stratégies d’autorisation des ressources Bureau à distance et des stratégies d’autorisation de connexion Bureau à distance supplémentaires pour définir plus spécifiquement quels utilisateurs doivent avoir accès à quelles ressources au sein de l’environnement sécurisé.
3. **Assurer le trafic dans les deux sens entre l’appareil de l’utilisateur final et la ressource spécifiée** : la passerelle Bureau à distance continue à effectuer cette tâche tant que la connexion est établie. Vous pouvez spécifier différentes propriétés de délai d’attente sur les serveurs de passerelle Bureau à distance pour maintenir la sécurité de l’environnement si l’utilisateur s’éloigne de l’appareil.

Vous trouverez plus d’informations sur l’architecture générale d’un déploiement Services Bureau à distance [dans l’architecture de référence d’hébergement de bureaux](desktop-hosting-reference-architecture.md).