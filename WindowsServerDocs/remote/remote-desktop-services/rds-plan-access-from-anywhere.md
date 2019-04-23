---
title: Services Bureau à distance - accès en tout lieu
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
ms.openlocfilehash: 2b10428ff90c50c4d7e113552ddda3cca5447b06
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845830"
---
# <a name="remote-desktop-services---access-from-anywhere"></a>Services Bureau à distance - accès en tout lieu

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Les utilisateurs finaux peuvent se connecter aux ressources réseau internes en toute sécurité à partir de l’extérieur du pare-feu d’entreprise via la passerelle Bureau à distance.

Quelle que soit la façon dont vous configurez les postes de travail pour vos utilisateurs finaux, vous pouvez facilement incorporer la passerelle Bureau à distance dans le flux de connexion pour une connexion rapide et sécurisée. Pour les utilisateurs finaux qui se connectent via un flux publiés, vous pouvez configurer la propriété de la passerelle Bureau à distance lorsque vous configurez les propriétés de déploiement global. Pour les utilisateurs finaux qui se connectent via à leurs bureaux sans un flux, ils peuvent facilement ajouter le nom de passerelle Bureau à distance de l’organisation comme propriété de connexion, quel que soit l’application de client de bureau à distance qu’ils utilisent.

Les trois principaux objectifs de la passerelle Bureau à distance, dans l’ordre de la séquence de connexion, sont :
1. **Établir un tunnel SSL chiffré entre l’appareil de l’utilisateur final et le serveur de passerelle Bureau à distance**: Pour pouvoir vous connecter via n’importe quel serveur de passerelle Bureau à distance, le serveur de passerelle Bureau à distance doit avoir un certificat installé qui reconnaît de l’appareil de l’utilisateur final. Dans les tests et les preuves de concepts, les certificats auto-signés peuvent être utilisés, mais seuls certificats approuvés publiquement à partir d’une autorité de certification doivent être utilisés dans n’importe quel environnement de production.
2. **Authentifier l’utilisateur dans l’environnement**: La passerelle Bureau à distance utilise la boîte de réception IIS pour exécuter l’authentification de service et peuvent utiliser même le protocole RADIUS pour tirer parti de [l’authentification multifacteur](rds-plan-mfa.md) solutions telles que Azure MFA. À part les stratégies par défaut est créés, vous pouvez créer des stratégies d’autorisation des ressources Bureau à distance (RD RAP) et stratégies d’autorisation de connexion Bureau à distance (RD CAP) supplémentaires pour définir plus spécifiquement les utilisateurs doivent avoir accès aux ressources au sein de la sécurité environnement.
3. **Passer le trafic dans les deux sens entre l’appareil de l’utilisateur final et la ressource spécifiée**: La passerelle Bureau à distance continue à effectuer cette tâche pour tant que la connexion est établie. Vous pouvez spécifier les propriétés de délai d’attente différente sur les serveurs de passerelle Bureau à distance pour maintenir la sécurité de l’environnement dans le cas où l’utilisateur éloigne de l’appareil.

Vous trouverez plus d’informations sur l’architecture générale d’un déploiement Services Bureau à distance [dans l’architecture de référence d’hébergement de bureau](desktop-hosting-reference-architecture.md).